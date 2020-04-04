<!-- TOC -->

- [键管理](#%e9%94%ae%e7%ae%a1%e7%90%86)
  - [单个键管理](#%e5%8d%95%e4%b8%aa%e9%94%ae%e7%ae%a1%e7%90%86)

<!-- /TOC -->
#  键管理
##  单个键管理
针对单个键的命令
*  键重命名。`rename key newkey`,`renamenx`只有在newkey不存在的时候才被覆盖。
*  随机返回一个键。`randomkey`
*  键过期。除了expire、ttl命令外，Redis还提供了expireat、pexpire、pexpireat、pttl、persist等一系列命令。
    *  `expire key second`：键在second秒后过期
    *  `expireat key timestamp`：键在秒级时间戳timestap后过期，
    *  ttl和pttl都可以查询键的剩余过期时间，但是pttl精度更高可以达到毫秒级别，有3中种返回值：
        *  大于等于0的整数：键剩余过期时间
        *  -1：键没有设置过期时间
        *  -2：键不存在
    
    expireat命令可以设置键的秒级过期时间戳。
使用Redis相关过期指令时需要注意：
    *  如果expire key的键不存在，返回结果为0
    *  如果过期时间为负值，键会立即被删除。
    *  persist命令可以将键的过期时间清除
    *  对于字符串类型键，执行set命令会去掉过期时间
    *  Redis不支持二级数据结构内部元素的过期功能，例如不能对列表类型的一个元素做过期时间设置
    *  setex命令作为set+expire的组合，不但是原子操作，同时减少了一次网络通讯的时间

*  迁移键
Redis发展历程中提供了move、dump+restore、migrate三组迁移键的方法
    *  move
    move命令用于在Redis内部进行数据迁移，Redis内部可以有多个数据库。
    ![move](https://i.loli.net/2020/03/26/UM75dlfIPcEWY8H.png)
    *  dump+restore
    ```
    dump key
    restore key ttl value
    ```
    dump+restore可以实现在不同的Redis实例之间进行数据迁移的功能：
        *  在源Redis上，dump命令会将键值序列化，格式采用的是RDB格式
        *  在目标Redis上，restore命令将上面序列化的值进行复原，其中ttl参数代表过期时间，如果ttl为0则表示没有过期时间
    
    整个迁移过程并非原子性的，而是通过客户端分布完成的。迁移过程是开启了两个客户端连接，所以dump的结果不是在源Redis和目标Redis之间进行传输
    *  migrate
    migrate命令也是用于在Redis实例之间进行数据迁移的，实际上migrate命令就是将dump、restore、del三个命令进行组合，从而简化操作流程。
*  遍历键
Redis提供了两个命令遍历所有的键，分别是keys和scan
    1.  全量遍历键：`keys pattern`，支持正则表达式
    2.  