# SQL基本语法
## 创建databases,table
create database 库名;
show databases;
use 库名;

mysqladmin -u root -p drop 库名;    删除数据库

create table 表名(字段1 类型,字段2 类型……);
show tables;

### table的字段修改、删除、添加、查找
desc 表名;
alter table 表名 add column 字段 属性;
alter table 表名 change 旧字段 新字段 新属性;
alter table 表名 drop column 字段;



## 项数据的增删改查
insert into 表名(指定字段1，指定字段2……) values(对应前面字段参数),(对应前面字段参数);       //其他默认

delete from 表名 where 字段=__;

update 表名 set 字段1=__,字段2=___    where 字段=____;
//后面的where是条件，没有则全都改

select *from 表名;


## 数据库导入导出
导出
mysqldump -u root -p 库名 表名 > 名字.sql;

导入
mysql -u root -p 库名< 名字.sql;
(注意：是在命令行中使用！！)


## 常用语句
### 逻辑运算符
select *from 表名 where 字段>1 and 字段<5;
                        字段 in(1,3,5)              //表示字段是这些值（1，3，5）的所有字段全都按行打印出来
                        字段 between 1 and 10;      //全闭区间

2种匹配
select *from 表名 where name like '王%'
                                  '王_'
                                  '%王%'
%表示匹配任意多个字符
_表示匹配一个字符


select *from 表名 where name regexp '^王.$';
正则表达式：
.任意多个字符
^开头
$结尾
[abc]其中一个字符
[a-z]范围内的一个字符
a|b  a或者b

** 注意：判空null和空字符区别： is null、is not null; 字符串= '' **
**select *from 中\* 表示匹配所有字段，如果想指定字段可以直接写字段（可多个）** 

### 排序
order by
select *from 表名 order by 字段/列号;
select *from 表名 where 字段=__ order by 字段;

注意：默认升序
使用降序，order by 字段 desc

复合排序
order by 字段1 desc ,字段2      //按字段1降序，如果相同再按字段2升序


### 聚合函数
avg()   //求平均值
count()     //统计集合数量
max()       //
min()       //
sum()       //求指定集合的字段值和

select avg(字段) from 表名 where 字段=__


### 分组与过滤
group by
having

select sex count(*)from 表名 group by sex having count(level)>4; 
//先按sex分组，再挑选组中元素数量大于4的组，打印其sex，以及组内元素数量


(tips:substr(name,1,1)可以用来获取姓氏，1位置，1获取长度)


### 打印数量限制与去重
limit 数字      //限制数量
limit 数字1，数字2  //指定从数字1后面数字2位

distinct
select distinct sex from 表名 where 
到时候选出来的sex会自动去重

### 并集，交集，差集
写在两个select语句之间
union 合并后会自动去重
union all 合并后不会去重（两个集合数量之和不变）

intersect

except

select *from 表名 where 字段=___
union all
select *from 表名 where 字段=___;


### 子查询
从前一次查询的结果集合中再次查询
1.起别名as
select 字段,round((select avg(字段)from 表名))as average,
level-average as diff from player;


2.再建1张表存储
create table 表名1 select *from 表名2 where 字段=____
insert into 表名1 select *from 表名2 where 字段=___

tips:判断一个查询是否有结果，返回0/1        exists
select exists (select *from )


## 表关联（有相同字段）
内连接  inner join
2个表按指定对应字段匹配连接，2个表的数据都会有

左连接  left join
返回左表所有数据以及右表匹配的部分数据，左表没有关联到的右表缺失数据用null


右连接  right join
返回右表所有数据以及左表匹配的部分数据，右表没有关联到的左表缺失数据用null

select *from 表名1 inner join 表名2 on 表名1.字段=表名2.字段;
select *from 表名1 p,表名2 q, where p.字段=q.字段;

(注意！！   使用连接时一定要有on、where 后面的条件，防止出现笛卡尔积)


## 索引
创建索引
create [unique|fulltext|spatial]index 索引名 on 表名(字段1，字段2，……);
//在表的指定字段上建立索引
//unique指定，fulltext全文，spatial局部

alter table 表名 add index 索引名(字段1，字段2，……);

展示索引
show index from 表名

删除索引
drop index 索引名 on 表名


## 视图（随数据表动态变化）
创建视图
create view 图名
as 
select *from 表名 order by 字段 desc limit 10;
//打印前10的榜单

更改视图
alter view 图名
as 
select *from 表名 order by 字段 limit 10;

删除视图
drop view 图名




