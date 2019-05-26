---
title: Redis学习日志
layout: article
tags: Redis
---
# Redis学习

## 目录

[TOC]

## Redis简介

​		Redis 是完全开源免费的，遵守BSD协议，是一个高性能(NOSQL)的key-value数据库,Redis是一个开源的使用ANSI [C语言](https://baike.baidu.com/item/C语言)编写、支持网络、可基于内存亦可持久化的日志型、Key-Value[数据库](https://baike.baidu.com/item/数据库)，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。(Vmware在资助着redis项目的开发和维护)。

### NoSQL介绍

​		NoSQL，泛指非关系型的数据库。随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。**NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题**。



### Redis特点

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持的类型 String, List, Hash, Set 及 Ordered Set 数据类型操作。
- 原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

### Redis优点

- 丰富的数据结构
- 高速读写，redis使用自己实现的分离器，代码量很短，没有使用lock（MySQL），因此效率非常高。

### Redis缺点

- 持久化。Redis直接将数据存储到内存中，要将数据保存到磁盘上，Redis可以使用两种方式实现持久化过程。定时快照（snapshot）：每隔一段时间将整个数据库写到磁盘上，每次均是写全部数据，代价非常高。第二种方式基于语句追加（aof）：只追踪变化的数据，但是追加的log可能过大，同时所有的操作均重新执行一遍，回复速度慢。 
- 耗内存，占用内存过高。 

## Redis安装

[菜鸟教程--redis安装](https://www.runoob.com/redis/redis-install.html  "redis安装")



## Redis常用命令

### Redis运行与关闭

进入redis目录

```shell
cd /path to redis/redis
```

运行redis服务端

```shell
./bin/redis-server ./redis.conf
```

运行redis客户端：

- 没有配置密码时：

  ```shell
  /path to redis/bin/redis-cli
  ```

- 配置密码后

  ```shell
  /path to redis/bin/redis-cli -a password
  ```

redis关闭

```shell
/path to reis/redis-cli shutdown
```



### Redis配置文件

Redis的配置文件位于Redis的安装目录下，名称为**redis.conf**，将其复制到安装好的位置下，并使用vim打开文件

```shell
cd /path to redis-install/
cp redis.conf /path to redis/
vim /path to redis/redis.conf
```



### Redis配置项

#### redis.conf配置文件说明（粗体配制项较常用）

**基本配置**

1. **Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程**

   ```
   daemonize no
   ```

2. 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定

   ```
    pidfile /var/run/redis.pid
   ```

3. **指定Redis监听端口，默认端口为6379。**

   ```
   port 6379
   ```

4. **绑定的主机地址**

   ```redis.conf
   bind 127.0.0.1
   ```

5. **当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能**

   ```redis.conf
    timeout 300
   ```

6. **指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose**

   ```
   loglevel verbose
   ```

7.  日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null

   ```
   logfile stdout
   ```

8. 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id

   ```
   databases 16
   ```

9. **指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合**

   ```
   # save <seconds> <changes>
   # Redis默认配置文件中提供了三个条件：
   
   save 900 1
   save 300 10
   save 60 10000
   
   #分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有#10个更改以及60秒内有10000个更改
   ```

10. 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大

    ```
    rdbcompression yes
    ```

11. **指定本地数据库文件名，默认值为dump.rdb**

    ```
    dbfilename dump.rdb
    ```

12. 指定本地数据库存放目录

    ```
    dir ./
    ```

13. 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步

    ```
    slaveof <masterip> <masterport>
    ```

14. 当master服务设置了密码保护时，slav服务连接master的密码

    ```
    masterauth <master-password>
    ```

15. **设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过`AUTH <password>`命令提供密码，默认关闭**

    ```
    requirepass foobared
    ```

16. 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息

    ```
    maxclients 128
    ```

17. **指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区**

    ```
    maxmemory <bytes>
    ```

18. **指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no**

    ```
    appendonly no
    ```

19. 指定更新日志文件名，默认为appendonly.aof

    ```
    appendfilename appendonly.aof
    ```

20. 指定更新日志条件，共有3个可选值：      **no**：表示等操作系统进行数据缓存同步到磁盘（快）      **always**：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）      **everysec**：表示每秒同步一次（折中，默认值）

    ```
    appendfsync everysec
    ```

21. 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）

    ```
    vm-enabled no
    ```

22. 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享

    ```
    vm-swap-file /tmp/redis.swap
    ```

23. 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0

    ```
    vm-max-memory 0
    ```

24. Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不 确定，就使用默认值

    ```
    vm-page-size 32
    ```

25. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。

    ```
    vm-pages 134217728
    ```

26. 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4

    ```
    vm-max-threads 4
    ```

27. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启

    ```
    glueoutputbuf yes
    ```

28. 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法

    ```
    hash-max-zipmap-entries 64
    hash-max-zipmap-value 512
    ```

29. 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）

    ```
    activerehashing yes
    ```

30. 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件

    ```
    include /path/to/local.conf
    ```

**高级配置--Redis中的内存维护策略**

redis作为优秀的中间缓存件，时常会存储大量的数据，即使采取了集群部署来动态扩容，也应该即使的整理内存，维持系统性能。

在redis中有两种解决方案：

- 一是为数据设置超时时间
- 二是采用LRU算法动态将不用的数据删除。

内存管理的一种页面置换算法，对于在内存中但又不用的数据块（内存块）叫做LRU，操作系统会根据哪些数据属于LRU而将其移出内存而腾出空间来加载另外的数据。

1. volatile-lru：设定超时时间的数据中,删除最不常使用的数据.

2. **allkeys-lru**：查询所有的key中最近最不常使用的数据进行删除。**这是应用最广泛的策略。**

3. volatile-random：在已经设定了超时的数据中随机删除.

4. allkeys-random：查询所有的key,之后随机删除.

5. volatile-ttl：查询全部设定超时时间的数据,之后排序,将马上将要过期的数据进行删除操作.

6. noeviction：如果设置为该属性,则不会进行删除操作,如果内存溢出则报错返回.

- volatile-lfu：从所有配置了过期时间的键中驱逐使用频率最少的键
- allkeys-lfu：从所有键中驱逐使用频率最少的键

[Redis的缓存淘汰策略LRU与LFU--简书](https://www.jianshu.com/p/c8aeb3eee6bc  "Redis的缓存淘汰策略LRU与LFU--简书")



#### 自定义配置文件

打开配置文件

```shell
vim /path to redis/redis.conf
```

将文件对应的配置项修改如下：

```
#开启守护进程
daemonize yes
#将 bind 127.0.0.1注释掉，可以使得外部访问redis，这个用于在后台中使用redis，和在其他平台上通过redis-desktop访问redis，只建议在开发过程事注释掉该命令。
# bind 127.0.0.1
#设置密码
requirepass axdii
#开启ROF-AOF混合模式
#注意，因为redis4.0版本以上默认开启aof之后的持久化方案为ROF-AOF混合模式，所以只需要把AOF开启后
#就能够使用ROF-AOF混合模式了
appendonly yes
#配置分配的最大内存，建议1G内存的话分配256MB，最大不能超过本机内存
maxmemory 256mb
#开启allkeys-lru淘汰策略
maxmemory-policy allkeys-lry
```

​		Redis采用的是单进程多线程的模式。当redis.conf中选项daemonize设置成yes时，代表开启守护进程模式。在该模式下，redis会在后台运行，并将进程pid号写入至redis.conf选项pidfile设置的文件中，此时redis将一直运行，除非手动kill该进程。但当daemonize选项设置成no时，当前界面将进入redis的命令行界面，exit强制退出或者关闭连接工具(putty,xshell等)都会导致redis进程退出。
​		**服务端开发的大部分应用都是采用后台运行的模式** 



## Redis学习

### Redis常用命令

- 关闭redis（会做持久化过程，如果用kill -9 pid就不行）：`shutdown`

- 开启redis服务器：`redis-server ./redis.conf`
- 设置key的过期时间：`expire [key] [second]`，如`expire axd 10`即该axd的key能存活10秒

- 以毫秒为单位设置key的过期时间：`pexpire [key] [milliseconds]`

- 查看key的过期时间：`ttl key`

- 查看key的毫秒过期时间：`ptl key`

- 移除b的过期时间：`persist b`

- 使用正则表达式搜索key：`keys [pattern]`

- 随机返回存在的key：`randomkey`

- 是选择数据库，默认存在16个数据库，默认选定的数据库为0：`select [index]`
- 将当前数据库的key移动到给定的数据库index当中：`move key [index]`
- 查看key类型：`type key`

### redis命令规范

redis单个key最大存入512M大小。如users表中有id，name，age，uname等相关信息，在redis中，最好如下命令：

- `users:1:name 维达堡`

- `users:2:name 呵呵哒`

  应该遵循的规范：

  1. key不要太长，尽量不要超过1024字节，这不仅消耗内存，而且会降低查找效率
  2. key也不要太短，太短的话，key的可读性能会降低
  3. 在一个项目中，key最好使用同一的命名模式，例如users:123:password
  4. key区分大小写

### redis数据类型

#### 字符串类型string（最大一个能存512MB）

1. string类型最大一个能存储512MB字节，所以用于存储序列化后的大型文件比较合适
2. string是二进制安全的

二进制安全是指：在传输数据时，保证二进制数据的信息安全，也就是不被篡改，破译等，如果被攻击，执行效率高。二进制安全特点：

- 编码、解码发生在客户端完成，执行效率高
- 不需要频繁的编码解码，不会出现乱码

3. string命令

- **普通赋值语法：`set key_name value`**
- 如果key不存在就赋值：`setnx key value`
- 获取值的一部分：`getrange start end`，例子`getrange yahaha 0 3`，返回从0开始到第三个字符，共四个
- `getset`：如果旧值存在，设置新值后返回旧值，不存在的话，设置后返回nil
- `strlen key`：返回key所存储的字符串的长度
- 删除字符串（通用）：`del key`
- **自增：`incr key_name`：incrby命令将key中存储得数字加上指定的增量值。如果key不存在，那么key的值将初始化为0，然后再执行incr操作**
- **自减：`decr key_name`**
- **`incrby key`      增量值：增加指定值**
- `decrby key` 减量值：减少指定值
- `append`：追加

4. 应用场景：

1. string通常用于保存单个字符串或json字符串数据

2. 因string是二进制安全的，可以存储图片、视频

3. 计数器（粉丝数）

#### 哈希类型hash（适合存储对象）【一张表能存储40多亿属性】

1. hash特点：适合存储对象，一张表能存储40多亿属性

2. 哈希常用命令

- `hset key field value`：存储一个哈希对象，key哈希表名，field属性名称，value属性值
- `hget key field`：获取存储再hash中的值，根据field得到value
- `hmset key field value [field,value]..`：同时将多个field-value（域-值）对设置到哈希表key中
- `hgetall key`：获取哈希表所有信息
- `hkeys key`：获取所有哈希表中的字段
- `kvals <key>`：列出该hash集合的所有value
- `hlen key`：获取哈希表中字段的数量
- 删除语法：`hdel key field1[field2]`：删除一个或多个hash表字段
- `hsetnx field value`：只有在字段field不存在时，设置哈希表字段的值
- `hincrby key field increment`：指定字段自增increment量
- `hincrbyfloat key field increment`：为哈希表key中的指定字段的浮点数值加上增量increment
- `hexists key field`：查看哈希表key中，指定的字段是否存在

3. hash的应用场景：用于存储一个用户信息对象数据

- 常用于存储一个对象
- 为什么不用string存储一个对象呢？因为hash更存储对象操作简单

#### 列表list

1. list特点：
   - 单键多值
   - redis列表是简单的字符串链表，按照插入顺序排序，可以往列表的头部尾部添加元素
   - 它的底层实际上是双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会比较差
   - 适合对数据量大的集合数据删减
   - （队列或栈，可以使用）【分页、列表数据显示、热点新闻等，队列可以用在用户系统登录注册短信、订单系统的下单流程等】
2. 指令

- - **`lpush/rpush <key> <value1><value2>...`**

  - - 从左边/右边插入一个或多个值

  - **`lpop/rpop <key>`**

  - - 从左边/右边吐出一个值
    - 值在健在，值光键忘，我大德玛

  - `rpoplpush <key1> <key2>`

  - - 从`<key1>`列表右边吐出一个·值，插到`<key2>`列表左边

  - **`lrange <key> <start> <stop>`**

  - - 按照索引下标获取元素（从左到右）

  - `lindex <key> <index>`

  - - 按照索引下标获取元素（从左到右）

  - **`llen <key>`**

  - - 获取列表长度

  - `linsert <key> before <value> <newvalue>`

  - - 在`<value>`得后面插入`<newvalue>`的值

  - `lrem <key> <n> <value>`

  - - 取值为正数，从左边删除n个value（从左到右）
    - 取值为负数，从右边删除n个value（从右到左）
    - 取值为0，将列表中符合value的值全部删除

  - **`ltrim <key> <start> <stop>`：对一个列表进行修建（trim），就是说，让泪飙只保留指定区间内的元素，不在指定区间之内的元素将被删除**

  - `lset <key> <index> <value>`:

  - - 修改key中位值为index的值

3. 使用场景：作为队列或栈；分页、列表数据显示、热点新闻等，队列可以用在用户系统登录注册短信、订单系统的下单流程等。

#### 集合set

1. 特点：常用于实现共同关注，共同喜好，二度好友等功能；利用唯一性，可以哦统计访问网站的所有独立IP。（唯一，数据无序，自动排重，与list近似）：redis的set是string类型的无序集合。底层是一张value为null的hash表，所以添加、删除、查找的复杂度为O（1）
2. 指令

- - **`sadd <key> <value1> <value2>`**

  - - 将一个或多个member元素添加到集合key当中，已存在于集合的member元素将被忽略

  - **`smembers <key>`**

  - - 去除该集合的所有值

  - **`sismember <key> <value>【很常用】`**

  - - 判断集合`<key>`是否含有该`<value>`值，有返回1，没有返回0

  - **`scard <key>`**

  - - 返回该集合的元素个数（大小）

  - **`srem <key> <value1> <value2>`**

  - - 删除集合中的一个或多个元素

  - `spop <key> <n>`

  - - **随机**从该集合中吐出多个值，可以不写n

  - `smove <source> <destination> <member>`

  - - 将member元素从source集合移动到destination集合

  - `srandmember <key> <n>`【抽奖功能】

  - - **随机**从该集合中取出n个值
    - 不会从集合中删除

  - `sinter <key1> <key2>`

  - - 返回两个集合的交集元素
    - `sinterstore destination key1 [key2]`返回给定所有集合的交集并储存在destination集合中

  - `sunion<key1> <key2>`

  - - 返回两个集合的并集元素
    - `sunionstore destination key1  [key2]`返回给定所有集合的并集并存储在destination集合中

  - `sdiff <key1> <key2>`

  - - 返回两个集合的差集元素
    - `sdiffstore destination key1 [key2]` 返回给定结合的差集并存储在destination集合中

#### 有序集合zset （sorted set）

1. 特点：redis有序集合zset与普通集合set非常相似，是一个没有重复元素的字符串集合，不同之处是有序结合的所有成员都关联了一个评分（double类型），这个评分被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但评分可以是重复的。

2. 应用场景：排行榜

3. 指令

- - **`zadd <key> <score1> <value1> <score2>       <value2>...`**

  - - 将一个或多个member元素及其socre值加入到有序集key当中

  - **`zcard <key>`**

  - - 获取有序集合的成员数

  - **`zrange <key> <start> <stop> [WITHSCORES]`**

  - - 返回有序集key中，下标在`<start>`-`<stop>` 之间的元素
    - 带WITHSCORES，可以让分数一起和值返回到结果集。

  - **`zrangebyscore key min max [withscores] [limit offset count]`**

  - - 返回有序集key中，所有socre值介于min和max之间（包括等于min或max）的成员。有序集成员按socre值递增（从小到大）次序排列。

  - `zrevrangebyscore key max min [withscores] [limit offset count]`

  - - 同上，改为从大到小排序

  - `zincrby <key> <increment> <value>`

  - - 为元素的score加上增量

  - `zrem <key> <value>`

  - - 删除该集合下，指定值的元素

  - `zramrangebyrank key start end`

  - - 根据排名（下标）删除key中的值

  - `zremrangebyscore key score min max`

  - - 删除分数为min到max里的值

  - `zcount <key> <min> <max>`

  - - 统计该集合，分数区间内的元素个数

  - **`zrank <key> <value>`**

  - - 返回该值在集合中的排名，从0开始

### 使用Java操作redis

#### 普通jedis连接

```java
Jedis jedis = new Jedis("39.108.156.64", 6379);
jedis.auth("1997022691");
```

测试：`jedis.ping();//返回pong`

#### Jedis连接池配置

```java
//1创建连接池Redis POLL 基本配置信息

JedisPoolConfig poolConfig = new JedisPoolConfig();

poolConfig.setMaxTotal(5);//最大连接数

poolConfig.setMaxIdle(1);//最大空闲数

//...其他配置

//2.连接池

String host = "39.108.156.64";

int port = 6379;

JedisPool pool = new JedisPool(poolConfig, host, port);//这是连接池

//获取jedis

Jedis jedis = pool.getResource();//获取jedis

jedis.auth("1997022691");//认证

jedis.close();关闭jedis
```

### Redis发布订阅

1.订阅：

- `subscribe channel [channel..]`

- - 订阅给定的一个或多个频道的信息

- `psubscribe pattern [pattern...]`

- - 订阅一个或多个符合给定模式的频道

2.发布消息：

- `publish channel message`

- - 将信息发送到指定的频道

3.退订频道：

- `unsubscribe channel [channel...]`

- - 指定退订给定的频道

- `punsubcribe [pattern] [pattern...]`

- - 退订所有给定模式的频道

4.应用场景：构建实时消息系统，比如说普通**即时聊天**、**群聊**等功能

（1）在一个博客网站中，有100个粉丝订阅了你，当你发布新文章，就可以推送消息给粉丝

（2）微信公众号模式

### Redis多数据库

Redis下，数据库是由一个整数索引标识，而不是由一个数据库名称，默认下，一个客户端连接到数据库0

- redis配置文件中下面的参数来控制数据库总数

  如：`database 16`//（从0开始1 2 .. 15）

- 数据库清空：

  `flushdb`   //清除当前数据库的所有key

  `flushall` //清除整个redis的数据库的所有key

- 切换数据库：`select db`

  例子 `select 1`，即切换到数据库1

### redis事务

Redis事务可以一次执行多个命令，（按顺序地串行化执行，执行中不会被其它命令插入，不许加塞）

特点：

- Redis会将一个事务中地所有命令序列化，然后按顺序执行
- 执行中不会被其他命令插入，不许出现加塞行为

Redis事务命令：

- `discard`

- - 取消事务，放弃执行事务块内的所有命令

- `exec`

- - 执行所有事务块内的命令

- `multi`

- - 标记一个事务块的开始

- `unwatch`

- - 取消watch命令对所有key的监视

- `watch key [key....]`

- - 监视一个（或多个）key，如果在事务执行之前这个（或这些）key被其他命令所改动，那么事务将被打断

```shell
#一个事物的例子：A向B转账50元
set account:a 80
set account:b 10
#开启事务
mutil

get account:a
get account:b

#a减去50元，b增加50元
decrby account:a 50
incrby account:b 50

#执行事务
exec    
```

输入multi命令开始，输入的命令都会依次进入命令队列中，但不会执行；直到输入exec后，redis会将之前的命令队列中的命令依次执行

- 事务的错误处理：（数值错误，不会回滚）

  如果执行的某个命令报出了错误，则只有报错的命令不会被执行，而其它的命令都会被执行，不会回滚

- 事物的错误处理：（语法出错， 才会回滚）

  队列中的某个敏玲出现报告错误，执行时整个的所有队列都会被取消

给事务加写锁：

`watch key [key...]`

监视一个（或多个）key， 如果在事务执行之前这个（或这些）key被其它命令所改动，那么事务将被打断。

例子:

```shell
watch account:b
multi
get account:b
decrby account:b 10
incrby account:a 10
exec
```

即如果事务中key被其它客户端修改的话，那么事务会被销毁，不执行

`unwatch key`：取消监视key

**事务应用场景：**

- 一组事务必须同时都执行，或者都不执行。

- 我们想要保证一组命令在执行得过程之中不被其它命令插入

### redis持久化

当内存不足时，redis会根据配置的缓存策略淘汰部分keys，以保证写入成功，当无淘汰策略时或灭有找到适合淘汰得key时，redis直接返回out of memory错误。

redis提供6种数据淘汰策略：

- volatile-lru：从已设置国企时间的数据集种挑选最近最少使用得数据淘汰
- volatile-lfu：从已设置过期得key种，删除一段时间内使用次数最少使用得
- volatilettl：从已设置过期时间得数据集中挑选最近将要过期的数据淘汰
- volatile-random：从已设置过期时间的数据集中随机选择数据淘汰
- allkeys-lru：从所有数据集中随机选择数据淘汰
- allkeys-ifu：congsuoyoukeys中，删除一段时间内使用次数为最少使用的
- allkeys-random：从数据集中随机选择数据淘汰
- no-enviction（驱逐）：禁止驱逐数据（不采用任何淘汰策略，默认即为此策略），针对写操作，返回错误信息

建议：了解了Redis的淘汰策略之后，在平时使用时尽量主动设置/更新key的expire时间，主动剔除不活跃的旧数据，有助于提升查询性能

#### RDB持久化：

简介：是redis的默认持久化机制，相当于照快照，保存的是一种状态。

RDB是默认的持久化方式。这种方式是将内存中数据以快照的方式写入二进制文件中，默认的文件名为dump.rdb

**优点**：

- 快照保存数据极快，还原数据极快

- 适用于灾难备份

**缺点**：

- 小内存机器不适合使用，RDB机制符合要求就会照快照

**快照条件**：

- 服务器正常关闭时  redis-cli shutdown

- key满足一定条件时，会进行快照



使用vim redis.conf打开配置文件后搜索save

save 900 1  //没900秒（15分钟）至少1个key发生变化，产生快照

save 300 10  //每300秒（5分钟）至少10个key发生变化，产生快照

save 60 10000  //没60秒（1分钟）至少10000个key发生变化，产生快照

#### AOF持久化：

由于快照方式是在一定间隔时间做一次的，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改。如果应用要求不能丢失任何修改的话，可以采用aof持久化方式。

Append-only 比快照方式更好的持久化性，是由于在使用aof持久化方式时，redis会将每一个收到的写命令都通过write函数追加到文件中（默认是appendonly.aof）。当redis重启时会通过重新执行文件中保存的写命令来在内存中重建整个数据库的内容

有三种方式如下（默认是：每秒fsync一次）

- appendonly yes #启用aof持久化方式
- \#appendfsync      always   #收到写命令就立即写入磁盘，最慢，但是保证完全的持久化
- appendfsync everysec      #每秒钟写入磁盘一次，在性能和持久化方面做了很好的折中
- \#appendfsync no      #完全依赖os，性能最好，持久化没保证

产生的问题：

aof的方式也同时带来了另一个问题。持久化文件会变得越来越大，例如我们调用`incr test`命令100次，文件中必须保存全部的100条命令，其实有99条都是多余的

**现在4.0以上可以使用RDB-AOF混合模式**

### Redis缓存与数据库一致性

1.解决方案

- 实时同步（如果数据修改了，缓存也要修改；修改缓存数据或者让缓存key消失）

- - 对强一致性要求比较高的，应采用实时同步方案，即查询缓存查询不到不再从DB中查询，而是将空结果保存到缓存；更新缓存时，先更新数据库，再将缓存设置过期（建议不要去更新缓存内容，直接设置缓存过期）

  - - @Cacheable：查询时使用，注意Long类型需转换为String类型，否则会抛出异常
    - @CachePut：更新时使用，使用此注解，一定会从DB上查询数据
    - @CacheEvict：删除时使用；
    - @Chaing：组合用法

  - 缓存穿透：缓存穿透是指查询一个一定不存在的数据，由于缓存是不命中时需要从数据库查询，查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到数据库去查询，造成缓存穿透。

  - - 解决办法：**持久层查询不到就缓存空结果**，查询时先判断缓存中是否exists（key），如果有直接返回空，没有则查询后返回。

- 异步队列

- - 对于并发程度较高的，可采用异步队列的方式同步，可采用kafka等消息中间件处消息生产和消费。

- 使用阿里的同步工具canal（读写分离）

- - canal实现方式是模拟mysql       slave和master的同步机制

- 采用UDF自定义函数的方式

- - 面对mysql的API进行编程，利用触发器进行缓存同步，但UDF主要是c/c++语言实现，学习成本较高

缓存雪崩：缓存集中在一段时间失效。解决方法：添加随机事件

### Redis集群相关

#### 高并发

提高系统并发能力的方式，方法论上主要有两种：垂直扩展（Scale Up）与水平扩展（Scale Out）。

- 垂直扩展

- - 提升单机处理能力，扩展的方式又有两种：

  - - 增强单机硬件性能
    - 提高单机架构性能

  - 水平扩展（现阶段主要选择）

  - - 增加服务器数量

#### Redis主从复制

概念：一个Redis服务可以有多个该服务的复制品，这个Redis服务称为Master（主库），其它复制称为Slaves（从库）。主库只负责写数据，每次有数据更新都将更新的数据同步到它所有的从库，而从库只负责读数据，这样一来有了两个好处：

- 读写分离，不仅可以提高服务器的负载能力，并且可以根据读请求的规模自由增加或者减少从库的数量。
- 数据被复制成好几份；就算有一台机器出现故障，也可以使用其它机器的数据快速恢复。

Redis主从复制配置：

在Redis中，只需要在从数据库的配置文件中添加如下命令即可：

1.主数据库不需要任务配置，创建一个从数据库：

`--port 6380`  //从服务器的端口

`--slaveof 127.0.0.1 6379`//指定主服务器

开启：`./bin/redis-server ./redis.conf -port 6380 -slaveof 127.0.0.1 6379`

加上slaveof参数启动另一个Redis实例作为从库，并且监听6380端口

2.登录到从服务客户端

`./bin/redis-cli -p 6380 -a password`

 

#### 集群：（增加服务器数量）

Redis集群搭建的方式有很多种，但从redis3.0之后版本支持redis-cluster集群，至少需要3（master）+3（Slave）才能建立集群。Redis-Cluster采用无中心结构，每个节点保存数据和整个集群的状态，每个节点都和其他所有节点连接。

Redis Cluster集群特点：

- 所有redis节点彼此互联（PING-PONG机制），内部使用二进制协议优化传输速度和带宽。
- 节点的fail是通过集群中超过半数的节点检测失效时才生效。
- 客户端与redis节点直连，不需要中间proxy层，客户端不需要连接集群所有节点，连接集群中任何一个可用节点即可。
- redis-cluster把所有的物理节点映射到[0-16383]slot上（不一定是平均分配），cluster复制维护
- redis集群预先分好16384个哈希槽，当需要在redis集群中放置一个key-value时，redis先对key使用crc16算法算出一个结果，然后把结果对16384求余数，这样每个key都会对应一个编号在0-16384之间的哈希槽，redis会根据节点数据量大致均等的将哈希槽映射到不同的节点。

 

Redis Cluster容错

容错性：是指如那件检测应用程序所运行的软件或硬件中发生的错误中恢复的能力，通常可以从系统的可靠性、可用性、可测性等几个方面来衡量。

redis-cluster投票：容错

1.投票过程是集群中所有master参与，如果半数以上master节点与master节点通信超时(cluster-node-timeout)，认为当前mster节点挂掉。

2.什么时候整个集群不可用？(cluster_state:fail)

​    如果集群任意master挂掉，且当前master没有slave。集群进入fail状态，也可以理解成集群的slot映射[0-16383]不完整时进入fail状态。如果集群超过半数以上master挂掉，无论是否有slave，集群进入fail状态。

 

#### Redis Cluster集群搭建**（后续更新方法）**

简介：集群中至少应该有奇数个节点，所以集群至少需要3台主机，同时每个节点至少有一个备份节点，所以至少需要6台机器，才能完成redis cluster集群（主节点、备份节点由redis-cluster集群确定）

创建假的redis集群。。的步骤：（一台机器，使用六个端口）



## 知识补充：

### 使用Pipeline的好处

- Pipeline和Linux的管道类似
- Redis基于请求/响应模型，单个请求处理需要一一应答
- Pipeline批量执行指令，节省多次IO往返时间
- 有顺序依赖的指令建议分批发送

### Redis的同步机制

主从同步原理：读写分离，没有爸爸机器，即所有机器都是平等的

全同步过程（salve是从数据库，master是主数据库）

- Salve发送sync命令到Master
- Master启动一个后台进程，将Redis中的数据快照保存到文件中
- Master将保存数据快照期间接受的写命令缓存起来
- Master完成写文件操作后，将该文件发送给Salve
- 使用新的AOF文件替换掉旧的AOF文件
- Master将这期间手机的增量写命令发送到Salve端

增量同步过程：

- Master接收到用户的操作指令，判断是否需要传播到Salve
- 将操作记录追加到AOF文件
- 将操作传播到其他Slave：1、对齐主从库；2、往响应缓存写入指令
- 将缓存中的数据发送给Slave

### Redis Sentinel（哨兵）【不常用，常用集群】

解决主从同步Master宕机后的主从切换问题：

- 监控：检查主从服务器是否运行正常
- 提醒：通过API向管理员或者其他应用程序发送故障通知
- 自动故障迁移：主从切换

### Redis集群原理：（后续更新）