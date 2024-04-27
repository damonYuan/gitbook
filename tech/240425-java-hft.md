Java HFT
====

## [Write Combining](https://mechanical-sympathy.blogspot.com/2011/07/write-combining.html)

The blog post titled "Write Combining" discusses the techniques employed by modern CPUs to counteract the latency cost of accessing main memory. It explains how CPU caches and write combining buffers are used to hide memory latency and improve performance. The post highlights that write combining buffers allow CPUs to continue processing instructions while data is being prepared for storage in the cache. By effectively filling these buffers, the transfer bus utilization can be improved. The post provides code examples and performance measurements to demonstrate the benefits of write combining. It also mentions the importance of considering hardware characteristics, such as the number of available buffers, when optimizing code. The post concludes by mentioning that hyper-threading can introduce competition for these buffers on the same CPU core.

