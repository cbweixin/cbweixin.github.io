title: zookeeper-distributed-lock
date: 2021-05-09 20:16:26
categories: distributed
tags: zookeeper
---

# Analysis of an implementation of zookeeper distributed lock

## reference 
[link](https://www.cnblogs.com/ysw-go/p/11444993.html)

## the mechanism of zookeeper distributed lock
let's assume there are two clients try to get the zk's lock
![image1](https://user-images.githubusercontent.com/1326906/117601860-242f5780-b104-11eb-85ef-cffd3ac7ac05.png)
as we see in the graph, zk has a lock, this lock is actually a node in zk. 
assume client A try to get the lock. then it would try to create a EPHEMERAL_SEQUENTIALnode under this node. 

简单来说，就是直接在"my_lock"这个锁节点下，创建一个顺序节点，这个顺序节点有zk内部自行维护的一个节点序号。

比如说，第一个客户端来搞一个顺序节点，zk内部会给起个名字叫做：xxx-000001。然后第二个客户端来搞一个顺序节点，zk可能会起个名字叫做：xxx-000002。大家注意一下，最后一个数字都是依次递增的，从1开始逐次递增。zk会维护这个顺序。

所以这个时候，假如说客户端A先发起请求，就会搞出来一个顺序节点，大家看下面的图，Curator框架大概会弄成如下的样子：

![image2](https://user-images.githubusercontent.com/1326906/117602018-89834880-b104-11eb-8f05-ba94b33b16ea.png)
接着客户端A创建完一个顺序节点。还没完，他会查一下"my_lock"这个锁节点下的所有子节点，并且这些子节点是按照序号排序的，这个时候他大概会拿到这么一个集合：

![image3](https://user-images.githubusercontent.com/1326906/117602296-29d96d00-b105-11eb-945a-b3ca63921bdf.png)

接着客户端A会走一个关键性的判断，就是说：唉！兄弟，这个集合里，我创建的那个顺序节点，是不是排在第一个啊？

如果是的话，那我就可以加锁了啊！因为明明我就是第一个来创建顺序节点的人，所以我就是第一个尝试加分布式锁的人啊！

bingo！加锁成功！大家看下面的图，再来直观的感受一下整个过程。
![image4](https://user-images.githubusercontent.com/1326906/117602355-4c6b8600-b105-11eb-951f-655f20b105fe.png)

