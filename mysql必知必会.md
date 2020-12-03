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







```

​		

























