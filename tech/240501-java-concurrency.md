Java Concurrency in Practice
====

## C1 Introduction

1. Benefits of threads
   - exploiting multiple process
   - simplicity of modeling
   - simplified handling of asynchronous events
   - more responsive user interfaces
2. Risk of threads
   - safety hazard
     - eg., race condition
     - safety: nothing bad ever happens
   - liveness hazard
     - eg., infinite loop, deadlock, starvation and livelock
     - liveness: something good eventually happens
   - performance hazard
     - eg., poor service time, responsiveness, throughput, resource consumption, scalability
       - context switch
         - saving and restore execution context
         - lose of locality
         - cpu time spent on scheduling threads rather than running them
       - threads sharing data
         - synchronization mechanism that can inhibit compiler optimization
         - flush or invalidate memory cache
         - create synchronization traffic on the shared memory bus
3. threads are everywhere
   - jvm housekeeping: garbage collection, finalization
   - Timer
   - JSP
   - RMI
## C2 Thread Safety
### 2.1 what is thread safety
1. Writing thread-safe code is, at its core, about managing access to state, and in particular to shared, mutable state.
   - synchronized
   - volatile
   - explicit locks
   - atomic variables
2. how to handle state across threads
   - don't share it
   - make it immutable
   - use proper synchronization mechanism whenever it's being accessed
3. Is a thread-safe program one that is constructed en- tirely of thread-safe classes? Not necessarily—a program that consists entirely of thread-safe classes may not be thread-safe, and a thread-safe program may con- tain classes that are not thread-safe.
4. a class is thread-safe when it continues to behave correctly when accessed from multiple threads.
5. Stateless objects are always thread-safe.
### 2.2 atomicity
1. race condition
   - check-then-act 
   - read-modify-write
### 2.3 locking   
1. To preserve state consistency, update related state variables in a single atomic operation.
2. intrinsic locks / monitor locks -> mutexes; and reentrant
### 2.4 Guarding state with locks
1. For each mutable state variable that may be accessed by more than one thread, all accesses to that variable must be performed with the same lock held. In this case, we say that the variable is guarded by that lock.
2. Every shared, mutable variable should be guarded by exactly one lock. Make it clear to maintainers which lock that is.
3. Code auditing tools like FindBugs can identify when a variable is frequently but not always accessed with a lock held, which may indicate a bug.
4. A common locking convention is to encapsulate all mutable state within an object and to protect it from concurrent access by synchronizing any code path that accesses mutable state using the object’s intrinsic lock. This pattern is used by many thread-safe classes, such as Vector and other synchronized collection classes.
5. For every invariant that involves more than one variable, all the variables involved in that invariant must be guarded by the same lock.
### 2.5 Liveness and performance
