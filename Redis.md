# Redis

#### 小结：

redis单个key 存入512M大小

redis支持多种类型的数据结构(string,list,hash.set.zset)

redis 是单线程   原子性    

redis可以持久化  因为使用了 RDB和AOF机制  

redis支持集群   而且redis 支持库(0-15) 16个库 

redis 还可以做消息队列  比如聊天室  IM 

#### 优点：

1.丰富的数据结构；

2.高速读写，redis使用自己实现的分离器，代码量很短，没有使用lock（MySQL），因此效率非常高。

####  缺点：

1. 持久化。Redis直接将数据存储到内存中，要将数据保存到磁盘上，Redis可以使用两种方式实现持久化过程。定时快照（snapshot）：每隔一段时间将整个数据库写到磁盘上，每次均是写全部数据，代价非常高。第二种方式基于语句追加（aof）：只追踪变化的数据，但是追加的log可能过大，同时所有的操作均重新执行一遍，回复速度慢。 
2. 耗内存，占用内存过高。 

#### 配置文件：

配置文件为redis.conf  ！！！

1. Redis默认**不是**以**守护进程**的方式运行，可以通过该配置项修改，使用**yes启用守护进程**

​    **daemonize no**

2. 当Redis以守护进程方式运行时，Redis默认会把**pid**写入**/var/run/redis.pid**文件，可以**通过pidfile指定**

​    **pidfile /var/run/redis.pid**

3. 指定Redis监听端口，默认端口为6379，

​    **port 6379**

4. 绑定的**主机地址**，若启用，则只有本机可以访问

​    bind 127.0.0.1

   5.当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能

​    timeout 300

6. 指定日志记录级别，Redis总共支持四个级别：**debug**、**verbose**、**notice**、**warning**，默认为verbose

​    **loglevel verbose**

9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合

​    **save <seconds> <changes>**

​    Redis默认配置文件中提供了三个条件：

​    save 900 1

​    save 300 10

​    save 60 10000

​    分别表示**900秒（15分钟）内有1个更改**，**300秒（5分钟）内有10个**更改以及**60秒内有10000个**更改

11. 指定本地数据库文件名，**默认值为dump.rdb**

​    dbfilename dump.rdb

15. 设置Redis连接密码，如果配置了连接密码，客户端在**连接Redis时需要通过AUTH <password>命令提供密码，默认关闭**

​    **requirepass foobared**



#### 常用命令：

**DEL KEY:**该命令用于在key存在时删除key。删除成功返回1，删除失败返回0；

**DUMP KEY:**序列化指定的key，并返回序列化的值；

**EXISTS KEY:**检查指定key是否存在。存在返回1，不存在返回0；

**EXPIRE KEY SECONDS:**为key设置过期时间 （ttl key 可以查询key的有效期，-1代表永久）;

**PERSIST KEY:**移除key的过期时间，将key持久保持;

**RENAME OLDNAME NEWNAME:**重命名key；

**MOVE KEY db:**将key移动到指定的db中；

**TYPE KEY :**返回key所存储的值的类型。



#### key的命名建议：

1.key不要太长，尽量不要超过1024字节，这样会很消耗内存，并且降低查询效率；

2.key也不要太短，否则可读性很差；

3.在一个项目中使用统一的命名方式；（例如user:123:password）;

4.key的名称区分大小写。



#### String类型简介：

String是redis的基本类型，一个key对应一个value;

String类型是二进制安全的，可以包含jpg图像。

#### String命令：

**SET key_name VALUE**  给key赋值，重复写则覆盖值；

**SETNX key_name VALUE**   只有key不存在时设置key的值为value；

**GET key_name** 不存在时返回nil，若key存储的值不是String类型，则返回错误；

**GETRANGE key_name STRAT END** 字符串的截取，闭合区间；

**GETSET   key_name VALUE**  返回key原来的值，并赋新值;

**STRLEN key_name**  返回key所存储的字符串的长度；

**DEL key_name** 删除指定key;

**INCR key_name**   执行自增，若初始为空，则自动从0开始加

**INCRBY key_name 增量**   按增量递增；

**DECR key_name**  

**DECR key_name 减量**

#### 应用场景：

1.String通常用于保存单个字符串或者JSON数据；

2.因为String是二进制安全的，所以可以存图片；

3.计数器，因为INCR 等命令的特性。



