# MYSQL进阶学习记录

## 基础篇

查看事务隔离级别

```mysql
show variables like 'transaction_isolation'
```

注：5.7版本前是 tx_isolation，5.7版本引入了 transaction_isolation用来替换 tx_isolation了，到8.0.3就去掉了后者了

|           事务级别           |                             描述                             |
| :--------------------------: | :----------------------------------------------------------: |
| READ UNCOMMITTED（读未提交） | 一个事务还没提交时，它做的变更就能被别的事务看到。会产生脏读。 |
|  READ COMMITTED（读已提交）  |       一个事务提交之后，它做的变更才会被其他事务看到。       |
| REPEATABLE READ（可重复读）  | 对其他事务来说，又名为“不可重复读”。一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。在可重复读隔离级别下，未提交变更对其他事务也是不可见的 |
| SERIALIZABLE（可串行化）     | 对于同一行记录，“写”会加“写锁”，“读”会加“读锁”。当出现读写锁冲突时，后访问的事务必须等前一个事务执行完成，才能继续执行。 |

查看是否存在长事务

```mysql
select * from information_schema.innodb_trx where TIME_TO_SEC(timediff(now(),trx_started))>60
```

## 索引篇

重建索引

```mysql
alter table T drop index k;
alter table T add index(k);
```

重建主键索引

```mysql
alter table T engine=INNODB
```

mysql选错索引时

1. 可以尝试该命令重新统计索引信息

   ```mysql
   analyze table T;
   ```

2. 强制使用你认为正确的索引

   ```mysql
   select * from t force index(a) where ...
   ```

3. 删掉不需要的索引

## 其他命令

告知Innodb你的IO能力，通过参数`innodb_io_capacity`设置，一般设置为磁盘的IOPS

查询

```mysql
show global variables like 'innodb_io_capacity';
```

设置(机械硬盘通常几百，固态上千)

```mysql
set global innodb_io_capacity = 1000;
```

一般是通过FIO工具得到IOPS，但是不要使用系统盘做测试，会坏掉。。。

测试命令

```shell
fio -filename=$filename -direct=1 -iodepth 1 -thread -rw=randrw -ioengine=psync -bs=16k -size=500M -numjobs=10 -runtime=10 -group_reporting -name=mytest 
```

