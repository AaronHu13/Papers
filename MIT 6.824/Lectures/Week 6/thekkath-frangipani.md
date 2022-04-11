### Frangipani

Caching coherence

distributed transaction

distributed crash recovery

caching

```
write back: when user create some files, at very first it will only make changes in memory, and finally write back to file system, which will largely increase performance
```

Challenges: caching, decentralized

1.  cache coherence--we want linearizability and caching at the same time

   Rules:

   - no cached data without lock
   - Aquire Lock and get data from PETAL
   - Write to PETAL and release lock

   Coherence Protocol

   - REQUEST: Workstation send a request to lock server

   - RANT: when lock is avaliable, lock server will grant this lock(lease) to workstation

   - REVOKE: lock server send a message to workstation implying that someone else is requesting this lock

   - RELEASE: If workstation has done with its job and cache is dirty, it will write back to PETAL. Then it will release this lock

     ```
     (1) write log to petal
     (2) write modified blocks for locks
     (3) send release
     ```

     

   Work station will automatically write cache back to PETAL every 30 seconds

2. Atomicity-- distributed transactions

   Acquire all locks

   ​		do updates

   ​		write back to PETAL

   Release locks

3. crash recovery for individual servers

   - crash with lock -- write ahead log
     - Not like many other systems, Frangipani give each workstation a log entry and append operation logs for corresponding workstation
     - Logs in PETAL
     - It has version number
     - when a workstation or recovery server is doing recovering, other workstations may be also holding locks. Need to fix this problem--lock







#### Question

1. Write ahead log 有什么用---方便recovery，而且除非system看到一个log complete的entry，否则其不会进行replay，从而解决了在写log时crash的情况
2. log里面不存储dirty cache或者其他的东西，那如何replay呢？--还是会记录本次操作修改了哪些数据这样的消息，这样就可以进行replay了