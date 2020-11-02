

**多版本并发控制**

多版本并发控制（Multi-Version Concurrency Control, MVCC）是 MySQL 的 InnoDB 存储引擎实现隔离级别的一种 具体方式，用于实现提交读和可重复读这两种隔离级别。而未提交读隔离级别总是读取新的数据行，无需使用 MVCC。可串行化隔离级别需要对所有读取的行都加锁，单纯使用 MVCC 无法实现。

