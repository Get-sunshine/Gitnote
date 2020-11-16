# 第六章 过滤数据

## 6.1 使用where子句

​	只检索所需数据需要指定搜索条件，搜索条件也称为过滤条件。

```mysql
select prod_name,prod_pric
from products
where prod_price =2.50;
```

​	只返回prod_price的值为2.50的行。

​	**where子句的位置** 在使用order by和where子句时，应该让order by 位于where 之后，否则会产生错误。

## 6.2 where 子句操作符

| 操作符  |        说明        |
| :-----: | :----------------: |
|    =    |        等于        |
|   <>    |       不等于       |
|   !=    |       不等于       |
|    <    |        小于        |
|   <=    |      小于等于      |
|    >    |        大于        |
|   >=    |      大于等于      |
| between | 在指定的两个值之间 |
|         |                    |

### 6.2.1 检查单个值

```mysql
select prod_name,prod_price
from products
where prod_name ='fuses';
```

MySQL在执行匹配时默认不区分大小写。fuses和Fuses是一样的。

### 6.2.2 不匹配检查

```mysql
select vend_id,prod_name
from products
where vend_id <> 1003;
```

查询结果为不是由供应商1003制造的所有产品。

**何时使用引号** 有的值括在单引号之内，而有的值未括起来。单引号用来限定字符串。如果将值与串类型的列进行比较，则需要限定引号。用来与数值列进行比较的值则不需要用引号。

使用!=

```mysql
select vend_id,prod_name
from products
where vend_id !=1003;
```

### 6.2.3 范围值检查

使用between检索价格在5美元到10美元之间的所有产品。

```mysql
select prod_name,prod_price
from products
where prod_price between 5 and 10;
```

between匹配范围中的所有值，包括指定的开始值和结束值。

### 6.2.4 空值检查

一个列不包含值时，称其为包含空值NULL。

**NULL 无值（no value）** 它与字段包含0，空字符串或仅包含空格不同。

```mysql
select prod_name
from products
where prod_price is null;
```

**NULL 与不匹配** 在通过过滤选择出不具有特定值的行时，你可能希望返回具有NULL值的行。但是，不行。因为未知具有特殊的含义，数据库不知道它们是否匹配，所以在匹配过滤或不匹配过滤时不返回它们。

因此，在过滤数据时，一定要验证返回数据中确实给出了被过滤列具有NULL的行。