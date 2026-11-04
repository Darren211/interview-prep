# MySQL 基础面试题

## MySQL 查询大致经过哪些阶段？

连接处理与权限校验之后，服务端解析 SQL、进行语义检查和优化，再调用存储引擎执行并返回结果。MySQL 8.0 已移除旧查询缓存，不应把“先查缓存”说成当前通用执行阶段。

## InnoDB 和 MyISAM 有什么区别？

InnoDB 支持事务、行级锁和崩溃恢复，并以聚簇索引组织数据；MyISAM 不支持事务，使用表级锁。不同版本的全文索引能力已有变化，比较时应说明版本，避免说 InnoDB 不支持全文索引。

## CHAR 和 VARCHAR 有什么区别？

CHAR 是定长类型，VARCHAR 按实际内容存储并带长度信息。选择需考虑字符集、最大长度、行格式和访问模式，不能只按“字节大小是否确定”判断。

## IN 和 EXISTS 有什么区别？

两者表达集合成员/相关子查询判断。优化器可能将其改写成类似的半连接计划，性能取决于数据规模、索引和执行计划，不应简单说 IN 只能用于固定值。

## DELETE、TRUNCATE 和 DROP 有什么区别？

DELETE 按行删除，可带 WHERE；TRUNCATE 清空表数据但保留表结构；DROP 删除表对象。事务、回滚、触发器、自增计数和外键限制因操作类型及数据库版本而异，生产操作前需确认语义和备份。

## 使用索引查询一定更快吗？

不一定。表很小、条件选择性差或回表成本高时，全表扫描可能更便宜。应通过执行计划、实际扫描行数和基准测试判断。

## 原始笔记中的 MySQL 参考链接

- [Dandelion Cloud MySQL 面试文章](https://www.dandelioncloud.cn/article/details/1570040448557477889)
- [JavaGuide MySQL 面试题](https://javabetter.cn/sidebar/sanfene/mysql.html)
