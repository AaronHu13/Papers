### Paper reading 
> https://www.usenix.org/conference/nsdi19/presentation/park


1. Crash Recovery
   How do they select primary when previous crash and recover from backup data, would there be some situations that backups' data is different
   
2. Replay can Cause Duplicate Executions
   
   ![image-20200316152347037](/Users/aaronhu/Library/Application Support/typora-user-images/image-20200316152347037.png)
   
   当x<--2被同步之后，不应该直接从witnesses里面删除从而避免重复操作嘛？为什么还要专门定义一个rule来解决这个问题呢
   
3. multiple witnesses-> 怎么操作的，有什么用嘛？commutative又是什么？

4. 

