[redis中文网](http://www.redis.cn/)  

Redis是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。它支持多种类型的数据结构，如==字符串（String）==、==散列（hashes）==、==列表（lists）==、==集合（sets）==、==有序集合（sorted sets）==与范围查询，==bitmaps==，==hyperloglogs==和==地理空间（geospatial）==索引半径查询。Redis内置了==复制（replication）==，==LUA脚本（Luascripting）==，==LRU驱动事件（LRU eviction）==，==事务（transactions）==和不同级别的==磁盘持久化（persistence）==，并通过==Redis哨兵（Sentinel）==和自动==分区（Cluster）==提供高可用性（high availability）。  

# Redis数据类型介绍
你也许已经知道Redis并不是简单的key-value存储，实际上它是一个数据结构服务器，支持不同类型的值。也就是说，你不必仅仅把字符串当作键所指向的值。下列这些数据类型都可作为值类型：  
- 二进制安全的字符串
- Lists：按插入顺序排序的字符串元素的集合。它们基本上就是链表（linked lists）。
- Sets：不重复且无序的字符串元素的集合。
- Sorted sets，类似Sets，但是每个字符串回元素都关联到一个叫score浮动数值（floating number value）。里面的元素总是通过Score进行着排序，所以不同的是，它是可以检索的一系列元素。
- Hashes，由field和关联的value组成的map。field和value都是字符串的。这和Ruby、Python的hashes很想。
- Bit arrays（或者说simply bitmaps）：通过特殊的命令，你可以将String值当作一些列bits处理：可以设置和清楚单独的bits，数出所有设为1的bits的数量，找到最前的被设为1或0的bit，等等
- HyperLogLogs：这是被用于估计一个set中元素数量的概率性的数据结构。

## Redis keys
Redis key值是二进制安全的，这意味着可以用任何二进制序列作为key值，从形如“foo”的简单字符串到一个JPEG文件的内容都可以。空字符串也是有效key值。  
关于key的几条规则：  
- 太长的键值不是个好主意，例如1024字节的键值就不是个好主意，不仅因为消耗内存，而且在数据中查找这类键值的计算成本很高。
- 太短的键值通常也不是好主意，如果你要用“u:1000:pwd”来代替“user:1000:password”，这没有什么问题，但后者更易阅读，并且由此增加的空间消耗相对于key object和value object本身来说很小。当然，没人组织您一定要用更短的键值节省一定点儿空间
- 最好坚持一种模式。例如：“object-type:id:field”就是个不错的主意。

## Redis Strings
这是最简单Redis类型。如果你只用这种类型，Redis就像一个可以持久化的memcached服务器（注：memcache的数据进保存在内存中，服务器重启后，数据将丢失）。  
我们用redis-cli来玩一下字符串类型：  
```
> set mykey somvalue
OK
> get mykey
"somevalue"
```
正如吗你所见到的，通常用SET command和GET command来设置和获取字符串值。  
值可以是任何种类的字符串（包括二进制数据），例如你可以在一个键下保存一副jpeg图片。值的长度不能超过512MB。  
SET命令有些有趣的操作，例如，当key存在时SET会失败，或相反的，当key不存在时它只会成功。  
```
> set mykey newval nx
(nil)
> set mykey newval xx
OK
```
虽然字符串是Redis的基本值类型，但你仍然能通过它完成一些有趣的操作。例如：原子递增：  
```
> set counter 100
OK
> incr counter
(integer) 101
> incr counter
(integer) 102
> incr counter 50
(integer) 152
```
INCR命令将字符串值解析成整形，将其加一，最后将结果保存为新的字符串值，类似的命令有INCRBY，DECR和DECRBY。实际上它们在内部就是同一个命令，只是看上去有点不同。  
INCR是原子操作意味着什么呢？就是说即使多个客户端对同一个key发出INCR命令，也绝不会导致竞争的情况。例如如下情况永远不可能发生：【客户端1和客户端2同时独处“10”，他们俩都会其加到11，然后将新值设置为11】.最终的值一定是12，read-increment-set操作完成时，其它客户端不会在同一事件执行任何命令。  
对字符串，另一个的令人感兴趣的操作时GETSET命令，行如其名：它为key设置新值并且返回原值。这有什么用处呢？例如：你的系统每当有新用户访问时就用INCR命令操作一个Redis key。你希望每小时对这个信息收集一次。你就可以GETSET这个key并给其复制0并读取原值。  
为减少等待事件，也可以一次存储或获取多个key对应的值，使用MSET和MGET命令：  
```
> mset a 10 b 20 c 30
OK
> mget a b c 
1) "10"
2) "20"
3) "30"
```
MGET命令返回值组成的数组。  
## 修改或查询键空间
有些执行不是针对任何具体的类型定义的，而是用于和整个见空间交互的。因此，它们可被用于任何类型的键。  
使用EXISTS命令返回1或0标识给定key的值是否存在，使用DEL命令可以删除key对应的值，DEL命令返回1或0标识值是被删除（值存在）或没被删除（key对应的值不存在）。  
```
> set mykey hello
OK
> exists mykey
(integer) 1
> del mykey
(integer) 1
> exists mykey
(integer) 0
```
