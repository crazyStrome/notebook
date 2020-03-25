<!-- TOC -->

- [字符串](#字符串)
    - [命令](#命令)
    - [内部编码](#内部编码)
    - [典型使用场景](#典型使用场景)

<!-- /TOC -->
## 字符串

字符串是最基础的数据结构。键都是字符串类型，而且其他几种数据结构都是在字符串类型的基础上构建的。

字符串类型的值实际可以是字符串（简单的字符串、复杂的字符串（例如JSON、XML））、数字（整数、浮点数），甚至是二进制（图片、音频、视频），但是值最大不能超过512MB。

---

###  命令

常用命令

- 设置值：`set key value [ex seconds] [px milliseconds] [nx|xx]`，ex seconds设置秒级过期时间；px milliseconds设置毫秒级过期时间；nx当键不存在时才可以设置成功，用于添加；xx当键存在才可以设置成功，用于更新。
- `setex key value；setnx key value`；分别和xx、nx功能一样
- 获取value：`get key`；当键不存在返回nil。
- 批量设置值：`mset key value [key value…]`
- 批量获取值：`mget key [key…]`
- 计数：`incr key`；incr用于对值做自增操作；当值不是整数，返回错误；当值是整数，返回自增后的结果；键不存在，按照0自增1，返回1 。
- `decr`：自减；`incrby`：自增指定数字；`decrby`：自减指定数字；`incrbyfloat`：自增浮点数。

不常用命令

- 追加值：`append key value`，向字符串尾追加值。
- 字符串长度：`strlen key`，返回value的长度。
- 设置并返回原值：`getset key value`。
- 设置指定位置的字符：`setrange key offset value`。
- 获取部分字符串：`getrange key start end`；start和end分别为开始和结束的偏移量。

---

###  内部编码

字符串内部编码有三种：

- int：8个字节的长整型。
- embstr：小于等于39个字节的字符串
- raw：大于39个字节的字符串。

---

### 典型使用场景

- 缓存：Redis作为缓存层，MySQL作为储存层。Redis支持高并发特性，所以缓存能起到加速读写和降低后端压力的作用。
- 计数：使用Redis的incr命令可以进行计数
- 共享session：集中管理所有session
- 输入验证码间隔限制：可以使用set nx指令，同时指定expire时间