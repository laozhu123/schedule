# Redis

[TOC]


## 简介
> Redis是一个速度非常快的非关系数据库（non-relational database），它可以存储键（key）与5种不同类型的值（value）之间的映射（mapping），可以将存储在内存的键值对数据持久化到硬盘，可以使用复制特性来扩展读性能，还可以使用客户端分片1来扩展写性能。

### 特性
1. 复制特性以及为解决问题而生的独一无二的数据模
2. Redis提供了5种不同类型的数据结构
3. 复制、持久化（persistence）和客户端分片（client-side sharding）
4. 非关系型数据库

### Redis与其他数据库和软件的对比

|名称|类型|数据存储选项|查询类型|附加功能|
|-|-|-|-|-|
|Redis|使用内存存储（in-memory）的非关系数据库|字符串、列表、集合、散列表、有序集合|每种数据类型都有自己的专属命令，另外还有批量操作（bulk operation）和不完全（partial）的事务支持|发布与订阅，主从复制（master/slave replication），持久化，脚本（存储过程，stored procedure）|
|memcached|使用内存存储的键值缓存|键值之间的映射|创建命令、读取命令、更新命令、删除命令以及其他几个命令|为提升性能而设的多线程服务器|
|MySQL|关系数据库|每个数据库可以包含多个表，每个表可以包含多个行；可以处理多个表的视图（view）；支持空间（spatial）和第三方扩展|SELECT、INSERT、UPDATE、DELETE、函数、存储过程|支持ACID性质（需要使用InnoDB），主从复制和主主复制 （master/master replication）|
|PostgreSQL|关系数据库|每个数据库可以包含多个表，每个表可以包含多个行；可以处理多个表的视图；支持空间和第三方扩展；支持可定制类型|SELECT、INSERT、UPDATE、DELETE、内置函数、自定义的存储过程|支持ACID性质，主从复制，由第三方支持的多主复制（multi-master replication）|
|MongoDB|使用硬盘存储（on-disk）的非关系文档存储|每个数据库可以包含多个表，每个表可以包含多个无schema（schema-less）的BSON文档|创建命令、读取命令、更新命令、删除命令、条件查询命令等|支持map-reduce操作，主从复制，分片，空间索引（spatial index）|

### 持久化
> Redis拥有两种不同形式的持久化方法，它们都可以用小而紧凑的格式将存储在内存中的数据写入硬盘：第一种持久化方法为时间点转储（point-in-time dump），转储操作既可以在“指定时间段内有指定数量的写操作执行”这一条件被满足时执行，又可以通过调用两条转储到硬盘（dump-to-disk）命令中的任何一条来执行；第二种持久化方法将所有修改了数据库的命令都写入一个只追加（append-only）文件里面，用户可以根据数据的重要程度，将只追加写入设置为从不同步（sync）、每秒同步一次或者每写入一个命令就同步一次。

### 分布式Redis

1. 主从服务器，主服务器将原始副本copy到从服务器，而后每次的更新操作都从主服务器发放到从服务器，保证所有服务器的数据库一致性，在查找的时候就可以多个服务器分流了。提高了查询速度。


### Redis提供的5种结构

|结构类型|结构存储的值|结构的读写能力|
|-|-|-|
|STRING|可以是字符串、整数或者浮点数|对整个字符串或者字符串的其中一部分执行操作；对整数和浮点数执行自增（increment）或者自减（decrement）操作|
|LIST|一个链表，链表上的每个节点都包含了一个字符串|从链表的两端推入或者弹出元素；根据偏移量对链表进行修剪（trim）；读取单个或者多个元素；根据值查找或者移除元素|
|SET|包含字符串的无序收集器（unordered collection），并且被包含的每个字符串都是独一无二、各不相同的|添加、获取、移除单个元素；检查一个元素是否存在于集合中；计算交集、并集、差集；从集合里面随机获取元素|
|HASH|包含键值对的无序散列表|添加、获取、移除单个键值对；获取所有键值对|
|ZSET（有序集合）|字符串成员（member）与浮点数分值（score）之间的有序映射，元素的排列顺序由分值的大小决定|添加、获取、删除单个元素；根据分值范围（range）或者成员来获取元素|


## 实战使用

### redis命令
[redis命令大全](http://redis.io/commands)

#### 全局操作
#查看所有key
keys *  或  keys "*"

#查看匹配前缀的keys
keys "miao*"

#清空redis
flushdb

#随机取出一个key
randomkey

#查看key的类型
type key

#查看数据库中key的数量
dbsize

#查看服务器信息
info

#查看redis正在做什么
monitor   #注意，有高手的文章说这个会急剧降低redis性能，只能在测试环境使用。

#查看日志
slowlog get
slowlog get 10

#### 普通操作

##### 设置value
set key value:设置key的值,若存在则覆盖
setnx key value:SET if Not eXists,若存在则不操作。
MSET key1 value1 key2 value2 ... keyN valueN:设置这些key的值，若存在则覆盖
MSETNX key1 value1 key2 value2 ... keyN valueN:同mset，但如果其中一个key已经存在了，则都不设置。这些操作都是原子的。
##### 修改获取删除value
append key value:向key的字符串追加拼接。
get key:获取key对应的值 MGET key1 key2 ... keyN:获取这些key对应的值 
EXISTS key:查看是否存在该元素。
GETSET key value:获取该元素的值，并给该元素设置新值。（通常和incr搭配使用，比如一个mycount一直incr，然后达到某些情况需要清零，清零之前需要知道mycount的值）.
del key:删除元素
RENAME oldkey newkey:重命名

##### redis提供原子自增操作incr,用来防止多线程并发出现数据错误
incr key:原子的+1；
DECR key：原子的-1；
DECRBY key integer：原子的-integer；
INCRBY key integer：原子的+integer

##### del.若数据不存在返回(nil)
> del miao 
(integer)1
get miao
(nil)

#### 持久化
#####  redis可以定时存储，即设置几秒后删除该变量
expire key 多少秒：设置多少秒后过期；  ttl key:Time To Live，查看还可以存活多久，-2表示key不存在；-1表示定时任务消失，永久存储。
SETEX key seconds value：等价于先设置变量再设置超时，即在缓存中使用：存储的同时设置超时时间，这个操作是原子的
persist key:取消过期时间
expireat key 时间戳：unix时间戳，1970.1.1之后，这个绝对时间，将在这个时间删除key。expireat pages:about 1356933600：在2012年12月31日上午12点删除掉关键字
SETEX KEY_NAME TIMEOUT VALUE：设置key的值为value，并在timeout秒后失效，key将被删除
##### 存储有序队列：list
rpush keyList value:向keyList添加元素,向后加,r表示右边

lpush keyList value:向keyList左边添加元素，LPUSH puts the new value at the start of the list.

lrange keyList beginIndex endIndex:获取keyList的元素，用两端的索引取出子集，endIndex=-1则表示全部取出

##### 无序且唯一集合set
> 和java中list与set的区别一样。这里的set无序且值唯一。

sadd key value : 向set添加元素
srem key value ：从set中移除元素
smembers key : 取出所有set元素
SISMEMBER key value: 查看value是否存在set中
SUNION key1 key2 ... keyN:将所有key合并后取出来，相同的值只取一次
scard key : 获取set中元素的个数
SRANDMEMBER key: Return a random element from a Set, without removing the element.随机取出一个
SDIFF key1 key2 ... keyN：获取第一set中不存在后面几个set里的元素。
SDIFFSTORE dstkey key1 key2 ... keyN：和sdiff相同，获取key1中不存在其他key里的元素，但要存储到dstkey中。
SINTER key1 key2 ... keyN:取出这些set的交集
SINTERSTORE dstkey key1 key2 ... keyN：取出这些key的交集并存储到dstkey
SMOVE srckey dstkey member：将元素member从srckey中转移到dstkey中，这个操作是原子的。

##### 有序集合sorted set
> 和set一样，唯一。但z多了个score用来排序。所以命令又像list一样：

ZADD key score member：向有序set中添加元素member，其中score为分数，默认升序；
ZRANGE key start end [WITHSCORES]:获取按score从低到高索引范围内的元素，索引可以是负数，-1表示最后一个，-2表示倒数第二个，即从后往前。withscores可选，表示获取包括分数。
ZREVRANGE key start end [WITHSCORES]：同上，但score从高到低排序。
ZCOUNT key min max：获取score在min和max范围内的元素的个数
ZCARD key:获取集合中元素的个数。
ZINCRBY key increment member:根据元素，score原子增加increment.
ZREMRANGEBYSCORE key min max:清空集合内的score位于min和max之间的元素。
ZRANK key member:获取元素的索引（照score从低到高排列）。
ZREM key member:移除集合中的该元素
ZSCORE key member:获取该元素的score

##### 对象存储Hashes
> 可以存储对象，比如人，编号，姓名，年龄等

HSET key field value:key是对象名，field是属性，value是值；
HMSET key field value [field value ...]:同时设置多个属性
HGET key field：获取该对象的该属性
HMGET key field value [field value ...]：获取多个属性值
HGETALL key:获取对象的所有信息
HKEYS key：获取对象的所有属性
HVALS key：获取对象的所有属性值
HDEL key field：删除对象的该属性
HEXISTS key field:查看对象是否存在该属性
HINCRBY key field value:原子自增操作，只能是integer的属性值可以使用；
HLEN key: Return the number of entries (fields) contained in the hash stored at key.获取属性的个数。