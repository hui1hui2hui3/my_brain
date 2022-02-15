---
tags: ["MySql"]
---

## DML
### select
**涉及关键字**
- select
- from
- where
- group by
- having
- order by
- limit

**关键字执行顺序**
1. from //来源
2. where //过滤
3. group by  //分组
4. having  //分组后再过滤，不可单独使用，必须和group by一起（？？？**可能having的顺序应该是在select后面**？？？）
5. select //查询
6. order by //排序
7. limit //分页

**as关键字**
as：重命名  ，可以省略as
```
select name as haha from table;
select name haha from table;
select name from table as t;
select name from table t;
```

#### 分组函数
**注意：由于分组函数必须要字分组后使用，所以where中不可用分组函数，但select，having中可以使用**
**注意：如果使用分组函数，且没有group by则会把整张表作为一组**
- sum //求和
`select sum(price) from table` //表示整个表的总价格计算，结果为1个
`select class_name,sum(price) from table group by class_name` //表示以每个班级进行分组，分别进行计算，计算出的结果是每个班级的总价格是多少
`select class_name,sum(price) as sumprice from table where sumprice>10 group by class_name` **错误：where时sumprice是select造出来的，这时还没出来，不可使用**
`select class_name,sum(price) as sumprice from table group by class_name having sumprice>200` //表示以每个班级进行分组，分别进行计算，然后过滤出总成绩大于200的班级
- avg/max/min  //平均/最大/最小
- count //计数

#### 子查询
可以出现的位置：
- select //作为查询结果，需要和左侧结果匹配，否则需要是唯一，否则就会出现`Subquery returns more than 1 row`错误
`select a,(select id from table_b where id=1) from table_a`
- from //只查询作为临时表
`select t.id from (select id from table_b where id=1) t`
- where //作为筛查条件数据集
`select t.id from table_a t where t.id in (select id from table_b)`

#### 连接查询
- inner join
```sql
select t.a,s.b from table1 t 
inner join table2 s on t.id = s.id
```
- left join
```sql
select t.a,s.b from table1 t 
left join table2 s on t.id = s.id
```
- right join
```sql
select t.a,s.b from table1 t 
right join table2 s on t.id = s.id
```

### insert
语法：`insert into table() values()`

### update
语法： `update table set column="value"`

### delete
delete: **删除表数据，可以根据条件删除，删除速度慢，可以恢复**
语法：`delete from table_name`

## DDL
### create
语法：
```
create table table_name(
	id int(11) primary_key,
	name varchar(32) not null,
	price double(12),
	create_date datetime
)
```

### alter
语法：
```
alter table table_name add colname varchar(20);
alter table table_name drop COLUMN colname;
alter table table_name modify COLUMN colname varchar(1);
```
### drop/truncate
truncate： **删除数据，只能为全表数据，表结构还在；速度快，无法恢复**
语法：
```
truncate table_name
```

drop: **删除表，删除后表结构和数据都不存在，速度快，无法恢复**
语法：
```
drop table_name
```

## 常见问题
### exists VS In的区别
in 优先执行子查询，然后在和外查询进行数据匹配过滤；适合子查询数据量较小的情况
exists 优先执行外查询，然后在和子查询进行数据匹配过滤；适合父查询数据量较小的情况

### inner join 和 left/right join的区别
**inner join** 不区分主从表，利用笛卡尔积，过滤出双方完全匹配的数据
**left/right join**，区分主从表，left join则左表为主表，right join则右表为主表，主表数据量不变，从表和主表匹配，如果无匹配，则null显示； 数据量一定大于等于inner join的数据量