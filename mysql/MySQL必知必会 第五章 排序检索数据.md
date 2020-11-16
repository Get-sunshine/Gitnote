# 第五章 排序检索数据

## 5.1 排序数据

​	select prod_name from products;

​	检索出来的数据没有特定的顺序。

​	**子句（clause）** SQL语句由子句构成，有些子句是必须的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。例如select语句的from子句。

​	使用order by 子句排序。

​	select prod_name fom products order by prod_name;

​	这条语句指示MySQL对prod_name列以字母顺序排列数据。

​	**通过非选择列进行排序** 通常，order by 子句中使用的列将是为显示所选择的列。但是，实际上不一定要这样做，用非检索的列排序数据也是完全合法的。

## 5.2 按多个列排序

​	select prod_id,prod_price,prod_name from products 

​	order by prod_price,prod_name;

​	这条语句指示MySQL先按照prod_price排序，再在相同的prod_price的数据中按prod_name排序。

##  5.3 指定排序方向

​	升序排序（A-Z），这是默认的排序顺序，asc.

​	降序排序（Z-A），用desc，关键字指定。

​	select prod_id,prod_price,prod_name

​	from products 

​	order by prod_pice desc;

​	desc关键字只应用到直接位于其前面的列名。

​	select prod_id,prod_price,prod_name

​	from products

​	order by prod_price desc,prod_name;

​	**在多个列上使用降序** 如果想在多个列上使用降序排序，必须对每个列指定desc关键字。