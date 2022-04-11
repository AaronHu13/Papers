## Review of Papers

#### MapReduce

1. 一种分布式计算模型
2. Mapper和reducer，所有的中间运算结果都要存储到磁盘里--会比较耗时--spark改进（基于内存的计算方式）
3. 输入和输出都基于GFS
4. 单独看Mapper和Reducer都是并发的，但是单个的Mapper延误会拖慢整体reducer的速度
5. 单个的Mapper或者reducer崩了怎么办？如何恢复的？---由master来协调worker（可以猜测master也是有备份的）---Master 是怎么存储Mapper和Reducer的输入数据的，是有类似于RDD那样的数据结构嘛



#### GFS

1. 分布式文件系统
2. 

