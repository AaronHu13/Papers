## Raft
1. Split Brain problem?-->inconsistent?
2. Majority is always the total number of servers even though some of servers may fail, but we still need to count them.
   2f + 1 --tolerate-> f failures
3. any two majority at least overlap on 1 server ??? for what? to make sure previous progress is recorded? ---> raft is relied on this properity to avoid split brain
4. after leader finds that it gets majority agrees, then it will send a commit message to all followers to ask these followers to commit this operation, at the same time, it will return a OK message to client
5. Leader election
   1. election timer for each server
   2. RequestVoteRPC
   3. 
6. current version Raft cannot handle the problem that: the outgoing network is okay so leader can send heartbeat to followers but followers cannot send ack or something else to leader. By adding two-way heartbeat this problem may be solved.
7. After candidate wins the election, it will send a heartbeat to notice all followers(empty appendEntryRPC)

===
8. Before execute appendixRPC, followers will need to check previous term content is matched.
9. 如果某次leader发送AppendixRPC的时候follower发现自己的previous term和leader的不匹配，会返回错误；对应的，leader会收到这一错误并将nextIndex--(即所要更新的内容继续往前推)，将nextIndex之后的所有消息打包发出，直到更新成功或者leader宕机
10. Why not longest log server be the leader
    Longest does not mean its log entries are accepted by the majority. In this way, if the longest log server is elected then the majority log entries will be erased, which is not correct.

    Not every server can be elected as server, there are some rules--> 5.4.1

    Classnotes: Vote "yes" only if higher term in last entry, or same last term and >= log length.
    higher term does not means longest log entry
11. Fast Backup
    ```
      XTerm, XIndex, XLen, a better way to accelerate search is Binary search

      case 1:
      4 5 5
      4 6 6 6

      case 2:
      4 4 4
      4 6 6 6

      case 3:
      4
      4 6 6 6
    ```
12. Persistence
    ```
      Log
      currentTerm
      VoteFor: avoiding duplicate vote
    ```
13. InstallSnapshot
    ```
      可能会比较麻烦，有的时候需要处理创建snapshot之后如何处理之前的log
    ```
14. 
> https://www.jianshu.com/p/8e4bbe7e276c