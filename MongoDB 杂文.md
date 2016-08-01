## MongoDB 杂文

#### [MongoDB 那些坑](https://ruby-china.org/topics/20128)

列举了四点：

- MongoDB 数据库级锁
- 建索引导致数据库阻塞
- 不合理使用 embed document
- 不合理使用 Array 字段

后两点主要是，对应使用场景下「写操作」格外密集，导致数据库阻塞，数据库设计中重点考虑 **读写比**。

使用建立索引的 [background](https://docs.mongodb.com/manual/core/index-creation/#index-creation-background) 熟悉就可以解决建索引阻塞的问题。

关于锁，不太懂。