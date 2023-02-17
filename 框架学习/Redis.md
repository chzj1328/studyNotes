

# Redis

## 一、Redis安装和概述

### 1.1、概述

> Redis是什么？

Redis（Remote Dictionary Server )，即远程字典服务，

Redis是一个开源的key-value存储系统，当下最热门的NoSQL 技术之一。

### 1.2、安装

```java
1.把redis-6.2.6.tar.gz上传到/opt文件夹
2.查看是否有gcc编译环境 gcc --version
    如果没有则进行安装 yum install gcc
3.解压redis-6.2.6.tar.gz文件 tar -zxvf redis-6.2.6.tar.gz
    解压之后进入到redis-6.2.6文件夹 cd redis-6.2.6
    输入不含参数make命令进行编译 make
    安装redis make install
```

### 1.3查看安装目录

#### 1.3.1、默认的目录：/usr/local

1.redis-benchmark:性能测试工具，可以在自己的本子运行看看性能如何

2.redis-check-aof:恢复有问题的AOF文件

3.redis-check-dump:恢复有问题的dump.rdb文件

4.redis-sentinel:Redis集群使用

5.redis-server:Redis服务器启动命令

6.redis-cli:客户端，操作入口

#### 1.3.2、后台启动Redis

1、进入/opt,查看目录，**把redis.conf复制到/etc文件名为redis.conf**

2、后台设置**daemonize no改成yes**

3、redis启动 **启动命令：redis-server /etc/redis.conf**

4、查看服务器进程：**ps -ef | grep redis**

5、通过客户端链接Redis **命令：redis-cli**

6、关闭Redis:

- shutdown
- exit
- kill -9 3837

#### 1.3.3、测试性能

**redis-benchmark**是一个压力测试工具！

redis-benchmark 命令参数	

```bash
redis-benchmark [option] [option value]
```

**简单测试一下：**

```bash
# 测试：100个并发连接   100000个请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

## 二、Redis键（key）

```
- keys  * ：查看当前库中所有的key

- set key value :向当前库中添加key和value

- exists key ：判断某个key是否存在

- type key :查看指定key的数据类型

- del key : 删除指定的key数据

- unlink key:根据value选择非阻塞删除

  紧将keys从keyspace元数据中删除，真正删除会在后续的异步操作

- expire key 10:10秒钟为key设置过期时间

- MOVE key db 将当前数据库的 key 移动到给定的数据库 db 当中。

- move name 1 移除当前的key

- PERSIST key 移除 key 的过期时间，key 将持久保持。

- RANDOMKEY 从当前数据库中随机返回一个 key 。

- RENAME key newkey 修改 key 的名称

- TYPE key 返回 key 所储存的值的类型。

- ttl key 查看还有多少秒过期，-1表示永不过期，-2表示已过期

- select 命令切换数据库

- dbsize:查看当前数据库的key的数量

- flushdb:清空当前库

- flushall：通杀全部库
```

## 三、redis字符串（String）

### 3.1、String

1、String是Redis的基本数据类型，一个keyduiyin

2、String是二进制安全的，可以包含任何数据。比如jpg图片或者序列化对象

3、String中字符串value最多可以是512M

### 3.2、String的基本操作

```nosql
set key value 添加键值对 
get <key> 查询对应的键值对
append <key><value> 将给定的<value>追加到原址的末尾
strlen <key> 获取值的长度
setnx <key><value> 只有在key不存在时设置key的值
setex <key><过期时间><value>
	-设置键的同时，设置过期时间，单位秒
incr <key>
	-将key中储存的数字值增1
	-只能对数字值操作，如果为空，新增值为1
decr <key>
	-将key中储存的数字值减1
	-只能对数字操作，如果为空，新增值为-1
incrby/decrby <key><步长> 将Key中储存的数字值增减。自定义步长
mset <key1><value1><key2><value2> 同时设置一个或多个key-value键值对
mget <key1><key2><key3> 同时获取一个或多个value
msetnx <key1><value1><key2><value2>...
	-同时设置一个或多个key-value对，当且仅当所给定的key不存在时
	-原子性，有一个失败则全都失败
getrange <key><起始位置><结束位置>
	-获得值的范围，雷士java中的substring,前包,后包
setrange <key><起始位置><value>
	-用<value>覆写<key>所储存的字符串的值，从<起始位置>开始（索引从0开始）
getset <key><value> 以旧换新，设置了新值的同时获得旧值。

set user:1 {name:zhangsan,age:20} 设置一个user:1对象 值为json字符串来保存一个对象

```

### 3.3、简单测试

```bash
127.0.0.1:6379> set key1 v1 # 设置值
OK
127.0.0.1:6379> get key1 # 获得值
"v1"
127.0.0.1:6379> EXISTS key1 # 判断key1是否存在
(integer) 1
127.0.0.1:6379> APPEND key1 "hello" # 在key1后面追加一个字符串
(integer) 7
127.0.0.1:6379> get key1 
"v1hello"
127.0.0.1:6379> STRLEN key1 # 获得value的长度
(integer) 7
127.0.0.1:6379> keys * # 获取所有的key
1) "key1"
127.0.0.1:6379> APPEND name "zhangsan" # 追加时如果key不存在，则创建一个新的key
(integer) 8
127.0.0.1:6379> keys *
1) "name"
2) "key1"
127.0.0.1:6379> set key2 "hello,world"
OK
127.0.0.1:6379> GETRANGE key2 0 3 # 截取字符串[0,3]
"hell"
127.0.0.1:6379> GETRANGE key2 0 -1 # 截取全部字符串 和 get key是一样的
"hello,world"
27.0.0.1:6379> mset k1 v1 k2 v2 k3 v3  # 同时设置多个key值
OK
127.0.0.1:6379> keys *
1) "name"
2) "k1"
3) "k3"
4) "key1"
5) "key2"
6) "k2"
127.0.0.1:6379> mget k1 k2 k3 # 同时获取多个key值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 20
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "20"
127.0.0.1:6379> 
```



**redis的操作是原子性的，不会被线程调度机制大段的操作；**

String的数据结构为简单的动态字符串。是可以修改字符串，采用预分配冗余空间的方式来减少内存的频繁分配

## 四、Redis列表（List）

### 4.1、基本介绍

#### 1、单键多值

- 简单的字符串列表，按照插入顺序排序。底层是一个双向链表

#### 2、常用命令

```nosql
lpush/rpush <key><value1><value2><value3>...从左边/右边插入一个或多个值
lpop/rpop <key> 从左边/右边吐出一个值。值在键在，值光键消
rpoppush <key1><key2>从<key1>列表右边吐出来一个值，插到<key2>列表右边
rpoplpush <key1><key2> 移除列表最后一个元素，将其添加到新的列表
lrange <key><start><stop> 按照索引获得元素（从左到右）
lindex <key><index> 按照所给下标获得元素（从左到右）
llne <key> 获取列表的长度 
linsert<key> before | after<value><newvalue> 在<value>的后面插入<newvalue>插入值
lrem<key><n><value>从左边删除n个value（从左到右）
lset<key><index><value>将列表key下标为<index>的值替换成<value>
```

**简单测试**

```bash
127.0.0.1:6379> LPUSH list one  # 将一个或多个值，插到列表的头部（左）
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1 # 获取全部的值
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> LRANGE list 0 1 # 通过区间获取具体的值
1) "three"
2) "two"
127.0.0.1:6379> LRANGE list 0 0
1) "three"
127.0.0.1:6379> RPUSH list hello # 将一个或多个值，插到列表的头部（右）
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "hello"
127.0.0.1:6379> LPOP list  # 移除list列表的第一个元素
"three"
127.0.0.1:6379> RPOP list  # 移除list列表的最后一个元素
"hello"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> LINDEX list 1 # 通过下标获得list中的某一个值
"one"
127.0.0.1:6379> LINDEX list 0
"two"
####################################################################
# LLEN 
127.0.0.1:6379> LPUSH list 1
(integer) 1
127.0.0.1:6379> LPUSH list 2
(integer) 2
127.0.0.1:6379> LPUSH list 3
(integer) 3
127.0.0.1:6379> LPUSH list 4
(integer) 4
127.0.0.1:6379> LLEN list # 返回list列表的长度
(integer) 4
####################################################################
# LREM 移除指定的值
127.0.0.1:6379> LRANGE list 0 -1
1) "4"
2) "3"
3) "2"
4) "1"
127.0.0.1:6379> LREM list 1 1 # 移除list集合中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> LRANGE list 0 -1
1) "4"
2) "3"
3) "2"
127.0.0.1:6379> LPUSH list 4
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "4"
2) "4"
3) "3"
4) "2"
127.0.0.1:6379> LREM list 2 4
(integer) 2
127.0.0.1:6379> LRANGE list 0 -1
1) "3"
2) "2"

####################################################################
127.0.0.1:6379> LPUSH list one two three four 
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "four"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379> LTRIM list 1 2 # 通过下标截取指定的长度，list已经被改变了，截断了只剩下截取的元素 
OK
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"

####################################################################
# rpoplpush 移除列表最后一个元素，将其添加到新的列表
127.0.0.1:6379> rpush list "hello"
(integer) 1
127.0.0.1:6379> rpush list "hello1"
(integer) 2
127.0.0.1:6379> rpush list "hello2"
(integer) 3
127.0.0.1:6379> rpush list "hello3"
(integer) 4
127.0.0.1:6379> rpoplpush list list1
"hello3"
127.0.0.1:6379> lrange list1 0 -1
1) "hello3"
127.0.0.1:6379> lrange list 0 -1
1) "hello"
2) "hello1"
3) "hello2"

####################################################################
# lset<key><index><value>将列表key下标为<index>的值替换成<value>
127.0.0.1:6379> EXISTS list
(integer) 0
127.0.0.1:6379> lset list 0 item # 如果不存在列表，就会报错
(error) ERR no such key
127.0.0.1:6379> lpush list hello
(integer) 1
127.0.0.1:6379> lset list 1 item
(error) ERR index out of range
127.0.0.1:6379> lset list 0 item # 如果存在就会替换当前下标的值
OK
127.0.0.1:6379> LRANGE list 0 0
1) "item"

####################################################################
# linsert<key> before | after <value><newvalue> 在<value>的后面插入<newvalue>插入值
127.0.0.1:6379> LPUSH list 1
(integer) 1
127.0.0.1:6379> LPUSH list 2
(integer) 2
127.0.0.1:6379> LINSERT list after 2 3
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1
1) "2"
2) "3"
3) "1"
127.0.0.1:6379> LINSERT list before 1 0
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "2"
2) "3"
3) "0"
4) "1"
```



## 五、Redis集合（set）

### 5.1、简介

- Redis set 对外提供的功能和List类似是一个列表功能，set可以**自动排重**
- Redis set是string类型的**无序集合**添加、删除、查找的复杂度都是O(1)

### 5.2、常用命令

```nosql
sadd <key> <value1> <value2> <value3>将一个或多个member元素加入到集合key中，已经存在的member元素将被忽略
smembers<key> 取出该集合的所有值
sismember <key><value>判断集合<key>是否含有该<value>值，有1，没有0
scard<key> 返回该集合的元素个数
srem<key><value1><value2>...删除集合中的某个元素。
spop<key>随机从该集合中吐出一个值。
srandmember<key><n>随机从该集合中取出n个值，不会从集合中删除
smove <source><distination> value把集合中一个值从一个集合移动到另一个集合
sinter <key1><key2>返回两个集合的交集元素
sunion<key1><key2>返回两个元素的并集元素
sdiff<key1><key2>返回两个集合的差集元素（key1中的，不包含key2中的）
```

**简单测试**

```bash
127.0.0.1:6379> sadd set "hello" # 向set集合中添加元素
(integer) 1
127.0.0.1:6379> sadd set "chen"
(integer) 1
127.0.0.1:6379> sadd set "world"
(integer) 1
127.0.0.1:6379> SMEMBERS set    # 查看指定set的所有值
1) "world"
2) "chen"
3) "hello"
127.0.0.1:6379> SISMEMBER set hello # 判断某一个元素是否在set中，返回1代表存在，返回0代表不存在
(integer) 1
127.0.0.1:6379> SISMEMBER set world!
(integer) 0
127.0.0.1:6379> SCARD set  # 获取set集合中的元素个数
(integer) 3
127.0.0.1:6379> SREM set world  # 移除set集合中的指定的元素
(integer) 1
127.0.0.1:6379> SCARD set
(integer) 2
127.0.0.1:6379> SMEMBERS set
1) "chen"
2) "hello"
127.0.0.1:6379> SRANDMEMBER set  # 随机抽选出某一个元素[也可以是指定的个数]
"hello"
127.0.0.1:6379> SRANDMEMBER set
"chen"
127.0.0.1:6379> SPOP set # 随机删除一些set集合里的元素
"hello"
127.0.0.1:6379> SADD set hello world chen 
(integer) 3
127.0.0.1:6379> SADD set1 hello
(integer) 1
127.0.0.1:6379> SMOVE set set1 world # 将指定的元素从一个集合移动到另外一个集合中去
(integer) 1
127.0.0.1:6379> SMEMBERS set
1) "chen"
2) "hello"
127.0.0.1:6379> SMEMBERS set1
1) "world"
2) "hello"

# 数字集合类：
# - 差集
# - 交集
# - 并集
127.0.0.1:6379> SADD key1 a
(integer) 1
127.0.0.1:6379> SADD key1 b
(integer) 1
127.0.0.1:6379> SADD key1 c
(integer) 1
127.0.0.1:6379> SADD key2 d
(integer) 1
127.0.0.1:6379> SADD key2 c
(integer) 1
127.0.0.1:6379> SADD key2 e
(integer) 1
127.0.0.1:6379> SDIFF key1 key2  # 差集
1) "b"
2) "a"
127.0.0.1:6379> SINTER key1 key2 # 交集
1) "c"
127.0.0.1:6379> SUNION key1 key2 # 并集
1) "a"
2) "c"
3) "b"
4) "e"
5) "d"
```



## 六、Redis 哈希（Hash）

### 6.1、简介

redis hash 是一个键值对集合，是一个string类型的field和value的映射表，hash特别适合储存对象

### 6.2、常用命令

```nosql
hset <key><field><value> 给<key>集合的filed赋值i<value>
hget <key1><filed> 从key1>集合<filed>取出<value>
hmset <key1><field><value1><key2><field><value2>批量设置hash的值
hexists<key1><field> 查看哈希表key中，给定域filed是否存在。
hkeys <key> 列出该hash集合的所有field
hvals <key>列出该hash集合的所有的value
hincrby <key><field><increment>为哈希表key中的域field的值加上增量 1 -1
hsetnx <key><field><value> 将哈希表key中的域field的值设置为value,当且仅当field不存在
```

### 6.3、简单测试

```bash
127.0.0.1:6379> HSET myhash filed1 value1  # set一个具体的 key-value
(integer) 1
127.0.0.1:6379> HGET myhash filed1 # 获取一个字段值
"value1"
127.0.0.1:6379> HMSET myhash filed1 value2 filed2 value3  # set 多个key-value
OK
127.0.0.1:6379> HMGET myhash filed1 filed2 # 获取多个值
1) "value2"
2) "value3"
127.0.0.1:6379> HGETALL myhash  # 获取全部的数据
1) "filed1" 
2) "value2"
3) "value3"
4) "value4"
5) "filed2"
6) "value3"
127.0.0.1:6379> HDEL myhash filed2 # 删除hash指定的key字段，对应的value值也被删除了
(integer) 1 
127.0.0.1:6379> HGETALL myhash
1) "filed1"
2) "value2"
3) "value3"
4) "value4"
127.0.0.1:6379> HMSET myhash filed1 hello filed2 world
OK
127.0.0.1:6379> HGETALL myhash
1) "filed1"
2) "hello"
3) "filed2"
4) "world"
127.0.0.1:6379> HLEN myhash  # 获取hash表的字段数量
(integer) 2
127.0.0.1:6379> HEXISTS myhash filed1 # 判断某一个字段是否存在，返回值1代表存在，否则是不存在
(integer) 1
127.0.0.1:6379> HEXISTS myhash filed3
(integer) 0
127.0.0.1:6379> HKEYS myhash # 获取所有的filed
1) "filed1"
2) "filed2"
127.0.0.1:6379> HVALS myhash # 获取所有的value  
1) "hello"
2) "world"
127.0.0.1:6379> HSET myhash f 1  # 指定增量
(integer) 1
127.0.0.1:6379> HINCRBY myhash f 1
(integer) 2
127.0.0.1:6379> HINCRBY myhash f -1
(integer) 1

```

## 七、Redis有序集合Zset

### 7.1、简介

Redis 有序集合zset与set 类似，是一个没有重复元素的字符串集合

与set不同的是zset是有序的，访问有序集合的中间元素也很快，因为能够使有序集合作为一个没有重复成员的智能列表

### 7.2、常用命令

```noosql
zadd <key><score1><value1><score2><value2> 将一个或多个member元素及其score值加入到有序集key中
zrange <key><start><stop>[WITHSCORES]
返回有序集合key中，下表在<start><stop>之间的元素，带WITHSCORES，可以让分数一起和值返回到结果集。
zrangebyscore key minmax[withscores][limit offset count]
返回有序集合key中，所有score值介于min和max之间(包括min和max)的成员。按score值递增次序排列
zrevrangebyscore key maxmin[withscores][limit offset count] 同上改为从大到小排列
zincrby <key><increment><value> 为元素的score加上增量
zrem <key><value> 删除该集合下的，指定值的元素
zcount <key><min><max> 统计该集合，分数区间内的元素个数
zrank <key><value> 返回该值在集合中的排名，从0开始。
```

### 7.3、简单测试

```bash
127.0.0.1:6379> ZADD myset 1 one  # 添加一个值
(integer) 1
127.0.0.1:6379> ZADD myset 2 two 3 three # 添加多个值
(integer) 2
127.0.0.1:6379> ZRANGE myset 0 -1
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> ZADD salary 2500 xiaohong
(integer) 1
127.0.0.1:6379> ZADD salary 5000 zhangsan 
(integer) 1
127.0.0.1:6379> ZADD salary 3000 chen
(integer) 1
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf # 显示全部的用户 从小到大
1) "xiaohong"
2) "chen"
3) "zhangsan"
127.0.0.1:6379> ZREVRANGE salary 0 -1 # 显示全部的用户 从大到小 
1) "zhangsan"
2) "xiaohong"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf withscores  # 显示全部用户并且附带成绩
1) "xiaohong"
2) "2500"
3) "chen"
4) "3000"
5) "zhangsan"
6) "5000" 
127.0.0.1:6379> ZREM salary chen # 移除有序集合中的指定元素
(integer) 1
127.0.0.1:6379> ZRANGE salary 0 -1 # 获取有序集合中的个数
1) "xiaohong"
2) "zhangsan"
127.0.0.1:6379> ZADD myset 1 hello 2 world 3 chen
(integer) 3
127.0.0.1:6379> ZCOUNT myset 1 2 # 获取指定区间中的成员数量
(integer) 2
127.0.0.1:6379> ZCOUNT myset 1 3
(integer) 3
127.0.0.1:6379> 
```

## 八、三种特殊数据类型

### 8.1、geospatial 地理位置

Redis的Geo可以推算地理位置的信息

只有6个命令：

- geoadd：添加地理位置的坐标。
- geopos：获取地理位置的坐标。
- geodist：计算两个位置之间的距离。
- georadius：根据用户给定的经纬度坐标来获取指定范围内的地理位置集合。
- georadiusbymember：根据储存在位置集合里面的某个地点获取指定范围内的地理位置集合。
- geohash：返回一个或多个位置对象的 geohash 值。

geoadd 用于存储指定的地理空间位置，可以将一个或多个经度(longitude)、纬度(latitude)、位置名称(member)添加到指定的 key 中。

geoadd 语法格式如下：

```bash
GEOADD key  latitude longitude member [latitude longitude member ...]
```

> geoadd：添加地理位置的坐标。

```bash
127.0.0.1:6379> GEOADD china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> GEOADD china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> GEOADD china:city 113.28 23.12 guangzhou
(integer) 1
127.0.0.1:6379> GEOADD china:city 113.66 34.75 henan
(integer) 1

```

> geopos：获取地理位置的坐标。

```bash
127.0.0.1:6379> GEOPOS china:city beijing  # 获取指定城市的精度和纬度
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
127.0.0.1:6379> GEOPOS china:city henan
1) 1) "113.65999907255172729"
   2) "34.74999926510690784"
127.0.0.1:6379> 

```

> geodist：计算两个位置之间的距离。

geodist 用于返回两个给定位置之间的距离。

geodist 语法格式如下：

```bash
GEODIST key member1 member2 [m|km|ft|mi]
```

member1 member2 为两个地理位置。

最后一个距离单位参数说明：

- m ：米，默认单位。
- km ：千米。
- mi ：英里。
- ft ：英尺。

```bash
127.0.0.1:6379> GEODIST china:city beijing henan km # 查看北京到河南的距离
"621.8822"
127.0.0.1:6379> GEODIST china:city beijing shanghai km # 查看北京到上海的距离
"1067.3788"
127.0.0.1:6379> 
```

> ### georadius、georadiusbymember

- georadius 以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。

- georadiusbymember 和 GEORADIUS 命令一样， 都可以找出位于指定范围内的元素， 但是 georadiusbymember 的中心点是由给定的位置元素决定的， 而不是使用经度和纬度来决定中心点。

- georadius 与 georadiusbymember 语法格式如下：

- ```bash
  GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
  GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
  ```

- 参数说明：

- - m ：米，默认单位。
  - km ：千米。
  - mi ：英里。
  - ft ：英尺。
  - WITHDIST: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。
  - WITHCOORD: 将位置元素的经度和纬度也一并返回。
  - WITHHASH: 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。
  - COUNT 限定返回的记录数。
  - ASC: 查找结果根据距离从近到远排序。
  - DESC: 查找结果根据从远到近排序

```bash
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km # 以110,30这个经纬度为中心，寻找方圆1000KM你的城市
1) "guangzhou"
2) "henan"
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km withdist # 显示到中心距离的位置
1) 1) "guangzhou"
   2) "831.7713"
2) 1) "henan"
   2) "630.2160"
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km withcoord # 显示他人的定位信息
1) 1) "guangzhou"
   2) 1) "113.27999979257583618"
      2) "23.1199990030198208"
2) 1) "henan"
   2) 1) "113.65999907255172729"
      2) "34.74999926510690784"
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km withdist withcoord count 1 # 筛选出指定的结果 
1) 1) "henan"
   2) "630.2160"
   3) 1) "113.65999907255172729"
      2) "34.74999926510690784" 
# 找出位于指定元素周围的其他元素
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 1000 km 
1) "henan" 
2) "beijing"
127.0.0.1:6379> GEORADIUSBYMEMBER china:city shanghai 1000 km 
1) "shanghai"
2) "henan"
```

### 8.2、Hyperloglog

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

**什么是基数?**

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

下表列出了 redis HyperLogLog 的基本命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [PFADD key element [element ...\]](https://www.runoob.com/redis/hyperloglog-pfadd.html) 添加指定元素到 HyperLogLog 中。 |
| 2    | [PFCOUNT key [key ...\]](https://www.runoob.com/redis/hyperloglog-pfcount.html) 返回给定 HyperLogLog 的基数估算值。 |
| 3    | [PFMERGE destkey sourcekey [sourcekey ...\]](https://www.runoob.com/redis/hyperloglog-pfmerge.html) 将多个 HyperLogLog 合并为一个 HyperLogLog |

```bash
127.0.0.1:6379> PFADD runoobkey "redis" "mongodb" "mysql" # 创建第一组元素
(integer) 1
127.0.0.1:6379> PFCOUNT runoobkey # 统计runoobkey 元素的基数数量
(integer) 3
127.0.0.1:6379> PFADD myset "redis" "mysql" "java" # 创建第二组 myset
(integer) 1 
127.0.0.1:6379> PFMERGE mykey myset runoobkey # 合并两组
OK
127.0.0.1:6379> PFCOUNT mykey
(integer) 4
```

### 8.3、Bitmaps

使用**bitmaps**来记录一周的打卡

查看某天是否打卡

```bash
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0
```

统计操作，统计打卡的天数

```bash
127.0.0.1:6379> bitcount sign  # 统计这周打卡的天数
(integer) 3
```

## 九、事务

Redis事务本质：一组命令的集合，一个事务中的所有命令都会被序列化，在食物执行过程中，会按照顺序执行！

一次性、顺序性、排他性！

一个事务从开始到执行会经历以下三个阶段：

- 开始事务。(MULTI)
- 命令入队。
- 执行事务。(EXEC)

**Redis事务没有隔离级别的概念**

**Redis单条命令保证原子性，但事务不保证原子性**

**Redis 事务命令**

下表列出了 redis 事务的相关命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [DISCARD](https://www.runoob.com/redis/transactions-discard.html) 取消事务，放弃执行事务块内的所有命令。 |
| 2    | [EXEC](https://www.runoob.com/redis/transactions-exec.html) 执行所有事务块内的命令。 |
| 3    | [MULTI](https://www.runoob.com/redis/transactions-multi.html) 标记一个事务块的开始。 |
| 4    | [UNWATCH](https://www.runoob.com/redis/transactions-unwatch.html) 取消 WATCH 命令对所有 key 的监视。 |
| 5    | [WATCH key [key ...\]](https://www.runoob.com/redis/transactions-watch.html) 监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。 |

```bash
127.0.0.1:6379> MULTI  # 开启事务
OK
# 命令入队
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> EXEC # 执行事务
1) OK
2) OK
3) "v2"
4) OK

```

> 放弃事务

```bash
127.0.0.1:6379>  MULTI # 开启事务
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> set k4 v4
QUEUED
127.0.0.1:6379(TX)> DISCARD #放弃事务
OK
127.0.0.1:6379> get k4 # 事务中的所有命令都不会被执行
(nil)
```

> 编译型异常（代码有问题，命令有错）事务中所有的命令都不会被执行

```bash
127.0.0.1:6379> MULTI 
OK
127.0.0.1:6379(TX)> set k1 v1 
QUEUED
127.0.0.1:6379(TX)> set k2 v2 
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> getset k3
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379(TX)> set k4 v4
QUEUED
127.0.0.1:6379(TX)> EXEC # 执行事务报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k4 # 所有命令都不会被执行
(nil)
```

> 运行时异常！如果事务队列中存在与发行错误，那么执行命令的时候，其他命令可以执行，错误命令抛出异常

```bash
127.0.0.1:6379> set k1 "hello"
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> INCR k1
QUEUED
127.0.0.1:6379(TX)> set k2 v2 
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> get k3
QUEUED
127.0.0.1:6379(TX)> EXEC
1) (error) ERR value is not an integer or out of range
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
127.0.0.1:6379> get k3
"v3"
```

> 监控！

```bash
# Redis监控测试
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money # 监视 money 对象
OK
127.0.0.1:6379> MULTI  # 事务正常结束，数据期间米有发生变动就正常执行成功
OK
127.0.0.1:6379(TX)> DECRBY money 20
QUEUED
127.0.0.1:6379(TX)> INCRBY out 20
QUEUED
127.0.0.1:6379(TX)> EXEC
1) (integer) 80
2) (integer) 20
```



## 十、Redis配置文件详解

> 网络

```bash
bind 127.0.0.1 -::1  		# 绑定的IP
protected-mode yes 			# 保护模式
port 6379					# 默认端口号
```

> 通用GENERAL

```bash
daemonize yes 						# 以守护进程的方式运行，默认是no，我们需要修改为yes
pidfile /var/run/redis_6379.pid		# 如果以后台方式运行，我们就需要指定一个pid文件

# 日志
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice
logfile "" # 日志的文件位置名
databases 16 # 数据库的数量，默认16个数据库
always-show-logo no		# 是否总显示LOGO
```

> 快照

持久化，在规定的时间内，执行了多少次操作，则会持久化到文件 .rdb .aof

redis 是内存数据库，如果没有持久化，那么数据断电即失！

```bash
# 如果3600秒内，如果至少有一个key进行了修改，我们就会进行持久化操作
save 3600 1
# 如果300秒内，如果至少有100个key进行了修改，我们就会进行持久化操作
save 300 100
# 如果60秒内，如果至少有10000个key进行了修改，我们就会进行持久化操作
save 60 10000
# 我们后续会定义自己的测试

stop-writes-on-bgsave-error yes		# 持久化如果出错，是否继续工作

rdbcompression yes					# 是否压缩rdb文件，需要消耗一些CPU的资源

rdbchecksum yes						# 保存rdb文件的时候，进行错误的检查校验

dir ./								# rdb文件保存的目录
```



> SECURITY 安全

设置密码

```bash
127.0.0.1:6379> CONFIG GET requirepass 		#获取redis的密码
1) "requirepass"
2) ""
127.0.0.1:6379> CONFIG SET requirepass "zjch1328"	# 设置redis的密码
OK
127.0.0.1:6379> CONFIG GET requirepass
1) "requirepass"
2) "zjch1328"
127.0.0.1:6379> CONFIG GET requirepass
(error) NOAUTH Authentication required.
127.0.0.1:6379> AUTH zjch1328		# 使用密码进行登录
OK
```

> CLIENTS 客户端

```bash
maxclients 10000		# 设置能连接上redis的最大客户端的数量
maxmemory <bytes>		# redis 配置最大的内存容量
maxmemory-policy noeviction		# 内存到达上限之后的处理策略
1、volatile-lru：只对设置了过期时间的key进行LRU（默认值） 
2、allkeys-lru ： 删除lru算法的key   
3、volatile-random：随机删除即将过期key   
4、allkeys-random：随机删除   
5、volatile-ttl ： 删除即将过期的   
6、noeviction ： 永不过期，返回错误
```

> APPEND ONLY  aof模式

```bash
appendonly no 		# 默认是不开启aof模式的
appendfilename "appendonly.aof" 	# 持久化的文件的名字

# appendfsync always	# 每次修改都会写入 sync
appendfsync everysec	# 美妙执行一次 sync,可能会丢失着一秒的数据
# appendfsync no		# 不执行 sync，这个时候操作系统自己同步数据，速度最快
```

### 10.2、网络相关的配置

- bind 默认情况 bind=127.0.0.1 只能接受本机的访问请求 注释掉bind=127.0.0.1
- 不写的情况下，无限制接受任何IP地址访问，生产环境肯定要写你的应用服务器的地址；服务器是需要远程访问的，所以要将其注释掉。
- protected-mode 开启之后，也只能本地链接 把protected-mode no
- tcp-backlog 设置tcp的backlog,backlog其实是一个链接队列，backlog队列=未完成三次握手的队列+王城三次握手的队列。在高并发下你需要一个高的backlog值来避免慢客户端的问题
- timeout 设置连接无操作超时时间

## 十二、Jedis

> 测试

1、导入对应的依赖

```xml
<dependencies>
        <!--导入jedis的依赖包-->
        <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.7.0</version>
        </dependency>
        <!--fastjson-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.79</version>
        </dependency>
</dependencies>
```

2、编码测试

- 连接数据库
- 操作命令
- 断开连接

```java
public class TestPing {
    public static void main(String[] args) {
        // 1、new Jedis()对象即可
        Jedis jedis = new Jedis("***.***.***.***",6379);
        // jedis 所有命令就是之前的所有指令
        jedis.auth("*******");
        System.out.println(jedis.ping());
    }
}
```

IDEA上连接jedis时报错：

```java
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

解决：

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>1.7.25</version>
</dependency>
```

```java
查看端口开放信息，如果没看见 6379，就需要设置
firewall-cmd --list-ports

开启 6379 端口
firewall-cmd --zone=public --add-port=6379/tcp --permanent

最后记得重载防火墙生效
重载：firewall-cmd --reload
```

## 十三、Redis的持久化

### RDB

**rdb保存的文件是dump.rdb**都在我们的配置文件快照中进行配置的！

> 触发机制

1、save的规则满足的情况下，会自动触发rdb规则

2、执行了flushall命令也会触发我们的rdb规则

3、退出redis，也会产生rdb文件

备份就自动生成一个dump.rdb

比如save 10 100表示10秒内有n次修改时，自动触发bgsave，还有其他配置。

stop-writes-on-bgsave-error：设置yes表示bgsave失败时redis停止接收数据

rdbcompression：是否压缩

rdbchecksum：持久化时是否进行数据校验

> 如何恢复rdb文件！

1、只需要将rdb文件放在我们redis启动目录就可以，redis启动的时候会自动检查dump.rdb恢复其中的数据！

2、查看需要存放的位置

```bash
127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/usr/local/bin"		# 如果在这个目录下存在dump.rdb文件，启动就会自动恢复其中的数据
```

**优点：**

1、适合大规模的数据恢复！

2、对数据的完整性要求不高！

**缺点：**

1、需要一定的时间间隔进行操作如果redis意外宕机了这个最后一次的修改数据就没了

2、fork进程的时候，会占用一定的内存空间

### AOF

**AOF是指Redis将每一条指令（这里指可能对数据状态产生变化的写指令）通过write写入到AOF文件中。为了解决aof文件越来越大问题，redis提供bgrewriteaof命令，将内存数据先写入到内存文件，再fork线程将文件重写到新的aof文件。![f7b9823e8f344a6795c16a035c1013c6](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/f7b9823e8f344a6795c16a035c1013c6.png)**

**AOF的三种触发机制**

1、always：每次修改立即写入磁盘，性能最差数据完整性最好

2、everysec：每秒将内存指令写入磁盘，可能会造成数据丢失

3、no：由操作系统决定将内存指令写入磁盘

> append

默认时不开启的，需要我们进行手动配置！ 我们只需要将appendonly 改为yes就开启了 aof ！

重启，redis就可以生效了！

如果这个aof文件有错误，这时候redis是启动不起来的，我们需要修复这个aof文件

redis 给我们提供了一个工具`redis-check-aof --fix`

> 优点和缺点！

```bash
appendonly no 		# 默认是不开启aof模式的
appendfilename "appendonly.aof" 	# 持久化的文件的名字

# appendfsync always	# 每次修改都会写入 sync
appendfsync everysec	# 美妙执行一次 sync,可能会丢失着一秒的数据
# appendfsync no		# 不执行 sync，这个时候操作系统自己同步数据，速度最快
```

优点：

1、每一次修改都会同步，文件的完整会更加好！

2、每秒同步一次，可能会丢失一秒的数据

3、从不同步，效率是最高的

缺点：

1、相对于数据文件来说，aof远远大于rdb，修复的速度也比rdb慢

2、aof运行的效率也要比 rdb 慢，所以我们redis默认的配置就是 rdb 持久化

## 十四、Redis发布订阅

Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

Redis 客户端可以订阅任意数量的频道。

### 发布与订阅相关命令

1. 订阅

```shell
subscribe channel [channel ...]
```

订阅一个或多个频道，返回订阅的频道数

2. 发布

```shell
publish channel message
```

向一个频道发布消息，返回向多少个客户端发送了消息

3. 按照模式订阅

```shell
psubscribe pattern [pattern ...]
```

如果 publish 命令发送的频道和订阅的模式成功匹配，那么客户端就会接收到发布的消息

4. 查看频道

```shell
pubsub channels [pattern]
```

返回有客户端订阅的频道，是用 pattern 参数，只返回与模式匹配的有客户端订阅的频道

5. 查看频道的订阅数

```shell
pubsub numsub [channel ...]
```

返回频道和其订阅数

6. 查看模式的订阅数

```shell
pubsub numpat
```

### 实现原理：

Redis将订阅关系保存在一个字典里面（Redis的字典是用哈希表实现的，基本可以认为字典就是哈希表），其中键是字符串类型就是频道名，值是一个链表存着所有订阅了该频道的客户端指针。

![图1](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1554983-20201214073312145-1697911907.png)

如上图所示，有三个频道：aaa，bbb和ccc。其中client-1订阅了全部三个频道，client-2订阅了 aaa 和 bbb，client-3订阅了 aaa。其中客户端指针指向客户端状态结构，这个结构中保存着套接字描述符以及各种其他信息，Redis可以通过此结构提供的信息向客户端发送数据。

1. 频道订阅
   当订阅一个频道的时候，只需要将客户端指针添加到该频道对应的链表的末尾即可，如果该频道还不是字典的键，那么创建键和对应的链表，并将客户端指针添加到链表末尾。比如，client-3 执行了 `subscribe ccc` ，那么字典结构将如下图所示：

![图二](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1554983-20201214073339944-1145852956.png)

2. 退订频道
   当一个客户端使用 unsubscribe 命令时，就会将客户端指针从要退订的频道对应的列表中删除。比如，在图二基础上，client-3 执行 `unsubscribe ccc`，那么字典结构将由图二变为图一。如果将客户端指针删去后，频道对应的列表为空了，那么也会将频道删去。比如，在图一基础上，client-1 执行`unsubscribe ccc`，那么字典结构将如下图所示：

![图三](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1554983-20201214073405517-191367311.png)

3. 查看频道
   当执行`pubsub channels`时，只需要遍历字典的所有键，将其返回即可。如果给了模式参数，只需要将与模式匹配的键返回即可。如果对图一结构的字典，使用`pubsub channels`命令，返回值将是：

```shell
1) "aaa"
2) "bbb"
3) "ccc"
```

4. 查看频道的订阅数
   当执行`pubsub numsub [channel ...]`命令时，只需要返回频道对应的列表的长度即可，如果键值不存在返回0即可，如果对图一结构的字典，使用`pubsub numsub aaa`，返回值将是：

```shell
1) "aaa"
2) (integer) 3
```

5. 订阅模式
   订阅模式和客户端的关系存在一个链表中，每一个节点都会保存模式字符串和客户端指针，链表结构如下图所示：

![图四](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1554983-20201214073428156-1906133328.png)

如果此时 client-4 执行```psubscribe c*```，那么链表结构将如下图所示：

![图五](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1554983-20201214073450637-1693164835.png)

6. 模式退订
   当执行`punsubscribe pattern`命令时，就会将链表中模式和客户端指针都对应相等的节点删去，比如，client-3 执行`punsubscribe b*`，那么链表结构将如下图所示：

![图六](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1554983-20201214073510160-518092311.png)b

7. 发布信息
   在发布信息的时候，需要分别将信息发送给订阅频道的客户端和订阅模式的客户端。

- 在将信息发送给订阅频道的客户端时，只需要在字典中找到该频道对应的链表，然后将信息发送给链表中的所有客户端。
- 在将信息发送给订阅模式的客户端时，需要遍历整个保存模式订阅的链表，如果发布的频道和模式匹配，则将其发送给这个客户端
  以图一和图五为例，如果执行命令`publish bbb message`，那么首先在字典中找到频道 bbb 对应的客户端链表，将其发送给 client-1，client-2，然后遍历模式链表，发现之后模式 b* 匹配，那么再将信息发送给 client-3。

> 测试

订阅端：

```bash
# 订阅
127.0.0.1:6379> SUBSCRIBE chenhao		# 订阅一个频道 chenhao
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "chenhao"
3) (integer) 1
# 等待读取推送的信息
1) "message"	# 消息
2) "chenhao"	# 频道名称
3) "Hey,hello!"	# 消息的内容
```

发布端：

```bash
# 发布
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> PUBLISH chenhao Hey,hello!  # 发布者发布消息到频道！
(integer) 1
127.0.0.1:6379> 
```

## 十五、Redis主从复制

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master)，后者称为从节点(slave)；数据的复制是单向的，只能由主节点到从节点。

默认情况下，每台Redis服务器都是主节点；且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。

### **主从复制的作用**

主从复制的作用主要包括：

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
2. 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。
3. 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
4. 高可用基石：除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。

### **环境配置**

只配置从库，不用配置主库

```bash
127.0.0.1:6379> info replication	# 查看当前库的信息
# Replication
role:master	# 角色 master
connected_slaves:0	# 连接的从机
master_failover_state:no-failover
master_replid:ed2c33065f3151b8e44586c757acd211a7d5fefc
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

复制3个配置文件，修改对应的信息

1、端口号

2、pid 的名字

3、log文件名

4、dump.rdb 名字

修改完毕之后，启动我们的3个redis服务可以通过进程信息查看

### 一主二从

**默认情况下，每台Redis服务器都是主节点；**我们一般情况下只用配置从机就好了！

一主(79)二从(80,81)

```bash
127.0.0.1:6380> SLAVEOF 127.0.0.1 6379		# SLAVEOF 127.0.0.1 6379  找谁当自己的老大
OK
127.0.0.1:6380> info replication
# Replication
role:slave		# 当前角色是从机
master_host:127.0.0.1		# 可以看到主机的信息
master_port:6379
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
slave_read_repl_offset:14
slave_repl_offset:14
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:8e1feea7c3142c373b9aba16869225bfd0046f2c
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:14
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:14


# 在主机中查看！
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:1		# 多了从机的配置
slave0:ip=127.0.0.1,port=6380,state=online,offset=266,lag=0		# 多了从机的配置
master_failover_state:no-failover
master_replid:8e1feea7c3142c373b9aba16869225bfd0046f2c
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:266
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:266
127.0.0.1:6379> 
```

如果两个都配置完成，就是有两个从机了

> 细节

主机可以写，从机不能写只能读！

主机写：

从机不能写只能读取内容：

> 复制原理

Slave 启动成功连接到master后会发送一个sync同步命令

Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集的命令，在后台进程执行完毕之后，master将传送整个数据文件到slave，并完成一次完全同步。

1. 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中
2. 增量复制：Master继续将新的所有收集到的修改命令依次传给slave，完成同步

但是只要是从新连接master，一次完全同步（全量复制）将被自动执行！我们的数据一定可以在从机中看到

- 如果主机断开了连接我们可以使用`SLAVEOF no one`让自己变成主机。其他的节点就可以手动连接到最新的这个主节点！

### 哨兵模式

（自动选举老大模式）

> 概述

主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务器不可用。这不是一种推荐的方式，更时候，我们优先考虑哨兵模式。

谋朝篡位的自动版，能够后台监控主机是否故障，如果故障了根据票数自动将从库转为主库。

哨兵模式是一种特殊的模式，首先Redis提供了哨兵命令，哨兵是一个独立进程，作为进程，他会独立运行，其原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**

> 测试

我们目前的状态是 一主二从！

1、配置哨兵配置文件sentinel.conf

```bash
# sentinel monitor 被监控的名称 host port 1
sentinel monitor myredis 127.0.0.1 6379 1
```

后面的数字1，代表主机挂了，slave投票看让谁接替成为主机，票数多的，就会成为主机！

2、启动哨兵！

```bash
[root@iZbp17w9kj059pgbpavd3qZ bin]# redis-sentinel cconfig/sentinel.conf 
```

```bsah
18493:X 02 Mar 2022 08:37:06.335 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
18493:X 02 Mar 2022 08:37:06.335 # Redis version=6.2.6, bits=64, commit=00000000, modified=0, pid=18493, just started
18493:X 02 Mar 2022 08:37:06.335 # Configuration loaded
18493:X 02 Mar 2022 08:37:06.336 * Increased maximum number of open files to 10032 (it was originally set to 1024).
18493:X 02 Mar 2022 08:37:06.336 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.2.6 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                  
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 18493
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           https://redis.io       
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

18493:X 02 Mar 2022 08:37:06.336 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
18493:X 02 Mar 2022 08:37:06.343 # Sentinel ID is c468f0be30188a4e81a9756804ca03d10a2d6d04
18493:X 02 Mar 2022 08:37:06.343 # +monitor master myredis 127.0.0.1 6379 quorum 1
18493:X 02 Mar 2022 08:37:06.344 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ myredis 127.0.0.1 6379
18493:X 02 Mar 2022 08:37:06.349 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
```

> 哨兵模式

优点：

1、哨兵集群，基于主从复制模式，所有的主从配置优点，它全都有！

2、主从可以切换，故障可以转移，系统的可用性就会更好

3、哨兵模式就是主从复制的升级，手动到自动，更加健壮。

缺点：

1、redis不好在线扩容，集群容量一旦达到上限，在线扩容非常麻烦，

2、实现哨兵模式的配置是十分麻烦的，里面有很多选择！

> 哨兵模式的全部配置

```bash
# Example sentinel.conf
 
# 哨兵sentinel实例运行的端口 默认26379
port 26379
 
# 哨兵sentinel的工作目录
dir /tmp
 
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 1
 
# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd
 
 
# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
 
# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
# 这个数字越小，完成failover所需的时间就越长，
# 但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
# 可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
 
 
 
# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。  
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
 
# SCRIPTS EXECUTION
 
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
 
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
#这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，
#一个是事件的类型，
#一个是事件的描述。
#如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
  sentinel notification-script mymaster /var/redis/notify.sh
 
# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh
```

## 十六、Redis缓存穿透和雪崩（面试高频，工作常用）

Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最要害的问题，就是数据一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。

另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案。

### 缓存穿透

> 概念

缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败。当用户很多的时候，缓存都没有命中(秒杀！)，于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。

> 解决方案

布隆过滤器：

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力；

但是这种方法会存在两个问题：

1.如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键。

2.即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。

### 缓存击穿(量太大，缓存过期)

> 概述

这里需要注意和缓存穿透的区别，缓存击穿，是指一个key非常热点，在不停 的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。

当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导致数据库瞬间压力过大。

>  解决方案

1.设置热点数据永不过期

从缓存层面来看，没有设置过期时间，所以不会出现热点key过期后产生的问题。

2.加互斥锁

分布式锁：使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大。

### 缓存雪崩

> 概念

缓存雪崩，是指在某一个时间段，缓存集中过期失效，Redis宕机！

产生雪崩的原因之一，比如在写本文的时候，马上就要到双十二零点，很快就会迎来一波抢购，这波商品时间比较集中的放入了缓存，假设缓存一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会到达存储层，存储层的调用量会暴增，造成存储层也会挂掉的情况。

其实集中过期，倒不是非常致命，比较致命的缓存雪崩，是缓存服务器某个节点宕机或断网。因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候，数据库也是可以顶住压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压垮。

### 解决方案
**redis高可用**
这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。（异地多活）

**限流降级**
这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

**数据预热**
数据预热的含义是，在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。