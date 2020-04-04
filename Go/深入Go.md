<!-- TOC -->

- [1. 基本数据结构](#1-%e5%9f%ba%e6%9c%ac%e6%95%b0%e6%8d%ae%e7%bb%93%e6%9e%84)
  - [1.1. slice](#11-slice)
    - [1.1.1. slice的扩容](#111-slice%e7%9a%84%e6%89%a9%e5%ae%b9)
    - [1.1.2. make和new](#112-make%e5%92%8cnew)
    - [1.1.3. slice和unsafe.Pointer相互转换](#113-slice%e5%92%8cunsafepointer%e7%9b%b8%e4%ba%92%e8%bd%ac%e6%8d%a2)
  - [1.2. map的实现](#12-map%e7%9a%84%e5%ae%9e%e7%8e%b0)
    - [1.2.1. 数据结构](#121-%e6%95%b0%e6%8d%ae%e7%bb%93%e6%9e%84)
    - [1.2.2. 增量扩容](#122-%e5%a2%9e%e9%87%8f%e6%89%a9%e5%ae%b9)
    - [1.2.3. 查找过程](#123-%e6%9f%a5%e6%89%be%e8%bf%87%e7%a8%8b)
    - [1.2.4. 插入过程分析](#124-%e6%8f%92%e5%85%a5%e8%bf%87%e7%a8%8b%e5%88%86%e6%9e%90)
  - [1.3. nil的语义](#13-nil%e7%9a%84%e8%af%ad%e4%b9%89)
    - [1.3.1. interface](#131-interface)
    - [1.3.2. string和slice](#132-string%e5%92%8cslice)
    - [1.3.3. channel和map](#133-channel%e5%92%8cmap)
- [2. 函数调用协议](#2-%e5%87%bd%e6%95%b0%e8%b0%83%e7%94%a8%e5%8d%8f%e8%ae%ae)
  - [2.1. go关键字](#21-go%e5%85%b3%e9%94%ae%e5%ad%97)
  - [2.2. defer关键字](#22-defer%e5%85%b3%e9%94%ae%e5%ad%97)
    - [2.2.1. defer的实现](#221-defer%e7%9a%84%e5%ae%9e%e7%8e%b0)
  - [2.3. 连续栈](#23-%e8%bf%9e%e7%bb%ad%e6%a0%88)
    - [2.3.1. 基本原理](#231-%e5%9f%ba%e6%9c%ac%e5%8e%9f%e7%90%86)
    - [2.3.2. 实现过程](#232-%e5%ae%9e%e7%8e%b0%e8%bf%87%e7%a8%8b)
    - [2.3.3. 具体细节](#233-%e5%85%b7%e4%bd%93%e7%bb%86%e8%8a%82)
      - [2.3.3.1. 如何捕获栈的空间不足](#2331-%e5%a6%82%e4%bd%95%e6%8d%95%e8%8e%b7%e6%a0%88%e7%9a%84%e7%a9%ba%e9%97%b4%e4%b8%8d%e8%b6%b3)
      - [2.3.3.2. 旧栈数据复制到新栈](#2332-%e6%97%a7%e6%a0%88%e6%95%b0%e6%8d%ae%e5%a4%8d%e5%88%b6%e5%88%b0%e6%96%b0%e6%a0%88)
  - [2.4. 闭包的实现](#24-%e9%97%ad%e5%8c%85%e7%9a%84%e5%ae%9e%e7%8e%b0)
    - [2.4.1. escape analyze](#241-escape-analyze)
    - [2.4.2. 闭包结构体](#242-%e9%97%ad%e5%8c%85%e7%bb%93%e6%9e%84%e4%bd%93)
- [3. Go语言程序初始化过程](#3-go%e8%af%ad%e8%a8%80%e7%a8%8b%e5%ba%8f%e5%88%9d%e5%a7%8b%e5%8c%96%e8%bf%87%e7%a8%8b)
  - [3.1. 系统初始化](#31-%e7%b3%bb%e7%bb%9f%e5%88%9d%e5%a7%8b%e5%8c%96)
    - [3.1.1. 本地线程存储](#311-%e6%9c%ac%e5%9c%b0%e7%ba%bf%e7%a8%8b%e5%ad%98%e5%82%a8)
    - [3.1.2. 初始化顺序](#312-%e5%88%9d%e5%a7%8b%e5%8c%96%e9%a1%ba%e5%ba%8f)
    - [3.1.3. 调度器初始化](#313-%e8%b0%83%e5%ba%a6%e5%99%a8%e5%88%9d%e5%a7%8b%e5%8c%96)
  - [3.2. main.main之前的准备](#32-mainmain%e4%b9%8b%e5%89%8d%e7%9a%84%e5%87%86%e5%a4%87)
    - [3.2.1. sysmon](#321-sysmon)
    - [3.2.2. scavenger](#322-scavenger)
- [4. goroutine调度](#4-goroutine%e8%b0%83%e5%ba%a6)
  - [4.1. 调度器相关的数据结构](#41-%e8%b0%83%e5%ba%a6%e5%99%a8%e7%9b%b8%e5%85%b3%e7%9a%84%e6%95%b0%e6%8d%ae%e7%bb%93%e6%9e%84)
    - [4.1.1. 结构体G](#411-%e7%bb%93%e6%9e%84%e4%bd%93g)
    - [4.1.2. 结构体M](#412-%e7%bb%93%e6%9e%84%e4%bd%93m)
    - [4.1.3. 结构体P](#413-%e7%bb%93%e6%9e%84%e4%bd%93p)
    - [4.1.4. Sched](#414-sched)
  - [4.2. goroutine的生老病死](#42-goroutine%e7%9a%84%e7%94%9f%e8%80%81%e7%97%85%e6%ad%bb)
    - [4.2.1. goroutine的创建](#421-goroutine%e7%9a%84%e5%88%9b%e5%bb%ba)
  - [4.3. 设计与演化](#43-%e8%ae%be%e8%ae%a1%e4%b8%8e%e6%bc%94%e5%8c%96)
- [5. 内存管理](#5-%e5%86%85%e5%ad%98%e7%ae%a1%e7%90%86)
  - [5.1. 内存池](#51-%e5%86%85%e5%ad%98%e6%b1%a0)
  - [5.2. 垃圾回收](#52-%e5%9e%83%e5%9c%be%e5%9b%9e%e6%94%b6)
- [6. 高级数据结构的实现](#6-%e9%ab%98%e7%ba%a7%e6%95%b0%e6%8d%ae%e7%bb%93%e6%9e%84%e7%9a%84%e5%ae%9e%e7%8e%b0)
  - [6.1. channel](#61-channel)
    - [6.1.1. channel的数据结构](#611-channel%e7%9a%84%e6%95%b0%e6%8d%ae%e7%bb%93%e6%9e%84)
    - [6.1.2. 读写channel操作](#612-%e8%af%bb%e5%86%99channel%e6%93%8d%e4%bd%9c)
    - [6.1.3. select的实现](#613-select%e7%9a%84%e5%ae%9e%e7%8e%b0)
  - [6.2. interface](#62-interface)
    - [6.2.1. Eface和Iface](#621-eface%e5%92%8ciface)
    - [6.2.2. 具体类型向接口类型赋值](#622-%e5%85%b7%e4%bd%93%e7%b1%bb%e5%9e%8b%e5%90%91%e6%8e%a5%e5%8f%a3%e7%b1%bb%e5%9e%8b%e8%b5%8b%e5%80%bc)
    - [6.2.3. reflect](#623-reflect)
  - [6.3. 方法调用](#63-%e6%96%b9%e6%b3%95%e8%b0%83%e7%94%a8)
    - [6.3.1. 普通的函数调用](#631-%e6%99%ae%e9%80%9a%e7%9a%84%e5%87%bd%e6%95%b0%e8%b0%83%e7%94%a8)
    - [6.3.2. 对象的方法调用](#632-%e5%af%b9%e8%b1%a1%e7%9a%84%e6%96%b9%e6%b3%95%e8%b0%83%e7%94%a8)
    - [6.3.3. 组合对象的方法调用](#633-%e7%bb%84%e5%90%88%e5%af%b9%e8%b1%a1%e7%9a%84%e6%96%b9%e6%b3%95%e8%b0%83%e7%94%a8)
    - [6.3.4. 接口的方法调用](#634-%e6%8e%a5%e5%8f%a3%e7%9a%84%e6%96%b9%e6%b3%95%e8%b0%83%e7%94%a8)
- [7. 网络](#7-%e7%bd%91%e7%bb%9c)
  - [7.1. 非阻塞IO](#71-%e9%9d%9e%e9%98%bb%e5%a1%9eio)
    - [7.1.1. 如何实现](#711-%e5%a6%82%e4%bd%95%e5%ae%9e%e7%8e%b0)
    - [7.1.2. 封装层次](#712-%e5%b0%81%e8%a3%85%e5%b1%82%e6%ac%a1)
    - [7.1.3. 文件描述符和goroutine](#713-%e6%96%87%e4%bb%b6%e6%8f%8f%e8%bf%b0%e7%ac%a6%e5%92%8cgoroutine)

<!-- /TOC -->
[原文链接-深入解析Go](https://tiancaiamao.gitbooks.io/go-internals/content/zh/)
# 1. 基本数据结构

## 1.1. slice

在内存中slice是包含三个域的结构体：指向slice中第一个元素的指针，slice的长度以及slice的容量。长度是下标操作的上界，容量是分割操作的上界。

数组的slice并不会复制一份数据，只是建立一个新的数据结构

### 1.1.1. slice的扩容

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

### 1.1.2. make和new

Go有两个数据结构创建函数：new和make。new(T)返回一个*T，返回的这个指针可以被隐式的消除引用。而make(T,args)返回一个普通的T。new返回一个指向已经清零内存的指针，make返回一个复杂结构。

### 1.1.3. slice和unsafe.Pointer相互转换

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

## 1.2. map的实现

Go的map在底层是用hash实现的

### 1.2.1. 数据结构

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

### 1.2.2. 增量扩容

如果扩容前的哈希表大小为$2^B$，扩容之后的大小为$2^\(B+1\)$，每次扩容都变为原来大小的两倍，哈希表大小始终为2的指数倍，则有(hash mod 2^B)等价于(hash & (2^B-1))。这样可以简化运算，避免了取余操作。

假设扩容之前容量为X，扩容之后容量为Y，对于某个哈希值hash，一般情况下(hash mod X)不等于(hash mod Y)，所以扩容之后要重新计算每一项在哈希表中的新位置。当hash表扩容之后，需要将那些旧的pair重新哈希到新的table上(源代码中称之为evacuate)， 这个工作并没有在扩容之后一次性完成，而是逐步的完成（在insert和remove时每次搬移1-2个pair），Go语言使用的是增量扩容。

为什么会增量扩容呢？主要是缩短map容器的响应时间。假如我们直接将map用作某个响应实时性要求非常高的web应用存储，如果不采用增量扩容，当map里面存储的元素很多之后，扩容时系统就会卡往，导致较长一段时间内无法响应请求。不过增量扩容本质上还是将总的扩容时间分摊到了每一次哈希操作上面。

扩容会建立一个大小是原来2倍的新的表，将旧的bucket搬到新的表中之后，并不会将旧的bucket从oldbucket中删除，而是加上一个已删除的标记。

正是由于这个工作是逐渐完成的，这样就会导致一部分数据在old table中，一部分在new table中， 所以对于hash table的insert, remove, lookup操作的处理逻辑产生影响。只有当所有的bucket都从旧表移到新表之后，才会将oldbucket释放掉。

扩容的填充因子是多少呢？如果grow的太频繁，会造成空间的利用率很低， 如果很久才grow，会形成很多的overflow buckets，查找的效率也会下降。 这个平衡点如何选取呢(在go中，这个平衡点是有一个宏控制的(#define LOAD 6.5), 它的意思是这样的，如果table中元素的个数大于table中能容纳的元素的个数， 那么就触发一次grow动作

### 1.2.3. 查找过程

1. 根据key计算出hash值
2. 如果存在old table，首先在old table中查找，如果找到的bucket已经evacuated，转到3
3. 在new table查找对应的key

高8位hash只是加快key的比较的，并不是作为offset的。

### 1.2.4. 插入过程分析

1. 根据key计算出hash，进而得出对应的bucket
2. 如果bucket在old table，将其重新散列到new table中
3. 在bucket中找到空闲的位置，如果一个存在需要插入的key，就更新对应的value
4. 如果对应的bucket一个full，重新申请新的bucket作为overflow bucket
5. 将key/value pair插入到bucket中

在扩容过程中，old table只能查找，但不会插入。如果在old table找打对应的key，将它迁移到新bucket后加入evalucated标记。并且还会额外迁移另一个pair。

## 1.3. nil的语义

按照Go语言规范，任何类型在未初始化时都对应一个零值：布尔类型是false，整型是0，字符串是""，而指针，函数，interface，slice，channel和map的零值都是nil。

### 1.3.1. interface

一个interface在没有初始化时，对应的值是nil。

### 1.3.2. string和slice

string的空值是""，占用两个机器字节长度。slice的空值不是空指针，而是结构体中的指针域为空，slice也站三个机器字长。

### 1.3.3. channel和map

channel在栈上只是一个指针，实际的数据都是由指针所指向的堆上面。

channel的操作规则：

- 读或者写一个nil的channel的操作会永远阻塞。
- 读一个关闭的channel会立刻返回一个channel元素类型的零值。
- 写一个关闭的channel会导致panic。

map也是指针，实际数据在堆中，未初始化为nil。

# 2. 函数调用协议

## 2.1. go关键字

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

## 2.2. defer关键字

defer用于资源的释放，会在函数返回之前进行调用。

使用defer时，可以将return语句拆为两句写，例如return xxx：

```c
返回值 = xxx
调用defer函数
空的return
```

### 2.2.1. defer的实现

defer的实现和go很类似，不同的是它调用的是`runtime.deferproc`而不是`runtime.newproc`。

在defer出现的地方，插入了指令`call runtime.deferproc`，然后再函数返回之前的地方，插入指令`call runtime.deferreture`。

## 2.3. 连续栈

每个goroutine需要栈。假如每个goroutine都分配固定的栈大小而且不能增长，太小则会溢出，太大会浪费空间。

所以，goroutine一开始给栈分配很小的空间，随着使用过程中的需要自动增长。

Go1.3之后使用的continuous stack。

### 2.3.1. 基本原理

每次执行函数调用的Go的runtime都会进行检测，若当前栈的大小不够用，则会触发“中断”，从当前函数进入到Go的运行时库，Go的运行时库会保存此时的函数上下文环境，然后分配一个新的足够大的栈空间，将旧的内容拷贝到新栈中，并做一些设置，使得函数恢复运行时，函数会在新分配的栈中继续执行。

### 2.3.2. 实现过程

1. 检测栈大小
2. 保存goroutine上下文
3. 分配新栈
4. 将旧栈拷贝到新栈
5. 恢复运行
6. 函数运行完进行栈的清理

### 2.3.3. 具体细节

#### 2.3.3.1. 如何捕获栈的空间不足

在Go的运行时库中，每个goroutine对应一个结构体G，大致相当于进程控制块的概念。这个结构体中有`stackbase`和`stackguard`，用于确定这个goroutine使用的栈空间信息。每个Go函数调用的前几条命令，先比较栈指针寄存器和g->stackguard，检测是否发生栈溢出。如果栈指针寄存器值超越了stackguard就需要扩展空间。

当栈指针寄存器值超过了stackguard时，就会调用runtime.morestack，该函数大概进行的是：将一些信息存在M结构体中，这些信息包括当前栈帧，参数，当前函数调用，函数返回地址。通过这些信息可以把新栈和旧栈链起来。

#### 2.3.3.2. 旧栈数据复制到新栈

runtime.morestack会调用runtime.newstack，newstack就是分配一个足够大的新的空间，将旧的栈中的数据复制到新的栈中，进行适当的修饰，伪装成调用过runtime.lessstack的样子。

在将旧栈数据复制到新栈中，需要考虑指针失效的问题。比如有某个指针，引用了旧栈中的地址，如果仅仅是将旧栈中的内容搬到新栈中，那么该指针就失效了，因为旧栈已经释放，应该修改这个指针让它指向新栈的对应地址。

具体做法是，对当前栈帧之前的每一个栈帧，对其中的每一个指针，检测指针指向的地址，如果指向地址是落在旧栈范围内的，则将它加上一个偏移使它指向新栈的相应地址。这个偏移值等于新栈基地址减旧栈基地址。

runtime.lessstack比较简单，它其实就是切换到m->g0栈之后调用runtime.oldstack函数。这时之前保存的那个Stktop结构体是时候发挥作用了，从上面可以找到旧栈空间的SP和PC等信息，通过runtime.gogo跳转过去，整个过程就完成了。

## 2.4. 闭包的实现

闭包是函数和它所引用的环境

### 2.4.1. escape analyze

逃逸分析技术可以把闭包中的变量分配到堆上，这样在函数结束后，变量不会回收。

### 2.4.2. 闭包结构体

返回闭包时并不是单纯返回一个函数，而是返回了一个结构体，记录下函数返回地址和引用的环境中的变量地址。

# 3. Go语言程序初始化过程

## 3.1. 系统初始化

### 3.1.1. 本地线程存储

这里解释一下本地线程存储。比如说每个goroutine都有自己的控制信息，这些信息是存放在一个结构体G中。假设我们有一个全局变量g是结构体G的指针，我们希望只有唯一的全局变量g，而不是g0，g1，g2...但是我们又希望不同goroutine去访问这个全局变量g得到的并不是同一个东西，它们得到的是相对自己线程的结构体G，这种情况下就需要本地线程存储。g确实是一个全局变量，却在不同线程有多份不同的副本。每个goroutine去访问g时，都是对应到自己线程的这一份副本。

设置好本地线程存储之后，就可以为每个goroutine和machine设置寄存器了。这样设置好了之后，每次调用get_tls(r)，就会将当前的goroutine的g的地址放到寄存器r中。

### 3.1.2. 初始化顺序

```asm
    CLD                // convention is D is always left cleared
    CALL    runtime·check(SB) //检测像int8,int16,float等是否是预期的大小，检测cas操作是否正常
    MOVL    16(SP), AX        // copy argc
    MOVL    AX, 0(SP)
    MOVQ    24(SP), AX        // copy argv
    MOVQ    AX, 8(SP)
    CALL    runtime·args(SB)    //将argc,argv设置到static全局变量中了
    CALL    runtime·osinit(SB)    //osinit做的事情就是设置runtime.ncpu，不同平台实现方式不一样
    CALL    runtime·hashinit(SB)    //使用读/dev/urandom的方式从内核获得随机数种子
    CALL    runtime·schedinit(SB)    //内存管理初始化，根据GOMAXPROCS设置使用的procs等等
```

### 3.1.3. 调度器初始化

让我们看一下runtime.schedinit函数。该函数其实是包装了一下其它模块的初始化函数。有调用mallocinit，mcommoninit分别对内存管理模块初始化，对当前的结构体M初始化。

接着调用runtime.goargs和runtime.goenvs，将程序的main函数参数argc和argv等复制到了os.Args中。

也是在这个函数中，根据环境变量GOMAXPROCS决定可用物理线程数目的：

```c
    procs = 1;
    p = runtime·getenv("GOMAXPROCS");
    if(p != nil && (n = runtime·atoi(p)) > 0) {
        if(n > MaxGomaxprocs)
            n = MaxGomaxprocs;
        procs = n;
    }
```

回到前面的汇编代码继续看：

```asm
    // 新建一个G，当它运行时会调用main.main
    PUSHQ    $runtime·main·f(SB)        // entry
    PUSHQ    $0            // arg size
    CALL    runtime·newproc(SB)
    POPQ    AX
    POPQ    AX

    // start this M
    CALL    runtime·mstart(SB)
```

还记得前面章节讲的go关键字的调用协议么？先将参数进栈，再被调函数指针和参数字节数进栈，接着调用runtime.newproc函数。所以这里其实就是新开个goroutine执行runtime.main。

runtime.newproc会把runtime.main放到就绪线程队列里面。本线程继续执行runtime.mstart，m意思是machine。runtime.mstart会调用到调度函数schedule

schedule函数绝不返回，它会根据当前线程队列中线程状态挑选一个来运行。由于当前只有这一个goroutine，它会被调度，然后就到了runtime.main函数中来，runtime.main会调用用户的main函数，即main.main从此进入用户代码。前面已经写过helloworld了，用gdb调试，一步一步的跟踪观察这个过程。

## 3.2. main.main之前的准备

### 3.2.1. sysmon

在main.main执行之前，Go语言的runtime库会初始化一些后台任务，其中一个就是`sysmon`。

```c
newm(sysmon, nil);
```

newm新建一个结构体M，第一个参数是这个结构体M的入口函数，也就是说会在一个新的物理线程中运行sysmon函数。整个函数是一个死循环的形式，主要处理两个事件：对于网络的epoll以及抢占式调度的检测。

```c
for(;;) {
    runtime.usleep(delay);
    if(lastpoll != 0 && lastpoll + 10*1000*1000 > now) {
        runtime.netpoll();
    }
    retake(now);    // 根据每个P的状态和运行时间决定是否要进行抢占
}
```

### 3.2.2. scavenger

scavenger是另一个后台任务，但是他的创建和sysmon有点区别

```c
runtime.newproc(&scavenger, nil, 0, 0, runtime.main);
```

newproc创建一个goroutine，第一个参数是goroutine运行的函数。scavenger的地位是没有sysmon那么高的。sysmon是由物理线程运行的，而scavenger是由goroutine运行的。

scavenger执行的是`runtime.MHeap_Scavenger`函数。它将一些不在使用的内存归还给操作系统。Go是一门垃圾回收的语言，垃圾回收会在系统运行过程中被触发，内存会被归还到go的内存管理系统中，Go的内存管理是基于内存池进行重用的，而这个函数会真正的将内存归还给操作系统。

# 4. goroutine调度

## 4.1. 调度器相关的数据结构

结构体G、结构体M、结构体P以及结构体Sched。前三个的定义在文件runtime/runtime.h中，Sched的定义在runtime/proc.c中。

### 4.1.1. 结构体G

G是goroutine的缩写，相当于操作系统的进程控制块，在这里就是goroutine的控制结构，是对goroutine的抽象。其中包括goid是这个goroutine的ID，status是这个goroutine的状态，如Gidle、Grunnable、Grunning、Gwaiting、Gdead等。

```c
struct G
{
    uintptr    stackguard;    // 分段栈的可用空间下界
    uintptr    stackbase;    // 分段栈的栈基址
    Gobuf    sched;        //进程切换时，利用sched域来保存上下文
    uintptr    stack0;
    FuncVal*    fnstart;        // goroutine运行的函数
    void*    param;        // 用于传递参数，睡眠时其它goroutine设置param，唤醒时此goroutine可以获取
    int16    status;        // 状态Gidle,Grunnable,Grunning,Gsyscall,Gwaiting,Gdead
    int64    goid;        // goroutine的id号
    G*    schedlink;
    M*    m;        // for debuggers, but offset not hard-coded
    M*    lockedm;    // G被锁定只能在这个m上运行
    uintptr    gopc;    // 创建这个goroutine的go表达式的pc
    ...
};
```

结构体G中的部分域如上所示。可以看到，其中包含了栈信息stackbase和stackguard，有运行的函数信息fnstart。这些就足够成为一个可执行的单元了，只要得到CPU就可以运行。

goroutine切换时，上下文信息保存在结构体的sched域中。goroutine是轻量级的`线程`或者称为`协程`，切换时并不必陷入到操作系统内核中，所以保存过程很轻量。看一下结构体G中的Gobuf，其实只保存了当前栈指针，程序计数器，以及goroutine自身。

```
struct Gobuf
{
    // The offsets of these fields are known to (hard-coded in) libmach.
    uintptr    sp;
    byte*    pc;
    G*    g;
    ...
};
```

记录g是为了恢复当前goroutine的结构体G指针，运行时库中使用了一个常驻的寄存器`extern register G* g`，这个是当前goroutine的结构体G的指针。这样做是为了快速地访问goroutine中的信息，比如，Go的栈的实现并没有使用%ebp寄存器，不过这可以通过g->stackbase快速得到。"extern register"是由6c，8c等实现的一个特殊的存储。在ARM上它是实际的寄存器；其它平台是由段寄存器进行索引的线程本地存储的一个槽位。在linux系统中，对g和m使用的分别是0(GS)和4(GS)。需要注意的是，链接器还会根据特定操作系统改变编译器的输出，例如，6l/linux下会将0(GS)重写为-16(FS)。每个链接到Go程序的C文件都必须包含runtime.h头文件，这样C编译器知道避免使用专用的寄存器。

### 4.1.2. 结构体M

M是machine的缩写，是对及其的抽象，每个m都是对应到一条操作系统的物理线程。M必须关联P才可以执行Go代码，但是当它处理阻塞或者系统调用中时，可以不需要关联P。

```
struct M
{
    G*    g0;        // 带有调度栈的goroutine
    G*    gsignal;    // signal-handling G 处理信号的goroutine
    void    (*mstartfn)(void);
    G*    curg;        // M中当前运行的goroutine
    P*    p;        // 关联P以执行Go代码 (如果没有执行Go代码则P为nil)
    P*    nextp;
    int32    id;
    int32    mallocing; //状态
    int32    throwing;
    int32    gcing;
    int32    locks;
    int32    helpgc;        //不为0表示此m在做帮忙gc。helpgc等于n只是一个编号
    bool    blockingsyscall;
    bool    spinning;
    Note    park;
    M*    alllink;    // 这个域用于链接allm
    M*    schedlink;
    MCache    *mcache;
    G*    lockedg;
    M*    nextwaitm;    // next M waiting for lock
    GCStats    gcstats;
    ...
};
```

这里也是截取结构体M中的部分域。和G类似，M中也有alllink域将所有的M放在allm链表中。lockedg是某些情况下，G锁定在这个M中运行而不会切换到其它M中去。M中还有一个MCache，是当前M的内存的缓存。M也和G一样有一个常驻寄存器变量，代表当前的M。同时存在多个M，表示同时存在多个物理线程。

结构体M中有两个G是需要关注一下的，一个是curg，代表结构体M当前绑定的结构体G。另一个是g0，是带有调度栈的goroutine，这是一个比较特殊的goroutine。普通的goroutine的栈是在堆上分配的可增长的栈，而g0的栈是M对应的线程的栈。所有调度相关的代码，会先切换到该goroutine的栈中再执行。

### 4.1.3. 结构体P

Go1.1中新加入的一个数据结构，它是Processor的缩写。结构体P的加入是为了提高Go程序的并发度，实现更好的调度。M代表OS线程。P代表Go代码执行时需要的资源。当M执行Go代码时，它需要关联一个P，当M为idle或者在系统调用中时，它也需要P。有刚好GOMAXPROCS个P。所有的P被组织为一个数组，在P上实现了工作流窃取的调度器。

```
struct P
{
    Lock;
    uint32    status;  // Pidle或Prunning等
    P*    link;
    uint32    schedtick;   // 每次调度时将它加一
    M*    m;    // 链接到它关联的M (nil if idle)
    MCache*    mcache;

    G*    runq[256];
    int32    runqhead;
    int32    runqtail;

    // Available G's (status == Gdead)
    G*    gfree;
    int32    gfreecnt;
    byte    pad[64];
};
```

结构体P中也有相应的状态：

```
Pidle,
Prunning,
Psyscall,
Pgcstop,
Pdead,
```

注意，跟G不同的是，P不存在`waiting`状态。MCache被移到了P中，但是在结构体M中也还保留着。在P中有一个Grunnable的goroutine队列，这是一个P的局部队列。当P执行Go代码时，它会优先从自己的这个局部队列中取，这时可以不用加锁，提高了并发度。如果发现这个队列空了，则去其它P的队列中拿一半过来，这样实现工作流窃取的调度。这种情况下是需要给调用器加锁的。

### 4.1.4. Sched

Sched是调度实现中使用的数据结构，该结构体的定义在文件proc.c中。

```
struct Sched {
    Lock;

    uint64    goidgen;

    M*    midle;     // idle m's waiting for work
    int32    nmidle;     // number of idle m's waiting for work
    int32    nmidlelocked; // number of locked m's waiting for work
    int3    mcount;     // number of m's that have been created
    int32    maxmcount;    // maximum number of m's allowed (or die)

    P*    pidle;  // idle P's
    uint32    npidle;  //idle P的数量
    uint32    nmspinning;

    // Global runnable queue.
    G*    runqhead;
    G*    runqtail;
    int32    runqsize;

    // Global cache of dead G's.
    Lock    gflock;
    G*    gfree;

    int32    stopwait;
    Note    stopnote;
    uint32    sysmonwait;
    Note    sysmonnote;
    uint64    lastpoll;

    int32    profilehz;    // cpu profiling rate
}
```

大多数需要的信息都已放在了结构体M、G和P中，Sched结构体只是一个壳。可以看到，其中有M的idle队列，P的idle队列，以及一个全局的就绪的G队列。Sched结构体中的Lock是非常必须的，如果M或P等做一些非局部的操作，它们一般需要先锁住调度器。

## 4.2. goroutine的生老病死

### 4.2.1. goroutine的创建

go关键字最终变成了runtime.newproc的调用。

runtime.newproc(size, f, args)功能就是创建一个新的G。这个函数不能用分段栈，因为它假设参数的放置顺序是紧接着函数f的，分段栈会破坏这个布局。它会调用newproc1，在newproc1中可以使用分段栈。真正的工作是在newproc1中完成的。

newproc1会进行如下操作：

首先，检查当前结构体M中的O，看是否有可用的G。如果有，直接从中取一个，否则，分配一个新的结构体G。如果分配了新的G，需要将它挂到runtime的相关队列中。

获取了结构体G之后，将调用参数保存到g的栈，将sp，pc等上下文保存在g的sched中，这样goroutine就准备好了，整个状态和一个运行中的goroutine被中断时一样，只要等到CPU，它就可以继续运行。

然后将这个G挂到当前M的P的队列中。这里会给与新的goroutine一次运行的机会，即：如果当前的P的数目没有达到上限，也没有正在自旋抢CPU的M，则调用wakep将P立即投入运行。

wakep函数唤醒P时，调度器会试着寻找一个可用的M来绑定P，必要时会新建M。

这个是新建M的newm函数：

```
// 新建一个m，它将以调用fn开始，或者是从调度器开始
static void
newm(void(*fn)(void), P *p)
{
    M *mp;
    mp = runtime·allocm(p);
    mp->nextp = p;
    mp->mstartfn = fn;
    runtime·newosproc(mp, (byte*)mp->g0->stackbase);
}
```

runtime.newm功能跟newproc相似,前者分配一个goroutine,而后者分配一个M。其实一个M就是一个操作系统线程的抽象，可以看到它会调用runtime.newosproc。

总算看到了从Go的运行时库到操作系统的接口，runtime.newosproc(平台相关的)会调用系统的runtime.clone(平台相关的)来新建一个线程，新的线程将以runtime.mstart为入口函数。

mstart是runtime.newosproc新建的系统线程的入口地址，新线程执行时会从这里开始运行。新线程的执行和goroutine的执行是两个概念，由于有m这一层对机器的抽象，是m在执行g而不是线程在执行g。所以线程的入口是mstart，g的执行要到schedule才算入口。函数mstart最后调用了schedule。

如果是从mstart进入到schedule的，那么schedule中逻辑非常简单，大概就这几步：

```
找到一个等待运行的g
如果g是锁定到某个M的，则让那个M运行
否则，调用execute函数让g在当前的M中运行
```

execute会恢复newproc1中设置的上下文，这样就跳转到新的goroutine去执行了。

## 4.3. 设计与演化

协程是轻量级的线程，它相对线程的优势就在于协程非常轻量级，进行切换以及保存上下文环境代价非常的小。

# 5. 内存管理

## 5.1. 内存池

Go中为每个系统线程分配一个本地的MCache(前面介绍的结构体M中的MCache域)，少量的地址分配就直接从MCache中分配，并且定期做垃圾回收，将线程的MCache中的空闲内存返回给全局控制堆。小于32K为小对象，大对象直接从全局控制堆上以页(4k)为单位进行分配，也就是说大对象总是以页对齐的。一个页可以存入一些相同大小的小对象，小对象从本地内存链表中分配，大对象从中心内存堆中分配。

大约有100种内存块类别，每一类别都有自己对象的空闲链表。小于32kB的内存分配被向上取整到对应的尺寸类别，从相应的空闲链表中分配。一页内存只可以被分裂成同一种尺寸类别的对象，然后由空闲链表分配器管理。

分配器的数据结构包括:

FixAlloc: 固定大小(128kB)的对象的空闲链分配器,被分配器用于管理存储
MHeap: 分配堆,按页的粒度进行管理(4kB)
MSpan: 一些由MHeap管理的页
MCentral: 对于给定尺寸类别的共享的free list
MCache: 用于小对象的每M一个的cache
我们可以将Go语言的内存管理看成一个两级的内存管理结构，MHeap和MCache。上面一级管理的基本单位是页，用于分配大对象，每次分配都是若干连续的页，也就是若干个4KB的大小。使用的数据结构是MHeap和MSpan，用BestFit算法做分配，用位示图做回收。下面一级管理的基本单位是不同类型的固定大小的对象，更像一个对象池而不是内存池，用引用计数做回收。下面这一级使用的数据结构是MCache。

## 5.2. 垃圾回收
Go使用标记清扫法进行回收。
支持精确的垃圾回收。

Go在这个版本中不仅实现了精确的垃圾回收，而且实现了并行的垃圾回收。标记算法本质上就是一个树的遍历过程，上面实现的是一个递归版本。

垃圾回收的触发是由一个gcpercent的变量控制的，当新分配的内存占已在使用中的内存的比例超过gcprecent时就会触发。比如，gcpercent=100，当前使用了4M的内存，那么当内存分配到达8M时就会再次gc。如果回收完毕后，内存的使用量为5M，那么下次回收的时机则是内存分配达到10M的时候。也就是说，并不是内存分配越多，垃圾回收频率越高，这个算法使得垃圾回收的频率比较稳定，适合应用的场景。

gcpercent的值是通过环境变量GOGC获取的，如果不设置这个环境变量，默认值是100。如果将它设置成off，则是关闭垃圾回收。

# 6. 高级数据结构的实现
## 6.1. channel
### 6.1.1. channel的数据结构
Go语言的channel是first-class的，意味着它可以被储存在变量中，可以作为参数传递给函数，也可以作为函数的返回值返回。channel就是一个结构体，结构体定义如下：
```c
struct    Hchan
{
    uintgo    qcount;            // 队列q中的总数据数量
    uintgo    dataqsiz;        // 环形队列q的数据大小
    uint16    elemsize;
    bool    closed;
    uint8    elemalign;
    Alg*    elemalg;        // interface for element type
    uintgo    sendx;            // 发送index
    uintgo    recvx;            // 接收index
    WaitQ    recvq;            // 因recv而阻塞的等待队列
    WaitQ    sendq;            // 因send而阻塞的等待队列
    Lock;
};
```
其中一个核心部分是存放channel数据的环形队列，由qcount和elementsize分别制定队列的容量和当前使用量。dataqsize是队列的大小。elemalg是元素操作的一个Alg结构体，记录下元素的操作，如copy函数，equal函数，hash函数等。

如果是带缓冲的chan，则缓冲区数据实际上是紧接着Hchan结构体分配的。
```c
c = (Hchan*)runtime.mal(n + hint*elem->size);
```
另一个重要的部分就是recvq和sendq两个链表，一个是因读这个通道而阻塞的goroutine，另一个是写这个通道而阻塞的goroutine。如果一个goroutine阻塞与channel了，那么它就被挂在recq或者sendq中。WaitQ是链表的一个定义，包含一个头结点和尾结点：
```c
struct    WaitQ
{
    SudoG*    first;
    SudoG*    last;
};
```
队列中的每一个成员都是SudoG结构体变量：
```c
struct    SudoG
{
    G*    g;        // g and selgen constitute
    uint32    selgen;        // a weak pointer to g
    SudoG*    link;
    int64    releasetime;
    byte*    elem;        // data element
};
```
该结构体主要就是一个g和elem。elem用于储存goroutine的数据。读通道时，数据会从Hchan的队列中拷贝到SudoG的elem域。写通道时，数据则是由SudoG的elem域拷贝到Hchan的队列中。
### 6.1.2. 读写channel操作
基本的写channel在底层运行时库中对应的是一个`runtime.chansend`函数。
```c
c <- v
```
在运行时库会执行：
```c
void runtime.chansend(ChanType *t, Hchan *c, byte *ep, bool * pres, void *pc)
```
其中c就是channel，ep是取变量v的地址。这里的传值约定是调用者负责分配好ep的空间，仅需要简单的取变量地址就够了。pres参数是在select中的通道操作使用的。

这个函数首先学会区分是同步还是异步。同步是指chan不带缓冲区的，因此可能写阻塞，而异步是指chan带缓冲区的，只有缓冲区满才阻塞。

在同步的情况下，由于channel本身是不带数据缓存的，这时首先会查看Hchan结构体中的recvq链表时否为空，即是否有因为读该管道而阻塞的goroutine。如果有则可以正常写channel，否则操作会阻塞。
recvq不为空的情况下，将一个SudoG结构体出队列，将传给通道的数据(函数参数ep)拷贝到SudoG结构体中的elem域，并将SudoG中的g放到就绪队列中，状态置为ready，然后函数返回。
如果recvq为空，否则要将当前goroutine阻塞。此时将一个SudoG结构体，挂到通道的sendq链表中，这个SudoG中的elem域是参数eq，SudoG中的g是当前的goroutine。当前goroutine会被设置为waiting状态并挂到等待队列中。

在异步的情况，如果缓冲区满了，也是要将当前goroutine和数据一起作为SudoG结构体挂在sendq队列中，表示因写channel而阻塞。否则也是先看有没有recvq链表是否为空，有就唤醒。

跟同步不同的是在channel缓冲区不满的情况，这里不会阻塞写者，而是将数据放到channel的缓冲区中，调用者返回。

读channel的操作也是类似的，对应的函数是runtime.chansend。一个是收一个是发，基本的过程都是差不多的。

需要注意的是几种特殊情况下的通道操作--空通道和关闭的通道。

空通道是指将一个channel赋值为nil，或者定义后不调用make进行初始化。按照Go语言的语言规范，读写空通道是永远阻塞的。其实在函数runtime.chansend和runtime.chanrecv开头就有判断这类情况，如果发现参数c是空的，则直接将当前的goroutine放到等待队列，状态设置为waiting。

读一个关闭的通道，永远不会阻塞，会返回一个通道数据类型的零值。这个实现也很简单，将零值复制到调用函数的参数ep中。写一个关闭的通道，则会panic。关闭一个空通道，也会导致panic。
### 6.1.3. select的实现
select-case中的chan操作编译成了if-else。
```go
select {
case v = <-c:
        ...foo
default:
        ...bar
}
```
会被编译成：
```c
if selectnbrecv(&v, c) {
        ...foo
} else {
        ...bar
}
```
接下来就是看一下selectnbrecv相关的函数了。其实没有任何特殊的魔法，这些函数只是简单地调用runtime.chanrecv函数，只不过设置了一个参数，告诉当runtime.chanrecv函数，当不能完成操作时不要阻塞，而是返回失败。也就是说，所有的select操作其实都仅仅是被换成了if-else判断，底层调用的不阻塞的通道操作函数。

在Go的语言规范中，select中的case的执行顺序是随机的，而不像switch中的case那样一条一条的顺序执行。那么，如何实现随机呢？

select和case关键字使用了下面的结构体：
```c
struct    Scase
{
    SudoG    sg;            // must be first member (cast to Scase)
    Hchan*    chan;        // chan
    byte*    pc;            // return pc
    uint16    kind;
    uint16    so;            // vararg of selected bool
    bool*    receivedp;    // pointer to received bool (recv2)
};

struct    Select
{
    uint16    tcase;            // 总的scase[]数量
    uint16    ncase;            // 当前填充了的scase[]数量
    uint16*    pollorder;        // case的poll次序
    Hchan**    lockorder;        // channel的锁住的次序
    Scase    scase[1];        // 每个case会在结构体里有一个Scase，顺序是按出现的次序
};
```
每个select都对应一个Select结构体。在Select数据结构中有个Scase数组，记录下了每一个case，而Scase中包含了Hchan。然后pollorder数组将元素随机排列，这样就可以将Scase乱序了。
## 6.2. interface
### 6.2.1. Eface和Iface
interface实际上就是一个结构体，包含两个成员。其中一个成员是指向具体数据的指针，另一个成员中包含了类型信息。空接口和带方法的接口略有不同，下面分别是空接口和带方法的接口是使用的数据结构：
```c
struct Eface
{
    Type*    type;
    void*    data;
};
struct Iface
{
    Itab*    tab;
    void*    data;
};
```
先看Eface，它是interface{}底层使用的数据结构。数据域中包含了一个void*指针，和一个类型结构体的指针。interface{}扮演的角色跟C语言中的void*是差不多的，Go中的任何对象都可以表示为interface{}。不同之处在于，interface{}中有类型信息，于是可以实现反射。

类型信息的结构体定义如下：
```c
struct Type
{
    uintptr size;
    uint32 hash;
    uint8 _unused;
    uint8 align;
    uint8 fieldAlign;
    uint8 kind;
    Alg *alg;
    void *gc;
    String *string;
    UncommonType *x;
    Type *ptrto;
};
```
其实在前面我们已经见过它了。精确的垃圾回收中，就是依赖Type结构体中的gc域的。不同类型数据的类型信息结构体并不完全一致，Type是类型信息结构体中公共的部分，其中size描述类型的大小，hash数据的hash值，align是对齐，fieldAlgin是这个数据嵌入结构体时的对齐，kind是一个枚举值，每种类型对应了一个编号。alg是一个函数指针的数组，存储了hash/equal/print/copy四个函数操作。UncommonType是指向一个函数指针的数组，收集了这个类型的实现的所有方法。

在reflect包中有个KindOf函数，返回一个interface{}的Type，其实该函数就是简单的取Eface中的Type域。

Iface和Eface略有不同，它是带方法的interface底层使用的数据结构。data域同样是指向原始数据的，而Itab的结构如下：
```c
struct    Itab
{
    InterfaceType*    inter;
    Type*    type;
    Itab*    link;
    int32    bad;
    int32    unused;
    void    (*fun[])(void);
};
```
Itab中不仅存储了Type信息，而且还多了一个方法表fun[]。一个Iface中的具体类型中实现的方法会被拷贝到Itab的fun数组中。

### 6.2.2. 具体类型向接口类型赋值
将具体类型数据赋值给interface{}这样的抽象类型，中间会涉及到类型转换操作。从接口类型转换为具体类型(也就是反射)，也涉及到了类型转换。这个转换过程中做了哪些操作呢？先看将具体类型转换为接口类型。如果是转换成空接口，这个过程比较简单，就是返回一个Eface，将Eface中的data指针指向原型数据，type指针会指向数据的Type结构体。

将某个类型数据转换为带方法的接口时，会复杂一些。中间涉及了一道检测，该类型必须要实现了接口中声明的所有方法才可以进行转换。这个检测是在编译过程中做的

类型转换时的检测就是比较具体类型的方法表和接口类型的方法表，看具体类型是实现了接口类型所声明的所有的方法。还记得Type结构体中是有个UncommonType字段的，里面有张方法表，类型所实现的方法都在里面。而在Itab中有个InterfaceType字段，这个字段中也有一张方法表，就是这个接口所要求的方法。这两处方法表都是排序过的，只需要一遍顺序扫描进行比较，应该可以知道Type中否实现了接口中声明的所有方法。最后还会将Type方法表中的函数指针，拷贝到Itab的fun字段中。

Type的UncommonType中有一个方法表，某个具体类型实现的所有方法都会被收集到这张表中。reflect包中的Method和MethodByName方法都是通过查询这张表实现的。表中的每一项是一个Method，其数据结构如下：
```c
struct Method
{
    String *name;
    String *pkgPath;
    Type    *mtyp;
    Type *typ;
    void (*ifn)(void);
    void (*tfn)(void);
};
```
Iface的Itab的InterfaceType中也有一张方法表，这张方法表中是接口所声明的方法。其中每一项是一个IMethod，数据结构如下：
```c
struct IMethod
{
    String *name;
    String *pkgPath;
    Type *type;
};
```
跟上面的Method结构体对比可以发现，这里是只有声明没有实现的。
Iface中的Itab的func域也是一张方法表，这张表中的每一项就是一个函数指针，也就是只有实现没有声明。

类型转换时的检测就是看Type中的方法表是否包含了InterfaceType的方法表中的所有方法，并把Type方法表中的实现部分拷到Itab的func那张表中。

### 6.2.3. reflect
reflect就是给定一个接口类型的数据，得到它的具体类型的类型信息，它的Value等。reflect包中的TypeOf和ValueOf函数分别做这个事情。
还有像
```go
    v, ok := i.(T)
```
这样的语法，也是判断一个接口i的具体类型是否为类型T，如果是则将其值返回给v。这跟上面的类型转换一样，也会检测转换是否合法。不过这里的检测是在运行时执行的。在runtime下的iface.c文件中，有一系统的assetX2X函数，比如runtime.assetE2T，runtime.assetI2T等等。这个实现起来比较简单，只需要比较Iface中的Itab的type是否与给定Type为同一个。
## 6.3. 方法调用
### 6.3.1. 普通的函数调用
普通的函数调用跟C语言中的调用方式基本上是一样的，除了多值返回的一些细微区别，见前面章节。

### 6.3.2. 对象的方法调用
根据Go语言文档，对象的方法调用相当于普通函数调用的一个语法糖衣。
```go
type T struct {
    a int
}
func (tv  T) Mv(a int) int         { return 0 }  // value receiver
func (tp *T) Mp(f float32) float32 { return 1 }  // pointer receiver

var t T
```
表达式
```go
T.Mv
```
得到一个函数，这个函数等价于Mv但是带一个显示的接收者作为第一个参数，也就是
```go
func(tv T, a int) int
```
下面这些调用是等价的：
```go
t.Mv(7)
T.Mv(t, 7)
(T).Mv(t, 7)
f1 := T.Mv; f1(t, 7)
f2 := (T).Mv; f2(t, 7)
```
### 6.3.3. 组合对象的方法调用
在Go中没有继承，但是有结构体嵌入的概念。将一个带方法的类型匿名嵌入到另一个结构体中，则这个结构体也会拥有嵌入的类型的方法。

这个功能是如何实现的呢？其实很简单。当一个类型被匿名嵌入结构体时，它的方法表会被拷贝到嵌入结构体的Type的方法表中。这个过程也是在编译时就可以完成的。对组合对象的方法调用同样也仅仅是普通函数调用的语法糖衣。
### 6.3.4. 接口的方法调用
接口的方法调用跟上述情况略有不同，不同之处在于它是根据接口中的方法表得到对应的函数指针，然后调用的，而前面是直接调用的函数地址。

对象的方法调用，等价于普通函数调用，函数地址是在编译时就可以确定的。而接口的方法调用，函数地址要在运行时才能确定。将具体值赋值给接口时，会将Type中的方法表复制到接口的方法表中，然后接口方法的函数地址才会确定下来。因此，接口的方法调用的代价比普通函数调用和对象的方法调用略高，多了几条指令。

# 7. 网络
## 7.1. 非阻塞IO
Go提供的网络接口，在用户层是阻塞的，这样符合人们的编程习惯。在runtime层面，是用epoll/kqueue实现的非阻塞io，为性能提供了保障。
### 7.1.1. 如何实现
所有的文件描述符都设置成非阻塞的，某个goroutine进行io操作，读或者写文件描述符，如果刺客io还没准备好，则这个goroutine会被放到系统地等待队列中，这个goroutine失去了运行全，并不是真正的整个系统阻塞于系统调用。

后台还有一个poller会不停地进行poll，所有的文件描述符都被添加到这个poller中，当某个时刻一个文件描述符就绪了，poller就会唤醒之前因它而阻塞的goroutine，于是，goroutine重新运行起来。

这个poller是在后台已知运行的，在proc.c文件中，runtime.main函数的第一行代码就是：
```c
newm(sysmon, nil);
```
这个意思就是新建一个M并让它运行sysmon函数，前面说过M就是机器的抽象，它会直接开一个物理线程。sysmon里面是个死循环，每睡眠一小会儿就会调用runtime.epoll函数，这个sysmon就是所谓的poller。
### 7.1.2. 封装层次
从最原始的epoll系统调用，到提供给用户的网络库函数，可以分成三个封装层次。这三个层次分别是，依赖于系统的api封装，平台独立的runtime封装，提供给用户的库的封装。

最后一层封装层次是提供给用户的net包。在net包中网络文件描述符都是用一个netFD结构体来表示的，其中有个成员就是pollDesc。
```go
// 网络文件描述符
type netFD struct {
    sysmu  sync.Mutex
    sysref int

    // must lock both sysmu and pollDesc to write
    // can lock either to read
    closing bool

    // immutable until Close
    sysfd       int
    family      int
    sotype      int
    isConnected bool
    sysfile     *os.File
    net         string
    laddr       Addr
    raddr       Addr

    // serialize access to Read and Write methods
    rio, wio sync.Mutex

    // wait server
    pd pollDesc
}
```

### 7.1.3. 文件描述符和goroutine
当一个goroutine进行io阻塞时，会去被放到等待队列。这里面就关键的就是建立起文件描述符和goroutine之间的关联。pollDesc结构体就是完成这个任务的。它的结构体定义如下：
```c
struct PollDesc
{
    PollDesc* link;    // in pollcache, protected by pollcache.Lock
    Lock;        // protectes the following fields
    int32    fd;
    bool    closing;
    uintptr    seq;    // protects from stale timers and ready notifications
    G*    rg;    // 因读这个fd而阻塞的G，等待READY信号
    Timer    rt;    // read deadline timer (set if rt.fv != nil)
    int64    rd;    // read deadline
    G*    wg;    // 因写这个fd而阻塞的goroutines
    Timer    wt;
    int64    wd;
};
```
这个结构体是重用的，其中link就是将它链起来。PollDesc对象必须是类型稳定的，因为在描述符关闭/重用之后我们会得到epoll/kqueue就绪通知。结构体中有一个seq序号，稳定的通知是通过使用这个序号实现的，当deadline改变或者描述符重用时，序号会增加。
runtime_pollServerInit的实现就是调用更下层的runtime·netpollinit函数。 runtime_pollOpen从PollDesc结构体缓存中拿一个出来，设置好它的fd。之所以叫Open而不是new，就是因为PollDesc结构体是重用的。 runtime_pollClose函数调用runtime·netpollclose后将PollDesc结构体放回缓存。
这些都还没涉及到fd与goroutine交互部分，仅仅是直接对epoll的调用。从下面这个函数可以看到fd与goroutine交互部分：
func runtime_pollWait(pd *PollDesc, mode int) (err int)
它会调用到netpollblock，这个函数是这样子的：
```c
static void
netpollblock(PollDesc *pd, int32 mode)
{
    G **gpp;

    gpp = &pd->rg;
    if(mode == 'w')
        gpp = &pd->wg;
    if(*gpp == READY) {
        *gpp = nil;
        return;
    }
    if(*gpp != nil)
        runtime·throw("epoll: double wait");
    *gpp = g;
    runtime·park(runtime·unlock, &pd->Lock, "IO wait");
    runtime·lock(pd);
}
```
最后的runtime.park函数，就是将当前的goroutine(调用者)设置为waiting状态。
上面这一部分是goroutine被放到等待队列的部分，下面看它被唤醒的部分。在sysmon函数中，会不停地调用runtime.epoll，这个函数对就绪的网络连接进行poll，返回可运行的goroutine。epoll只能知道哪个fd就绪了，那么它怎么知道哪个goroutine就绪了呢？原来epoll的data域存放的就是PollDesc结构体指针。因此就可以得到其中的goroutine了。