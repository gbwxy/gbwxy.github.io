# MySQL

* [MySQL 总结](/docs/Mysql/mysql-questions-01.md)
* [MySQL 索引](/docs/Mysql/mysql-index.md)
* [MySQL 锁](/docs/Mysql/Lock.md)
* [MySQL 三大日志](/docs/Mysql/mysql-logs.md)
* [MySQL 使用规范](/docs/Mysql/mysql-high-performance-optimization-specification-recommendations.md)
* [MySQL 调优](/docs/Mysql/Optimization.md)
* [MySQL 事务隔离级别](/docs/Mysql/transaction-isolation-level.md)
* [MySQL 高可用](/docs/Mysql/ha.md)
* [MySQL 查询缓存](/docs/Mysql/mysql-query-cache.md)
* [InnoDB 实现 MVCC](/docs/Mysql/innodb-implementation-of-mvcc.md)
* [日期类型选择](/docs/Mysql/some-thoughts-on-database-storage-time.md)
* [一条SQL的执行过程](/docs/Mysql/how-sql-executed-in-mysql.md)
* [SQL执行计划分析](/docs/Mysql/mysql-query-execution-plan.md)
* [MyBatis常见问题总结](/docs/Mysql/mybatis-interview.md)
* [MySQL 自增主键一定是连续的吗](/docs/Mysql//mysql-auto-increment-primary-key-continuous.md)
* [MySQL 读写分离和分库分表](/docs/Mysql/read-and-write-separation-and-library-subtable.md)
* [MySQL 隐式转换造成索引失效](/docs/Mysql/index-invalidation-caused-by-implicit-conversion.md)
* [MySQL 学习笔记](/docs/Mysql/a-thousand-lines-of-mysql-study-notes.md)
* [深度分页介绍及优化建议](/docs/Mysql/deep-pagination-optimization.md)

## 简介
包括 索引、调优、MVCC机制、binlog/undolog/redolog、MySQL组成、HA主从同步、SQL如何执行的等

## 隐式字段
每行记录除了我们自定义的字段外，还有数据库隐式定义的 DB_TRX_ID,DB_ROLL_PTR,DB_ROW_ID等字段
- DB_TRX_ID：6byte，最近修改(修改/插入)事务ID：记录创建这条记录/最后一次修改该记录的事务ID
- DB_ROLL_PTR：7byte，回滚指针，指向这条记录的上一个版本（存储于rollback segment里）
- DB_ROW_ID：6byte，隐含的自增ID（隐藏主键），如果数据表没有主键，InnoDB会自动以DB_ROW_ID产生一个聚簇索引
- 实际还有一个删除flag隐藏字段, 既记录被更新或删除并不代表真的删除，而是删除flag变了

DB_ROW_ID是数据库默认为该行记录生成的唯一隐式主键，DB_TRX_ID是当前操作该记录的事务ID,而DB_ROLL_PTR是一个回滚指针，用于配合undo日志，指向上一个旧版本

