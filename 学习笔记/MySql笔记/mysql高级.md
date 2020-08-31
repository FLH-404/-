



[MYSQL高级脑图](https://www.processon.com/view/link/5ed6514c1e08537197d76e35)







show engines;
show variables like "%storage_engine%";


select b.boyfriend_id ,bo.boyName
from beauty b
inner join boys bo 
on b.boyfriend_id = bo.id;

select b.boyfriend_id ,bo.boyName
from beauty b
left join boys bo 
on b.boyfriend_id = bo.id;


select b.boyfriend_id ,bo.boyName
from beauty b
right join boys bo 
on b.boyfriend_id = bo.id;


create database `oemp`;

use `oemp`;

#部门表
create table `tb_dept` (
`id` int(11) not null auto_increment,
`name` varchar(30) default null,
`storey` varchar(40) default null,
primary key(`id`)
)  engine = innodb auto_increment=1 default charset=utf8;

#员工表
create table `tb_emp` (
`id` int(11) not null auto_increment,
`name` varchar(30) default null,
`dept_id` int(11) default null,
primary key(`id`),
key `idx_dept_id`(`dept_id`)
#, constraint `fk_dept_id` foregign key(`dept_id`) references `tb_dept` (`id`)
)  engine = innodb auto_increment=1 default charset=utf8;

#部门数据
insert into `tb_dept`(`id`, `name`, `storey`) values('1', 'RD', '11');
insert into `tb_dept`(`id`, `name`, `storey`) values('2', 'HR', '12');
insert into `tb_dept`(`id`, `name`, `storey`) values('3', 'MK', '13');
insert into `tb_dept`(`id`, `name`, `storey`) values('4', 'MIS', '14');
insert into `tb_dept`(`id`, `name`, `storey`) values('5', 'FD', '15');

#员工数据
insert into `tb_emp`(`name`, `dept_id`) values('z3', 1);
insert into `tb_emp`(`name`, `dept_id`) values('z4', 1);
insert into `tb_emp`(`name`, `dept_id`) values('z5', 1);

insert into `tb_emp`(`name`, `dept_id`) values('w5', 2);
insert into `tb_emp`(`name`, `dept_id`) values('w6', 2);

insert into `tb_emp`(`name`, `dept_id`) values('s7', 3);
insert into `tb_emp`(`name`, `dept_id`) values('s8', 4);
insert into `tb_emp`(`name`, `dept_id`) values('s9', 51);



#内连接 交集
select * 
from tb_emp a 
inner join tb_dept b 
on a.dept_id = b.id;

#左外 主+共
select * 
from tb_emp a 
left join tb_dept b 
on a.dept_id = b.id;

#右外 副+共
select * 
from tb_emp a 
right join tb_dept b 
on a.dept_id = b.id;

#左差集
select * 
from tb_emp a 
left join tb_dept b 
on a.dept_id = b.id
where b.id is null;

#右差集
select * 
from tb_emp a 
right join tb_dept b 
on a.dept_id = b.id
where a.dept_id is null;

#全集 (mysql 不支持)sql 99
select * 
from tb_emp a 
full outer join tb_dept b 
on a.dept_id = b.id;

#解决 使用联合查询 union 自动去重
select * 
from tb_emp a 
left join tb_dept b 
on a.dept_id = b.id
union
select * 
from tb_emp a 
right join tb_dept b 
on a.dept_id = b.id;



#差集
select * 
from tb_emp a 
full outer join tb_dept b 
on a.dept_id = b.id;
where a.dept_id is null or b.id is null;

#解决

select * 
from tb_emp a 
right join tb_dept b 
on a.dept_id = b.id
where a.dept_id is null
union
select * 
from tb_emp a 
left join tb_dept b 
on a.dept_id = b.id
where b.id is null;





#exlpain+sql 执行计划

explain select * 
from tb_emp a 
right join tb_dept b 
on a.dept_id = b.id
where a.dept_id is null
union
select * 
from tb_emp a 
left join tb_dept b 
on a.dept_id = b.id
where b.id is null;







explain select id  from tb_dept where id=1;

show index from tb_dept;


#单表

create table if not exists `article` (
`id` int(11) auto_increment not null,
`author_id` int(11) not null,
`category_id`  int(11) not null,
`views` int(11) not null,
`comments` int(11) not null,
`title` varchar(255) not null,
`content` text not null,
primary key(id)
);


insert into `article`(`author_id`, `category_id`, `views`, `comments`, `title`, `content`) values 
(1, 1, 1, 1, '1', '1'),
(2, 2, 2, 2, '2', '2'),
(3, 3, 3, 3, '3', '3'),
(4, 4, 4, 4, '4', '4');


select * from `article`;

#查询 category_id 为 1 且 comments 大于 1 的情况下， views 最多的 article_id


explain select id ,author_id from article 
where category_id = 1 and comments > 1 order by views desc limit 1 ;

show index from article;

#开始优化
#1.1新建索引+删除索引
#第一种
create index idx_article_ccv on article(category_id,comments,views);
#第二种
alter table article add index idx_article_ccv rticle(category_id,comments,views);


show index from article;

#再次尝试
explain select id ,author_id from article 
where category_id = 1 and comments > 1 order by views desc limit 1 ;


explain select id ,author_id from article 
where category_id = 1 and comments = 1 order by views desc limit 1 ;


#删除索引

drop index idx_article_ccv on article;

explain select id ,author_id from article 
where category_id = 1 and comments = 1 order by views desc limit 1 ;

explain select id ,author_id from article 
where category_id = 1 and comments > 1 order by views desc limit 1 ;


create index idx_ccv on article(category_id,views);

explain select id ,author_id from article 
where category_id = 1 and comments > 1 order by views desc limit 1 ;





#多表
use oemp;

#图书分类
create table if not exists `class` ( 
`id` int(11) auto_increment not null,  
`card` int(11) not null, 
primary key(`id`) 
);

#图书
create table if not exists `book` ( 
`id` int(11) auto_increment not null,  
`card` int(11) not null, 
primary key(`id`) 
);

delete from `book`;
delete from `class`;

#图书分类
insert into `class`(`id`, `card`) values(1, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(2, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(3, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(4, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(5, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(6, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(7, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(8, floor(1 + (rand() * 20)));

#图书
insert into `book`(`id`, `card`) values(1, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(2, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(3, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(4, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(5, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(6, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(7, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(8, floor(1 + (rand() * 20)));

select * from `class`;
select * from `book`;


#下面的 expain 分析
explain select * from class left join book on class.card = book.card;
#结论 type 有 ALL

alter table book add index(card);

explain select * from class left join book on class.card = book.card



drop index Y on book;


alter table class add index Y(card);

explain select * from class left join book on class.card = book.card;

drop index Y on class;

#左右相反



三表

use oemp;

#分类
create table if not exists `class` ( 
`id` int(11) auto_increment not null,  
`card` int(11) not null, 
primary key(`id`) 
);

#图书
create table if not exists `book` ( 
`id` int(11) auto_increment not null,  
`card` int(11) not null, 
primary key(`id`) 
);

#手机
create table if not exists `phone` ( 
`id` int(11) auto_increment not null,  
`card` int(11) not null, 
primary key(`id`) 
);

delete from `book`;
delete from `class`;
delete from `phone`;

#图书分类
insert into `class`(`id`, `card`) values(1, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(2, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(3, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(4, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(5, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(6, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(7, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(8, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(9, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(10, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(11, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(12, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(13, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(14, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(15, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(16, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(17, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(18, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(19, floor(1 + (rand() * 20)));
insert into `class`(`id`, `card`) values(20, floor(1 + (rand() * 20)));


#图书
insert into `book`(`id`, `card`) values(1, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(2, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(3, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(4, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(5, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(6, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(7, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(8, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(9, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(10, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(11, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(12, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(13, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(14, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(15, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(16, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(17, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(18, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(19, floor(1 + (rand() * 20)));
insert into `book`(`id`, `card`) values(20, floor(1 + (rand() * 20)));
#手机
insert into `phone`(`id`, `card`) values(1, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(2, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(3, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(4, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(5, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(6, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(7, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(8, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(9, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(10, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(11, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(12, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(13, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(14, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(15, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(16, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(17, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(18, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(19, floor(1 + (rand() * 20)));
insert into `phone`(`id`, `card`) values(20, floor(1 + (rand() * 20)));


select * from book;

select * from phone;


show index from class;

show index from book;

show index from phone;


explain select  * from class c 
left join book b on c.card = b.card
left join phone p on b.card = p.card;


alter table book add index X(card);

alter table phone add index Y(card);


drop index X on book;

drop index Y on phone;

#【结论】
#join 语句的优化

1. 尽可能的减少 join 语句中的 NestedLoop 

​       的循环总次数：”永远用小结果集驱动大的结果集“。alter

2. 优先优化 NestedLoop 的内层循环；

3. 保证 Join 语句中被驱动表上的 Join 条件字段已经被索引。

4. 当无法保证被驱动表的join 条件字段被索引且内存资源充足的前提下， 大家不要吝啬      

​      JoinBuffer 的设置。

#索引优化一

create table staffs (
id int primary key auto_increment,
name varchar(24) not null default '' comment '姓名',
age int not null default 0 comment '年龄',
pos varchar(20) not null default '' comment '职位',
add_time timestamp not null default current_timestamp comment '入职时间' 
) charset utf8 comment '员工记录表';

insert into staffs(name, age, pos, add_time) values ('z3', 22, 'manager', now());
insert into staffs(name, age, pos, add_time) values ('July', 23, 'dev', now());
insert into staffs(name, age, pos, add_time) values ('2000', 23, 'dev', now());


select * from staffs;

alter table staffs add index idx_staffs_nap(name, age, pos);

show index  from staffs;

explain select * from staffs where name ='July';

explain select * from staffs where name ='July' and age = 25;

explain select * from staffs where name ='July' and age = 25 and pos='dev';


#alter table staffs add index idx_staffs_nap(name, age, pos);
#1.全值匹配
select * from staffs where  age = 23 and pos='dev';

#对比

explain select * from staffs where  age = 23 and pos='dev';

explain select * from staffs where pos='dev';

explain select * from staffs where name ='July';

show index from staffs;

#2.建立索引 idx_staffs_nap 使用 第一字段必须使用 ，中间不能断，断了仅使用没断之前的索引 
#最佳左前缀法则

explain select * from staffs where name ='July' and pos='dev';

explain select * from staffs where  age = 23 and pos='dev' and name ='July';


#3. 不在索引列上左任何操作 （计算、函数、（自动 or 手动）类型转换），    会导致索引失效而转向全表扫描
#索引列上少计算
select * from staffs where name ='July';

select * from staffs where left(name,4) ='July';

explain select * from staffs where left(name,4) ='July';

explain select * from staffs where name ='July' and age = 25 and pos = 'manager';

#4. 存储引擎不能使用索引中范围条件右边的列
#范围之后全失效  pos 列的索引失效  **这个范围不是条件书写顺序是建表时列的顺序**

explain select * from staffs where name ='July' and age > 25 and pos = 'manager';

explain select * from staffs where name ='July' and age > 25 ;

#5. 尽量使用覆盖索引（只访问索引的查询（索引列和查询列一致）），减少 select *

desc staffs;

explain select * from staffs where name ='July' and age = 25 and pos = 'manager';
#相比下面的比较好   覆盖索引
explain select name , age , pos from staffs where name ='July' and age = 25 and pos = 'manager';


#6. mysql 在适应不等于 （!= 或者 <>）的时候无法使用索引会导致全表扫描

> mysql 8.0  之前索引会失效  8.0的type 是rage extra是using index condition 索引不失效 

explain select * from staffs where name !='July'; #索引会失效根据业务也能写性能低了些

#7. is null, is not null 也无法使用索引

explain select * from staffs where name is null;

explain select * from staffs where name is not null;


#8. like以通配符开头 （'%abc ...'）mysql 索引失效会变成全表扫描的操作
#LIKE 百分写最右， 覆盖索引不写星
失效
explain select * from staffs where name  like '%July%';
失效
explain select * from staffs where name  like '%July';
未失效
explain select * from staffs where name  like 'July%';

#解决两边失效的办法  覆盖索引 不要写*

#like 关键字 '%%' 
create table `tb_user` (
`id` int(11) not null auto_increment,
`name` varchar(20) default null,
`age` int(11) default null,
`email` varchar(20) default null,
primary key(id)
) engine = innodb auto_increment=1 default charset = utf8;

select * from `tb_user`;
#drop table `tb_user`;

insert into tb_user(name, age, email) values ('1aa1', 21, 'b@163.com');
insert into tb_user(name, age, email) values ('2aa2', 222, 'a@163.com');
insert into tb_user(name, age, email) values ('3aa3', 256, 'c@163.com');
insert into tb_user(name, age, email) values ('4aa4', 21, 'd@163.com');

select * from tb_user where name like '%aa%';

EXPLAIN select * from tb_user where name like '%aa%';


EXPLAIN select name, age, email from tb_user where name like '%aa%';

EXPLAIN select name from tb_user where name like '%aa%';

EXPLAIN select age from tb_user where name like '%aa%';

EXPLAIN select email from tb_user where name like '%aa%';

EXPLAIN select* from tb_user where name like '%aa%';

EXPLAIN select name, age, email from tb_user where name like '%aa%';
#创建索引
create index idx_name_age on tb_user(name, age);

EXPLAIN select name, age from tb_user where name like '%aa%';


#id 主键索引自带
EXPLAIN select id from tb_user where name like '%aa%';

EXPLAIN select name from tb_user where name like '%aa%';

#### 这里会失效 字段不匹配

EXPLAIN select * from tb_user where name like '%aa%';

EXPLAIN select name, age, email from tb_user where name like '%aa%';

#9. 字符串不加单引号索引失效

select * from staffs where name ='2000';

EXPLAIN select * from staffs where name ='2000';

#索引会失效 会自动类型转换 请看第三条

#varchar 必须使用单引号

select * from staffs where name =2000;

EXPLAIN select * from staffs where name =2000;

#10. 少用 or, 用它来连接时会索引失效

select * from staffs where name ='张三' or name ='李四';

EXPLAIN select * from staffs where name ='张三' or name ='李四';



总结 

![image-20200602174810549](F:\学习笔记\MySql笔记\image-20200602174810549.png)





![image-20200602191750742](F:\学习笔记\MySql笔记\image-20200602191750742.png)



create table test03 (
id int primary key not null auto_increment, 
c1 varchar(10),
c2 varchar(10),
c3 varchar(10),
c4 varchar(10),
c5 varchar(10)
);

insert into test03 (c1, c2, c3, c4, c5) values ('a1', 'a2', 'a3', 'a4', 'a5');
insert into test03 (c1, c2, c3, c4, c5) values ('b1', 'b2', 'b3', 'b4', 'b5');
insert into test03 (c1, c2, c3, c4, c5) values ('c1', 'c2', 'c3', 'c4', 'c5');
insert into test03 (c1, c2, c3, c4, c5) values ('d1', 'd2', 'd3', 'd4', 'd5');
insert into test03 (c1, c2, c3, c4, c5) values ('e1', 'e2', 'e3', 'e4', 'e5');

select * from test03;

create index idx_test03_c1234 on test03(c1, c2, c3, c4);


SHOW index from test03;

select * from test03 where c1='a1'and c2='a2'and  c3='a3'and  c4='a4';

(1)
explain select * from test03 where c1='a1';
explain select * from test03 where c1='a1'and c2='a2';
explain select * from test03 where c1='a1'and c2='a2'and  c3='a3';


# 常量 虽然表面无差别 但最好怎么建顺序怎么来
(2)
explain select * from test03 where c1='a1'and c2='a2'and  c3='a3'and c4='a4';
explain select * from test03 where c1='a1'and c2='a2'and  c4='a4'and c3='a3';
explain select * from test03 where c4='a4'and c3='a3'and  c2='a2'and c1='a1';


##
(3)

#使用3个
explain select * from test03 where c1='a1'and c2='a2'and  c3>'a3'and c4='a4';

(4)
#使用4个
explain select * from test03 where c1='a1'and c2='a2'and  c4>'a4'and c3='a3';


(5) 定值、范围还是排序，一般order by是给一个范围
#使用2个
explain select * from test03 where c1='a1'and c2='a2'and  c4='a4' order by c3;


(6)

#使用2个
explain select * from test03 where c1='a1'and c2='a2' order by c3;


(7)


#使用2个 出现 Using filesort 事故
explain select * from test03 where c1='a1'and c2='a2' order by c4;

(8)
#使用1个
explain select * from test03 where c1='a1'and c5='a5' order by c2,c3;

#使用1个 出现 Using filesort 事故 索引的顺序不能变
explain select * from test03 where c1='a1'and c5='a5' order by c3,c2;

(9)

#两者相同
explain select * from test03 where c1='a1'and c2='a2' order by c2,c3;

explain select * from test03 where c1='a1'and c2='a2' and c5='a5' order by c2,c3;

(10)
#使用1个 其余正常
explain select * from test03 where c1='a1'and c2='a2' and c5='a5' order by c2,c3;

#使用1个  出现严重事故 出现 Using filesort
explain select * from test03 where c1='a1'and c5='a5' order by c3,c2;

#c2已经是常量不需要排序  像 order by c3,10 ;
explain select * from test03 where c1='a1'and c2='a2' and c5='a5' order by c3,c2;

(11) group by

#使用1个
explain select * from test03 where c1='a1'and c4='a4' group by c2,c3;

#使用1个 出现严重事故 1.出现临时表和文件排序 temporary&& filesort
explain select * from test03 where c1='a1'and c4='a4' group by c3,c2;

#错误
#explain select * from test03 where c1='a1'and c4='a4' group by c2,c3
> 1055 - Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'oemp.test03.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
> /*网上出现较多的解决办法： 

1.查询mysql 相关mode

select @@global.sql_mode;
可以看到模式中包含了ONLY_FULL_GROUP_BY，只要没有这个配置即可。 
我的Mysql版本是8.0.11，默认是带了ONLY_FULL_GROUP_BY模式。

 ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,
 ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION 
2.重设模式值

set @@global.sql_mode=`STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE, 
ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION`;
3.重启Mysql即可
但，我的按照这个流程试了很多遍，无法去掉“ONLY_FULL_GROUP_BY”，执行set方法会出现下面错误。

后来在网上看到一篇文章，是修改my.ini,后面跟着改，结果可行。具体方法如下：

1.在my.ini文件中添加下面一句

[mysqld]
sql_mode = STRICT_TRANS_TABLES，NO_ZERO_IN_DATE，NO_ZERO_DATE，ERROR_FOR_DIVISION_BY_ZERO，NO_AUTO_CREATE_USER，NO_ENGINE_SUBSTITUTION

2.重启mysql服务*/

总结：1.定值、范围还是排序，一般order by是给一个范围
		  2.group by 基本上都是需要排序的， 会有临时表产生



### 查询截取分析

![image-20200602192303566](F:\学习笔记\MySql笔记\image-20200602192303566.png)





# 查询优化
# 永远小表驱动大表 类似嵌套循环 Nested Loop
use oemp;

1. in 和 exists 
select * from tb_emp;

select * from tb_dept;

select * from  tb_emp where id in(select id from tb_dept);

select * from  tb_emp e where exists (select 1 from tb_dept d where e.dept_id = d.id);


2.order by 子句，尽量使用 index 方式排序， 避免使用filesort 方式排序


# 建表SQL
create table tb_a(
id int primary key not null auto_increment,
age int,
birth timestamp not null
);

insert into tb_a(age, birth) values (22, now());
insert into tb_a(age, birth) values (23, now());
insert into tb_a(age, birth) values (24, now());

create index idx_a_ab on tb_a(age, birth);

select * from tb_a;

explain select * from tb_a where age >20 order by age ; 

explain select * from tb_a where age >20 order by age, birth;

#产生Using filesort
explain select * from tb_a where age >20 order by birth;
#产生Using filesort
explain select * from tb_a where age >20 order by birth ,age ;
#产生Using filesort
explain select * from tb_a order by birth ;
# 2020-06-02 20:43:22
#产生Using filesort
explain select * from tb_a where birth> '2020-06-02 20:43:22' order by birth ;
#没有 产生Using filesort
explain select * from tb_a where birth> '2020-06-02 20:43:22' order by age ;
#产生Using filesort  order by 默认升序
explain select * from tb_a order by age asc,birth desc;

#MySQL 支持两种方式的排序， FileSort 和 Index,  Index 效率高
指 MySQL 扫描索引本省完成排序， FlleSort 方式效率低

#ORDER BY 满足的两种情况， 会使用Index方式排序
1.ORDER BY 语句使用索引最左前列
2.使用 Where 子句与 Order By 子句条件列组合满足索引左前列



![image-20200602211443891](F:\学习笔记\MySql笔记\image-20200602211443891.png)



show variables like '%slow_query_log%';

#开启 慢日志查询 默认不开 当前仅仅开启当前会话 永久生效需改配置 .cnf
set global slow_query_log = 1;

#设置sql查询的时长最小
show VARIABLES like 'long_query_time%';

set global long_query_time =3;


select sleep(4);

show global status like '%Slow_queries%';



**查看**

![image-20200602215814917](F:\学习笔记\MySql笔记\image-20200602215814917.png)



#日志分析工具 mysqldumpslow
#linux 命令
mysqldumpslow --help



#插入1000w条数据
create database big_data;
use big_data;

# dept
create table dept(
			id int primary key auto_increment,
			deptno mediumint not null default 0,
			dname varchar(20) not null default '',
			loc varchar(13) not null default ''
) engine = innodb default charset = utf8;

# emp
create table emp(
				id int primary key auto_increment,
				empno mediumint not null default 0,
				ename varchar(20) not null default '',
				job varchar(9) not null default '' comment '工作',
				mgr mediumint not null default 0 comment '上级编号',
				hirdate date not null comment '入职时间',
				sal decimal(18,2) not null comment '薪水',
				comm decimal(18,2) not null comment '红利',
				deptno mediumint not null default 0 comment '部门编号'
) engine = innodb default charset = utf8;


show variables like 'log_bin_trust_function_creators';

set global log_bin_trust_function_creators = 1;

SELECT * FROM emp;

#使用函数 随机字符串
select now() ;

delimiter $$
create function rand_str(n int) returns varchar(255)
begin 
	  declare chars_str varchar(100) DEFAULT    'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
		declare return_str varchar(255) DEFAULT '';
		declare i int default 0 ;
		while i<n 
		do
		set  return_str = concat(return_str,SUBSTRING(chars_str,floor(1+rand()*52),1));
		set i = i+1;
		end while;
		return return_str;
end $$

drop function rand_string;


select  rand_str(10);

# 随机产生部门编号

delimiter $$
create function rand_num() returns int(5)
begin
    declare i int default 0;
		set i = floor(100+rand()*10);
		return i;
end $$
		
select  rand_num();	


#创建存储过程

#表 dept
delimiter $$
create procedure insert_dept(in start int(10), in max_num int(10))
begin 
declare i int default 0;
set autocommit = 0;
repeat 
set i = i+1;
insert into dept (deptno, dname, loc)
values ((start+i), rand_num() , rand_str(6));
until i = max_num
end repeat;
commit;
end $$



#表 emp
delimiter $$
create procedure insert_emp(in start int(10), in max_num int(10))
begin 
declare i int default 0;
set autocommit = 0;
repeat 
set i = i+1;
insert into emp (empno, ename, job , mgr , hirdate , sal , comm , deptno)
values ((start+i), rand_str(6), 'SALESMAN', 001, curdate(), 2000, 400, rand_num());
until i = max_num
end repeat;
commit;
end $$


delimiter ;

call insert_dept(100, 10);

select * from  emp;

#truncate table wp_comments;

truncate table dept;

# dept 表中插入数据
call insert_dept(100, 1000000);
call insert_dept(2000000, 1000000);
call insert_dept(3000000, 1000000);
call insert_dept(4000000, 1000000);
call insert_dept(5000000, 1000000);
call insert_dept(6000000, 1000000);
call insert_dept(7000000, 1000000);
call insert_dept(8000000, 1000000);
call insert_dept(9000000, 1000000);


call insert_dept(100, 10);

# emp 表中插入数据
call insert_emp(100, 1000000);
call insert_emp(2000000, 1000000);
call insert_emp(3000000, 1000000);
call insert_emp(4000000, 1000000);
call insert_emp(5000000, 1000000);
call insert_emp(6000000, 1000000);
call insert_emp(7000000, 1000000);
call insert_emp(8000000, 1000000);
call insert_emp(9000000, 1000000);
call insert_emp(10000000, 1000000);



show profiles;

show variables like 'profiling';

show tables;

select * from dept limit 1000;

call insert_dept(100, 1000000);


select * from dept where id%10  ORDER BY id desc  limit 150000;

show profiles;


#细致化查看

#日常开发需要注意的事项

#1.converting HEAP to MyISAM 查询结果太大，内存都不够用了往磁盘上面搬了
#2.Create tmp table 创建临时表
#3.Copying to tmp table on disk 把内存中的临时表复制到磁盘， 危险！！！！

show profile cpu,block io for query 87;

#全局查询日志

#临时开启
#
set global general_log =1;
set global log_output='TABLE';

#永久开启在my.cnf 配置
#永远不要在生产环境启用这个功能

#查看 
select * from mysql.general_log;





lock table mylock read; 

show open tables;

unlock tables;

select * from mylock;

update mylock set name ='test' where id =2; 
#读索 共享
#当前会话 可读 不可写 其他会话 可读 不可写（阻塞）


lock table mylock write;

show open tables;


select * from mylock;

update mylock set name ='b' where id =1;

unlock tables;
#写索 不共享
#当前会话 不可读（阻塞） 可写 其他会话 不可读（阻塞） 不可写（阻塞）

show status like 'table%';





#行锁
create table test_innodb_lock (
id int primary key auto_increment,
b varchar(16)
)engine = innodb;

insert into test_innodb_lock values (1, 'b2');
insert into test_innodb_lock values (2, '3');
insert into test_innodb_lock values (3, '3000');
insert into test_innodb_lock values (4, '4000');
insert into test_innodb_lock values (5, '5000');
insert into test_innodb_lock values (6, '6000');
insert into test_innodb_lock values (7, '7000');
insert into test_innodb_lock values (8, '8000');
insert into test_innodb_lock values (9, '9000');
insert into test_innodb_lock values (10, 'b1');


select * from test_innodb_lock;

#show open tables;

#关闭自动提交
set autocommit =0;


select * from test_innodb_lock;


update test_innodb_lock set b = 4001 where id =4;

commit;


update test_innodb_lock set b = 4002 where id =4;

select * from test_innodb_lock;

commit;

show index from test_innodb_lock;

#索引失效会导致行锁变表锁
#如果添加索引 如果修改varchar列不加 '' 号 自动类型转换  索引失效会导致行锁变表锁



#间隙锁的危害



#面试题：如何锁定一行
#手工上锁

select * from test_innodb_lock where id = 4 for update

行锁分析

【如何分析行锁定】
通过检查 InnoDB_row_lock 状态变量来分析系统上的行锁的争夺情况。



对各个状态量的说明如下：

Innodb_row_lock_current_waits : 当前正在等待锁定的数量；
Innodb_row_lock_time： 从系统启动到现在锁定总时间长度；
Innodb_row_lock_time_avg： 每次等待所花费的平均时间；
Innodb_row_lock_time_max: 从系统启动到现在等待最长的一次所花的时间；
Innodb_row_lock_waits：系统启动到现在总共等待的次数；

对于这5个状态变量，比较重要的是：

Innodb_row_lock_time_avg (等待平均时长）
Innodb_row_lock_waits (等待总次数）
Innodb_row_lock_time (等待时长）

尤其是当等待次数很高，而且每次等待时长也不小的时候，我们就需要分析系统中为什么有如此多的等待，然后分析等待着手指定优化计划。


show status like 'innodb_row_lock%';



主从复制








































​			



