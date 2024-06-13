---
layout: post
title: MySQL数据库表分区
date: 2024-06-06 10:49:31
tags:
- 数据库表分区
category:
- 数据库
---
## 1.背景
当前数据库中，数据库表已经存在，同时该数据库表的数据还在每天不断增长。因为数据库表太大，导致检索过程耗时，为提高检索效率，故对相关数据库表进行分区处理。

## 2.MySQL分区
分区就是将一个表分解成多个区块进行操作和保存，从而降低每次操作的数据，提高性能。

而对应用来说是透明的，从逻辑上看是只有一个表，但在物理上这个表可能是由多个物理分区组成的，每个分区都是一个独立的对象，可以进行独立处理

**四种常见的分区类型：**
+ `RANGE分区`：最为常用，基于属于一个<i style="color:red;">给定连续区间的列值</i>，把多行分配给分区。最常见的是基于时间字段。

+ `LIST分区`：LIST分区和RANGE分区类似，区别在于<i style="color:red;">LIST是枚举值列表的集合</i>，RANGE是连续的区间值的集合。

+ `HASH分区`：基于<i style="color:red;">用户定义的表达式的返回值来进行选择的分区</i>，该表达式使用将要插入到表中的这些行的列值进行计算。这个函数可以包含MySQL中有效的、产生非负整数值的任何表达式。

+ `KEY分区`：类似于按HASH分区，区别在于<i style="color:red;">KEY分区只支持计算一列或多列</i>，且MySQL服务器提供其自身的哈希函数。必须有一列或多列包含整数值。

### 3.示例
#### 3.1 RANGE分区
情景：现有`hourdb`数据库表，想要基于该数据库表的`time`字段进行分区。
实现：
```sql
-- 1.对hourdb表进行分区
-- 分区规则：
-- 2019年及之前
-- 2020年上半年--2020年下半年
-- 2021年上半年--2021年下半年
-- 2022年上半年--2022年下半年
-- 2023年上半年--2023年下半年
-- 2024年上半年--2024年下半年
-- 2025年上半年--2025年下半年
-- 2026年及之后

-- 基于time字段产生time_year_month字段，time_year_month=year*100+month
alter table hourdb add column time_year_month int generated always 
as(year(STR_TO_DATE(time,'%Y-%m-%d %H:%i:%s'))*100+month(STR_TO_DATE(time,'%Y-%m-%d %H:%i:%s'))) 
stored;

-- 创建新的分区表hourdb_partitioned，相比较原hourdb表，增加time_year_month字段
create table if not exists `hourdb_partitioned` (
 `senid` decimal(38,0) comment '传感器点号',
 `time` varchar(255) comment '时标',
 `v` decimal(11,3) comment '整点值',
 `avgv` decimal(11,3) comment '平均值',
 `maxv` decimal(11,3) comment '最大值',
 `maxt` varchar(255) comment '最大值发生时间',
 `minv` decimal(11,3) comment '最小值',
 `mint` varchar(255) comment '最小值发生时间',
 `s` decimal(38,0) comment '整点值标识',
 `avgs` decimal(38,0) comment '平均值标识',
 `maxs` decimal(38,0) comment '最大值标识',
 `mins` decimal(38,0) comment '最小值标识',
 `span` decimal(38,0) comment '计算时段',
 time_year_month int generated always 
 as(year(STR_TO_DATE(time,'%Y-%m-%d %H:%i:%s'))*100+month(STR_TO_DATE(time,'%Y-%m-%d %H:%i:%s'))) stored
)comment '小时数据信息'
partition by range(time_year_month)(
 partition p202000 values less than (202001) comment '2020年之前',
 partition p202001 values less than (202007) comment '2020年上半年',
 partition p202002 values less than (202101) comment '2020年下半年',
 partition p202101 values less than (202107)comment '2021年上半年',
 partition p202102 values less than (202201)comment '2021年下半年',
 partition p202201 values less than (202207)comment '2022年上半年',
 partition p202202 values less than (202301)comment '2022年下半年',
 partition p202301 values less than (202307)comment '2023年上半年',
 partition p202302 values less than (202401)comment '2023年下半年',
 partition p202401 values less than (202407)comment '2024年上半年',
 partition p202402 values less than (202501)comment '2024年下半年',
 partition p202501 values less than (202507)comment '2025年上半年',
 partition p202502 values less than (202601)comment '2025年下半年',
 partition p202511 values less than (maxvalue)comment '2026年及以后'
);

-- 将原数据库数据插入新的数据库
INSERT INTO `hourdb_partitioned`  (`senid`,`time`,`v`,`avgv`,`maxv`,`maxt`,`minv`,`mint`,`s`,`avgs`,`maxs`,`mins`,`span`)
select `senid`,`time`,`v`,`avgv`,`maxv`,`maxt`,`minv`,`mint`,`s`,`avgs`,`maxs`,`mins`,`span` from `hourdb` ;


-- 重命名表
rename table `hourdb` to `hourdb_backup`,`hourdb_partitioned` to `hourdb`;

-- 测试
select count(senid) from `hourdb` partition (p202401);
```
**注意：**在分区时，一定要按照从小到大的顺序，已经分区的内容将不再参与后续的分区。

#### 3.2 LIST分区
```sql
CREATE TABLE `staff_partition_list` (
  `id` int(4) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `sex` tinyint(255) DEFAULT NULL,
  `age` int(3) DEFAULT NULL,
  PRIMARY KEY (`id`)
)ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8
PARTITION BY List (id) (
	PARTITION p0 VALUES in (1,2,3,5),
	PARTITION p1 VALUES in (7,9,10),
	PARTITION p2 VALUES in (11,15)
);

```

#### 3.3 HASH分区
```sql
CREATE TABLE `staff_partition_hash` (
  `id` int(4) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `sex` tinyint(255) DEFAULT NULL,
  `age` int(3) DEFAULT NULL,
  PRIMARY KEY (`id`)
)ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8
PARTITION BY HASH (id)
PARTITIONS 3;
```
每次插入、更新、删除一行，hash表达式都要计算一次；这意味着非常复杂的表达式可能会引起性能问题，尤其是在执行同时影响大量行的运算（例如批量插入）的时候。

#### 3.4 KEY分区
```sql
CREATE TABLE `staff_partition_key` (
  `id` int(4) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `sex` tinyint(255) DEFAULT NULL,
  `age` int(3) DEFAULT NULL,
  PRIMARY KEY (`id`)
)ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8
PARTITION BY LINEAR Key (id)
PARTITIONS 3;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202406130922462.png)

### 4.分区管理
#### 4.1删除分区
```sql
alter table staff_partition drop partition p0;
```
上述语句删除p0分区，同时也会删除p0分区的数据。
```sql
 alter table staff_partition remove partitioning;
```
上述语句只删除p0分区，但是保留了p0分区的数据。

```sql
-- 对于hash分区和key分区
alter table staff_partition_hash COALESCE PARTITION 2;
```
上述命令将当前的分区数量减少到 2 个分区，数据库系统会自动选择哪些分区合并，重新分配数据
#### 4.2增加分区
**RANGE和LIST分区**
```sql
alter table staff_partition add partition(partition p3 values LESS THAN (20));
```
+ 对于RANGE分区的表，只可以添加新的分区到分区列表的高端
+ 对于List分区的表，不能添加已经包含在现有分区值列表中的任意值

**HASH和KEY分区**
```sql
alter table test_staff_partition_hash add PARTITION partitions 2;
```

#### 4.3重新分区
```sql
ALTER TABLE tbl_name REORGANIZE PARTITION partition_list INTO 
(partition_definitions)

-- 示例
ALTER TABLE example_table
  REORGANIZE PARTITION p1 INTO (
    PARTITION p1_1 VALUES LESS THAN (100),
    PARTITION p1_2 VALUES LESS THAN (200)
  );
```
#### 4.4优化分区
如果从分区中删除了大量的行，或者对一个带有可变长度的行（也就是说，有VARCHAR，BLOB，或TEXT类型的列）作了许多修改，可以使用`ALTER TABLE ... OPTIMIZE PARTITION`来收回没有使用的空间，并整理分区数据文件的碎片。
```sql
alter table test_staff_partition OPTIMIZE PARTITION p2,p3;
```
