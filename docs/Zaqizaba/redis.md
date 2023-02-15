## 简介

REmote Dictionary Server(Redis) 是一个由 Salvatore Sanfilippo 写的 key-value 存储系统，是跨平台的非关系型数据库。

Redis 是一个开源的使用 ANSI C 语言编写、遵守 BSD 协议、支持网络、可基于内存、分布式、可选持久性的键值对(Key-Value)存储数据库，并提供多种语言的 API。

Redis 通常被称为数据结构服务器，因为值（value）可以是字符串(String)、哈希(Hash)、列表(list)、集合(sets)和有序集合(sorted sets)等类型。

!!! note "什么是 BSD 协议"

    BSD开源协议是一个给于使用者很大自由的协议。可以自由的使用，修改源代码，也可以将修改后的代码作为开源或者专有软件再发布。当你发布使用了BSD协议的代码，或者以BSD协议代码为基础做二次开发自己的产品时，需要满足三个条件：

    如果再发布的产品中包含源代码，则在源代码中必须带有原来代码中的BSD协议。
    如果再发布的只是二进制类库/软件，则需要在类库/软件的文档和版权声明中包含原来代码中的BSD协议。
    不可以用开源代码的作者/机构名字和原来产品的名字做市场推广。
    BSD代码鼓励代码共享，但需要尊重代码作者的著作权。BSD由于允许使用者修改和重新发布代码，也允许使用或在BSD代码上开发商业软件发布和销 售，因此是对商业集成很友好的协议。

    很多的公司企业在选用开源产品的时候都首选BSD协议，因为可以完全控制这些第三方的代码，在必要的时候可以修改或者 二次开发。

Redis 与 其他 Key-value 缓存产品相比有以下三个特点：

1. Redis 支持数据的持久化。它运行在内存中，但可以持久化到磁盘。
2. Redis 不仅支持简单的 key-value，同时支持 list, set, zset, hash 等数据结构的存储。
3. Redis 支持数据的备份，即 master-slave 模式的数据备份。

它的优势：

1. 性能极高。Redis 能读的速度是 110000 次/s，写的速度是 81000 次/s。
2. 丰富的数据类型
3. 原子操作。每个操作要么成功执行要么失败完全不执行。单个操作是原子的。多个操作也支持事务，即原子性，通过 MULTI 和 EXEC 指令包起来。
4. 丰富的特性。Redis 支持 publish/subscribe、通知、key 过期等特性。

## 安装

官网安装链接：[https://redis.io/docs/getting-started/installation/](https://redis.io/docs/getting-started/installation/)

## 配置

在 Redis 中查看或者设置配置的命令结构如下：

```sehll
redis 127.0.0.1:6379> CONFIG GET CONFIG_SETTING_NAME

redis 127.0.0.1:6379> CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
```

一些例子：

```shell
# 获取所以配置
redis 127.0.0.1:6379> CONFIG GET *

# 将 loglevel 设置为 notice
redis 127.0.0.1:6379> CONFIG SET loglevel "notice"
```

## 命令

本地启动 redis-server 的命令是：

```shell
# method 1
$ service redis start

# method 2
$ redis-server
```

本地启动 redis 客户端的命令是：

```shell
$ redis-cli
```

远程启动 redis 客户端的命令是：

```shell
$redis-cli -h host -p port -a password
```

如果启动客户端后做查询有中文乱码，可以在 redis-cli 后面加上 `--raw`，就可以避免中文乱码了。

## 数据类型

### 字符串

Redis 字符串数据类型是最基本的类型。

```shell
127.0.0.1:6379> SET str_key "kpole"
OK
127.0.0.1:6379> GET str_key
"kpole"
```

### 哈希

Redis hash 是一个 string 类型的 field 和 value 的映射表，它特别适合用于存储对象。

Redis 中每个 hash 可以存储 $2^{32}-1$ 个键值对。

```shell
127.0.0.1:6379> HMSET hash_key name "kpole" description "acmer" age 24
OK
127.0.0.1:6379> HGET hash_key name
"kpole"
127.0.0.1:6379> HINCRBY hash_key age 1
(integer) 25
127.0.0.1:6379> HGETALL hash_key
1) "name"
2) "kpole"
3) "description"
4) "acmer"
5) "age"
6) "25"
```

### 列表
Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

一个列表最多可以包含 $2^{32} - 1$ 个元素。

```shell
127.0.0.1:6379> LPUSH q_key redis
(integer) 1
127.0.0.1:6379> RPUSH q_key mysql
(integer) 2
127.0.0.1:6379>  LPUSH q_key leveldb
(integer) 3
127.0.0.1:6379> LRANGE q_key 0 3
1) "leveldb"
2) "redis"
3) "mysql"
```

### 集合

Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

集合对象的编码可以是 intset 或者 hashtable。

Redis 中集合是通过哈希表实现的，所以添加、删除、查找的复杂度都是 $O(1)$。

集合中最大的成员数是 $2^{32} - 1$。

```shell
127.0.0.1:6379> SADD s_key redis
(integer) 1
127.0.0.1:6379> SADD s_key mysql
(integer) 1
127.0.0.1:6379> SADD s_key leveldb
(integer) 1
127.0.0.1:6379> SMEMBERS s_key
1) "mysql"
2) "leveldb"
3) "redis"
127.0.0.1:6379> SCARD s_key
(integer) 3
```

### 有序集合

Redis 有序集合也是 string 类型元素的集合，且不允许重复的成员。不同的是每个元素都会关联一个 double 类型的分数。redis 正是通过分数来进行排序的。

分数可以重复。

当元素内容超过 64 的时候，会同时使用 hash 和 skiplist 两种设计来实现。这也会为了排序和查找性能做优化。

```shell
127.0.0.1:6379> ZADD zs_key 1 redis
(integer) 1
127.0.0.1:6379> ZADD zs_key 3 mysql
(integer) 1
127.0.0.1:6379> ZADD zs_key 2 leveldb
(integer) 1
127.0.0.1:6379> ZRANGE zs_key 0 3 WITHSCORES
1) "redis"
2) "1"
3) "leveldb"
4) "2"
5) "mysql"
6) "3"
```

## 参考

- https://www.runoob.com/redis/redis-tutorial.html