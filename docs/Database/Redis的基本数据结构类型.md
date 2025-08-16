Redis有以下这五种基本类型：

- String（字符串）
- Hash（哈希）
- List（列表）
- Set（集合）
- zset（有序集合）

还有三种特殊的数据结构类型：

- Geospatial
- Hyperloglog
- Bitmap

![](https://cdn.nlark.com/yuque/0/2023/webp/35016921/1675906919613-d04dbade-541d-49aa-ac84-bda56ba8e7b0.webp#averageHue=%23f7f7f7&clientId=ueee50c78-d297-4&from=paste&id=u65f50c95&originHeight=647&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4666e5b9-7bfe-42c2-a48c-628388830a8&title=)

![](https://cdn.nlark.com/yuque/0/2023/webp/35016921/1675927378160-66610797-6c40-444d-8ed9-a4da7073e7c7.webp#averageHue=%23f3f1ea&clientId=u3f6750cb-7c88-4&from=paste&id=uf82e3a14&originHeight=384&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u684f6cb0-350d-40ac-bb57-fffec00ed54&title=)
#### String（字符串）

- 简介：String是Redis最基础的数据结构类型，它是二进制安全的，可以存储图片或者序列化的对象，值最大存储为512M
- 简单使用举例：set key value、get key
- 应用场景：共享session、分布式锁，计数器、限流。
- 内部编码有3种：int(8字节长整型)、embstr(小于等于39字节字符串)、raw(大于39个字节字符串)

Redis的字符串使用SDS(simple dynamic string)封装，源码如下：
```c
struct sdshdr{
    unsigned int len; //标记buf的长度
    unsigned int free; //标记buf中未使用的元素个数
    char buf[]; //存放元素的值
}
```
![](https://cdn.nlark.com/yuque/0/2023/webp/35016921/1675910547524-80ae349a-c621-44c4-b695-02626d1554af.webp#averageHue=%23f8f7f3&clientId=ueee50c78-d297-4&from=paste&id=uce78a3ea&originHeight=269&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u19cbc379-3fcc-413a-bb18-ce6785e8f65&title=)

#### Hash（哈希）

- 简介：在Redis中，哈希类型是指值本身是一个键值对（k-v）结构
- 简单使用举例：hset key field value 、hget key field
- 内部编码：ziplist（压缩列表） 、hashtable（哈希表）
- 应用场景：缓存用户信息等。
- **注意点**：如果开发使用hgetall，哈希元素比较多的话，可能导致Redis阻塞，可以使用hscan。而如果只是获取部分field，建议使用hmget。

![](https://cdn.nlark.com/yuque/0/2023/webp/35016921/1675910814254-ce8e7506-f876-41b3-8af8-9ddf049d0f5c.webp#averageHue=%23faf7f0&clientId=ueee50c78-d297-4&from=paste&id=u8fe3f00f&originHeight=611&originWidth=1065&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ufd74f158-e9f3-4fc5-a0cb-589d4377699&title=)

#### List（列表）

- 简介：列表（list）类型是用来存储多个有序的字符串，一个列表最多可以存储2^32-1个元素。
- 简单实用举例：lpush key value [value ...] 、lrange key start end
- 内部编码：ziplist（压缩列表）、linkedlist（链表）
- 应用场景：消息队列，文章列表,

![](https://cdn.nlark.com/yuque/0/2023/webp/35016921/1675910899158-15014c4d-5d73-4ac9-80be-643259522eea.webp#averageHue=%23fafafa&clientId=ueee50c78-d297-4&from=paste&id=u99d65853&originHeight=269&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u013611b2-d21c-4845-965f-32020c9e98e&title=)
> list应用场景参考以下：

lpush+lpop=Stack（栈）
lpush+rpop=Queue（队列）
lpsh+ltrim=Capped Collection（有限集合）
lpush+brpop=Message Queue（消息队列）
#### Set（集合）

- 简介：集合（set）类型也是用来保存多个的字符串元素，但是不允许重复元素
- 简单使用举例：sadd key element [element ...]、smembers key
- 内部编码：intset（整数集合）、hashtable（哈希表）
- **注意点**：smembers和lrange、hgetall都属于比较重的命令，如果元素过多存在阻塞Redis的可能性，可以使用sscan来完成。
- 应用场景：用户标签，生成随机数抽奖、社交需求。

![](https://cdn.nlark.com/yuque/0/2023/webp/35016921/1675911037754-54c9c000-c5ae-4682-9664-c4a009405cdc.webp#averageHue=%23faf8f2&clientId=ueee50c78-d297-4&from=paste&id=u1974768e&originHeight=411&originWidth=1073&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u52c03c6c-dceb-4707-a463-5f678534509&title=)

#### Zset（有序集合）

- 简介：已排序的字符串集合，同时元素不能重复
- 简单格式举例：zadd key score member [score member ...]，zrank key member
- 底层内部编码：ziplist（压缩列表）、skiplist（跳跃表）
- 应用场景：排行榜，社交需求（如用户点赞）。
#### 三种特殊类型

- Geo：Redis3.2推出的，地理位置定位，用于存储地理位置信息，并对存储的信息进行操作。
- HyperLogLog：用来做基数统计算法的数据结构，如统计网站的UV。
- Bitmaps：用一个比特位来映射某个元素的状态，在Redis中，它的底层是基于字符串类型实现的，可以把bitmaps成作一个以比特位为单位的数组
