# NoSql 概述
## 为什么要使用 NoSql？
NoSql 是一种解决方法，解决服务器性能问题。
NoSql 意为不仅仅是数据库。
## 特点
1. 不遵循 SQL 标准。
2. 不支持 ACID。
3. 远胜于 SQL 的性能。

## 使用场景
高并发
海量数据

# Redis 的安装
使用 bt 宝塔安装即可。手动安装需要 gcc 支持，查看教程即可。使用宝塔安装 `redis` ，在 `src` 目录下使用命令。

Redis 推荐 `后台启动`。后台启动需要去切换修改配置文件，设置为允许后台运行。

## Redis 相关知识
默认端口号：`6379`
Redis 默认是 16 个数据库，类似数组小标，从 0 开始，默认使用 0 数据库。
使用 `select dbid` 来切换数据库，例如 `select 8`。
统一密码管理，所有库的密码相同。
Dbsize 查看当前数据库的 key 的数量。
`Flushdb ` 清空当前库
`Flushall` 通杀所有库。
`redis` 底层使用的是单线程+多路 IO 复用。

# 数据类型操作
## Key 操作
Redis 是一种基于内存的键值对存储数据库，常用于缓存、消息队列、会话管理等场景。下面是 Redis 常用的 key 操作：
1. SET key value：设置一个 key-value 对。
2. GET key：获取指定 key 的 value。
3. DEL key：删除指定 key。
4. EXISTS key：判断指定 key 是否存在。
5. EXPIRE key seconds：设置 key 的失效时间，单位为秒。
6. TTL key：获取 key 的剩余失效时间，单位为秒。 -1 永不过期， -2 已经过期，无法获取。
7. INCR key：将 key 中存储的数字值加一。
8. DECR key：将 key 中存储的数字值减一。
9. INCRBY key increment：将 key 中存储的数字值加上指定的增量。
10. DECRBY key decrement：将 key 中存储的数字值减去指定的减量。
11. APPEND key value：将指定的 value 追加到 key 的值之后。
12. MGET key 1 [key 2..]：获取多个 key 的值。
13. MSET key 1 value 1 [key 2 value 2..]：同时设置多个 key-value 对。
14. KEYS pattern：查找所有符合给定模式的 key。
15. SCAN cursor [MATCH pattern] [COUNT count]：迭代数据库中的 key。

以上是 Redis 常用的一些 key 操作，可以帮助你有效地管理 Redis 中的数据。

## Redis 字符串（String）
String 类型是 redis 中最常用的类型，表现形式为 key-value 。
String 是 `二进制安全的`。随意 redis 的 String 可以包含任何数据。比如 jpg 图片或者序列化图片。
String 类型的 value 最多是 `512MB`。

### 常用命令
在 Redis 中，String 类型是最基本和常用的数据类型之一，它可以存储字符串、整数或者浮点数。下面是 Redis 中 String 类型的一些常用操作：

1. SET key value：设置 key 的值为 value。
2. GET key：获取 key 的值。
3. GETRANGE key start end：获取 key 值的子字符串，从 start 到 end 的位置。
4. SETEX key seconds value：设置 key 的值，并指定 key 的过期时间，单位为秒。
5. INCR key：将 key 中存储的数字值加一。
6. DECR key：将 key 中存储的数字值减一。
7. INCRBY key increment：将 key 中存储的数字值加上指定的增量。
8. DECRBY key decrement：将 key 中存储的数字值减去指定的减量。
9. APPEND key value：将指定的 value 追加到 key 的值之后。
10. MSET key 1 value 1 [key 2 value 2..]：同时设置多个 key-value 对。

除了以上操作，还有一些其他的操作，比如：GETSET、MGET、MSETNX、STRLEN、BITCOUNT 等。需要根据实际业务需求选择合适的操作。值得注意的是，Redis 中的 String 类型最大长度为 512 MB，如果需要存储更大的数据，可以考虑使用其他数据类型，比如：List、Hash、Set、ZSet 等。

Redis 的操作是 `原子性 ` 的，他的操作是不会被 `线程调优打断的。`

使用 Redis 中的原子性操作可以保证数据的一致性和可靠性，特别是在并发环境下更加重要，能够避免数据竞争和死锁等问题。

Redis 的字符串底层是一个动态字符串，会自动扩容，最大为 512, MB。

## 列表 List
Redis 列表是简单的字符串列表，按照插入顺序排序，你可以添加一个元素到列表的头部后者尾部。
他的底层实际是个 `双向链表`，对两端的操作性都很高，通过索引操作中间值下标性能会差。
### 常用指令
Redis list 是 Redis 中常用的数据结构之一，它是一个双向链表结构，可以存储一个有序的字符串列表。以下是 Redis list 常用的指令：

1. LPUSH：将一个或多个值从列表的左侧插入，即头部插入。语法为 LPUSH key value [value …]。

2. RPUSH：将一个或多个值从列表的右侧插入，即尾部插入。语法为 RPUSH key value [value …]。

3. LPOP：从列表的左侧弹出一个值，并返回该值。语法为 LPOP key。

4. RPOP：从列表的右侧弹出一个值，并返回该值。语法为 RPOP key。

5. LINDEX：获取列表中指定索引的值。语法为 LINDEX key index。

6. LRANGE：获取列表中指定区间的值。语法为 LRANGE key start stop。

7. LLEN：获取列表的长度。语法为 LLEN key。

8. LINSERT：将一个值插入到列表中指定值的前面或后面。语法为 LINSERT key BEFORE|AFTER pivot value。

9. LREM：从列表中删除指定个数的指定值。语法为 LREM key count value。

10. BLPOP：在指定时间内阻塞并弹出列表左侧的值。语法为 BLPOP key [key …] timeout。

11. BRPOP：在指定时间内阻塞并弹出列表右侧的值。语法为 BRPOP key [key …] timeout。

12. RPOPLPUSH：将一个列表的最后一个值弹出，并将弹出的值插入到另一个列表的最左侧。语法为 RPOPLPUSH source destination。

以上是 Redis list 常用的指令，还有其他指令可以参考 Redis 官方文档。

## Set 集合
 在 Redis 中，Set（集合）是一种无序、不可重复的数据结构，它的内部实现使用了哈希表来支持快速的插入、删除、查找等操作。Set 可以存储多个字符串类型的元素，每个元素都是唯一的，且不按照任何顺序排列。Set 的常用命令包括：

1. SADD key member [member ...]

向集合 key 中添加一个或多个成员，返回添加成功的成员个数。

示例：
```
SADD myset "apple" "banana" "orange"
```
2. SCARD key

返回集合 key 中的元素个数。

示例：
```
SCARD myset
```
3. SMEMBERS key

返回集合 key 中的所有成员。

示例：
```
SMEMBERS myset
```
4. SISMEMBER key member

判断 member 元素是否是集合 key 的成员，返回 1 表示是，返回 0 表示否。

示例：
```
SISMEMBER myset "apple"
```
5. SREM key member [member ...]

从集合 key 中移除一个或多个成员，返回成功移除的成员个数。

示例：
```
SREM myset "apple"
```
6. SUNION key [key ...]

返回多个集合的并集，即包含所有集合中的元素，去除重复元素后的结果。

示例：
```
SUNION myset1 myset2
```
除了上述常用命令外，Set 还支持交集、差集、随机取值等操作，可以根据具体需求选择相应的命令进行操作。

## Hash 哈希
Redis 中的 hash 是 key-value 集合。
Redis hahs 是一个 String 类型的 `field` 和 `value` 的映射表，hash 特别适合存储对象，类似 java l 中的 Map< String, Object>。
>我们通过使用 `key+filed` 来获得想要获取的值。
>例如：我们存储了 user ，有 username 和 password 两个属性，当我们需要得到 username 属性时，使用 user: username = “123”这种取得方式来获得。


![](has对应方式.canvas)

在 Redis 中，Hash（哈希）是一种类似于 Map 的数据结构，它可以存储多个键值对，其中键是一个字符串类型的字段名，值可以是字符串、整数或浮点数等数据类型。Hash 可以用于存储对象、记录、配置等数据。Hash 的常用命令包括：

1. HSET key field value

将哈希表 key 中的字段 field 的值设为 value，如果字段不存在，则创建一个新的字段，并将其值设为 value。

示例：

```
HSET myhash name "John"
```

2. HGET key field

返回哈希表 key 中指定字段 field 的值。

示例：

```
HGET myhash name
```

3. HGETALL key

返回哈希表 key 中所有字段和值。

示例：

```
HGETALL myhash
```

4. HDEL key field [field ...]

删除哈希表 key 中一个或多个指定字段，返回成功删除的字段数量。

示例：

```
HDEL myhash name
```

5. HEXISTS key field

判断哈希表 key 中是否存在指定字段 field，返回 1 表示存在，返回 0 表示不存在。

示例：

```
HEXISTS myhash name
```

6. HINCRBY key field increment

将哈希表 key 中的指定字段 field 的值增加 increment，increment 可以为负数。

示例：

```
HINCRBY myhash age 1
```

除了上述常用命令外，Hash 还支持批量设置、批量获取、获取字段数量等操作，可以根据具体需求选择相应的命令进行操作。

## Zset 有序集合
在 Redis 中，Zset（有序集合）是一种类似于 Set 的数据结构，也比较像 java 中的 Map< String，Double>。它的每个元素都关联了一个分数（score），根据分数排序并去重。Zset 可以用于存储排行榜、计分板等数据。Zset 的常用命令包括：

1. ZADD key score member [score member ...]

向有序集合 key 中添加一个或多个成员，每个成员都有一个对应的分数。如果成员已经存在，则更新其分数。

示例：

```
ZADD myzset 90 "John" 85 "Tom" 95 "Lucy"
```

2. ZCARD key

返回有序集合 key 的元素个数。

示例：

```
ZCARD myzset
```

3. ZRANK key member

返回有序集合 key 中成员 member 的排名，排名从 0 开始，按照分数从小到大排序。

示例：

```
ZRANK myzset "Tom"
```

4. ZREVRANK key member

返回有序集合 key 中成员 member 的排名，排名从 0 开始，按照分数从大到小排序。

示例：

```
ZREVRANK myzset "Tom"
```

5. ZRANGE key start stop [WITHSCORES]

返回有序集合 key 中排名在 start 和 stop 之间（包含 start 和 stop）的成员，按照分数从小到大排序。如果指定了 WITHSCORES 选项，则同时返回成员的分数。

示例：

```
ZRANGE myzset 0 -1 WITHSCORES
```

6. ZREVRANGE key start stop [WITHSCORES]

返回有序集合 key 中排名在 start 和 stop 之间（包含 start 和 stop）的成员，按照分数从大到小排序。如果指定了 WITHSCORES 选项，则同时返回成员的分数。

示例：

```
ZREVRANGE myzset 0 -1 WITHSCORES
```

除了上述常用命令外，Zset 还支持根据分数范围获取成员、移除成员等操作，可以根据具体需求选择相应的命令进行操作。

## BitMaps
BitMaps 本身不是一个数据类型，实际上是字符串（key-value）。但是他可以对字符串进行 `位操作`。
Redis 提供了一种称为 Bitmaps 的数据结构，它是一种紧凑的位图数据结构，适用于处理大量二进制数据的场景。Bitmaps 可以帮助我们高效地存储和操作二进制数据，如用户在线状态、用户签到记录、网站访问量、日活跃用户等信息。

Bitmaps 通常使用字符串数据类型来表示，其中每个位都表示某个状态或计数值。Redis 提供了一些位操作命令，如 SETBIT、GETBIT、BITCOUNT、BITOP 等，用于快速设置、获取、计算和操作位图中的位。

以下是一些常见的 Bitmaps 应用场景：

1. 用户在线状态：使用一个位表示每个用户的在线状态，0 表示离线，1 表示在线。
2. 用户签到记录：使用一个位表示每个用户在某个日期是否签到过，0 表示未签到，1 表示已签到。
3. 网站访问量：使用一个位表示每个用户在某个日期是否访问过网站，0 表示未访问，1 表示已访问。
4. 日活跃用户：使用一个位表示每个用户在某个日期是否活跃，0 表示不活跃，1 表示活跃。

Bitmaps 的优点是占用空间小、操作简单、快速和高效，但缺点是只能表示二进制状态，不适用于存储非二进制数据。因此，在使用 Bitmaps 时需要根据具体场景和需求来选择合适的数据结构。

### 偏移量
在 Redis 中，Bitmaps 中的偏移量是指每个二进制位在位图中的位置。位图中的每个二进制位都有一个唯一的偏移量，用于标识该位在位图中的位置。

偏移量从0开始，一直到位图的长度减1。例如，一个长度为8的位图的偏移量范围是0~7，分别对应着该位图中的8个二进制位。

### 常用指令
以下是 Redis 中 Bitmaps 的常用指令及其示例：

1. SETBIT key offset value

将位图 key 中偏移量为 offset 的位的值设置为 value（0或1）。

示例：

```
SETBIT online_user 1001 1
```

上述命令将位图 online_user 中偏移量为 1001 的位的值设置为 1，表示用户 1001 在线。

2. GETBIT key offset

获取位图 key 中偏移量为 offset 的位的值（0或1）。

示例：

```
GETBIT online_user 1001
```

上述命令将返回位图 online_user 中偏移量为 1001 的位的值，如果该位为 1，则表示用户 1001 在线。

3. BITCOUNT key [start end]

计算位图 key 中指定范围内的位数（start 和 end 为可选参数，用于指定范围，默认计算整个位图）。

示例：

```
BITCOUNT online_user
```

上述命令将计算位图 online_user 中所有位的值为 1 的位的数量，即在线用户的数量。

4. BITOP operation destkey key [key ...]

对一个或多个位图进行指定的位运算，并将结果保存到目标位图 destkey 中（operation 可以是 AND、OR、XOR、NOT）。

示例：

```
BITOP AND online_user1 online_user2
```

上述命令将计算位图 online_user 1 和 online_user 2 的 AND 运算结果，并将结果保存到 online_user 1 中，即在线用户的交集。

5. BITPOS key bit [start] [end]

查找位图 key 中第一个出现指定位 (bit)的位置（start 和 end 为可选参数，用于指定查找范围）。

示例：

```
BITPOS online_user 1
```

上述命令将查找位图 online_user 中第一个值为 1 的位的位置，即查找在线用户中的第一个用户。
***

## HyperLogLog
Redis HyperLogLog 是一种基数算法，去除重复，只留下基数。用于估计一个集合的基数（即集合中元素的数量）。
Mysql 中的基数操作有去重。
与传统的集合数据结构相比，HyperLogLog 数据结构具有更小的内存占用、更快的计算速度和更高的扩展性，因此在处理大规模集合数据时具有很好的性能和效果。它常用于统计网站 UV、PV 等数据，以及进行数据分析和推荐等应用场景。 
### 常用命令
Redis HyperLogLog 中常用的命令包括 PFADD、PFCOUNT、PFMERGE 等。以下是这些命令的示例：

1. PFADD key element [element ...]

向 HyperLogLog 数据结构中添加元素。

示例：

```
PFADD hll_key user1 user2 user3
```

上述命令将向 HyperLogLog 数据结构 hll_key 中添加三个元素：user 1、user 2 和 user3。

2. PFCOUNT key [key ...]

计算 HyperLogLog 数据结构中不同元素的数量。

示例：

```
PFCOUNT hll_key
```

上述命令将返回 HyperLogLog 数据结构 hll_key 中不同元素的数量，即添加到该数据结构中的元素数量的近似值。

3. PFMERGE destkey sourcekey [sourcekey ...]

将多个 HyperLogLog 数据结构合并为一个数据结构。

示例：

```
PFMERGE hll_key1 hll_key2 hll_key3
```

上述命令将合并三个 HyperLogLog 数据结构 hll_key 1、hll_key 2 和 hll_key 3，并将结果保存到 hll_key1中。

需要注意的是，HyperLogLog 数据结构的近似值会随着添加元素的数量增加而变得更加准确。因此，为了获得更准确的结果，需要在添加元素时选择合适的哈希函数和位数，并根据实际需求定期合并 HyperLogLog 数据结构。

## GEOspatial
Redis GEOspatial 是 Redis 提供的一种用于存储和处理地理位置信息的数据结构。

在 Redis 中，GEOspatial 数据结构是基于有序集合实现的，其中每个成员表示一个地理位置，每个成员都有一个唯一的名称和一个经纬度坐标。成员的分数用于存储成员的得分，可以用于排序和过滤等操作。

Redis GEOspatial 提供了多种命令，用于添加、查询、计算和操作地理位置信息。以下是一些常用命令的介绍：

1. GEOADD key longitude latitude member [longitude latitude member ...]

向指定的 GEOspatial 数据结构中添加地理位置信息。

示例：

```
GEOADD cities 116.40 39.90 Beijing 121.47 31.23 Shanghai 113.26 23.13 Guangzhou
```

上述命令将向名为 cities 的 GEOspatial 数据结构中添加三个地理位置信息，分别是北京、上海和广州。

2. GEODIST key member 1 member 2 [unit]

计算两个地理位置之间的距离。

示例：

```
GEODIST cities Beijing Guangzhou km
```

上述命令将计算名为 cities 的 GEOspatial 数据结构中北京和广州之间的距离，并返回结果以千米为单位的浮点数。

3. GEOPOS key member [member ...]

获取一个或多个地理位置的经纬度坐标。

示例：

```
GEOPOS cities Beijing
```

上述命令将获取名为 cities 的 GEOspatial 数据结构中北京的经纬度坐标。

4. GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]

查询指定范围内的地理位置信息。

示例：

```
GEORADIUS cities 116.40 39.90 1000 km WITHDIST WITHCOORD
```

上述命令将查询名为 cities 的 GEOspatial 数据结构中距离经度为 116.40，纬度为 39.90 的位置不超过 1000 千米的所有位置信息，并返回每个位置的距离和经纬度坐标等信息。

Redis GEOspatial 提供了丰富的操作和查询功能，可以广泛应用于各种需要处理地理位置信息的场景。

# 配置文件
## Utils
配置单位大小，redis 只支持 bytes，大小写不敏感。

## Include
可以包含其他文件的一些内容。

## Network
网络配置

## LIMITS、
设置链接 redis 最大连接数。
设置内存最大容量。

# Redis 的发布和订阅
Reids 的发布订阅是一种消息通信模式，
Redis 客户端可以定义任意的频道。

Redis 的发布与订阅是一种消息传递模型，用于在 Redis 客户端之间传递消息。它允许多个客户端订阅一个或多个频道，并在消息发布到这些频道时接收通知。

发布方通过将消息发布到指定的频道，而所有订阅该频道的客户端都会收到该消息。这种方式非常适合于广播通知、实时聊天、事件通知等场景。

发布与订阅模型的基本思想是：发布者（Publisher）将消息发布到频道（Channel），订阅者（Subscriber）订阅相应的频道，从而接收到发布者发布的消息。在 Redis 中，客户端可以通过 PUBLISH 命令将消息发布到指定的频道，而其他客户端可以通过 SUBSCRIBE 命令订阅这些频道，并接收到发布的消息。


# Jedis
## HelloWorld
使用 Jedis 链接 redis 时，使用 ping 方法查看是否可以连接成功，连接前要检查 reids 是否可以远程访问和端口号是否正确和没有被防火墙拦截。

## 常用命令
以下是 Java 版 Jedis 常用 API 的演示：

1. 连接 Redis 服务器：

```java
Jedis jedis = new Jedis("localhost", 6379);
jedis.auth("password"); // 如果 Redis 服务器设置了密码，需要进行认证
```

2. 设置键值对：

```java
jedis.set("key", "value");
```

3. 获取键值对：

```java
String value = jedis.get("key");
```

4. 执行事务：

```java
Transaction transaction = jedis.multi();
transaction.set("key1", "value1");
transaction.set("key2", "value2");
transaction.exec();
```

5. 执行管道：

```java
Pipeline pipeline = jedis.pipelined();
pipeline.set("key1", "value1");
pipeline.set("key2", "value2");
pipeline.sync();
```

6. 扫描 Redis 数据库：

```java
ScanParams params = new ScanParams().match("key*").count(10);
String cursor = "0";
do {
    ScanResult<String> result = jedis.scan(cursor, params);
    List<String> keys = result.getResult();
    // 处理扫描得到的键
    cursor = result.getStringCursor();
} while (!cursor.equals("0"));
```

以上演示了 Jedis 中常用的一些 API，可以用于 Redis 数据库的基本操作、事务、管道、扫描等操作。

# StringBoot 整合 Redis
1 引入依赖
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-redis</artifactId>  
</dependency>  
  
<dependency>  
    <groupId>org.apache.commons</groupId>  
    <artifactId>commons-pool2</artifactId>  
</dependency>
```

2 在 spingboot 配置文件进行配置
```properties
spring.redis.host=192.168.200.130  
spring.redis.port=6379  
spring.redis.database=0  
spring.redis.timeout=18000  
spring.redis.lettuce.pool.max-active=20  
spring.redis.lettuce.pool.max-wait=-1  
spring.reids.lettuce.pool.max-idle=5  
spring.reids.lettuce.pool.min-idle=0
```

3 添加 redis 配置类
```java
@Configuration
public class RedisConfig {
    @Bean
    public JedisConnectionFactory jedisConnectionFactory() {
        RedisStandaloneConfiguration config = new RedisStandaloneConfiguration();
        config.setHostName("localhost");
        config.setPort(6379);
        config.setPassword("yourpassword");
        JedisConnectionFactory factory = new JedisConnectionFactory(config);
        return factory;
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(jedisConnectionFactory());
        return template;
    }
}
```

# Redis 事务
Redis 中的事务，是一个单独的隔离操作，事务中的所有命令都会序列化，按照顺序执行，事务在执行过程中，不会被其他客户端发送过来的命令所打断。

Redis 事务主要作用就是 `串联多个命令`，防止别的命令插队。

## 基本命令
Redis 中的事务主要由 `MULTI`、`EXEC`、`WATCH` 和 `DISCARD` 四个命令组成。

1. `MULTI` 命令：用于开启一个事务。

2. `EXEC` 命令：用于执行事务中的所有命令。

3. `WATCH` 命令：用于监视一个或多个键，如果这些键在执行事务期间被修改，事务将被回滚。

4. `DISCARD` 命令：用于取消事务，放弃事务中的所有命令。

使用 Redis 事务的基本流程如下：

1. 使用 `WATCH` 命令监视一个或多个键。

2. 使用 `MULTI` 命令开启一个事务。

3. 执行一系列 Redis 命令，这些命令会被添加到事务队列中。

4. 使用 `EXEC` 命令执行事务中的所有命令，如果事务中的某个命令执行失败，整个事务将被回滚。

5. 如果需要取消事务，可以使用 `DISCARD` 命令。

以下是一个 Redis 事务的示例：

```
WATCH key1 key2
MULTI
SET key1 value1
SET key2 value2
EXEC
```

以上示例中，首先使用 `WATCH` 命令监视了 `key1` 和 `key2` 两个键，然后使用 `MULTI` 命令开启了一个事务，接着执行了两个 `SET` 命令，最后使用 `EXEC` 命令执行事务中的所有命令。如果在执行事务期间，被监视的 `key1` 或 `key2` 被其他客户端修改了，事务将被回滚。

除了基本命令之外，Redis 还提供了一些其他的事务命令，例如：

1. `UNWATCH` 命令：用于取消监视所有键。

2. `DISCARD` 命令：用于取消事务，放弃事务中的所有命令。

3. `EVAL` 和 `EVALSHA` 命令：用于在事务中执行 Lua 脚本。

4. `MULTI` 和 `EXEC` 命令的批量执行版本 `MULTI-EXEC` 命令。

总之，在使用 Redis 事务时，需要注意以上命令的使用方法和注意事项，以确保事务的正确执行和数据的一致性。

## 事务错误处理
事务组队阶段出现错误，执行事务，所有命令都不执行，返回错误信息。

执行阶段出现错误，只有出现错误的命令执行失败，其他正常执行。

## Redis 使用悲观锁和乐观锁
悲观锁：`当你在操作数据时候，就会上锁，不允许其他人去操作该数据，知道你释放锁。`

乐观锁：`操作数据时会对数据的版本号进行判断，只有版本号相同才能进行修改。`
Redsi 使用的是乐观锁机制。乐观锁适用于多读的场景。

乐观锁是一种并发控制机制，它假定在访问共享资源时不会发生冲突，因此不会一直占用锁。相反，它会先读取共享资源的当前版本号或时间戳，然后在执行写操作之前再次检查版本号或时间戳是否已更改。如果没有更改，则执行写操作；如果已更改，则表示有其他线程修改了共享资源，需要进行相应的冲突处理。

乐观锁通常用于处理读操作，因为读操作不需要对共享资源进行修改，因此并发读操作不会导致数据不一致或丢失。乐观锁可以提高系统的并发性能，因为它不会阻塞其他线程的访问，并且只有在发生冲突时才会执行回滚操作。

乐观锁的缺点是，如果冲突发生的概率较高，则需要频繁地进行版本号或时间戳的检查，这可能会导致性能问题。此外，乐观锁需要对数据结构进行适当的设计，以确保版本号或时间戳的正确性和唯一性。

总之，乐观锁和悲观锁各有其优点和缺点，应根据具体的场景和需求选择合适的并发控制机制。

## Reids 事务三大特性
单独的隔离操作。
	事务的所有命令斗都会序列化，安顺序的执行。事务执行过程中，不会被其他的命令所打断。

没有隔离概念
	队列中的命令没有提交之前都不会执行操作。

不保证原子性
	事务中如果有一条命令执行失败，其他命令依然会执行，不会回滚。


# 使用 redis 模拟秒杀
```java
/**  
     *     * @param uid 用户id  
     * @param prodid 商品id  
     * @return     */    public boolean doSecKill(String uid, String prodid) {  
//        1判断uid和prodid是否为空  
        if (uid == null || prodid == null) {  
            return false;  
        }  
  
//        2链接redis  
        Jedis jedis = new Jedis("192.168.200.130", 6379);  
  
//        3拼接key  
        String kcKey = "sk:" + prodid + ":qt";  
        String userKey = "sk:" + prodid + ":user";  
//        4如果库存等于null 说明秒杀还没有开始  
        String kc = jedis.get(kcKey);  
        if (kc == null) {  
            System.out.println("秒杀还没有开始，请稍等");  
            jedis.close();  
            return false;  
        }  
  
//        5 判断用户是否重复秒杀  
        if (jedis.sismember(userKey, uid)) {  
            System.out.println("你已经秒杀成功，不可以重复秒杀。");  
            jedis.close();  
            return false;  
        }  
  
//        6 判断商品库存是否小于等于0  
        if (Integer.parseInt(kc) <= 0) {  
            System.out.println("商品库存不足。秒杀结束");  
            jedis.close();  
            return false;  
        }  
//        秒杀过程  
//        库存减一  
        jedis.decr(kcKey);  
//        把秒杀成功的用户添加到用户清单里面  
        jedis.sadd(userKey, uid);  
        System.out.println("秒杀成功");  
        jedis.close();  
        return true;  
    }
```

## 秒杀出现问题超卖和连接超时

### 连接超时
当频繁链接 jedis，可能会出现 jedis 链接超时，可以使用连接池来解决。

### 超卖问题
使用乐观锁来解决超卖问题。
使用 jedis 开启事务。
```java
//        秒杀过程  
//        库存减一  
        Transaction multi = jedis.multi();  
        multi.decr(kcKey);  
        multi.sadd(userKey, uid);  
        List<Object> exec = multi.exec();  
        if (exec == null || exec.size() == 0) {  
            System.out.println("秒杀失败！");  
            jedis.close();  
            return false;  
        }  
//        把秒杀成功的用户添加到用户清单里面  
        System.out.println("秒杀成功");  
        jedis.close();  
        return true;
```

### 库存遗留问题
乐观锁造成的库存遗留问题。
可以使用 `Lua脚本语言` 来解决。
Lua 脚本的优势，他可以把 reids 中的许多命令写成一个脚本，一次提交给 redis 执行，减少反面链接 reids 的次数，提升性能。
Lua 脚本是类似 reids 的事务，有一定原子性，不会被其他命令插队，可以完成一些 redis 事务的操作。
Reids 2.6 才可以使用 Lua 脚本。
	**2.6 版本通过 redis 解决争抢问题，实际上是 reids 利用单线程的特性，用任务队列的方式解决多任务并发问题。**

# Redis 持久化
Reids 持久化分为 `RDB` 和 `AOF`。

## 备份是如何进行的？
Redis 会创建一个子进程（Fork），来进行持久化。先将数据写入到一个临时文件中。待临时文件同步完成，用这个 `临时文件替换上次持久化好的文件`。整个过程中，主进程是不进行 IO 。确保了性能，RDB 要比 AOF 方式更加高效，RDB 的缺点是最后一次持久化后数据可能丢失。

## Fork
复制一个与当前进程完全一样的进程，所有数据都一样，但是一个新的进程，并作为原进程的子进程。
 

## RDB
在指定的时间间隔内将内存中的数据集快照写入磁盘。
RDB 底层用的 `写时复制技术`。
可以在配置文件对持久化进行设置。

## 常用命令
Redis 提供了两种不同的持久化方式：RDB 和 AOF。下面是一些常用的 Redis 持久化命令：

1. `SAVE`：该命令用于创建一个 RDB 文件，其中包含当前 Redis 数据集的快照。执行该命令会阻塞 Redis 服务器，直到快照创建完毕为止。该命令对于大型数据集来说可能会非常耗时，因此应该谨慎使用。

2. `BGSAVE`：该命令与 `SAVE` 命令类似，但它会在后台创建 RDB 文件，而不会阻塞 Redis 服务器。执行该命令后，Redis 服务器会立即返回一个提示信息，告诉用户后台持久化进程已经开始。该命令通常用于在生产环境中创建备份。

3. `LASTSAVE`：该命令返回一个时间戳，表示 Redis 最近一次创建 RDB 文件的时间。

4. `BGREWRITEAOF`：该命令会重写 AOF 文件，以便于减少 AOF 文件的大小。执行该命令会创建一个新的 AOF 文件，其中包含 Redis 数据库当前状态的所有操作。与 `BGSAVE` 命令一样，该命令会在后台执行，并且不会阻塞 Redis 服务器。

5. `CONFIG SET`：该命令用于修改 Redis 的配置选项。例如，可以使用 `CONFIG SET` 命令来设置 RDB 持久化的触发条件，或者设置 AOF 持久化的缓冲区大小等选项。

6. `CONFIG GET`：该命令用于获取 Redis 的配置选项。例如，可以使用 `CONFIG GET` 命令来查看 RDB 持久化的触发条件，或者查看 AOF 持久化的缓冲区大小等选项。

注意：以上命令并非所有 Redis 持久化命令，还有其他不太常用的命令，具体可以参考 Redis 官方文档。

优势：
适合大规模的数据恢复
对数据完整性和一致性要求不高可以使用
节省磁盘空间
恢复速度快

## Rdb 数据的恢复
将备份好的文件进行保存，替换即可。
查看配置文件保存好的备份文件保存在哪哪里，去覆盖她就可以。

## AOF
Redis AOF（Append-Only File）是 Redis 中的一种数据持久化机制，它记录在 Redis 数据库上执行的每个写操作。AOF 文件是一个简单的文本文件，它包含自上次保存 AOF 以来在数据库上执行的所有写入命令的顺序日志。`但不会保存读操作`。

当 Redis 启动时，它会加载 AOF 文件，并回放文件中的所有写操作以在内存中重建数据库。Redis 还会在发生新的写操作时将其附加到 AOF 文件中，确保 AOF 文件始终反映数据库的当前状态。这意味着在发生数据库崩溃或其他问题时，Redis 可以通过回放 AOF 文件将数据库恢复到先前的状态。

AOF 持久化机制可以根据所需的数据安全和性能级别进行不同的配置。例如，您可以选择使用“everysec”选项启用 AOF，该选项将 AOF 文件每秒写入磁盘，或者使用“always”选项，该选项在每次写入操作发生时将其写入磁盘。后者提供了最高级别的数据安全，但可能会影响性能，而前者在安全性和性能之间提供了良好的平衡。

AOF 默认不开启，在配置文件进行开启配置。
当 `AOF` 和 `RDB` 同时开启，redis 默认读取 `AOF` 数据。

### 异常恢复
修改配置文件 appendonly 为 yes
如果遇到 AOF 文件损坏，通过../bin/redis-ckeck-aof--fix-【需要修复的文件名】 进行恢复。
备份被写坏的 AOF 文件。
恢复重启 reids，重新加载即可。

### AOF 同步频率设置
AOF 可以设置同步的频率。
Always 总是同步，每次的操作都会被立刻同步。性能差，但是完整性比较好。

Ecerysrc 每秒同步，每一秒同步一次，如果宕机，本地数据丢失。

No 不主动进行同步，把同步时机交给操作系统。

## Rewrite 操作
Redis Rewrite 是 Redis 中的一种 AOF（Append-Only File）重写机制，旨在解决 AOF 文件过大的问题。由于 AOF 文件记录了每个写入操作，因此随着时间的推移，AOF 文件可能会变得非常大，从而影响 Redis 的性能和存储效率。

为了解决这个问题，Redis Rewrite 会创建一个新的 AOF 文件，并将 AOF 文件中记录的操作进行重写和压缩，以减小 AOF 文件的大小。重写过程中，Redis 会分析内存中已有的数据，然后将其重新写入新的 AOF 文件中，以便在将来重建数据库时使用。此过程可以使用 BGREWRITEAOF 命令手动触发，也可以自动触发，例如在 Redis 达到指定大小或使用时间后。

Redis Rewrite 具有以下优点：

1. 减小 AOF 文件的大小，节约存储空间。
2. 提高 Redis 的性能，因为较小的 AOF 文件可以更快地重建数据库。
3. 减少 AOF 文件在磁盘上的碎片，提高磁盘利用率。

需要注意的是，Redis Rewrite 是一种非常消耗资源的操作，因此在执行此操作时可能会对 Redis 的性能产生负面影响。因此，建议在空闲时间或低负载时间执行此操作，并根据实际情况进行配置。

## 如何选择?
官方推荐两个都开启。
如果对数据不敏感，可以单独使用 RDB。
不建议单独使用 AOF，因为可能会出现 bug。
如果做纯内存缓存，两个都不需要开启。

# 主从复制
Redis 主从复制是一种数据同步机制，它允许将一个 Redis 实例（称为主节点）的数据复制到其他 Redis 实例（称为从节点）上。主节点可以进行写操作，从节点则只能进行读操作，因此主从复制通常用于提高读取性能和数据冗余。

主从复制的工作原理如下：

1. 从节点向主节点发送 SYNC 命令，请求进行复制。
2. 主节点接收到 SYNC 命令后，开始进行 BGSAVE 操作，将当前内存中的数据写入磁盘生成 RDB 文件，并将 RDB 文件发送给从节点。
3. 主节点将所有新的写入操作记录在内存中，并将这些操作发送给从节点进行复制。
4. 从节点接收到RDB文件后，将其加载到内存中，并从主节点复制之后的所有写入操作，以保持与主节点相同的数据状态。

主从复制的优点包括：

1. 提高读取性能：从节点可以进行读操作，从而分担主节点的读取请求，提高读取性能。
2. 数据冗余：从节点存储着主节点的完整数据备份，因此可以在主节点故障时快速切换到从节点。
3. 负载均衡：可以将读操作分配到从节点，从而降低主节点的负载，提高整个系统的吞吐量。

需要注意的是，主从复制并不是完全实时的，从节点的数据可能会比主节点稍微滞后一些。此外，主从复制还有一些其他的限制和缺点，例如复制延迟、单点故障等问题，需要在使用时进行考虑和解决。

## 配置一主两从
要创建一个 Redis 主从复制的架构，您需要执行以下步骤：

1. 准备 Redis 服务器：您需要在三台服务器上安装 Redis，并确保它们都可以相互访问。
2. 配置主节点：在主节点的 redis. Conf 配置文件中，将 bind 选项设置为主节点 IP 地址，并将 slaveof 选项保留为空。然后启动 Redis 服务器。
3. 配置从节点 1：在从节点 1 的 redis. Conf 配置文件中，将 bind 选项设置为从节点 1 的 IP 地址，并将 slaveof 选项设置为主节点的 IP 地址和端口号。然后启动 Redis 服务器。
4. 配置从节点 2：在从节点 2 的 redis. Conf 配置文件中，将 bind 选项设置为从节点 2 的 IP 地址，并将 slaveof 选项设置为主节点的 IP 地址和端口号。然后启动 Redis 服务器。
5. 验证主从复制：在主节点上执行一些写操作，然后在从节点上执行一些读操作，以确保数据已经被成功复制。

以下是一个示例配置文件，用于创建一个包含一个主节点和两个从节点的 Redis 主从复制架构：

```
# Redis主节点配置
bind 192.168.0.1
port 6379
daemonize yes
pidfile /var/run/redis/redis.pid

# Redis从节点1配置
bind 192.168.0.2
port 6380
daemonize yes
pidfile /var/run/redis/redis-slave-1.pid
slaveof 192.168.0.1 6379

# Redis从节点2配置
bind 192.168.0.3
port 6381
daemonize yes
pidfile /var/run/redis/redis-slave-2.pid
slaveof 192.168.0.1 6379
```

在这个示例中，主节点的 IP 地址是 192.168.0.1，从节点 1 的 IP 地址是 192.168.0.2，从节点 2 的 IP 地址是 192.168.0.3。主节点使用默认端口号 6379，从节点 1 使用端口号 6380，从节点 2 使用端口号6381。
**具体自己需要的配置可以在配置文件中进行覆盖。**

查看当前服务器是主服务器还是子服务器
```
info replication
```

配从不配主，输入命令可以成为某个服务器的从服务器。在从服务器执行：
```
slaveof ip 端口号
```
这种指令当从机重启后就会失效。在配置文件的是主要配置。

## 一主二仆
当从服务器挂掉的时候，主服务器添加了数据，从服务器依然会同步所有的数据。
当主服务器挂掉之后，从服务器依然还是从服务器，不会取代主服务器。

## 薪火相传
当我们需要管理很多从机时，我们只需要管理最初的从机，让最初的从机去管理更多的从机。
![](薪火相传.canvas)

## 反客为主
但一个主机宕机了，后的从机可以立刻升级为主机，其他从机不需要修改操作。

使用命令 `slaveof no one` 将从机变为主机。

## 哨兵模式
哨兵模式就是 `反客为主自动版。` 能够后台自动监控主机是否故障，故障了将根据投票数自动将从机升级主机。


当主机宕机后，会根据一定规则选取新的主机，就算主机重新启动，他也是作为新主机的从机存在。

### 复制延迟
由于所有写操作都在主机进行，然后同步到从机，所以会有一定延迟，当系统繁忙时，延迟问题会更加严重，从机的数量也会增加这个影响。

### 从机选择规则
1配置文件中的 `slave-priority` 的值越小，优先级越高。
2 谁和主机的同步更完整，选取谁。
3 选择 runid（每个 reids 实例启动后，会随机生成 40 位的 runid） 最小的服务。

### 哨兵模式开启
写一个配置文件，启动它即可。

# 集群
Redis 集群是一种分布式的 Redis 解决方案，它可以将数据分散到多个节点上，并且提供高可用性和可扩展性。Redis 集群使用分片技术将数据分散到多个节点上，每个节点负责存储部分数据，并且提供读写服务。当节点发生故障时，Redis 集群可以自动进行故障转移，从而保证系统的高可用性。

Redis 集群的主要特点包括：

1. 分布式架构：Redis 集群采用分布式架构，将数据分散到多个节点上，每个节点负责存储部分数据，从而提高了系统的可扩展性和吞吐量。

2. 自动故障转移：Redis 集群可以自动进行故障转移，当某个节点发生故障时，集群会自动将数据迁移到其他节点上，并且选举新的主节点，从而保证了系统的高可用性。

3. 主从复制：Redis 集群中的每个节点都可以配置为主节点或从节点，主节点负责接收写操作，从节点负责接收读操作。主节点将数据同步到从节点上，从节点可以直接提供读服务，从而提高了系统的读性能。

4. 节点间通信：Redis 集群使用 Gossip 协议进行节点间通信，每个节点会周期性地向其他节点发送消息，以便了解集群中其他节点的状态和数据分布情况。

需要注意的是，在使用 Redis 集群时，需要注意以下几点：

1. Redis 集群中的每个节点都需要占用大量的内存，因此需要在选择硬件时考虑节点数量和内存大小。

2. Redis 集群的分片机制可能会导致数据分布不均，需要进行合理的分片策略，以充分利用各个节点的性能。

3. Redis 集群中的每个节点都需要配置相同的参数和持久化方式，以保证数据的一致性。

4. Redis 集群中的节点数量不能太小或太大，通常建议将节点数量控制在 6 到 12 个之间，以便充分利用系统资源并且保证系统的稳定性。

综上所述，Redis 集群是一种高可用、可扩展的分布式 Redis 解决方案，可以应用于多种场景，如缓存、消息队列等。在使用 Redis 集群时，需要注意合理的节点数量和分片策略，并且保证节点的一致性和稳定性。

![](无中心化集群.canvas)

## 集群搭建
Redis 集群的搭建需要以下几个步骤：

1. 安装 Redis：在搭建 Redis 集群之前，需要先安装 Redis。可以从 Redis 官网下载 Redis 的安装包，然后解压缩安装包，即可完成 Redis 的安装。

2. 配置 Redis 节点：在搭建 Redis 集群之前，需要先配置 Redis 节点。可以在每个节点上创建一个 Redis 实例，并且在 redis. Conf 文件中进行配置，包括节点的 IP 地址、端口号、密码等。

3. 启动 Redis 节点：在完成 Redis 配置后，需要启动每个节点上的 Redis 实例。可以使用命令 `redis-server /path/to/redis.conf` 启动 Redis 实例。

4. 创建 Redis 集群：使用 Redis 提供的 `redis-trib.rb` 脚本创建 Redis 集群。该脚本位于 Redis 源代码的 `src` 目录下。可以使用以下命令来创建 Redis 集群：

```
redis-trib.rb create --replicas 1 <node1>:<port> <node2>:<port> ... <nodeN>:<port>
```

此命令会创建一个包含多个 Redis 节点的集群，并且指定每个主节点的从节点数量。其中，`<nodeX>:<port>` 表示 Redis 节点的 IP 地址和端口号。

5. 验证 Redis 集群：创建 Redis 集群后，可以使用 `redis-cli` 命令来验证集群是否正常工作，如下所示：

```
redis-cli -c
cluster info
```

此命令会打印出 Redis 集群的信息，包括集群节点数量、槽位分布情况、主从关系等。

需要注意的是，在进行 Redis 集群的搭建时，应该遵循以下几个原则：

1. Redis 集群中的每个节点都应该配置相同的参数和持久化方式，以保证数据的一致性。

2. Redis 集群中的节点数量应该控制在 6 到 12 个之间，以充分利用系统资源并且保证系统的稳定性。

3. Redis 集群中的每个节点都需要占用大量的内存，因此需要在选择硬件时考虑节点数量和内存大小。

综上所述，Redis 集群的搭建需要进行节点配置、启动节点、创建集群等步骤，需要注意节点数量、一致性和稳定性等问题。

 分配原则保证主机和主机运行在不同的 IP 上，主机和从机也在的 IP 上。

## Slots
每个集群会有固定 16384 个槽位，用来存储信息，他会自动分配给每个主机范围，用自己一套算法来决定 key 放在那个槽位。缓解服务器压力。

**想在集群中插入多条数据需要用组来插入，以组的名字来计算他的插槽。使用{}来进行组操作。**
```
 meset name{user} zhangsan age{user} 18
```

Redis 使用分片技术将数据分散到多个节点上，每个节点负责存储部分数据，并且提供读写服务。在 Redis 中，分片的方式是通过使用 slots（槽位）来实现的。Redis 将所有的键都映射到一个 0 到 16383 的整数范围内，每个整数对应一个槽位，每个槽位都存储一些键值对。每个 Redis 节点负责维护一部分槽位，其中一些槽位作为主节点，另一些槽位作为从节点。主节点负责接收写操作，从节点负责接收读操作。

在 Redis 集群中，使用以下命令可以查看各个节点的槽位信息：

```
cluster nodes
```

此命令会打印出集群中各个节点的 ID、IP 地址、端口号、槽位信息等。可以根据这些信息来判断每个节点负责哪些槽位。

在 Redis 集群中，如果某个节点发生故障，它负责的槽位会被迁移到其他节点上。Redis 集群使用 Gossip 协议来实现节点间的通信，每个节点会周期性地向其他节点发送消息，以便了解集群中其他节点的状态和数据分布情况。

需要注意的是，在使用 Redis 分片技术时，应该注意以下几点：

1. Redis 集群中的各个节点应该配置相同的参数和持久化方式，以保证数据的一致性。

2. Redis 集群中的节点数量应该控制在 6 到 12 个之间，以充分利用系统资源并且保证系统的稳定性。

3. Redis 集群中的每个节点都需要占用大量的内存，因此需要在选择硬件时考虑节点数量和内存大小。

综上所述，Redis 使用槽位来实现分片技术，每个节点负责维护一部分槽位，从而实现数据的分散存储和高可用性。在使用 Redis 分片技术时，需要注意节点数量、一致性和稳定性等问题。

## 集群细节 
宕集群中一对服务器宕机了，根据配置文件的 `cluster-requite-full-coverage` 为 yes，那个整个集群都挂掉，反之则宕机的服务器的插槽不能提供服务，其他正常使用。

## Jedis 操作集群
Jedis 是 Redis 的 Java 客户端，支持连接单个 Redis 节点和 Redis 集群。在 Jedis 中使用 Redis 集群需要以下几个步骤：

1. 导入 Jedis 依赖：在 Java 项目中使用 Jedis 需要导入相应的依赖，例如使用 Maven 构建项目时，在 pom. Xml 文件中添加以下依赖：

```
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.6.1</version>
</dependency>
```

2. 创建 JedisCluster 对象：JedisCluster 是 Jedis 提供的用于连接 Redis 集群的对象，需要指定 Redis 集群的节点信息和连接池配置等参数，例如：

```java
import redis.clients.jedis.HostAndPort;
import redis.clients.jedis.JedisCluster;

import java.util.HashSet;
import java.util.Set;

public class RedisClusterClient {
    public static void main(String[] args) {
        //设置Redis集群节点信息
        Set<HostAndPort> nodes = new HashSet<>();
        nodes.add(new HostAndPort("127.0.0.1", 7000));
        nodes.add(new HostAndPort("127.0.0.1", 7001));
        nodes.add(new HostAndPort("127.0.0.1", 7002));
        //创建JedisCluster对象
        JedisCluster jedisCluster = new JedisCluster(nodes);
        //执行Redis操作
        jedisCluster.set("key", "value");
        //关闭连接
        jedisCluster.close();
    }
}
```

此代码会创建一个 JedisCluster 对象，指定 Redis 集群的节点信息，并且将一个键值对（key=value）存储到 Redis 中。

需要注意的是，在使用 JedisCluster 连接 Redis 集群时，应该注意线程安全和连接池等问题，避免出现资源竞争和连接泄漏等问题。同时，在使用 Redis 存储数据时，应该注意数据的格式和一致性，以充分利用系统资源并且减少存储成本。

综上所述，使用 Jedis 连接 Redis 集群需要导入 Jedis 依赖、创建 JedisCluster 对象，并且注意线程安全和连接池等问题。

## 集群的优点和缺点
Redis 集群是 Redis 分布式部署的一种方式，可以将数据分散存储在多个节点上，从而提高系统的可用性和性能。Redis 集群具有以下优点和缺点：

优点：
1. 高可用性：Redis 集群可以在多个节点之间复制数据，从而实现数据的备份和故障转移。当某个节点出现故障时，集群会自动将请求转发到其他健康节点上，保证系统的可用性。

2. 高性能：Redis 集群可以将数据分散存储在多个节点上，从而提高系统的读写性能。同时，集群还支持多个节点之间的数据复制和同步，保证数据的一致性。

3. 扩展性好：Redis 集群可以动态添加或删除节点，从而实现系统的横向扩展。当系统的负载增加时，可以通过添加节点来提高系统的性能。

4. 灵活性高：Redis 集群支持多种数据结构和操作，可以满足不同业务场景的需求。同时，集群还提供了多种配置选项，可以根据实际情况进行调整。

缺点：
1. 部署和维护难度大：Redis 集群需要部署和维护多个节点，需要考虑节点之间的通信、数据同步和故障转移等问题。同时，集群还需要进行监控和管理，保证系统的稳定性和可用性。

2. 部分命令不支持：Redis 集群中的部分命令不支持跨节点操作，需要将多个命令打包成一个事务进行操作。这可能会导致一些性能问题和操作复杂性。

3. 存储数据的限制：Redis 集群中每个节点只存储部分数据，因此无法支持一些特殊的数据结构和操作，如 Lua 脚本、SORT 命令和 SCAN 命令等。

综上所述，Redis 集群具有高可用性、高性能、扩展性好和灵活性高等优点，但是也存在部署和维护难度大、部分命令不支持和存储数据的限制等缺点。在使用 Redis 集群时，应该根据实际业务场景选择合适的部署方式，并且注意集群的监控和管理，保证系统的稳定性和可用性。

# 缓存穿透
 Redis 缓存穿透是指客户端请求的数据在 Redis 缓存中不存在，而且在数据源（如数据库）中也不存在，导致请求无法命中缓存，每次都要访问数据源，从而导致缓存和数据源的负载增加，甚至可能导致数据源崩溃。

缓存穿透可能是由于恶意攻击、数据异常或者业务逻辑错误等原因导致的。为了解决 Redis 缓存穿透问题，可以采取以下措施：

1. 缓存空值：在 Redis 中缓存空值，即在数据源中不存在的数据，在缓存中也存储一个空值，这样下次请求时就可以从缓存中获取空值，而不必每次访问数据源。

2. 布隆过滤器（Bloom Filter）：使用布隆过滤器可以在 Redis 缓存中快速判断某个请求的数据是否存在，如果不存在就直接返回，避免访问数据源。布隆过滤器可以高效地判断某个数据是否在一个集合中，但是可能会存在误判的情况。

3. 限流措施：可以设置一个阈值，当某个 IP 地址或者用户请求的频率超过一定限制时就进行限流，避免恶意攻击和大量无效请求导致的缓存穿透。

4. 数据预热：可以在系统启动时，将数据源中的热门数据预先加载到 Redis 缓存中，避免请求命中率低导致的缓存穿透。

5. 异步更新缓存：可以使用异步更新缓存的方式，即先更新数据源中的数据，再异步更新 Redis 缓存中的数据，避免缓存和数据源不一致导致的缓存穿透。

需要注意的是，为了避免缓存穿透问题，应该在设计系统时考虑缓存的一致性和可靠性，选择合适的数据结构和缓存策略，并且避免恶意攻击和数据异常等情况。

# 缓存击穿
Redis 缓存击穿是指某个热点数据在缓存中过期或者被删除，而此时有大量请求同时访问这个数据，导致大量请求都命中数据源（如数据库），从而导致数据源的负载增加，甚至可能导致数据源崩溃。

为了解决 Redis 缓存击穿问题，可以采取以下措施：

1. 加锁：可以在请求访问数据前加锁，避免多个请求同时访问数据源。例如使用 Redis 的分布式锁（如 Redlock 算法）或者使用 Java 的锁（如 synchronized 关键字）。

2. 热点数据永不过期：可以将某些热点数据的过期时间设置为永久，即在 Redis 缓存中永久保存这些数据，避免过期时间到达导致的缓存击穿。

3. 预加载：可以在系统启动时，将某些热点数据预先加载到 Redis 缓存中，避免过期时间到达导致的缓存击穿。

4. 二级缓存：可以使用二级缓存的方式，即在 Redis 缓存中保存热点数据的 ID，而不是保存具体的数据，当数据过期时，先从二级缓存中获取热点数据的 ID，再从数据源中获取具体数据，然后更新 Redis 缓存中的热点数据 ID。

需要注意的是，为了避免缓存击穿问题，应该在设计系统时考虑缓存的一致性和可靠性，选择合适的数据结构和缓存策略，并且避免热点数据过多导致的缓存击穿。同时，在使用锁的方式时，应该注意避免死锁和锁粒度过大或过小导致的性能问题。

# 缓存雪崩
Redis 缓存雪崩是指在缓存中大量的数据同时过期或者缓存服务宕机，导致大量请求无法命中缓存而访问数据源，从而导致数据源的负载骤增，甚至可能导致数据源崩溃。

为了解决 Redis 缓存雪崩问题，可以采取以下措施：

1. 缓存数据过期时间随机化：可以将缓存数据的过期时间随机化，避免大量数据同时过期导致的缓存雪崩。例如在缓存数据的过期时间上增加一个随机值，避免多个数据同时过期。

2. 缓存数据永不过期：可以将某些热点数据的过期时间设置为永久，即在 Redis 缓存中永久保存这些数据，避免缓存过期导致的缓存雪崩。

3. 分布式锁：可以在缓存数据过期时，使用分布式锁避免多个请求同时更新缓存数据。例如使用 Redis 的分布式锁（如 Redlock 算法）或者使用 Java 的锁（如 synchronized 关键字）。

4. 限流措施：可以设置一个阈值，当请求的频率超过一定限制时就进行限流，避免大量请求同时访问数据源。

5. 备份缓存：可以使用备份缓存的方式，即在 Redis 缓存宕机时，从备份缓存中获取数据，避免大量请求同时访问数据源。


# 分布式锁
分布式锁是一种用于分布式系统中的并发控制机制，它可以保证多个进程或者服务器之间的互斥访问。在分布式系统中，由于数据分散在多个节点上，因此需要分布式锁来保证数据的一致性和安全性。

分布式锁的实现方式有很多种，常见的实现方式包括：

1. 基于数据库的实现：可以使用数据库中的行级锁或者悲观锁来实现分布式锁，但是这种方式需要考虑锁竞争和死锁等问题。

2. 基于 ZooKeeper 的实现：可以使用 ZooKeeper 的有序节点和 Watcher 机制来实现分布式锁，但是这种方式需要依赖 ZooKeeper，并且存在性能问题。

3. 基于 Redis 的实现：可以使用 Redis 的 SETNX 和 EXPIRE 等命令来实现分布式锁，具有高性能和可靠性。

分布式锁的实现需要考虑以下问题：

1. 锁的唯一性：为了避免多个进程或者服务器同时获取同一个锁，需要保证锁的唯一性，一般使用一个唯一标识作为锁的名称。

2. 锁的过期时间：为了避免锁长时间占用，需要设置锁的过期时间，并且在获取锁成功后需要及时更新锁的过期时间。

3. 锁的释放：为了避免锁长时间占用，需要及时释放锁，并且在释放锁时需要判断是否是当前进程或者服务器持有的锁，避免误删其他进程或者服务器的锁。

4. 锁的可重入性：如果一个进程或者服务器已经持有锁，再次获取锁时需要考虑锁的可重入性，避免死锁或者锁竞争问题。

需要注意的是，在使用分布式锁时，应该根据实际需求选择合适的锁实现方式，并且需要考虑锁的性能、可靠性和安全性等问题。同时，在使用锁的方式时，应该避免死锁和锁粒度过大或过小导致的性能问题。

## 基于 redis 分布式锁
使用命令 `sexnx` 上锁，通过 `del` 释放锁。
如果锁一直没有释放，可以设置 key 的过期时间，来自动释放。
当上锁之后出现异常，无法设置过期时间，我们可以在上锁的同时设置过期时间。
```
set key值 value值 nx ex 过期时间
```

## UUID 解决 redis 释放不同锁问题
当我们使用分布式锁时，会出现一种场景
我们设置值 10 秒自动释放。当我们 a 线程抢到了锁，进行上锁，对数据进行操作，此时我们的服务器宕机了，并超过十秒，此时锁自动释放。B 线程又抢到了锁，上锁进行操作，在 b 操作数据时，服务器恢复了，a 线程进行了锁的释放，他此时释放的是 b 的锁。但是 b 的操作还没完成。出现了问题。

### 解决方法
我们可以使用 uuid 作为 key，当我们要进行释放锁的时候，进行判断，此时数据的 key 和是否和操作的 key 相同。相同则释放，否则不释放。

## Lua 脚本解决释放不同锁
当我们 a UUID 判断通过，正要删除，但是还没有删除锁的时候，自动删除时间到了。此时操作权限给到 b，b 在操作的时候，a 把 b 的锁删除了。

使用 Lua 脚本保证原子性来解决问题。

# Redis 6 新特性

## ACL
Redis ACL（Access Control Lists）是 Redis 6.0 版本中引入的一项新功能，它提供了更细粒度的访问控制，可以限制对 Redis 命令的使用，以及对特定键的读写权限。类似 mysql 的用户权限设置。下面是一些常用的 Redis ACL 命令：

1. 创建用户并分配权限：

```
ACL SETUSER alice +@all
```

这条命令创建了一个名为“alice”的用户，并为其分配了所有命令的权限。"+"符号表示授予权限，"@all"表示所有命令。

2. 查看已创建的用户：

```
ACL USERS
```

这条命令列出了所有已创建的用户及其权限。

3. 删除用户：

```
ACL DELUSER alice
```

这条命令删除了名为“alice”的用户。

4. 限制 IP 地址：

```
ACL SETUSER bob +@read ~127.0.0.1
```

这条命令创建了一个名为“bob”的用户，并为其分配了只读权限，同时限制了该用户只能从本地 IP 地址（127.0.0.1）访问 Redis 服务器。

5. 保存和加载 ACL 配置：

```
ACL SAVE
ACL LOAD
```

这两条命令分别用于将当前的 ACL 配置保存到文件中，以及从文件中加载 ACL 配置。

需要注意的是，Redis ACL 是在 Redis 6.0 版本中引入的，如果您使用的是旧版本的 Redis，可能不支持 ACL 功能。另外，在使用 ACL 时，建议使用 TLS（Transport Layer Security）加密协议，以确保通信安全。

## 多线程

这里的多线程值得 redis 解析网络是多线程，执行命令依然是单线程。
Redis 6.0 引入了多线程支持，可以更好地利用多核处理器，并提高 Redis 的 IO 性能。要开启 Redis 的多线程 IO 功能，需要按照以下步骤进行操作：

1. 在 Redis 的配置文件 redis. Conf 中，将以下两个参数的值设置为1：

```
io-threads-do-reads yes
io-threads-do-writes yes
```

2. 重新启动 Redis 服务器。

3. 使用 redis-cli 命令连接到 Redis 服务器，并执行以下命令：

```
CONFIG SET io-threads 4
```

其中“4”是要启用的 IO 线程数。可以根据具体的需要设置不同的数值。需要注意的是，启用的 IO 线程数不能超过系统的 CPU 核心数。

4. 如果需要查看 Redis 的 IO 线程状态，可以执行以下命令：

```
INFO THREADPOOL
```

这条命令会显示 Redis 的线程池状态，包括 IO 线程的数量、状态和处理的任务数等信息。

需要注意的是，启用多线程 IO 功能会增加 Redis 服务器的 CPU 和内存开销，因此需要根据实际情况进行调整。另外，启用多线程 IO 功能可能会影响 Redis 的稳定性和可靠性，因此需要仔细评估并进行充分的测试。

## 工具支持 Cluster
Redis 6 已经集成了 Cluster 工具。

# 总结
