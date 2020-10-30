[TOC]





























# 关系型数据库
关系型数据库，是指采用了关系模型来组织数据的数据库，其以行和列的形式存储数据，以便于用户理解，关系型数据库这一系列的行和列被称为表，一组表组成了数据库。用户通过查询来检索数据库中的数据，而查询是一个用于限定数据库中某些区域的执行代码。

三大范式：
1. 第一范式：要求数据库表的每一列都是不可分割的原子数据项。
2. 第二范式：在第一范式的基础上，第二范式要求表中的每一列必须依赖于整个主键，而不能只依赖于部分主键。消除复合主键就可以避免部分依赖。
3. 第三范式：在第二范式的基础上，第三范式要求表中的每一列是直接依赖于主键，而不是间接依赖。


# 非关系型数据库
NoSQL（Not Only SQL），泛指非关系型的数据库。随着互联网web2.0网站的兴起，传统的关系数据库在处理web2.0网站，特别是超大规模和高并发的SNS类型的动态网站已经显得力不从心，出现了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。

非关系型数据库可以分为多个类别：
- 键值数据库：主要会使用到一个哈希表，这个表中有一个特定的键和一个指针指向特定的数据，比如Redis。
- 列存储数据库：通常是用来应对分布式存储的海量数据。键仍然存在，但是它们的特点是指向了多个列。
- 文档数据库：可以看作是键值数据库的升级版，允许之间嵌套键值，在处理网页等复杂数据时，文档型数据库比传统键值数据库的查询效率更高。比如MongoDB。
- 图形数据库：图形结构的数据库同其他行列以及刚性结构的SQL数据库不同，它是使用灵活的图形模型，并且能够扩展到多个服务器上



# Redis

> [Redis-JavaGuide](https://gitee.com/SnailClimb/JavaGuide/blob/master/docs/database/Redis/redis-all.md)

Redis是一个非关系型（NoSQL）内存键值数据库，主要用来做缓存，可以存储键和五种不同类型的值之间的映射。键的类型只能为字符串，值支持五种数据类型：字符串、列表、集合、散列表、有序集合。

Redis支持很多特性，例如将内存中的数据持久化到硬盘中，使用主从复制来扩展读性能。除了做缓存之外，Redis 也经常用来做分布式锁，甚至是消息队列。


## 缓存数据库
缓存的基本思想其实很简单，就是用空间换时间，可以提高系统的性能。通常的做法是在数据库之上增加了缓存数据库，把数据库中用户经常访问的热点数据保存在缓存中，提高用户的请求速度。

缓存数据库的主要优点有：
- 高性能：提高系统的性能，因为访问缓存速度比较快；
- 高并发：提高系统的并发量，Redis的并发量可以达到MySQL的10倍甚至更高；

一般像MySQL这类的数据库的QPS（Query Per Second，每秒查询次数）大概都在 1w 左右（4 核 8g），但是使用 Redis 缓存之后很容易达到 10w+，甚至最高能达到 30w+（就单机 redis 的情况，redis 集群的话会更高）。所以，直接操作缓存能够承受的数据库请求数量是远远大于直接访问数据库。

使用缓存数据库也会带来一些问题：
- 系统复杂性增加：引入缓存之后，需要维护缓存和数据库的数据一致性、维护热点缓存等。
- 系统开发成本往往会增加：引入缓存意味着系统需要一个单独的缓存服务，需要花费相应的成本的，并且这个成本还是很贵的，毕竟耗费的是宝贵的内存。

缓存数据库使用的比较多的主要是 Memcached 和 Redis，现在公司一般都是用 Redis 来实现缓存。

Memcached 和 Redis的共同点有：
- 都是基于内存的数据库，一般都用来当做缓存使用。
- 都有过期策略。
- 两者的性能都非常高。

Memcached 和 Redis的不同点有：
- Redis 支持更丰富的数据类型。Redis 不仅仅支持简单的 k/v 类型的数据，同时还提供 list，set，zset，hash 等数据结构的存储。Memcached 只支持最简单的 k/v 数据类型。
- Redis 支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用,而 Memecache 把数据全部存在内存之中。
- Redis 有灾难恢复机制。 因为可以把缓存中的数据持久化到磁盘上。
- Redis 在服务器内存使用完之后，可以将不用的数据放到磁盘上。但是，Memcached 在服务器内存使用完之后，就会直接报异常。
- Memcached 没有原生的集群模式，需要依靠客户端来实现往集群中分片写入数据；但是 Redis 目前是原生支持 cluster 模式的.
- Memcached 是多线程，非阻塞 IO 复用的网络模型；Redis 使用单线程的多路 IO 复用模型。 （Redis 6.0 引入了多线程 IO ）
- Redis 支持发布订阅模型、Lua 脚本、事务等功能，而 Memcached 不支持。并且，Redis 支持更多的编程语言。
- Memcached过期数据的删除策略只用了惰性删除，而 Redis 同时使用了惰性删除与定期删除。


## 缓存数据库的处理流程/缓存读写模式
处理流程一般为：
- 如果用户请求的数据在缓存中，就直接返回。
- 如果缓存中不存在，就看数据库中是否存在。
- 如果数据库中存在，就更新缓存中的数据。
- 如果数据库中不存在，就返回空数据。

缓存读写模式有三种，不存在最佳模式，根据具体的业务场景选择适合自己的缓存读写模式。

### 旁路缓存模式
在旁路缓存模式中，服务端需要同时维系数据库和缓存，并且是以数据库的结果为准。
- 读数据时，先从缓存中读取数据，如果存在，则直接返回；如果不存在，则去数据库中查询，成功后存入缓存。
- 写数据时，先更新数据库，成功后再删除缓存。

旁路缓存模式是使用比较多的一个缓存读写模式，比较适合读请求比较多的场景。在这种模式下可以尽可能的保证缓存的一致性，适合对一致性要求较高的场景。

另外，旁路缓存模式有首次请求数据一定不在缓存的问题，对于热点数据可以提前放入缓存中。


### 读写穿透模式
在旁路缓存模式中，业务应用需要同时维护数据库和缓存两个数据存储。

在读写穿透模式中，服务端封装了所有的数据处理细节：
- 读数据时，先从缓存中读取数据，如果存在，就直接返回；如果不存在，就先从数据库加载，写入到缓存后在返回。
- 写数据时，先查缓存，如果缓存中不存在，直接更新数据库。如果缓存中存在，就先更新缓存，然后缓存负责更新数据库。

存储服务封装了所有的数据处理细节（cache + DB），业务应用端代码只用关注业务逻辑本身，系统的隔离性更佳。


### 异步缓存写入模式
异步缓存写入模式类似于读写穿透模式。

在异步缓存写入模式下，更新数据时只更新缓存，不直接更新数据库，通过异步的方式将缓存结果批量或合并后再更新到数据库。这种方式的特点是效率非常高，但不是强一致，甚至会丢失数据，适用于访问量、点赞数等对于性能要求高，但一致性要求不高的场景。

## 基本数据结构
### String
String 数据结构可以用来保存字符串、整数或者浮点数。虽然 Redis 是用 C 语言写的，但是 Redis 并没有使用 C 的字符串表示，而是自己构建了一种简单动态字符串（Simple Dynamic String，SDS）。相比于 C 的原生字符串，SDS 不光可以保存文本数据还可以保存二进制数据，并且获取字符串长度复杂度为 O(1)（C 字符串为 O(N)）。

常用命令包括: 
- set：set key value，设置key
- get：get key，获取
- getset: getset key value，设置key并取值
- append：append key value2, 向key后添加value2
- del：del key，删除key
- expire：expire key seconds，设置过期时间
- mset：mset key1 value1 key2 value2，批量设置
- mget：mget key1 key2，批量获取
- exists：exists key，判断key是否存在
- setnx：setnx key value，当key不存在时设置key，key存在则失败
- strlen：strlen key，获取key的长度
- incr：incr key，当key对应的字符串是数值，加一
- incrby：incrby key increment
- decr：decr key，当key对应的字符串是数值，减一
- decrby：decrby key decrement
- setex：setex key value seconds，同时设置数值和过期时间，仅适用于数值
- ttl：ttl key，返回XX表示过期时间，返回-1表示永久有效，返回-2表示已经过期的数据或被删除的数据或未定义的数据。


应用场景：一般常用在需要计数的场景，比如用户的访问次数、热点文章的点赞转发数量等等

基本操作：
```
127.0.0.1:6379> set key value #设置 key-value 类型的值
OK
127.0.0.1:6379> get key # 根据 key 获得对应的 value
"value"
127.0.0.1:6379> exists key  # 判断某个 key 是否存在
(integer) 1
127.0.0.1:6379> strlen key # 返回 key 所储存的字符串值的长度。
(integer) 5
127.0.0.1:6379> del key # 删除某个 key 对应的值
(integer) 1
127.0.0.1:6379> get key
(nil)
```

批量设置：
```
127.0.0.1:6379> mset key1 value1 key2 value2 # 批量设置 key-value 类型的值
OK
127.0.0.1:6379> mget key1 key2 # 批量获取多个 key 对应的 value
1) "value1"
2) "value2"
```

用来表示数字：
```
127.0.0.1:6379> set number 1
OK
127.0.0.1:6379> incr number # 将 key 中储存的数字值增一
(integer) 2
127.0.0.1:6379> get number
"2"
127.0.0.1:6379> decr number # 将 key 中储存的数字值减一
(integer) 1
127.0.0.1:6379> get number
"1"
```

设置过期：
```
127.0.0.1:6379> set key value
OK
127.0.0.1:6379> expire key 60 # 数据在 60s 后过期
(integer) 1
127.0.0.1:6379> setex key 60 value # 原子操作，相当于set + expire
OK
127.0.0.1:6379> ttl key # 查看数据还有多久过期
(integer) 56
```


### Hash
Hash数据结构用来保存键值对，类似于Java中的HashMap，内部实现也是数组和链表。Hash是一个 string 类型的 field 和 value 的映射表，特别适合用于存储对象，后续操作的时候，你可以直接仅仅修改这个对象中的某个字段的值。 比如我们可以 hash 数据结构来存储用户信息，商品信息等等。

常用命令包括：
- hset：
- hmset：
- hsetnx：
- hexists：
- hget：
- hmget：
- hgetall：
- hkeys：
- hvals：
- hdel：
- hlen：

应用场景: 系统中对象数据的存储。

基本操作：
```
127.0.0.1:6379> hset userInfoKey name "guide"
OK
127.0.0.1:6379> hset userInfoKey description "dev"
OK
127.0.0.1:6379> hset userInfoKey age "24"
OK
127.0.0.1:6379> hexists userInfoKey name # 查看 key 对应的 value中指定的字段是否存在
(integer) 1
127.0.0.1:6379> hget userInfoKey name # 获取存储在哈希表中指定字段的值。
"guide"
127.0.0.1:6379> hget userInfoKey age
"24"
127.0.0.1:6379> hgetall userInfoKey # 获取在哈希表中指定 key 的所有字段和值
1) "name"
2) "guide"
3) "description"
4) "dev"
5) "age"
6) "24"
127.0.0.1:6379> hkeys userInfoKey # 获取 key 列表
1) "name"
2) "description"
3) "age"
127.0.0.1:6379> hvals userInfoKey # 获取 value 列表
1) "guide"
2) "dev"
3) "24"
127.0.0.1:6379> hset userInfoKey name "Guide" # 修改某个字段对应的值
127.0.0.1:6379> hget userInfoKey name
"Guide"
```


### List
List类型底层是双向链表。链表是一种非常常见的数据结构，特点是易于数据元素的插入和删除并且且可以灵活调整链表长度，但是链表的随机访问困难。许多高级编程语言都内置了链表的实现比如 Java 中的 LinkedList，但是 C 语言并没有实现链表，所以 Redis 实现了自己的链表数据结构。

常用命令：
- rpush
- rpop
- lpop
- lpush
- lrange
- llen：

应用场景：发布与订阅或者说消息队列。

实现队列：
```
127.0.0.1:6379> rpush myList value1 # 向 list 的头部（右边）添加元素
(integer) 1
127.0.0.1:6379> rpush myList value2 value3 # 向list的头部（最右边）添加多个元素
(integer) 3
127.0.0.1:6379> lpop myList # 将 list的尾部(最左边)元素取出
"value1"
127.0.0.1:6379> lrange myList 0 1 # 查看对应下标的list列表， 0 为 start,1为 end
1) "value2"
2) "value3"
127.0.0.1:6379> lrange myList 0 -1 # 查看列表中的所有元素，-1表示倒数第一
1) "value2"
2) "value3"
```

实现栈：
```
127.0.0.1:6379> rpush myList2 value1 value2 value3
(integer) 3
127.0.0.1:6379> rpop myList2 # 将 list的头部(最右边)元素取出
"value3"
```

通过 lrange 查看对应下标范围的列表元素：
```
127.0.0.1:6379> rpush myList value1 value2 value3
(integer) 3
127.0.0.1:6379> lrange myList 0 1 # 查看对应下标的list列表， 0 为 start,1为 end
1) "value1"
2) "value2"
127.0.0.1:6379> lrange myList 0 -1 # 查看列表中的所有元素，-1表示倒数第一
1) "value1"
2) "value2"
3) "value3"
```


### Set
Set类似于 Java 中的 HashSet 。Redis 中的 set 类型是一种无序集合，集合中的元素没有先后顺序。当你需要存储一个列表数据，又不希望出现重复数据时，set 是一个很好的选择，并且 set 提供了判断某个成员是否在一个 set 集合内的重要接口，这个也是 list 所不能提供的。可以基于 set 轻易实现交集、并集、差集的操作。

常用命令：
- sadd
- spop
- smembers
- sismember
- scard
- sinterstore
- sunion

应用场景：需要存放的数据不能重复以及需要计算多个数据源的交集和并集等场景。


### Zset（sorted set）
和 set 相比，Zset 增加了一个权重参数 score，使得集合中的元素能够按 score 进行有序排列，还可以通过 score 的范围来获取元素的列表。有点像是 Java 中 HashMap 和 TreeSet 的结合体。

常用命令：
- zadd
- zcard
- zscore
- zrange
- zrevrange
- zrem

应用场景：排行榜等需要排序的地方。

基本操作：
```
127.0.0.1:6379> zadd myZset 3.0 value1 # 添加元素到 sorted set 中 3.0 为权重
(integer) 1
127.0.0.1:6379> zadd myZset 2.0 value2 1.0 value3 # 一次添加多个元素
(integer) 2
127.0.0.1:6379> zcard myZset # 查看 sorted set 中的元素数量
(integer) 3
127.0.0.1:6379> zscore myZset value1 # 查看某个 value 的权重
"3"
127.0.0.1:6379> zrange  myZset 0 -1 # 顺序输出某个范围区间的元素，0 -1 表示输出所有元素
1) "value3"
2) "value2"
3) "value1"
127.0.0.1:6379> zrange  myZset 0 1 # 顺序输出某个范围区间的元素，0 为 start  1 为 stop
1) "value3"
2) "value2"
127.0.0.1:6379> zrevrange  myZset 0 1 # 逆序输出某个范围区间的元素，0 为 start  1 为 stop
1) "value1"
2) "value2"
```


#### 跳表
Zset底层数据结构是跳表（skip list）。跳表是一种比较优秀的动态数据结构，可以支持快速的插入、删除、查找操作，比较容易理解。插入、删除、查找的时间复杂度都是O(lgn)。

跳表由多层链表组成，最底层是第一层，每层包含一个链表，每个链表都是有顺序的。链表中的结点包含两个指针，一个指向同一层的下一个结点，另一个指向下一层的结点。需要注意的是，一个结点和指向下一层的结点的值是相同的。第一层的链表包含了所有数据，往上每一层的链表都只包含了下一层的部分数据，一般是二分之一。上层的数据可以当作下一层的索引。

插入数据比较重要。因为跳表有多层，不可能在每一层都插入数据，需要计算插入数据的层数。具体做法是通过随机数计算一个层数K，在第1-K层插入数据。

查找比较简单，从最高层逐层比较，直到第一层。

删除数据需要借助查找，找到数据后，删除该数据及其跨越的层数即可。


## 高级数据结构
> [Redis的3个高级数据结构](https://blog.csdn.net/wufaliang003/article/details/82016385)

### Bitmaps
bitmaps不是一个真实的数据结构，本质上还是一个String，只是主要面向bit操作的集合。由于strings是二进制安全的blob，并且它们的最大长度是512m，所以bitmaps能最大设置2^32个不同的bit。

bit操作被分为两组：
- 恒定时间的单个bit操作，例如把某个bit设置为0或者1，或者获取某bit的值。
- 对一组bit的操作。例如给定范围内bit统计（例如人口统计）。

常用命令：
bitmaps不是一个真实的数据结构。而是String类型上的一组面向bit操作的集合。由于strings是二进制安全的blob，并且它们的最大长度是512m，所以bitmaps能最大设置2^32个不同的bit。

bit操作被分为两组：
- SETBIT key offset value
- GETBIT key offset
- bitcount
- bitop op dest key1, key2, key3...

基本操作：
```
127.0.0.1:6380> setbit dupcheck 10 1
(integer) 0
127.0.0.1:6380> getbit dupcheck 10 
(integer) 1
```


### HyperLogLog
HyperLogLog是用于独立信息统计，通过计算唯一事物的概率数据结构（从技术上讲，这被称为估计集合的基数）。如果统计唯一项，项目越多，需要的内存就越多。因为需要记住过去已经看过的项，从而避免多次统计这些项。

在Redis中，可以使用标准错误小于1％的估计。这个算法的神奇在于不再需要与需要统计的项相对应的内存，取而代之，使用的内存一直恒定不变。最坏的情况下只需要12KB，就可以计算接近2^64个不同元素的基数。或者如果您的HyperLogLog（我们从现在开始简称它为HLL）已经看到的元素非常少，则需要的内存要要少得多。

在redis中HLL是一个不同的数据结构，它被编码成Redis字符串。因此可以通过调用GET命令序列化一个HLL，也可以通过调用SET命令将其反序列化到redis服务器。

HLL的API类似使用SETS数据结构做相同的任务，SETS结构中，通过SADD命令把每一个观察的元素添加到一个SET集合，用SCARD命令检查SET集合中元素的数量，集合里的元素都是唯一的，已经存在的元素不会被重复添加。

而使用HLL时并不是真正添加项到HLL中（这一点和SETS结构差异很大），因为HLL的数据结构只包含一个不包含实际元素的状态，API是一样的：
- PFADD：用于添加一个新元素到统计中。
- PFCOUNT：用于获取到目前为止通过PFADD命令添加的唯一元素个数近似值。
- PFMERGE：执行多个HLL之间的联合操作。

```
127.0.0.1:6380> PFADD hll a b c d d c
(integer) 1
127.0.0.1:6380> PFCOUNT hll
(integer) 4
127.0.0.1:6380> PFADD hll e
(integer) 1
127.0.0.1:6380> PFCOUNT hll
(integer) 5
```


### GeoHash
Redis的GEO特性在 Redis3.2 版本中推出，这个功能可以将用户给定的地理位置（经度和纬度）信息储存起来，并对这些信息进行操作。GEO相关命令只有6个：
- GEOADD：GEOADD key longitude latitude member [longitude latitude member …]，将指定的地理空间位置（纬度、经度、名称）添加到指定的key中。例如：GEOADD city 113.501389 22.405556 shenzhen；
- GEOHASH：GEOHASH key member [member …]，返回一个或多个位置元素的标准Geohash值，它可以在http://geohash.org/使用。
- GEOPOS：GEOPOS key member [member …]，从key里返回所有给定位置元素的位置（经度和纬度）。
- GEODIST：GEODIST key member1 member2 [unit]，返回两个给定位置之间的距离。GEODIST命令在计算距离时会假设地球为完美的球形。在极限情况下，这一假设最大会造成0.5%的误差。
- GEORADIUS：GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]，以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。这个命令可以查询某城市的周边城市群。
- GEORADIUSBYMEMBER：GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]，这个命令和GEORADIUS命令一样，都可以找出位于指定范围内的元素，但是GEORADIUSBYMEMBER的中心点是由给定的位置元素决定的，而不是像 GEORADIUS那样，使用输入的经度和纬度来决定中心点。


## 事务
Redis的事务是通过MULTI，EXEC，DISCARD和WATCH这四个命令来完成。Redis的单个命令都是原子性的，所以这里确保事务性的对象是命令集合。Redis将命令集合序列化并确保处于一事务的命令集合连续且不被打断的执行。Redis不支持回滚的操作。

相关命令：
- MULTI：MULTI，用于标记事务块的开始。Redis会将后续的命令逐个放入队列中，然后使用EXEC命令原子化地执行这个命令序列。
- EXEC：EXEC，在一个事务中执行所有先前放入队列的命令，然后恢复正常的连接状态。
- DISCARD：DISCARD，清除所有先前在一个事务中放入队列的命令，然后恢复正常的连接状态。
- WATCH：WATCH key [key....]，当某个事务需要按条件执行时，就要使用这个命令将给定的键设置为受监控的状态。注：该命令可以实现redis的乐观锁。
- UNWATCH：UNWATCH，清除所有先前为一个事务监控的键。

为什么redis不支持事务回滚：
1. 大多数事务失败是因为语法错误或者类型错误，这两种错误，在开发阶段都是可以避免的
2. Redis为了性能方面就忽略了事务回滚


## Redis单线程模型
Redis 基于 Reactor 模式开发了自己的网络事件处理器：这个处理器被称为文件事件处理器（file event handler）。文件事件处理器使用 I/O 多路复用（multiplexing）程序来同时监听多个套接字，并根据套接字目前执行的任务来为套接字关联不同的事件处理器。

由于文件事件处理器是单线程方式运行的，所以我们一般都说 Redis 是单线程模型。

既然是单线程，那怎么监听大量的客户端连接呢？

Redis通过**I/O多路复用程序**来监听来自客户端的大量连接（或者说是监听多个 socket），当被监听的套接字准备好执行连接应答（accept）、读取（read）、写入（write）、关 闭（close）等操作时，与操作相对应的文件事件就会产生，这时文件事件处理器就会调用套接字之前关联好的事件处理器来处理这些事件。

这样的好处非常明显：I/O多路复用技术让Redis不需要额外创建多余的线程来监听客户端的大量连接，降低了资源的消耗。

虽然文件事件处理器以单线程方式运行，但通过使用 I/O 多路复用程序来监听多个套接字，文件事件处理器既实现了高性能的网络通信模型，又可以很好地与 Redis 服务器中其他同样以单线程方式运行的模块进行对接，这保持了 Redis 内部单线程设计的简单性。

可以看出，文件事件处理器（file event handler）主要是包含 4 个部分：
- 多个 socket（客户端连接）
- IO 多路复用程序（支持多个客户端连接的关键）
- 文件事件分派器（将 socket 关联到相应的事件处理器）
- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

![文件事件处理器](https://gitee.com/SnailClimb/JavaGuide/raw/master/docs/database/Redis/images/redis-all/redis%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E5%99%A8.png)


## Redis6.0之后为何引入了多线程？
Redis6.0 之前为什么不使用多线程的原因有:
- 单线程编程容易并且更容易维护；
- Redis 的性能瓶颈不在 CPU，主要在内存和网络；
- 多线程就会存在死锁、线程上下文切换等问题，甚至会影响性能。

Redis6.0 引入多线程主要是为了提高网络 IO 读写性能，因为这个算是 Redis 中的一个性能瓶颈（Redis 的瓶颈主要受限于内存和网络）。

虽然，Redis6.0 引入了多线程，但是 Redis 的多线程只是使用在网络数据的读写这类耗时操作上，执行命令仍然是单线程顺序执行。因此，不需要担心线程安全问题。

Redis6.0 的多线程默认是禁用的，只使用主线程。如需开启需要修改 redis 配置文件 redis.conf：
```
io-threads-do-reads yes
```

开启多线程后，还需要设置线程数，否则是不生效的。同样需要修改 redis 配置文件 redis.conf：
```
io-threads 4 # 官网建议4核的机器建议设置为2或3个线程，8核的建议设置为6个线程
```


## Redis 给缓存数据设置过期时间有啥用？
一般情况下，我们设置保存的缓存数据的时候都会设置一个过期时间。

因为内存是有限的，如果缓存中的所有数据都是一直保存的话，分分钟直接Out of memory。

Redis 自带了给缓存数据设置过期时间的功能，比如：
```
127.0.0.1:6379> exp key  60 # 数据在 60s 后过期
(integer) 1
127.0.0.1:6379> setex key 60 value # 数据在 60s 后过期 (setex:[set] + [ex]pire)
OK
127.0.0.1:6379> ttl key # 查看数据还有多久过期
(integer) 56
```

很多时候，我们的业务场景就是需要某个数据只在某一时间段内存在，比如我们的短信验证码可能只在1分钟内有效，用户登录的 token 可能只在 1 天内有效。如果使用传统的数据库来处理的话，一般都是自己判断过期，这样更麻烦并且性能要差很多。


## Redis 如何删除过期数据？
Redis数据库中有两个字典：一个是数据字典，保存了数据库中的所有数据（键值对）；另一个是过期字典，保存了数据库中所有设置过期的数据的地址及数据的过期时间。

常用的过期数据的删除策略有三个：
- 定时删除（立即删除）：在设置键的过期时间时，创建一个定时器，当过期时间到达时，由定时器立即执行键的删除操作。无论CPU此时负载量多高，占用CPU，影响Redis的性能。
- 惰性删除：在某个键值过期后，不会马上被删除，而是在取出key的时候才对数据进行过期检查，没有过期则返回正常数据，过期则返回不存在。这样对CPU比较友好，但是可能会造成太多过期 key 没有被删除，浪费内存。
- 定期删除：每隔一段时间抽取一批 key 执行删除过期key操作。这样对内存比较友好，但是消耗CPU。Redis底层会并通过限制删除操作的时间和频率来减少删除操作对CPU的影响。

定期删除对内存更加友好，惰性删除对CPU更加友好。两者各有千秋，所以Redis采用的是定期删除+惰性删除。Redis默认每100ms检查是否有过期的key，有过期的key则删除。需要说明的是redis不是每个100ms将所有的key检查一次，而是随机抽取进行检查。


## Redis 内存淘汰机制
通过给 key 设置过期时间还是有问题的。因为还是可能存在定期删除和惰性删除漏掉了很多过期 key 的情况。这样就导致大量过期 key 堆积在内存里，然后就Out of memory了。

要解决这个问题，需要使用Redis 内存淘汰机制。

Redis 提供 8 种数据淘汰策略：
1. volatile-lru（least recently used）：从已设置过期时间的数据中挑选最近最少使用的数据淘汰
2. volatile-ttl：从已设置过期时间的数据中挑选将要过期的数据淘汰
3. volatile-random：从已设置过期时间的数据中任意选择数据淘汰
4. allkeys-lru（least recently used）：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 key（这个是最常用的）
5. allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
6. no-eviction：禁止驱逐数据，也就是说当内存不足以容纳新写入数据时，新写入操作会报错。这个应该没人使用吧！
7. volatile-lfu（least frequently used）：从已设置过期时间的数据挑选最不经常使用的数据淘汰
8. allkeys-lfu（least frequently used）：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的 key

在关于lru的两个策略中，使用LRU数据结构和算法即可。

在关于lfu的两个策略中，需要设置一个最近的时间值，给定时间内才能计算最不经常使用的数据。


## 持久化
很多时候我们需要持久化数据，也就是将内存中的数据写入到硬盘里面，大部分原因是为了防止数据的意外丢失，确保数据安全性，比如机器故障之后可以借助持久化恢复数据。

Redis 不同于 Memcached 的很重要一点就是，Redis 支持持久化，而且支持两种不同的持久化操作：一种持久化方式叫快照（snapshotting，RDB），另一种方式是只追加文件（append-only file, AOF）。

RDB和AOF的比较：
- RDB占用存储空间小，因为是二进制文件；AOF占用空间大，但是可以重写，
- RDB存储速度较满，AOF速度快。
- RDB恢复速度快，AOF恢复速度慢；
- RDB安全性低，会丢失数据，AOF丢失数据比较少；
- RDB资源消耗比较大，AOF比较轻量。
- RDB启动优先级低，AOF优先级高，先启动。

### RDB
Redis 可以通过创建快照来获得存储在内存里面的数据在某个时间点上的副本。Redis 创建快照之后，可以对快照进行备份，可以将快照复制到其他服务器从而创建具有相同数据的服务器副本（Redis 主从结构，主要用来提高 Redis 性能），还可以将快照留在原地以便重启服务器的时候使用。

快照持久化是 Redis 默认采用的持久化方式，命令是`save`，每执行一次就进行一次持久化：
```
127.0.0.1:6379> save
```

注意：`save`指令的执行会阻塞Redis服务器，直到当前RDB持久化过程完成为止，如果数据量过大有可能造成长时间阻塞（因为Redis是单线程模型），后面的命令均需要等待。所以，线上环境不建议使用。

`save`命令会造成阻塞。另一种RDB持久化的命令是`bgsave`，表示启动后台RDB持久化，并不是立即执行。
```
127.0.0.1:6379> bgsave
```

`bgsave`的原理是执行`bgsave`命令后立即返回，Redis生成一个子进程来执行RDB持久化，创建RDB文件，持久化后返回消息，消息会存放在日志文件中。

如下图所示：

![bgsave](https://note.youdao.com/yws/api/personal/file/CD0C8F1CCCAD4A0B8151946B928BC5F0?method=download&shareKey=2e07fe0b899b6dd17b2c87bccd5c85ed)

bgsave命令是针对save阻塞问题做的优化。Redis内存所有涉及到RDB操作都建议采用bgsave的方式，save命令可以放弃使用。

save和bgsave的区别在于：
- 读写：save是同步方式，bgsave是异步
- 阻塞客户端指令：save阻塞，bgsave非阻塞
- 额外内存消耗：save不消耗，bgsave消耗
- 启动新进程：save不启动，bgsave启动

save命令和bgsave命令都需要手动执行，比较麻烦。更好的方式是在配置文件配置RDB持久化的策略：
```
save 900 1      # 在900秒(15分钟)之后，如果至少有1个key发生变化，Redis就会自动触发BGSAVE命令创建快照。
save 300 10     # 在300秒(5分钟)之后，如果至少有10个key发生变化，Redis就会自动触发BGSAVE命令创建快照。
save 60 10000   # 在60秒(1分钟)之后，如果至少有10000个key发生变化，Redis就会自动触发BGSAVE命令创建快照。
```

save配置底层使用的是bgsave。

如何基于持久化的RDB文件热启动Redis：
```
redis-server conf/redis.conf
```

RDB的优点：
- 保存二进制文件，存储效率高
- 保存的是某个时间点的数据，非常适合备份、全量复制等场景
- RDB恢复数据比较快，快于AOF
- 应用：服务器中每X小时执行bgsave备份，并将RDB文件拷贝到远程机器中，用于灾难恢复。

RDB的缺点：
- 基于快照思想，如果数据量比较大，IO开销比较大。
- 无论是指令还是配置，无法做到实时持久化，存在丢失数据的风险。
- bgsave每次执行都要执行fork子进程，牺牲了一定的性能。
- Redis众多版本中没有进行RDB文件格式的统一，有可能出现无法兼容的情况。


### AOF
AOF对操作命令进行记录。与快照持久化相比，AOF 持久化的实时性更好，因此已成为主流的持久化方案。默认情况下，Redis 没有开启 AOF 方式的持久化，可以在配置文件中设置：
```
appendonly yes
```

开启 AOF 持久化后，每执行一条会更改 Redis 中的数据的命令，Redis 就会将该命令写入硬盘中的 AOF 文件。AOF 文件的保存位置和 RDB 文件的位置相同，文件名需要在配置文件中设置，默认的文件名是 appendonly.aof。

在 Redis 的配置文件中，可以设置三种不同的 AOF 持久化方式，它们分别是：
```
appendfsync always    # 只要有数据修改，就写入AOF文件，数据没有误差，但是会降低Redis的性能，不建议使用
appendfsync everysec  # 每秒钟同步一次，准确性较高，性能较高，是默认配置
appendfsync no        # 让操作系统决定何时进行同步，整体过程不可控
```

为了兼顾数据和写入性能，通常使用appendfsync everysec 选项 ，让 Redis 每秒同步一次 AOF 文件。即使出现 Redis 崩溃，最多只会丢失一秒之内的数据。当硬盘忙于执行写入操作的时候，Redis 还会优雅的放慢自己的速度以便适应硬盘的最大写入速度。

#### AOF 重写
命令执行越来越多，AOF文件越来越大，但是有些命令覆盖了前面的命令，可以删除或整合一些命令。

AOF重写可以产生一个新的 AOF 文件，这个新的 AOF 文件和原有的 AOF 文件所保存的数据库状态一样，但体积更小。

AOF的优点的作用和优点包括：
- 降低磁盘占用量，提高磁盘利用率；
- 提高持久化效率，降低持久化写时间；
- 降低数据恢复用时，提高数据恢复效率。

AOF 重写是一个有歧义的名字，该功能是通过读取数据库中的键值对来实现的，程序无须对现有 AOF 文件进行任何读入、分析或者写入操作。

手动执行AOF重写的命令是：
```
127.0.0.1:6379> bgrewriteaof
```

bgrewriteaof 命令的过程如下图所示：

![bgrewriteaof](https://note.youdao.com/yws/api/personal/file/12662644942741EFABCC282ADDE5A4BA?method=download&shareKey=2e07fe0b899b6dd17b2c87bccd5c85ed)

bgrewriteaof的原理是：在执行 bgrewriteaof 命令时，Redis 服务器会维护一个 AOF 重写缓冲区，该缓冲区会在子进程创建新 AOF 文件期间，记录服务器执行的所有写命令。当子进程完成创建新 AOF 文件的工作之后，服务器会将重写缓冲区中的所有内容追加到新 AOF 文件的末尾，使得新旧两个 AOF 文件所保存的数据库状态一致。最后，服务器用新的 AOF 文件替换旧的 AOF 文件，以此来完成 AOF 文件重写操作。

在配置文件中配置AOF自动重写：
```
auto-aof-rewrite-min-size size
auto-aof-rewrite-percentage percentage
```

## 集群
> [Redis集群详解](https://blog.csdn.net/miss1181248983/article/details/90056960)

Redis有三种集群模式：主从模式、哨兵模式、集群（Cluster）模式。

### 主从模式
主从模式是三种模式中最简单的，在主从复制中，数据库分为两类：主数据库（master）和从数据库（slave）。主从模式有如下特点：
- 主数据库可以进行读写操作，当读写操作导致数据变化时会自动将数据同步给从数据库
- 从数据库一般都是只读的，并且接收主数据库同步过来的数据
- 一个master可以拥有多个slave，但是一个slave只能对应一个master
- slave挂了不影响其他slave的读和master的读和写，重新启动后会将数据从master同步过来
- master挂了以后，不影响slave的读，但redis不再提供写服务，master重启后redis将重新对外提供写服务
- master挂了以后，不会在slave节点中重新选一个master

主从复制的作用：读写分离、负载均衡、故障恢复、数据冗余、高可用的基础。

主从模式的工作机制：当slave启动后，主动向master发送SYNC命令。master接收到SYNC命令后在后台保存快照（RDB持久化）和缓存保存快照这段时间的命令，然后将保存的快照文件和缓存的命令发送给slave。slave接收到快照文件和命令后加载快照文件和缓存的执行命令。复制初始化后，master每次接收到的写命令都会同步发送给slave，保证主从数据一致性。

缺点：从上面可以看出，master节点在主从模式中唯一，若master挂掉，则redis无法对外提供写服务。


### 哨兵（Sentinel）模式
主从模式的弊端就是不具备高可用性，当master挂掉以后，Redis将不能再对外提供写入操作，因此哨兵模式应运而生。

sentinel中文含义为哨兵，顾名思义，它的作用就是监控redis集群的运行状况，特点如下：
- sentinel模式是建立在主从模式的基础上
- 如果master挂了，sentinel会在slave中选择一个做为master，并修改它们的配置文件，其他slave的配置文件也会被修改，比如slaveof属性会指向新的master
- 当master重新启动后，它将不再是master而是做为slave接收新的master的同步数据

因为sentinel也是一个进程有挂掉的可能，所以sentinel也会启动多个形成一个sentinel集群
- 当主从模式配置密码时，sentinel也会同步将配置信息修改到配置文件中
- 一个sentinel或sentinel集群可以管理多个主从Redis，多个sentinel也可以监控同一个redis
- sentinel最好不要和Redis部署在同一台机器，不然Redis的服务器挂了以后，sentinel也挂了

工作机制：
- 每个sentinel以每秒钟一次的频率向它所知的master，slave以及其他sentinel实例发送一个 PING 命令 
- 如果一个实例距离最后一次有效回复 PING 命令的时间超过 down-after-milliseconds 选项所指定的值， 则这个实例会被sentinel标记为主观下线。 
- 如果一个master被标记为主观下线，则正在监视这个master的所有sentinel要以每秒一次的频率确认master的确进入了主观下线状态
- 当有足够数量的sentinel（大于等于配置文件指定的值）在指定的时间范围内确认master的确进入了主观下线状态， 则master会被标记为客观下线 
- 在一般情况下， 每个sentinel会以每 10 秒一次的频率向它已知的所有master，slave发送 INFO 命令 
- 当master被sentinel标记为客观下线时，sentinel向下线的master的所有slave发送 INFO 命令的频率会从 10 秒一次改为 1 秒一次 
- 若没有足够数量的sentinel同意master已经下线，master的客观下线状态就会被移除；若master重新向sentinel的 PING 命令返回有效回复，master的主观下线状态就会被移除


### Cluster模式
sentinel模式基本可以满足一般生产的需求，具备高可用性。但是当数据量过大到一台服务器存放不下的情况时，主从模式或sentinel模式就不能满足需求了，这个时候需要对存储的数据进行分片，将数据存储到多个Redis实例中。

Cluster模式的出现就是为了解决单机Redis容量有限的问题，将Redis的数据根据一定的规则分配到多台机器。

Cluster可以说是sentinel和主从模式的结合体，通过Cluster可以实现主从和master重选功能，所以如果配置两个副本三个分片的话，就需要六个Redis实例。因为Redis的数据是根据一定规则分配到Cluster的不同机器的，当数据量过大时，可以新增机器进行扩容。

使用集群，只需要将Redis配置文件中的cluster-enable配置打开即可。每个集群中至少需要三个主数据库才能正常运行，新增节点非常方便。

特点：
- 多个Redis节点网络互联，数据共享
- 所有的节点都是一主一从（也可以是一主多从），其中从不提供服务，仅作为备用
- 不支持同时处理多个key（如MSET/MGET），因为redis需要把key均匀分布在各个节点上，并发量很高的情况下同时创建key-value会降低性能并导致不可预测的行为
- 支持在线增加、删除节点
- 客户端可以连接任何一个主节点进行读写。



## 常见问题
### 缓存穿透
缓存穿透是指请求缓存和数据库中都没有的数据，缓存中没有，请求直接到了数据库上。比如黑客故意制造无效的 key 发起大量请求，导致大量请求落到数据库。

最基本的办法就是做参数校验，对一些不合法的参数请求直接抛出异常信息返回给客户端。比如查询的数据库 id 不能小于0、传入的邮箱格式不对的时候直接返回错误消息给客户端等等。还有：
1. 缓存无效 key。如果缓存和数据库都查不到某个 key 的数据就写一个到 Redis 中去并设置过期时间，具体命令如下： SET key value EX 10086 。这种方式可以解决请求的 key 变化不频繁的情况，并不能从根本上解决此问题。
2. 布隆过滤器：布隆过滤器是一个非常神奇的数据结构，通过它我们可以非常方便地判断一个给定数据是否存在于海量数据中。布隆过滤器说某个元素存在，小概率会误判；布隆过滤器说某个元素不在，那么这个元素一定不在。


### 缓存击穿
缓存击穿是指请求缓存中没有但数据库中有的数据（一般是缓存时间到期），这时由于并发用户特别多，缓存没读到数据，就去数据库去取数据，引起数据库压力瞬间增大。

解决方案：
- 设置热点数据永远不过期。
- 加互斥锁。


### 缓存雪崩
缓存雪崩描述的就是这样一个简单的场景：缓存在同一时间大面积的失效，后面的请求都直接落到了数据库上，造成数据库短时间内承受大量请求。这就好比雪崩一样，摧枯拉朽之势，数据库的压力可想而知，可能直接就被这么多请求弄宕机了。

解决办法包括：
1. 采用 Redis 集群，避免单机出现问题整个缓存服务都没办法使用。
2. 限流，避免同时处理大量的请求
3. 设置不同的失效时间比如随机设置缓存的失效时间。
4. 缓存永不失效。


### Redis中的Rehash机制
> [redis rehash机制](https://www.jianshu.com/p/6114d0eabd67)

在Redis中，键值以哈希表的方式进行存储，使用链地址法解决哈希冲突。在键值对的数目比较多时，哈希冲突的次数就会变多，有的链表就会越来越长，会降低检索效率。为了减少哈希表中的地址冲突次数，Redis会增加数组空间，并把数据移到新的空间中，这个过程称为rehash，类似于Java中HashMap的扩容。

Redis中维护了一个大小为2的数组ht，存放了两个存储数据的哈希表，ht[0]是旧表，ht[1]是新表。当开始rehash时，为新表申请一个更大的空间，把旧表中的元素往新表中迁移。旧表中的数据迁移完之后，释放旧表的空间，交换两张表。

Redis什么时候进行rehash？
- 服务器目前没有在执行BGSAVE命令或者BGREWRITEAOF命令，并且哈希表的数据数量与哈希表大小的比例大于或等于1。
- 服务端目前正在执行BGSAVE命令或者BGREWRITEAOF命令，并且哈希表的数据数量与哈希表大小的比例大于或等于5。
- 当哈希表的哈希表的数据数量与哈希表大小的比例小于0.1时，Redis会自动开始对哈希表进行缩容操作。

rehash是渐进式的过程，并不是一次性完成，而是分步完成的：每步完成一个bucket（桶）的迁移，直至所有数据迁移完毕。一个bucket对应哈希表数组中的一条链表。Redis中维护了一个索引计数器`rehshidx`，可以用来表示是否正在进行rehash（-1表示没有进行rehash），还可以用来记录迁移的位置。具体过程为：
- 为ht[1]分配空间，让字典同时持有ht[0]和ht[1]两个哈希表。
- 在字典中维持一个索引计数器变量rehashidx，并将它的值设置为0，表示rehash工作正式开始。
- 在rehash执行期间，每次对字典执行添加、删除、查找或者更新操作时，程序除了执行指定的操作外，还会顺带将ht[0]哈希表在rehashidx索引上的所有键值对rehash到ht[1]，当rehash工作完成后，程序将rehashidx属性的值增1。
- 随着字典操作的不断执行，最终在某个时间点上，ht[0]的所有键值对都会被rehash至ht[1]，这时程序将rehashidx属性的值设为-1，表示rehash操作已完成。

rehash为什么要渐进式迁移？如果dict数据结构中存储了海量的数据，那么一次性迁移势必带来Redis性能的下降，而且Redis是单线程模型，在实时性要求高的场景下这可能是致命的。而渐进式哈希则将这种代价可控地分摊了，调用方可以在插入、删除、更新数据的时候执行dictRehash()，最小化数据迁移的代价。在迁移的过程中，数据是在新表还是旧表中并不重要，数据并不会丢失，在旧表中找不到再到新表中寻找就是了。


### 如何用Redis实现分布式锁
> [Redis分布式锁的正确实现方式](cnblogs.com/moxiaotao/p/10829799.html)

分布式锁一般有三种实现方式：1、数据库乐观锁；2、基于Redis的分布式锁；3、基于ZooKeeper的分布式锁。

为了确保分布式锁可用，我们至少要确保锁的实现同时满足以下四个条件：
1. 互斥性。在任意时刻，只有一个客户端能持有锁。
2. 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。
3. 具有容错性。只要大部分的Redis节点正常运行，客户端就可以加锁和解锁。
4. 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了。

#### 加锁
加锁实际上就是在redis中，给Key键设置一个值，为避免死锁，并给定一个过期时间。

redis命令为：
```
SET lock_key random_value NX PX 5000
```

说明：
- random_value 是客户端生成的唯一的字符串。
- NX 代表只在键不存在时，才对键进行设置操作。
- PX 5000 设置键的过期时间为5000毫秒。

如果上面的命令执行成功，则证明客户端获取到了锁。

Java代码实现如下所示：
```java
public class RedisTool {
    private static final String LOCK_SUCCESS = "OK";
    private static final String SET_IF_NOT_EXIST = "NX";
    private static final String SET_WITH_EXPIRE_TIME = "PX";

    /**
     * 尝试获取分布式锁
     * @param jedis Redis客户端
     * @param lockKey 锁
     * @param requestId 请求标识
     * @param expireTime 超期时间
     * @return 是否获取成功
     */
    public static boolean tryGetDistributedLock(Jedis jedis, String lockKey, String requestId, int expireTime) {
        String result = jedis.set(lockKey, requestId, SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, expireTime);
        if (LOCK_SUCCESS.equals(result)) {
            return true;
        }
        return false;
    }
}
```

可以看到，我们加锁就一行代码：jedis.set(String key, String value, String nxxx, String expx, int time)，这个set()方法一共有五个形参：
- 第一个为key，我们使用key来当锁，因为key是唯一的。
- 第二个为value，我们传的是requestId，很多童鞋可能不明白，有key作为锁不就够了吗，为什么还要用到value？原因就是我们在上面讲到可靠性时，分布式锁要满足第四个条件解铃还须系铃人，通过给value赋值为requestId，我们就知道这把锁是哪个请求加的了，在解锁的时候就可以有依据。requestId可以使用UUID.randomUUID().toString()方法生成。
- 第三个为nxxx，这个参数我们填的是NX，意思是SET IF NOT EXIST，即当key不存在时，我们进行set操作；若key已经存在，则不做任何操作；
- 第四个为expx，这个参数我们传的是PX，意思是我们要给这个key加一个过期的设置，具体时间由第五个参数决定。
- 第五个为time，与第四个参数相呼应，代表key的过期时间。

总的来说，执行上面的set()方法就只会导致两种结果：1. 当前没有锁（key不存在），那么就进行加锁操作，并对锁设置个有效期，同时value表示加锁的客户端。2. 已有锁存在，不做任何操作。


#### 解锁
解锁的过程就是将Key键删除。但也不能乱删，不能说客户端1的请求将客户端2的锁给删除掉。

为了保证解锁操作的原子性，一般用Lua脚本完成这一操作。先判断当前锁的字符串是否与传入的值相等，是的话就删除Key，解锁成功。

Lua脚本如下所示：
```lua
if redis.call('get',KEYS[1]) == ARGV[1] then 
   return redis.call('del',KEYS[1]) 
else
   return 0 
end
```

Java代码如下所示：
```java
public class RedisTool {
    private static final Long RELEASE_SUCCESS = 1L;

    /**
     * 释放分布式锁
     * @param jedis Redis客户端
     * @param lockKey 锁
     * @param requestId 请求标识
     * @return 是否释放成功
     */
    public static boolean releaseDistributedLock(Jedis jedis, String lockKey, String requestId) {
        String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
        Object result = jedis.eval(script, Collections.singletonList(lockKey), Collections.singletonList(requestId));
        if (RELEASE_SUCCESS.equals(result)) {
            return true;
        }
        return false;
    }
}
```

至此，单节点Redis的分布式锁的实现就已经完成了。比较简单，但是问题也比较大，最重要的一点是，锁不具有可重入性。

最后，如果你的项目中Redis是多机部署的，那么可以尝试使用Redisson实现分布式锁，这是Redis官方提供的Java组件。相对于Jedis而言，Redisson非常强大。当然，随之而来的就是它的复杂性。它里面也实现了分布式锁，而且包含多种类型的锁。
















