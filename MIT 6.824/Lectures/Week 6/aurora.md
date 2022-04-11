### Aurora

1. 分布式锁能否用来实现分布式的事务--ZooKeeper
2. write ahead lock是什么
3. Aurora just send log to sync, that's quite small
4. only need majority(4 out of 6) acks to 
5. FT goals: 
   1. write with one AZ dead
   2. read with one AZ dead + one server dead
   3. transient slow???
   4. fast re-replication
6. Qourum replication
   1. N replicas
   2. W for writes
   3. R for reads
   4. W + R >= N + 1, AKA, we need overlap
   5. 可以根据IO的类型来设置read和write的值，如果是R的overhead比较高，可以把W的值设置的高一点；如果W的overhead比较高，就把R的值设置的高一点
   6. Qourum read is not necessary, service can keep track of the lastest version of pages and directly read it, instead of batch reading. Qourum is used when crash happens
7. 存储的时候数据segment分隔好，这样在backup的时候能够提速不少