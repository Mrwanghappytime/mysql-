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



























