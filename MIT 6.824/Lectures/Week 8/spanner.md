### Spanner

sharding--类似于AWS的region，为了避免failure和提高访问速度

read only transactions and real time system



#### Questions

1. Snapshot Isolation?如何理解

   一组Transaction，Transaction manager会记录第一个R/W操作的时间戳并以此来寻找最恰当的历史版本信息

2. 为什么如果把Read only的Transaction time设置的过小，会导致miss recent committed writes? which would cause not externally consistancy



> https://www.jianshu.com/p/f307bd2023f5
