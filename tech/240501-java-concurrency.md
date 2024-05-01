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