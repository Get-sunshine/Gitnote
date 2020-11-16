# 第四章 检索数据

## 4.1  SELECT 语句

​	大概，最经常使用的SQL语句就是SELECT语句了，它的用途是从一个或多个表中检索信息。

​	为了使用SELECT检索表数据，必须至少给出两条信息--想选择什么，以及从什么地方选择。

## 4.2 检索单个列

​	select prod_name from products;

​	所需的列名，在select关键字后给出，from关键字之处从其中检索数据的表名。

​	**结束SQL语句** 多条SQL语句必须加（;）分隔。MySQL如同多数DBMS一样，不需要在单条SQL语句后加分号，但是特定的DBMS可能必须在单条SQL语句后加上分号。可以总是加上分号。如果使用的是mysql命令行，必须加上分号。

​	**SQL语句大小写** SQL语句是不区分大小写的。但是开发人员喜欢对所有的SQL关键字使用大写，而对所有的列和表名使用小写，这样使代码更容易调试和阅读。

​	**使用空格** 处理SQL语句时，其中所有的空格都将被忽略。SQL语句可以在一行上给出，也可以分成许多行。

## 4.3  检索多个列

​	select prod_id,prod_name,prod_price from products;

## 4.4 检索所有列

​	select * from products;

​	如果给定一个通配符（*），则返回表中所有的列。

​	使用通配符有个大优点，由于不明确指定列名，所以能检索出名字未知的列。

## 4.5 检索不同的行

​	去重，使用distinct关键字。

​	select distinct vend_id from products;

​	此语句告诉MySQL返回不同的vend_id行。如果使用distinct关键字，它必须直接放在列名的前面。

​	**不能部分使用distinct**  distinct关键字应用于所有列而不仅是前置它的列。如果给出select distinct vend_id,prod_price,除非指定的两个列不同，否则所有行都将被检索出来。（说的我太迷糊了。。。）

## 4.6 限制结果

​	select prod_name from products limt 5;

​	返回不多于五行的数据。（因为如果没有五行，就返回不了那么多。）

​	select prod_name from products limit 5,5;

​	返回从第五行（包含第五行）开始的五行。第一个数为开始位置，第二个数为要检索的行数。MySQL的行是从0开始的。

​	 **MySQL 5的limit语法** MySQL 5 为了更清晰的表达语义，使用limit的另一种语法：limit 4 offset 3. 代表选取从第3行开始的4行数据。

## 4.7 完全限定的表名

​	同时使用表名和列名

​	select products.prod_name from products;

​	同时使用数据库名和表名

​	select products.prod_name from crashcourse.products;

