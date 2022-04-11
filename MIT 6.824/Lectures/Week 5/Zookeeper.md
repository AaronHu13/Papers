### Zookeeper

1. Linearizability：put all read/write request, check whether there is cicle or not

2. Client retransimission will not reset first request time

3. Why ZK?

   1. how to design API for general purpose(coordinate service) and others can use them to build some service

   2. whether n times servers can give n times better performance

      That dependends on how you use these servers

4. ZK not provide linearizability

5. ZK guarantees

   1. Linearizable writes

   2. FIFO client order
      1. Write-client specified order
      2. Read--use zxid to avoid stale data
      
   3. How does ZooKeeper guarantee the Linearizable writes and FIFO client order?????

      If its implementation is based on Raft or some other backup model, then it can use 2 RTTs request and response to make sure the writes is linearizable; for FIFO client order, it's showed in above

   4. ZooKeeper can read stale values

6. Configuration change/replace: "ready" + watch

7. What if ZooKeeper crashes?

8. ZNode type

   1. regular
   2. ephemeral
   3. SEQ->Never repeat given number?

9. APIs

   1. Create-->Exclusive, so if a file already exists, then this function call would fail
   2. Delete
   3. exisis

10. ZooKeeper guarantee that reads will be processed exactly between two writes and after writes

11. 分布式锁的问题：

    当一个程序或者节点进入到临界区之后，上一个执行单元可能是因为节点故障而释放锁，从而导致临界区内的数据有问题，甚至对于新的执行单元来说全是垃圾。所以如果要这么使用锁，要做好清理垃圾的准备







#### P. S.

1. 分布式锁：并没有特定的死板实现方式，借助已有或者新创的数据结构，遵守本地锁的几个原则进行实现即可。常见的几种实现方式有

   - MySql
   - Zk
   - Redis
   - 自研分布式锁:如谷歌的Chubby

   > https://juejin.im/post/5bbb0d8df265da0abd3533a5

2. zookeeper的提出背景以及要解决的问题

   > https://www.jianshu.com/p/0311a05248d9
   >
   > https://www.jianshu.com/p/84ad63127cd1

3. 