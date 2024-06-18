Java Concurrency in Practice
====

@startmindmap
* Java Concurrency in Practice

** Part I
*** C1 Introduction
****_ benefits of threads
****_ risk of threads
*****_ safety hazard
*****_ liveness hazard
*****_ performance hazard

*** C2 Thread Safety
****_ how
*****_ don't share
*****_ immutable and/or stateless
*****_ synchronization mechanism
******_ synchronized
******_ volatile
******_ explicit locks
*******_ intrinsic/monitor locks
*******_ encapsulation of variables
******_ atomicity
*******_ automic variables
*******_ race conditions
********_ check-then-act
********_ read-modify-write

*** C3 Sharing Objects
****_ Visibility
****_ Publication and Escape
****_ Thread Confinement
****_ Immutability
****_ Safe Publication
@endmindmap
