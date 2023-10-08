Memory Order
====

```
std::atomic<size_t>         enqueue_pos_;
pos = enqueue_pos_.load(std::memory_order_relaxed);
```
The above example coms from [Bounded MPMC queue](https://www.1024cores.net/home/lock-free-algorithms/queues/bounded-mpmc-queue).

The order of instruction execution is important. The compiler / java VM and the CPU are allowed to reorder instructions in the program for performance reasons, as long as the semantic meaning of the instructions remain the same. For instance, look at the following instructions,

```
int a = 1;
int b = 2;

a++;
b++;
```

These instructions could be reordered to the following sequence without losing the semantic meaning of the program:

```
int a = 1;
a++;

int b = 2;
b++;
```


There are different options when loading the atomic variable, this blog is to discuss the difference between them.

# memory_order_relaxed

This memory ordering option provides the fewest ordering guarantees. It allows the greatest level of optimization freedom for the compiler and the CPU. It does not enforce any synchronization or ordering constraints on other memory accesses. It is useful in situations where strict ordering is not required or when the synchronization is handled through other means (e.g., locks or other synchronization primitives).

This type of atomic operation can have various optimizations performed on them, and they do not guarantee an order concerning locking and normal memory accesses.

# memory_order_acquire

Reads from and writes to other variables cannot be reordered to occur before a read of a volatile variable, if the reads / writes originally occurred after the read of the volatile variable. 

Notice that it is possible for reads of other variables that occur before the read of a volatile variable can be reordered to occur after the read of the volatile. Just not the other way around. 

From before to after is allowed, but from after to before is not allowed.

# memory_order_consume

This performs the same operation as `memory_order_acquire`, except that the ordering guarantees only apply to dependent data.

# memory_order_release

Reads from and writes to other variables cannot be reordered to occur after a write to a volatile variable, if the reads / writes originally occurred before the write to the volatile variable.

The reads / writes before a write to a volatile variable are guaranteed to "happen before" the write to the volatile variable. Notice that it is still possible for e.g. reads / writes of other variables located after a write to a volatile to be reordered to occur before that write to the volatile. Just not the other way around. 

From after to before is allowed, but from before to after is not allowed.

# memory_order_acq_rel

This is a hybrid of memory_order_acquire and memory_order_release. Unlike the sequentially consistent model, this hybrid applies a happens-before relationship to dependent variables. This variant allows the synchronization requirements between independent readings and writes to be relaxed.

# memory_order_seq_cst

This operates when all access to memory (that may have visible side effects on the other threads involved) has already happened. This is the strictest memory order variant. Thus, it guarantees the most negligible unexpected side effects between the thread interactions through non-atomic memory accesses.



# Reference
- [Memory Ordering for Atomic Operations in C++0x](https://www.developerfusion.com/article/138018/memory-ordering-for-atomic-operations-in-c0x/)
- [Acquire and Release Fences](https://preshing.com/20130922/acquire-and-release-fences/)
- [What is memory_order in C?](https://www.educative.io/answers/what-is-memoryorder-in-c)
- [The Purpose of memory_order_consume in C++11](https://preshing.com/20140709/the-purpose-of-memory_order_consume-in-cpp11/)
- [False Sharing](https://dzone.com/articles/false-sharing)
- [Java Volatile Keyword](https://jenkov.com/tutorials/java-concurrency/volatile.html)