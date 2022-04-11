## CRAQ

1. Chain Replication
2. chain replication can not defense split brain problem, it cannot use along to do replication.
   1. You can hardly know no connections between two nodes is caused by network failure or nodes crash
   2. As a result, chain replication usually comes with an configuration manager（Raft, Paxos, ZK, etc）
3. One replication can largely slow whole replication process

