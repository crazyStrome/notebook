[TOC]

## 哈希

在Redis中，哈希类型是指键值本身又是一个键值对结构，形如`value={{field1 value1}...}`

 

![计算机生成了可选文字: value 字符串 字符串 亠一宀 哈希 fieldvalue 图2．14 字符串和哈希类型对比](https://i.loli.net/2020/03/22/4joSgbxFEMetWln.png)

哈希表中的映射关系是field-value。

---

### 命令

- 设置值：`hset key field value`；
- 获取值：`hget key field`；
- 删除field：`hdel field [field…]`
- 计算key的个数：`hlen key`
- 批量获取设置field-value：`hmget key field [field…]`；`hmset key field value [field value…]`
- 判断field是否存在：`hexists key field`
- 获取所有的field：`hkeys key`，返回哈希键所有的field
- 获取素有的value：`hvals key`；
- 获取所有的field-values：`hgetall key`；
- `hincrby`、`hincrfloat`和`incrby`和`incrbyfloat`命令一样，作用域是field
- 计算value的字符串长度：`hstrlen key field`，需要Redis3.2以上

---

### 内部编码

哈希表的内部编码有两种：

- ziplist：当哈希类型元素小于==hash-max-ziplist-entries==配置（默认512）、同时所有的值都小于==hash-max-ziplist-value==配置（默认64字节）时，使用ziplist作为内部实现，ziplist使用更加紧凑的结构实现多个元素的连续存储，所以在节省内存方面比hashtable更加优秀。
- hashtable：当哈希类型无法满足ziplist时，Redis用hashtable作为内部实现，因为此时ziplist的读写效率会降低，而hashtable的读写时间复杂度为o(1)。

---

### 使用场景

- 哈希表存储用户数据

