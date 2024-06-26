# Damon's Blog

![@ Opera House in the Rain 240208](.gitbook/assets/opera-house.jpg "@ Opera House in the Rain 240208")

- **Email:** [d@damonyuan.com](mailto:d@damonyuan.com)
- **GitHub:** [https://github.com/damonYuan](https://github.com/damonYuan)

## A Learning A Day

- 2024
  - 02 Jul.: [An Interactive Intro to CRDTs](https://jakelazaroff.com/words/an-interactive-intro-to-crdts/) and [Building a collaborative text editor in Go](https://databases.systems/posts/collaborative-editor)
    > CRDT stands for “Conflict-free Replicated Data Type”. 
    > It’s a kind of data structure that can be stored on different computers (peers). Each peer can update its own state instantly, without a network request to check with other peers. 
    > Peers may have different states at different points in time, but are guaranteed to eventually converge on a single agreed-upon state. That makes CRDTs great for building rich collaborative apps, like Google Docs and Figma — without requiring a central server to sync changes.
  - 28 Jun.: [Acquire and Release Semantics](https://preshing.com/20120913/acquire-and-release-semantics/)
    > Acquire semantics -> LoadLoad + LoadStore
    > Release semantics -> LoadStore + StoreStore 
  - 27 Jun.: [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/)
    > Types of Memory Barrier/Reordering: LoadLoad, StoreStore, LoadStore, StoreLoad
  - 26 Jun.: [Memory Reordering Caught in the Act](https://preshing.com/20120515/memory-reordering-caught-in-the-act/)
  - 24 Jun.: [mintomic](http://mintomic.github.io/lock-free/memory-fences/)
  - 23 Jun.: [An Introduction to Lock-Free Programming](https://preshing.com/20120612/an-introduction-to-lock-free-programming/)
    > Sequential consistency means that all threads agree on the order in which memory operations occurred, and that order is consistent with the order of operations in the program source code. 
  - 21 Jun.: [Kiss Linux](https://kisslinux.github.io/)
  - 20 Jun.: [Story: Redis and its creator antirez](https://blog.brachiosoft.com/en/posts/redis/)
  - 19 Jun.: [User-space RCU: Memory-barrier menagerie](https://lwn.net/Articles/573436/#Quick%20Quiz%202)
    > The truth is that pairs of memory barriers provide conditional ordering guarantees.
  - 18 Jun.: [What is move semantics?](https://stackoverflow.com/questions/3106110/what-is-move-semantics)
