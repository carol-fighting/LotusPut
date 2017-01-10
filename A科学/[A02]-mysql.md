----------
# 索引的数据结构


 - 索引本身很大，不能全存在内存，以索引文件的形式存在磁盘
 - 查找过程产生磁盘IO消耗，尽量减少磁盘IO的存取次数
	 -  B树定义，检索一次最多访问h个节点，O(logdN)根据，d大于100,h最多为3
	 - 节点的大小设为页大小，只要一次IO就可以完全载入
	 

----------
# B树、B+ 树
 - B树：

    - 每个节点都是关键字

 - B+树：

    - 非叶节点仅具有索引作用，关键字只存放在叶节点

    - 叶节点构成有序链表，可按关键码排序的次序遍历所有记录

----------
# 索引分类
 - 索引由**存储引擎**实现，本质是数据结构
 - 普通索引：create index 索引名字 on 表名 (字段(长度));
 - 唯一索引：索引列的值必须唯一，create unique index 

 - 主键索引：建表的同时，默认生成的
 - 全文索引：full text，仅可用于myisam，char、varchar或text列上创建
 - 复合索引
 - 最左前缀

----------
# 存储引擎
 
- myisam：非聚集索引
	- 在磁盘存三个文件,存储表定义、数据文件、索引文件
	- 强调性能，不提供事务支持
	- 表锁，一次获得所需锁，不会死锁
- innodb：聚集索引
	- “表空间”数据文件和日志文件
	-  提供事务支持和外键等高级数据库功能
	-  行锁，锁索引，逐步获得锁，可能死锁
		- 如果一条sql语句操作了主键索引，MySQL就会锁定这条主键索引
		- 如果一条语句操作了非主键索引，MySQL会先锁定该非主键索引，再锁定主键索引。
			 
	    - 当两个事务同时执行，一个锁住了主键索引，在等待其他相关索引
		- 另一个锁定了非主键索引，在等待主键索引。这样就会发生死锁。
		
----------
# MySql优化
 - 设置字段为主键，自动添加索引
 - not in 替换成 not exists
 - <>目标 替换成>目标 or <目标
 - 不在列上进行运算 year(udate)<2007 改成 udate>"2007-01-01"
 - 删除不再使用或很少使用的索引
 - 创建复合索引，最左前缀
	 - 若创建(area,age,salary)的复合索引
	 - 相当于创建了(area,age,salary)、(area,age)、area三个索引
 - 使用前缀来索引
	 - 比如CHAR(255)的列，前10个字符内，大多数值是唯一的
 - like语句
	 - like "%目标%" 不会使用索引
	 - like "目标%" 可以使用索引

 ----------
# KEY   
- PRIMARY KEY是一个唯一KEY，所有的关键字列必须定义为NOT NULL。
- 一个表只有一个PRIMARY KEY
- KEY未必都是外键，KEY是索引约束
    - 对表中字段进行索引约束，都是通过primary foreign unique等创建
- KEY主要是用来加快查询速度

----------

# 建表语句 
    CREATE TABLE `PictureTimeInfo` (
      `PictureId` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
      `TypeId` int(11) NOT NULL COMMENT '卷业务类型',
      `PictureUrl` varchar(500) NOT NULL COMMENT '图片URL',
      `BeginTime` datetime NOT NULL COMMENT '起始时间',
      `EndTime` datetime NOT NULL COMMENT '结束时间',
      `AddTime` datetime NOT NULL COMMENT '添加时间',
      `UpdateTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '最后修改时间',
      PRIMARY KEY (`PictureId`),
      KEY `IX_BeginTime_EndTime` (`BeginTime`,`EndTime`),
      KEY `IX_AddTime` (`AddTime`),
      KEY `IX_UpdateTime` (`UpdateTime`))
    ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8 COMMENT='背景图方案表';


----------
# mysql执行顺序

 `[7]`select distinct(s1.spot_id_no),s2.spot_name
`[1]`from point_info s1
`[3]`left join spot_info s2
`[2]`on s1.spot_id_no=s2.spot_id_no
`[4]`where s1.spot_id_no=s2.spot_id_no
`[5]`group by s1.spot_id_no
`[6]`having count(s1.spot_id_no)>10
`[8]`order by s2.spot_name desc
`[9]`limit 0,1

----------
# 连接查询

 - 内连接 `join`
 - 外连接
	 - 左外连接 `left join ` A:有 B:NULL
	 - 右外连接 `right join` A:NULL B:有
	 - 完全连接 `full join` 以上两种都有
 - 交叉连接
	
----------
# NoSql

 -  sql是基于表的数据库，而nosql数据库有基于文档的、键值对的
 -  对于复杂的查询
 - 分层次的数据存储
 - ACID和CAP持久性、可用性、分区容忍性

----------
横纵扩展
----------


 - 横向扩展Scale-out
	 - 再买一个小水缸，放在旁边，连通
 - 纵向拓展
	 - 买一个大水缸，把鱼、水草重新布置到大水缸

----------