# Redis

关系型数据库。内存缓存数据库。

### Redis数据结构

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

#### String

键值对. 通过set和get命令存取.

 redis 的 string 可以包含任何数据。比如jpg图片或者序列化的对象。

```mysql
redis 127.0.0.1:6379> SET name "runoob"
OK
redis 127.0.0.1:6379> GET name
"runoob"
```

Redis在string类型上会消耗较多内存，可以使用dict（hash表）压缩存储以降低内存耗用。

#### Hash

哈希表。在String的基础上再加上一层: 一个hash可以存多个String键值对( hash 是一个键值(key=>value)对集合。). get方法指定hash 和键值对名.

Redis在string类型上会消耗较多内存，可以使用dict（hash表）压缩存储以降低内存耗用。

```mysql
redis> HMSET myhash field1 "Hello" field2 "World"//myhash为一个hash数据结构, 其中存储有两个键值对.
redis> HGET myhash field1
"Hello"
redis> HGET myhash field2
"World"
redis> HKEYS myhash//列出所有键
redis> HVALS myhash// 列出所有值
redis> HGETALL myhash //会依次列出每个键和它所对应的值
redis> HDEL myhash field1 //从哈希表中删除指定的键值对。
```

##### 应用

```
存储、读取、修改用户属性
```

#### List

双向链表(不是键值对, 按照插入顺序编号). 头尾都可以添加元素.

```mysql
redis 127.0.0.1:6379> lpush runoob redis
redis 127.0.0.1:6379> lpush runoob mongodb
redis 127.0.0.1:6379> lpush runoob rabitmq
redis 127.0.0.1:6379> lrange runoob 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
```

runoob是一个list, 前三行使用lpush命令把三个元素插入到队列, 第四行列出runoob队列中的前0-10编号的元素.

##### 应用

```
1.微博 TimeLine
2.消息队列
```

#### Set集合

实现：小数据量- intset, 大数据量-字典

对于小集合使用intset来存储，主要的原因是节省内存。特别是当存储的元素个数较少的时候，dict所带来的内存开销要大得多（包含两个哈希表、链表指针以及大量的其它元数据）。所以，当存储大量的小集合而且集合元素都是数字的时候，用intset能节省下一笔可观的内存空间。

什么呢？实际上，key就是要添加的集合元素，而value是NULL。

集合. 无视重复添加的字符串.

```mysql
sadd set num1
sadd set num2
sadd set num2
smembers set
"num1"
"num2"
```

set为一个集合.

#### ZSet--Sorted Set有序集合

实现：HashMap + 压缩表/跳表

跳跃表按score从小到大保存所有集合元素。而字典则保存着从member到score的映射，这样就可以用O(1)的复杂度来查找member对应的score值。虽然同时使用两种结构，但它们会通过指针来共享相同元素的member和score，因此不会浪费额外的内存。

sorted set 有序集合. Sorted Sets是将 Set 中的元素增加了一个权重参数 score，使得集合中的元素能够按 score 进行有序排列。

一个存储全班同学成绩的 Sorted Sets，其集合 value 可以是同学的学号，而 score 就可以是其考试得分，这样在数据插入集合的时候，就已经进行了天然的排序。另外还可以用 Sorted Sets 来做带权重的队列，比如普通消息的 score 为1，重要消息的 score 为2，然后工作线程可以选择按 score 的倒序来获取工作任务。让重要的任务优先执行。

```
1.带有权重的元素，比如一个游戏的用户得分排行榜
2.比较复杂的数据结构，一般用到的场景不算太多？
```
