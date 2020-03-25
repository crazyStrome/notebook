<!-- TOC -->

- [基本数据结构](#基本数据结构)
    - [slice](#slice)
        - [slice的扩容](#slice的扩容)
        - [make和new](#make和new)
        - [slice和unsafe.Pointer相互转换](#slice和unsafepointer相互转换)
    - [map的实现](#map的实现)
        - [数据结构](#数据结构)
        - [增量扩容](#增量扩容)
        - [查找过程](#查找过程)
        - [插入过程分析](#插入过程分析)
    - [nil的语义](#nil的语义)
        - [interface](#interface)
        - [string和slice](#string和slice)
        - [channel和map](#channel和map)
- [函数调用协议](#函数调用协议)
    - [go关键字](#go关键字)
    - [defer关键字](#defer关键字)
        - [defer的实现](#defer的实现)
    - [连续栈](#连续栈)
        - [基本原理](#基本原理)
        - [实现过程](#实现过程)
        - [具体细节](#具体细节)
            - [如何捕获栈的空间不足](#如何捕获栈的空间不足)

<!-- /TOC -->
#  基本数据结构

##  slice

在内存中slice是包含三个域的结构体：指向slice中第一个元素的指针，slice的长度以及slice的容量。长度是下标操作的上界，容量是分割操作的上界。

数组的slice并不会复制一份数据，只是建立一个新的数据结构

###  slice的扩容

```go
struct    Slice
{    // must not move anything
    byte*    array;        // actual data
    uintgo    len;        // number of elements
    uintgo    cap;        // allocated number of elements
};
```

当对slice进行append操作时，可能会造成slice的自动扩容。其扩容的增长规则：

* 如果新的大小是当前两倍，则大小增长为新大小
* 否则循环以下操作：如果当前大小小于1024，按每次2倍增长，否则按当前大小的1/4增长。直到增长的大小超过或等于新大小

###  make和new

Go有两个数据结构创建函数：new和make。new(T)返回一个*T，返回的这个指针可以被隐式的消除引用。而make(T,args)返回一个普通的T。new返回一个指向已经清零内存的指针，make返回一个复杂结构。

###  slice和unsafe.Pointer相互转换

从slice得到一块内存地址

```go
s :=make([]byte, 200)
ptr := unsafe.Pointer(&s[0])
```

从内存指针构造出Go语言的slice结构相对麻烦一些：

```go
var ptr unsafe.Pointer
s := ((*[1<<10]byte)(ptr))[:200]
```

或者

```go
var ptr unsafe.Pointer
var s1 = struct {
    addr uintptr
    len int
    cap int
}{ptr, length, length}
s := *(*[]byte)(unsafe.Pointer(&s1))
```

或者使用sliceHeader来构造：

```go
var o []byte
sliceHeader := (*reflect.SliceHeader)((unsafe.Pointer(&o)))
sliceHeader.Cap = length
sliceHeader.Len = length
sliceHeader.Data = uintptr(ptr)
```

##  map的实现

Go的map在底层是用hash实现的

###  数据结构

哈希表的数据结构：

```go
struct Hmap
{
    uint8   B;    // 可以容纳2^B个项
    uint16  bucketsize;   // 每个桶的大小

    byte    *buckets;     // 2^B个Buckets的数组
    byte    *oldbuckets;  // 前一个buckets，只有当正在扩容时才不为空
};
```

使用的Bucket的数组，而不是Bucket*的数组，这意味着，第一个Bucket是连续的内存空间，而之后扩容的Bucket是mallocgc分配的。

每次扩容，会增大到上次大小的两倍。

具体的Bucket的结构如下：

```go
struct Bucket
{
    uint8  tophash[BUCKETSIZE]; // hash值的高8位....低位从bucket的array定位到bucket
    Bucket *overflow;           // 溢出桶链表，如果有
    byte   data[1];             // BUCKETSIZE keys followed by BUCKETSIZE values
};
```

其中BUCKETSIZE是用宏定义的8，每个Bucket只能最多存放8个key-value对，如果多于8个，那么会申请一个新的bucket，并将它和之前的bucket链起来。

按key的类型采用相应的hash算法得到key的hash值。将hash值的低位当做Hmap结构体中buckets数组的index，找到key所造的bucket。将hash的高8位储存在bucket的tophash中。**这里，高8位不是用来挡在key/value在bucket内部的offset的，而是作为一个主键，在查找时对tophash数组的每一项进行顺序匹配的**。先比较hash的高位与bucket的tophash[i]是否相等，如果相等则再比较bucket的第i个key与所给的key是否相等。如果相等，则返回对应的value，反之，在overflow buckets中继续寻找。

go考虑到字节对齐，将key和value分开放置。

###  增量扩容

如果扩容前的哈希表大小为$$2^B$$，扩容之后的大小为$$2^(B+1)$$，每次扩容都变为原来大小的两倍，哈希表大小始终为2的指数倍，则有(hash mod 2^B)等价于(hash & (2^B-1))。这样可以简化运算，避免了取余操作。

假设扩容之前容量为X，扩容之后容量为Y，对于某个哈希值hash，一般情况下(hash mod X)不等于(hash mod Y)，所以扩容之后要重新计算每一项在哈希表中的新位置。当hash表扩容之后，需要将那些旧的pair重新哈希到新的table上(源代码中称之为evacuate)， 这个工作并没有在扩容之后一次性完成，而是逐步的完成（在insert和remove时每次搬移1-2个pair），Go语言使用的是增量扩容。

为什么会增量扩容呢？主要是缩短map容器的响应时间。假如我们直接将map用作某个响应实时性要求非常高的web应用存储，如果不采用增量扩容，当map里面存储的元素很多之后，扩容时系统就会卡往，导致较长一段时间内无法响应请求。不过增量扩容本质上还是将总的扩容时间分摊到了每一次哈希操作上面。

扩容会建立一个大小是原来2倍的新的表，将旧的bucket搬到新的表中之后，并不会将旧的bucket从oldbucket中删除，而是加上一个已删除的标记。

正是由于这个工作是逐渐完成的，这样就会导致一部分数据在old table中，一部分在new table中， 所以对于hash table的insert, remove, lookup操作的处理逻辑产生影响。只有当所有的bucket都从旧表移到新表之后，才会将oldbucket释放掉。

扩容的填充因子是多少呢？如果grow的太频繁，会造成空间的利用率很低， 如果很久才grow，会形成很多的overflow buckets，查找的效率也会下降。 这个平衡点如何选取呢(在go中，这个平衡点是有一个宏控制的(#define LOAD 6.5), 它的意思是这样的，如果table中元素的个数大于table中能容纳的元素的个数， 那么就触发一次grow动作

###  查找过程

1. 根据key计算出hash值
2. 如果存在old table，首先在old table中查找，如果找到的bucket已经evacuated，转到3
3. 在new table查找对应的key

高8位hash只是加快key的比较的，并不是作为offset的。

###  插入过程分析

1. 根据key计算出hash，进而得出对应的bucket
2. 如果bucket在old table，将其重新散列到new table中
3. 在bucket中找到空闲的位置，如果一个存在需要插入的key，就更新对应的value
4. 如果对应的bucket一个full，重新申请新的bucket作为overflow bucket
5. 将key/value pair插入到bucket中

在扩容过程中，old table只能查找，但不会插入。如果在old table找打对应的key，将它迁移到新bucket后加入evalucated标记。并且还会额外迁移另一个pair。

##  nil的语义

按照Go语言规范，任何类型在未初始化时都对应一个零值：布尔类型是false，整型是0，字符串是""，而指针，函数，interface，slice，channel和map的零值都是nil。

###  interface

一个interface在没有初始化时，对应的值是nil。

###  string和slice

string的空值是""，占用两个机器字节长度。slice的空值不是空指针，而是结构体中的指针域为空，slice也站三个机器字长。

###  channel和map

channel在栈上只是一个指针，实际的数据都是由指针所指向的堆上面。

channel的操作规则：

- 读或者写一个nil的channel的操作会永远阻塞。
- 读一个关闭的channel会立刻返回一个channel元素类型的零值。
- 写一个关闭的channel会导致panic。

map也是指针，实际数据在堆中，未初始化为nil。

#  函数调用协议

##  go关键字

go关键字用来创建并运行一条协程

下面是go f(1, 2, 3)生成的代码：

```asm
    MOVL    $1, 0(SP)
    MOVL    $2, 4(SP)
    MOVL    $3, 8(SP)
    PUSHQ   $f(SB)
    PUSHQ   $12
    CALL    runtime.newproc(SB)
    POPQ    AX
    POPQ    AX
```

对比一个会发现，前面部分跟普通函数调用是一样的，将参数存储在正常的位置，并没有新建一个辅助的结构体。接下来的两条指令有些不同，将f和12作为参数进栈而不直接调用f，然后调用函数`runtime.newproc`。

12是参数占用的大小。`runtime.newproc`函数接受的参数分别是：参数大小，新的goroutine是要运行的函数，函数的n个参数。

在`runtime.newproc`中，会新建一个栈空间，将栈参数的12个字节拷贝到新栈空间中并让栈指针指向参数。这时的线程状态有点像当被调度器剥夺CPU后一样，寄存器PC、SP会被保存到类似于进程控制块的一个结构体struct G内。f被存放在了struct G的entry域，后面进行调度器恢复goroutine的运行，新线程将从f开始执行。

总结一个，go关键字的实现仅仅是一个语法糖衣而已，也就是：

```go
    go f(args)
```

可以看作

```c
    runtime.newproc(size, f, args)
```

##  defer关键字

defer用于资源的释放，会在函数返回之前进行调用。

使用defer时，可以将return语句拆为两句写，例如return xxx：

```c
返回值 = xxx
调用defer函数
空的return
```

###  defer的实现

defer的实现和go很类似，不同的是它调用的是`runtime.deferproc`而不是`runtime.newproc`。

在defer出现的地方，插入了指令`call runtime.deferproc`，然后再函数返回之前的地方，插入指令`call runtime.deferreture`。

##  连续栈

每个goroutine需要栈。假如每个goroutine都分配固定的栈大小而且不能增长，太小则会溢出，太大会浪费空间。

所以，goroutine一开始给栈分配很小的空间，随着使用过程中的需要自动增长。

Go1.3之后使用的continuous stack。

###  基本原理

每次执行函数调用的Go的runtime都会进行检测，若当前栈的大小不够用，则会触发“中断”，从当前函数进入到Go的运行时库，Go的运行时库会保存此时的函数上下文环境，然后分配一个新的足够大的栈空间，将旧的内容拷贝到新栈中，并做一些设置，使得函数恢复运行时，函数会在新分配的栈中继续执行。

###  实现过程

1. 检测栈大小
2. 保存goroutine上下文
3. 分配新栈
4. 将旧栈拷贝到新栈
5. 恢复运行
6. 函数运行完进行栈的清理

###  具体细节

####  如何捕获栈的空间不足

