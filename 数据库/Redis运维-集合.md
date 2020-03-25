[TOC]

##  集合

集合类型中不允许有重复元素。

###  命令

####  集合内操作

* 添加元素：`sadd key element [element...]`，返回添加成功的元素的个数
* 删除元素：`srem key element [element...]`，返回成功删除的元素个数
* 计算元素个数：`scard key`，时间复杂度o(1)，它不会遍历集合所有元素，而是直接使用Redis内部的变量
* 判断元素是否在集合中：`sismember key element`；
* 随机从集合中返回指定个数元素：`srandmember key [count]`；count是可选参数，如果不选默认为1
* 从集合中随机弹出元素：`spop key`
* 获取所有元素：`smembers key`，`smembers`和`lrange`、`hgetall`都属于比较重的命令，如果元素过多存在阻塞Redis的可能性

####  集合间操作

* 求多个集合的交集：`sinter key [key...]`
* 求多个集合的并集：`sunion key [key...]`
* 求多个集合的差集：`sdiff key [key...]`

![1584771291231](https://i.loli.net/2020/03/22/xSVAwQnaLh4kT1R.png)

* 将交集、并集、差集的结果保存：`sinterstore des key [key...]`、`sunionstore des key [key...]`、`sdiffstore des key [key...]`，结果保存在des中

###  内部编码

集合类型的内部编码有两种：

* intset：当集合中的元素都是整数且元素个数小于`set-max-intset-entries`配置（默认512个）时，Redis会选用intset来实现集合
* 当集合类型无法满足intset的条件时，Redis会使用hashtable作为内部实现

###  使用场景

* 给用户添加标签：`sadd user tag`
* 给标签添加用户：`sadd tag user`
* 删除用户下的标签：`srem user tag`
* 删除标签下的用户：`srem tag user`
* 计算用户共同感兴趣的标签：`sinter user1 user2`
* 生成随机数（抽奖）：`srandmember/spop`