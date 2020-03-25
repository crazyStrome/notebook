<!-- TOC -->

- [列表](#列表)
    - [命令](#命令)
        - [添加操作](#添加操作)
        - [查找](#查找)
        - [删除](#删除)
        - [修改](#修改)
        - [阻塞操作](#阻塞操作)
    - [内部编码](#内部编码)

<!-- /TOC -->
## 列表

列表用来储存多个有序的字符串，类表中每个字符串称为元素（lelement），一个列表最多储存232-1个元素。在Redis可以对列表两端插入和弹出操作，还可以获取指定范围的元素列表，获取指定索引下标的元素等。

列表类型有两个特点：第一、列表中的元素是有序的，这就意味着可以

通过索引下标获取某个元素或者某个范围内的元素列表，例如要获取图2-19的第5个元素，可以执行`lindex user：1：message4`（索引从0算起）就可以得到元素e。第二、列表中的元素可以是重复的，例如图2-20所示列表中包含了两个字符串a。

---

###  命令

#### 添加操作

* 从右边插入元素：`rpush key value [value…]`

* 从左边插入元素：`lpush key value [value…]`

* 向某个元素前或者后嵌入元素：`linsert key before|after privot value`

#### 查找

获取指定范围内的元素列表：`lrange key start end`，索引下标从左到右是0到N-1；从右到左是-1到-N。end包含自身。

获取列表指定下标的元素：`lindex key index`

获取列表长度：`llen key`

#### 删除

从列表左侧弹出元素：`lpop key`

从列表右侧弹出元素：`rpop key`

删除指定元素：`lrem key count value`；count>0时从左到右删除最多count个元素；count<0时，从右到左删除最多count绝对值个元素；count为0时删除所有。

按照索引范围修剪列表：`ltrim key start end`；范围外的数据被丢弃

#### 修改

修改指定下标元素：`lset key index newvalue`

#### 阻塞操作

* `blpop key [key…] timeout`

* `brpop key [key…] timeout`

1. 列表为空：如果timeout=3，那么客户端要等3秒返回，如果timeout=0，那么客户端已知阻塞下去。如果再次期间添加了数据，客户端立即返回。

2. 列表不为空：客户端会立即返回。

---

### 内部编码

列表的内部编码有两种：

* ziplist：当列表的元素小于`list-max-ziplist-entries`配置（默认512），同时列表的每个元素都小于`lsit-max-ziplist-value`配置（64字节）时，Redis会使用ziplist作为列表的内部实现。
* linkedlist：当列表类型无法满足ziplist的条件时，Redis会使用linkedlist作为列表的内部实现。