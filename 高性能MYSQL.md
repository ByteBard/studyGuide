# 第一章 MYSQL架构
#### MYSQL三层逻辑架构
>* 连接/线程层： 一个连接分配一个线程；短链接/长连接/连接池
>* server层：查询缓存/解析器/分析器/优化器/执行器
>* 引擎层：数据存取
	
	
#### 并发控制
>* 读写锁：读锁可以多个并发读取,写锁会阻塞其他的读锁和写锁
>* 锁粒度：表级锁 行级锁


#### 事务
>* ACID：原子性/一致性/隔离性/持久性
>* 隔离级别：未提交读，提交读，可重复读（MVCC解决幻读），序列化
>* 事务日志：更新语句直接修改内存拷贝，不用每次写入磁盘,异步自动merge或查询merge
>* 事务自动提交
>* 隐式和显式锁定

#### 多版本并发控制 （MVCC）
>* 每行新增两列记录：行创建version，行删除version


#### TODO
>* MYSQL连接长连接，连接池对比，如何实现?
>* MYSQL读写锁实现原理?
>* MYSQL锁的种类?区别?用途?
>* ACID实现原理？
>* 死锁如何检测？如何避免？
>* 两阶段锁协议？


# 第四章 SCHEMA和数据类型优化

#### 字段类型优化规则
>* 字段越小越好
>* 字段越简单越好，整型比字符串好
>* 尽量避免NULL，加索引MYSQL内部不好维护，需要额外存储空间

#### 字段类型
>* 整数类型： 8位tinyint unsigned正数会扩大一倍（-128 - 127）扩大到0-255
>* 浮点类型： 可以用整型替代。
>* 字符串类型： varchar变长（额外字段记录列长度，小于255一个字节，大于需要2个字节）   char定长（列尾空格会截掉）
>* BOLB和TEXT类型：二进制存储 字符方式存储
>* 枚举类型：
>* Datetime和timestamp: 1001-9999年，8字节，与时区无关； 1970-2038年，4字节，与时区有关
>* 位数据类型:

#### 设计SCHEMA陷阱
>* 太多的列
>* 太多的关联。少于12张表
>* 枚举乱用，会造成ALTER TABLE

#### 缓存表和汇总表

#### 加快ALTER TABLE的速度
>* 只修改.frm文件,不会重建表。


#### TODO
>* 临时表和文件排序
>* VARCHAR（11） 11表示什么
>* IP如何存储？
>* 哪些操作会锁表？如何处理大表修改？


# 第六章 查询性能优化
#### 优化count查询
>* 作用：统计表达式not null 的值； 统计行数
>* MyISAM记录了总函数，所以在where条件为空的时候count效率很高
>* 优化：总行数-where条件的值
>* 优化：使用近似值
>* 优化：使用汇总表或membercache等外部缓存