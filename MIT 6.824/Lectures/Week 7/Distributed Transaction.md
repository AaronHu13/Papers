### Distributied Transaction

concurrency control

atomic commit

two phase locking--是不是有点像读写锁啊？

> https://zhuanlan.zhihu.com/p/59535337
>
> Not good for performance

Two-phase commit

TC - Transaction coordinator--类似于master感觉

participants--before participants send yes/commit to transaction coordinator, they must write log or at least save their state to disks

If participants reply yes to "prepare" request, then it must keep waiting until it get message from transaction manager

Disadvantages:

- need to write state to log before send commit, also, it must wait for writing into disk, which is relatively slow
- too much stages