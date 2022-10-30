## 题目 01- 请你说一说 MySQL 的锁机制
要求：

### 按照锁的粒度，锁的功能来分析
按锁功能划份，MySQL 有：  
共享锁（shared lock，S）也叫读锁，S锁，读锁之间互相不阻塞，所以是共享锁。  
排他锁 （exclusive lock，X）也叫写锁，X锁，写锁阻塞其他读锁和写锁，所以是排他锁。  
按粒度划份，MySQL 有  
全局锁：全局锁住整个数据库DB，由SQL Layer 层实现，阻塞 DML, DDL 及已经更新但未提交的语句。  
表级锁：锁住表Table，由SQL Layer 层实现，表读锁阻塞表的写，不阻塞表的读，表写锁阻塞表的读与写。  
行级锁：锁住行Row的索引，由存储引擎 InnoDB 实现, 行读锁阻塞目标行的写，不阻塞目标行的读，行写锁阻塞目标行的读与写。  
### 什么是死锁，为什么会发生，如何排查？
死锁是当两个 SQL session 事务同时互相尝试获取对方的锁而陷入阻塞执行並且无限等待的状态。发生死锁的原因是程序逻辑的顺序交叠。死锁可以通过日志排查。  
### 行锁是通过加在什么上完成的锁定？  
行锁是通过加在Row的索引上完成的锁定。  
### 详细说说这条 SQL 的锁定情况： delete from tt where uid = 666 ;
事务A给记录加上行级写锁，同时给表加上意向写锁，如果这时事务B要给表加上写锁就会被阻塞，直到事务A完成delete，然后把行级写锁和表的意向写锁释放了，事务B才能给表加上写锁再继续操作。  
## 题目 02- 请你说一说 MySQL 的 SQL 优化
查看 SQL 执行计划，在 SQL 语句前加上 explain。
重点关注type访问类型, 按照以下性能排序从好到差：  
1. system
2. const
3. eq_ref
4. ref
5. fulltext
6. ref_or_null
7. unique_subquery
8. index_subquery
9. range
10. index_merge
11. index
12. ALL
参照以上排序，修改SQL语句以得到更高的性能。


