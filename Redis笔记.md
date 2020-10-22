# Redis

## 1、命令

- 切换数据库
  - select 1  （0 1 2 3 ...）
- 删除当前库中所有的key
  - flushdb
- 删除所有数据库中的key
  - flushall

## 2、五种数据结构

![redis五种核心数据结构图](F:\studynote\StudyNotes\assets\redis五种核心数据结构图.png)

### 2.1、String结构

#### 1、字符串常用操作：

- set  key  value  //存入字符串键值对
- mset  key  value  [key  value  ...]  //批量存储字符串键值对
- setnx  key  value  //存入一个不存在的字符串键值对
- get  key  //获取一个字符串
- mget  key  [key  ...]  //批量获取字符串
- del  key  //删除一个键
- expire  key  seconds  //设置一个键的过期时间

#### 2、原子加减

- incr  key  //将key中存储的数字+1
- decr  key  //将key存储的数字-1
- incrby  key  increment  //将key所存储的值加上increment
- decrby  key  increment  //将key所存储的值减去increment

### 2.2、String应用场景

#### 1、单值缓存

- set  key  value
- get  key

#### 2、对象缓存

- set  user:id  value(json数据格式)
- mset  user:id:name  zhangsan  user:id:age  18  user:id:address  北京
- mget  user:id:name   user:id:age

#### 3、分布式锁

```shell
线程1：setnx  product:1001  true  //返回1代表取锁成功
线程2：setnx  product:1001  true  //返回0代表取锁失败
。。。执行业务操作
del  product:1001  //执行完业务释放锁

set  product:1001 true  ex  10  nx  //设置key的过期时间，防止程序意外死锁
```

#### 4、计数器

- incr  article:readcount:id  //对于文章的点赞数或者朋友圈点赞之类的操作需要原子加减
- get  article:readcount:id  //

#### 5、web集群session共享

- spring  session  +  redis实现session共享

#### 6、分布式系统全局序列号

- incrby  orderId  1000  //redis批量生成序列号，唯一不重复，也可以使用雪花算法实现

### 2.3、Hash常用操作

- hset  key  field  value  //存储一个哈希表key的键值对
- hget  key  field  //获取哈希表中key对应的filed键值
- hsetnx  key  field  value  //存储一个不存在的哈希表key的键值对
- hmset  key  field  value  [field  value  ...]  //在一个哈希表key中批量存储键值对
- hmget  key  field  [field  ...]  //批量获取哈希表中key对应的多个键值对
- hdel  key  field  [field  ...]  //批量删除哈希表中field键值
- hlen  key  //返回哈希表key中的field键值数量
- hgetall  key  //返回哈希表中所有的键值对
- hincrby  key  field  increment  //为哈希表key中的field键加上增量increment

### 2.4、Hash应用场景

#### 1.电商购物车

- 以用户id为key
- 商品id为field
- 商品数量为value

#### 2.购物操作

- 添加商品：hset  cart:1001  10088  1
- 增加数量：hincrby  cart:1001  10088  1
- 商品总数：hlen  cart:1001
- 删除商品： hdel  cart:1001  10088
- 获取购物车的所有商品：hgetall  cart:1001

### 2.5、Hash结构优缺点

#### 1.优点

- 同类数据归类整合存储，方便数据管理
- 相比string操作消耗内存与cpu更小
- 相比string存储更节省空间

#### 2.缺点

- 过期功能不能使用在field上，只能用在key上
- redis集群架构下不适合大规模使用

### 2.6、List常用操作

- lpush  key  value  [value ...]  //将一个或多个值value插入到key列表的表头（最左边）
- rpush  key  value  [value ...]  //将一个或多个值value插入到key列表的表尾（最右边）
- lpop  key  //移除并返回key列表的头元素
- rpop  key  //移除并返回key列表的尾元素
- lrange  key  start  stop  //返回列表key中指定区间内的元素，区间以偏移量start和stop指定
- blpop  key  [key  ...]  timeout  //从key列表表头弹出一个元素，若列表中没有元素，阻塞等待timeout秒，如果timeout为0，则一直阻塞等待
- brpop  key  [key  ...]  timeout  //从key列表表尾弹出一个元素，若列表中没有元素，阻塞等待timeout秒，如果timeout为0，则一直阻塞等待
- ![image-20200816100435376](C:\Users\hacker\AppData\Roaming\Typora\typora-user-images\image-20200816100435376.png)

### 2.7、应用场景

- Stack（栈）= lpush  +  lpop  --> filo
- Queue（队列）= lpush  +  rpop 
- Blocking MQ（阻塞队列）= lpush  +  brpop

#### 1.微博和微信公众号信息流，订阅号消息

微博消息和微信公众号消息

- 小明关注了MacTalk，备胎说车等大V
  - MacTalk发微博，消息ID为10086
    - lpush  msg:id  10086	
  - 备胎说车发微博，消息ID为10088
    - lpush  msg:id  10088
  - 查看最新微博消息
    - lrange  msg:id  0  4

### 2.8、set常用操作

- sadd  key  member  [member  ...]  //往集合key中存放元素，元素存在则忽略
- srem  key  member  [member  ...]  //从集合key中删除元素
- smembers  key  //获取集合key中所有的元素
- scard  key  //获取集合key中元素个数
- sismember  key  member  //判断元素member是否存在于集合key中
- srandmember  key  [count]  //从集合key中随机选出count个元素，元素不从key中删除
- spop  key   //从集合key中选出count个元素，元素从key中删除

运算操作：

- sinter  key  [key  ...]  //交集运算
- sinterstore  destination  key  [key  ...]  //将交集结果存入新集合destination中
- sunion  key  [key  ...]  //并集运算
- sunionstore  destination  key  [key  ...]  //将并集结果存入新的集合destination中
- sdiff  key  [key  ...]  //差集运算
- sdiffstore  destination  key  [key  ...]  //将差集结果存入新的集合destination中

### 2.9、set应用场景

#### 1、微信抽奖小程序

- 点击参与抽奖加入集合
  - sadd  key  {userId}
- 查看参与抽奖的所用用户
  - smembers  key
- 抽取count名中奖者
  - srandmember  key  [count]  /  spop key

#### 2、微信微博点赞，收藏，标签

- 点赞
  - sadd  like:{消息Id}  {用户Id}
- 取消点赞
  - srem  like:{消息Id}  {用户Id}
- 检查用户是否点赞
  - sismember  like:{消息Id}  {用户Id}
- 获取点赞的用户列表
  - smembers  like:{消息Id}
- 获取点赞用户的数量
  - scard  like:{消息Id}

#### 3、集合操作实现微博微信关注模型

- 小明关注的人：
  - mingSet  -->  sadd  xiaohong   xiaolan  liuyifei
- 小红关注的人：
  - hongSet  -->  sadd  xiaoming  xiaolan  liuyifei  xiaolongnv
- 小兰关注的人：
  - lanSet  -->  sadd   xiaoming   xiaolongnv  liuyifei  yaoyao
- 小明和小红共同关注的人：
  - sinter  mingSet  hongSet   --->  {xiaolan   liuyifei}
- 我关注的人也关注她（liuyifei）：
  - sismember   lanSet  liuyifei
  - sismember   hongSet  liuyifei

### 2.10、zset常用操作

- zadd  key  score  member  [[score  member]   [score  member]  ...]  //将一个或多个 `member` 元素及其 `score` 值加入到有序集 `key` 当中。当member已存在则重新插入，score可以是整数或双精度浮点数
- zscore  key  member  //返回key中member的score值
- zincrby  key  increment  member  //为key中的member的score加上增量increment，不存在member则插入
- zcard  key  //返回集合key中元素的个数
- zcount  key  min  max  //返回集合key中在min与max之间元素的数量（-inf，+inf代表所有）
- zrange  key  start  stop  [withscores]  //返回key中指定区间的成员，按照score值从小到大排序，withscores返回对应的分数
- zrevrange  key  start  stop  [withscores]  //返回key中指定区间的成员，按照score值从大到小排序，withscores返回对应的分数
- zrangebyscore  key  min  max  [withscores] [limit  offset  count]  //返回集合key中，分数在指定区间的元素，按照score值从小到大排序，withscores返回对应的分数，limit分页
- zrevrangebyscore  key  min  max  [withscores] [kimit  offset  count]  //返回集合key中，分数在指定区间的元素，按照score值从大到小排序，withscores返回对应的分数，limit分页
- zrank  key  member  //返回集合key中member的排名，其中score值按照从小到大排列，排名从0开始
- zrevrank  key  member  //返回集合key中member的排名，其中score值按照从大到小排列，排名从0开始
- zrem  key  member  [member  ...]  //移除key中一个或多个member成员
- zremrangebyrank  key start stop  //移除集合key中指定排名区间内的所有成员
- zremrangebyscore  key  start stop  //移除集合key中指定分数区间内的所有成员
- zinterstore  destination  numkeys  key  [key ...]  [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]  //计算交集，其中给定 key 的数量必须以 numkeys 参数指定，新集合score 值是所有给定集下该成员 score 值之和. WEIGHTS为 每个 给定有序集 分别 指定一个乘法因子，默认为1.AGGREGATE以将所有集合中某个成员的 score 值之 和 作为结果集中该成员的 score 值
- zunionstore  destination  numkeys  key  [key ...]  [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]  //计算并集，可选参数含义同上

### 2.11、zset应用场景

#### 1、zset集合操作实现排行榜

- 点击新闻
  - zincrby  hotnews:20200819  1  守护香港
- 展示当日排行前十：
  - zrerange  hotnews:20200819  0  10  withscores
- 七日搜索榜单计算：
  - zunionstore hotnews:20200819-20200825 7 hotnews:20200819  hotnews:20200820 ...  hotnews:20200825
- 展示七日排行榜前十
  - zrevrange hotnews:20200819-20200825 0 9 withscores