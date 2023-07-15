---
description: MVCC in Database
---

# MVCC in Database

> * Author: [Damon Yuan](https://www.damonyuan.com)
> * Date: 2020-09-10

Most of MySQL’s transactional storage engines don’t use a simple row-locking mechanism. Instead, they use row-level locking in conjunction with a technique for increasing concurrency known as multiversion concurrency control (MVCC). 


![MVCC](../.gitbook/assets/mvcc.png "MVCC")

## Reference

- ![High Performance MySQL, 3rd Edition](https://learning.oreilly.com/covers/urn:orm:book:9781449332471/400w/ "High Performance MySQL, 3rd Edition")