
<!-- TOC -->

- [有序集合](#有序集合)
    - [命令](#命令)
        - [集合内](#集合内)
        - [集合间的操作](#集合间的操作)
    - [内部编码](#内部编码)
    - [使用场景](#使用场景)

<!-- /TOC -->
##  有序集合

有序集合保留了集合不能有重复元素的特性，但是有序集合的元素可以排序。它给每一个元素设置一个分数（score）作为排序依据。有序集合提供了获取指定分数和元素范围查询、计算成员排名等功能。

![1584772059341](https://i.loli.net/2020/03/22/OemUrSLFuQf5JXR.png)

有序集合中的元素不能重复，但是score可以重复。

![1584772195575](https://i.loli.net/2020/03/22/8GF9lUvbphHysTA.png)

###  命令

####  集合内

* 添加成员：`zadd key score member [score member...]`
* 计算成员个数：`zcard key`
* 计算某个成员的分数：`zscore key member`，如果成员不存在，返回nil
* 计算成员的排名：`zrank key member`，`zrank`是分数从低到高排，`zrevrank`反之，排名从0开始
* 删除成员：`zrem key member [member...]`，返回成功删除的个数
* 返回指定排名范围的成员：`zrange key start end [withscores]`、`zrevrange key start end [withscores]`，如果加上withscore选项，则会同时返回成员的数据
* 返回指定分数范围的成员：`zrangebyscore key min max [withscore] [limit offset count]`，分数从低到高，`zrevrangebyscore`反之，[limit offset count]选项可以限制输出的起始位置和个数，max和min还支持开区间和闭区间，-inf和+inf代表无限小和无限大
* 返回指定分数范围成员个数：`zcount key min max`
* 删除指定排名内的升序元素：`zremrangebyrank key start end`
* 删除指定分数范围内的成员：`zremrangebyscore key min max`

####  集合间的操作

* 交集：`zinterstore des numkeys key [key...] [weights weight [weight ...]] [aggregate sum|min|max]`；des：交集结果保存的位置；numkeys：需要做交集的键的个数；key：做交集的键；weights weight[weight...]：每个键的权重，在交集时，每个建中的member会将自己分数乘以这个权重，每个键的权重默认为1；aggreget sum|min|max：计算成员交集后，分值可以按照sum、min、max做汇总，默认是sum
* 并集：`zunionstore destination numkeys key [key ...] [weights weight [weight ...]] [aggregate sum|min|max]`

###  内部编码

有序集合的内部编码有两种：

* ziplist：当有序集合个数小于`zset-max-ziplist-entries`的配置（默认128个），同时每个元素的值都小于`zset-max-ziplist-value`配置（默认64字节）时，Redis使用ziplist作为内部实现
* skiplist：当ziplist不满足条件时，使用skiplist

###  使用场景

* 添加用户赞数：`zadd user name count`、`zincrby user name`
* 取消用户攒数：`zrem user name`
* 展示获取赞数最多的是个用户：`zrevrangebyrank user 0 9`
* 展示用户信息以及用户分数：`zscore user name`、`zrank user name`

