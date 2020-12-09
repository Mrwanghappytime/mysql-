## mysql必知必会

#### 1.mysql的使用

- **mysql登录**

  ```c
  mysql -u [username] -p 随后输入密码
  ```

- **选择数据库**

  ```sql
  use database(name);
  ```

- **show命令**

  ```sql
  show databases;//查看所有的数据库
  show tables;//查看对应数据的所有表
  describe table_name;//查看表的信息，也可用show columns from table_name;
  show grants;//查看权限
  ```

#### 2.select命令（查询）	

```sql
select column_name[,column_name] from table_name;(sql语句分为多行可以增加易读性,默认不排序，输出可能为写入数据库的顺序)
select * from table_name; (查看所有列)
//DISTINCT关键字
select DISTINCT columns_name from table_name;同时一个值只出现一次，当后接多个列名时，当所有的列都不相同才不显示
//limit关键字，用于限制输出多少行
select columns_name from table_name limit number;//表示最多输出number行
select columns_name from table_name limit number1,number2//表示输出在number1行开始的number2行，从0开始
select table.column from database.table;//这为全限定命名的方式


//排序order by
select colomns from table_name order by columns_name desc/ASC //order by 的列可以不被查看，且合法，order by 可指定多个列，当指定多个列，前面的列先起作用，每个列需单独指定升序和降序

//WHERE =等于 !=不等于 <>不等于 <小于 >大于 between 位于某某之间 //between 表示前后都包括，条件之间用and连接条件还可以为is null 和 is not null
select columns from table_name where columns 条件
//and运算符用于多个条件同时满足 or表示或，当多个and和or时，and的优先级比or高，需要or优先级高的情况可以用()
select columns from table_name where 条件1 or 条件2 and 条件3;
select columns from table_name where (条件1 or 条件2) and 条件3;
//上述两种语句的使用情况不一样，最好都是用括号，用于好分辨;
//in运算符
select columns from table_name where x in()；

//like
select columns from table_name where x like 通配符;

% 用于匹配任意字符除去null
_ 只能匹配一个字符
//REGEXP 用于匹配正则表达式


//计算字段concat();
select concat(id,'(' , name ,')') from book order by id limit 3;
//rtrim用于去除两侧的空格
//AS用于将表达式作为某个列输出四则运算也可以使用
now()用于表示当前时间

//mysql时间函数
mysql时间处理函数：mysql日期表示可以为yyyy-mm-dd 此为标准格式，例如当用日期作为where条件时
select date from tablename where date(date) = 'yyyy-mm-dd'
select id , adddate(curdate(),number) as date from book;
adddate(); //增加一个日期
select id , addtime(time(),0) as date from book;
addtime(); //增加一个time
selec id , curdate() as date from book;
curdate(); //返回当前日期
curtime(); //获取当前time
date(); //返回日期时间的日期部分
datediff(); //计算日期只差
date_add(); //高度灵活的日期运算符

date_format() ;// 日期事件格式转换
day(); //返回一个日期的天数部分
dayofweek(); //返回星期几
hour(); //返回时间的小时部分
minute();//返回时间的分钟部分
month(); //返回时间的月份部分
now();//返回时间和日期
second() ; // 返回秒部分
time(); // 返回时间部分
year(); //返回年份部分

```

### 3.select高级函数

```sql
AVG();//返回平均值
select avg(shuxing) from table_name;
count();//返回行数 count(*) 会计算null count(columns_name)不会计算null
select count(*) as count from table_name;
max();//返回某列的最大值
min();//返回某列的最小值
sum();返回某列的总数
select count(*) as count , sum(id) as sum ,avg(id) as avg min(id) as min,max(id) as max from user;
//上述函数可以使用distinct进行修饰，例如avg(distinct columns)

//group by 用于对聚合函数进行修饰，用于分组
select password ,id,count(password) from user group by password,id ;
//having 用于对group by数据后的条件限制
select password , count(password) from user group by password having count(password) > 1

//自己总结如下：
select sum(password*id) as sum ,max(password*id) as max from user group by password;
mysql select修饰符有哪些，他们的顺序为如何？
修饰符除了select * from tables;顺序为：
where > group by > having > order by > limit;
select count(*) from user where id > 3 goup by password having count(*) > 1 order by count(*) desc limit 1;
```

### 4.使用子查询

```
主要有两种用法
1.select * from tables where id in (select id from table)
2.select id ,(select count(*) from article where article.id = article_tags.aid) from article//后续如何修改？
```

### 5.联结表

```sql
1.直接连接
use vueblog2;
select article.id,article_tags.id from article,article_tags where article.id = article_tags.aid 返回article和article——Tags笛卡尔积 叫做cross join（叉链接）
2.内部连接
select article.id,article_tags.id from article inner join article_tags on article.id = article_tag.id;//可以达到和上述一样的效果，但是下者可能效率更加高效
3.table也可以有别名
例如：select id from article as a where a.id > 10
4.自联结，将自己作为两个表，满足一定的条件进行连接
select a1.id , a2.id from article as a1,article as a2 where a1.id = a2.id and a1.id > 100;或者
select a1.id ,a2.id from article as a1 inner join article as a2 on a1.id = a2.id;
5.外联阶，分为两种，一种为左连接 left outer join 一种为有链接 right outer join
左连接以左表为标准，右连接以右表为标准，不为标准的表可以为null
语法为 left outer join on conditon / right outer join on condition
select a.id , a2.id from article as a left outer join article_tags as a2 on a1.id = a2.aid;//当一个表中存在以另一个表的主键的外键时，可以将此表作为标准
//组合查询
将多个查询记录集合在几个表中，并集
格式为select语句 union select2 [union select]
union 会默认去掉重复的行，union all可以显示重复的行
全文搜索在后续面试题中在进行学习
```

### 6.insert，update，del（相比较而言select更加重要）

```
insert into table_name values();
insert info table_Name(columns) values(values);//在插入自动增长的主键时可以用null，使mysql自己得到主键
多条insert语句可以写成
insert into table_Name(columns) values(values),(values);也可以
insert info tables_name(columns) select columns from tables_name;
//insert del update比较耗费资源，LOW_PRIORITY
insert LOW_PRIORITY into 可降低insert的优先级

update table_name set columns_value = '' where;
//当update多个列时，当某个列出先错误，则表回到更新前，可用ignore规避这种现象
delete from table_name where condition 从表中删除行
删除所有行可以delect from table where 1=1;但是效率极低
可用truncate table,此为删除表，然后重新建立一张表


```

### 7.创建表和视图

```sql
create table if not exists table_name (
	id int not null auto_increment,
	name char(30) not null,
	address char(50) not null,
	city char(50) not null,
	primary key(id)
)engine=InnoDB;

也可以用组合主键
primary key(key1,key2)//表示key1和key2的组合不能有相同的值
在使用自动增长主键的时候可以使用last_insert_id()获取最后一个插入的主键
2。mysql 引擎类型
在不执行engine=InnoDB时，此时默认的引擎为MyISAM
InnoDb支持事务，但是不支持全文搜索
MyISAN支持全文搜索，但是不支持事务
外键不能跨引擎

3.更新表
ALTER TABLE TABLE_NAME ADD VEND CHAR(20);//增加列
ALTER TABLE TABLE_NAME DROP COLUMN VEND; //删除列
alter table table_name add constraint constraint_name forgien key (column) refrence table_name2 (column);
4.删除表
drop table table_name;
5.重命名表
rename table table_Name to table_name2
6.视图（需满足下面的条件）
与表一样，视图必须唯一命名（不能给视图取与别的视图或表相
同的名字）。  对于可以创建的视图数目没有限制。
 为了创建视图，必须具有足够的访问权限。这些限制通常由数据
库管理人员授予。
 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造
一个视图。
 ORDER BY可以用在视图中，但如果从该视图检索数据SELECT中也
含有ORDER BY，那么该视图中的ORDER BY将被覆盖。
 视图不能索引，也不能有关联的触发器或默认值。
 视图可以和表一起使用。例如，编写一条联结表和视图的SELECT
语句。
show create view viewname;
DROP VIEW VIEWNAME;
CREATE OR REPLACE VIEW;  用于替换试图
create view viewname as select（连接语句）
视图在多数情况下用来进行查询，查询方式和表一致，更新则会更新到基表，但是当使用了一些条件后会导致无法更新
```

### 8.存储过程

```sql
存储过程，sql程序的集合，类似于包装函数
创建过程如下：
create procedure avg_id()
begin
	select avg(id) from article;
end//
如果直接输入上述语句，mysql会报错，此时我们可以修改mysql语句终止符用来确保语句执行成功，但是在切换后必须切回来，用delimiter  后街字符表示，但是不能用字符\
比如delimiter // delimiter ;
使用call procedure_name用来调用存储过程

删除存储过程为 drop procedure arg_id;
下面的代码为手写几段存储过程代码
```

```sql
--  同时有in参数和out参数
delimiter //
create procedure avg_id(IN inid INT,OUT avg_id INT)
begin 
	select avg(id) from article where id > inid into avg_id;
end//

create procedure jisuan(in inid INT,OUT avg_id int,out countid int,out total int)
begin
	select avg(id)  from article where id > inid into avg_id;
	select count(*) from article where id > inid into countid;
	select avg_id*countid into total;
end//

delimiter ;
传递参数的方式，以jisuan为例
call jisuan(100,@avgid,@countid,@total);
获取参数方式
select @avgid;其中@字符不能忘记
```

### 9.触发器

```
1.定义，在更新表时使其自动的执行一些内容（update,delete,insert），通俗意义上将，在事件发生时，触发器自动触发,所以触发器主要分为三种为update insert delete

2.创建触发器和删除触发器
create trigger trigger_name;
drop trigger trigger_name;

3.insert触发器,在触发器中可以访问一个new的虚拟表，insert触发器可以选择之前之后都可以
例如：
create trigger trigger1 before insert on article for each row select new.id;

4.delete可以访问一张old表，可读不可写

5。update 同时满足上者
//后续需进行补强
```

### 10.mysql事务

```sql
1.并非所有的引擎都支持事务，其中MyIsam不支持事务，InnoDB支持事务，下面为事务的一些关键词
事务：一组sql语句
回退：指撤销指定SQL语句的过程
提交：指将未存储的SQL语句结果提交写入到数据表
保留点：指事务处理中设置的临时占位符（placeholder），你可以对它发布回退
2.可以使用start transaction开启一个事务，用rollback回退事务，但是事务回退的语句只有insert，update,delete语句，其他语句不可以回退，例如：
select id from article;
start transaction;
select id from article;
delete from article;
drop table ueer if exists;
rollback;
select id from article;
3.commit可以用于显示提交事务，事务不会隐形提交，但是sql语句会隐形提交
start transaction;
sql group;
commit;
4.保留点,可使事务回退到指定点，不会全部回退
start transaction;
select * from tags;
delete from tags;
savepoint ponitname;
select * from tags;
rollback;
select * from tags;

start transaction;
select * from tags;
delete from tags;
savepoint ponitname;
select * from tags;
rollback to pointname;
select * from tags;
讨论上述两者的区别；
5.默认的提交行为执行一条sql语句隐形提交，可以使用
set autocommit=0;改变这种情况，知道autocommit被设置成1为止;

例如
set autocommit=0;
insert into article(id) values(1);
set autocommit=1;//在此语句之前，前一句不会对具体的表生效
```

### 11.全球化和本地化

```sql
1.查看MySQL支持的字符集
show character set;
2.查看校对顺序
show collation;
//后续详解
```

### 12.安全管理*

```sql
1.管理用户
所有的用户都被存储在mysql数据库中，其中的表user为用户信息
use mysql
select * from user;
2.创建用户
create user ben identified by '123456' 其中123456为密码
3.修改用户的用户名：
rename user ben to newname;
4.删除用户
drop user newname;
5.查看用户权限
show grants for ben;
6.修改权限
grant select on vueblog2.* to ben;
7.撤销权限
revoke select on vueblog2.* to ben;
GRANT和REVOKE可在几个层次上控制访问权限：
整个服务器，使用GRANT ALL和REVOKE ALL； 
整个数据库，使用ON database.*；
特定的表，使用ON database.table； 
特定的列；
特定的存储过程
8.权限如下图
```



![image-20201209223159408](C:\Users\汪浩锋\AppData\Roaming\Typora\typora-user-images\image-20201209223159408.png)

### 