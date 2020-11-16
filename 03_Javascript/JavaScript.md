# JavaScript

## 1.JSON

json:JavaScript Object Notation(JavaScript对象标记法)

json是一种存储和交换数据的语法。

json是通过JavaScript对象标记法书写的文本。

JSON属于文本，可以将任何JavaScript对象转换为JSON，然后发送到服务器。

也可以将从服务器接收到的JSON转换为JavaScript对象。

将JavaScript对象转换为JSON，使用JSON.stringfy()。

将JSON转换为JavaScript对象，使用JSON.parse()。

## 2.数组的迭代方法

every():对数组的每一项运行给定函数，若该函数对每一项都返回true，则返回true。

filter():对数组的每一项运行给定函数，返回使该函数返回true的项组成的数组。

forEach():对数组中，每一项运行给定函数，没有返回值。

map():对数组中每一项运行给定函数，返回每次调用函数的结果组成的数组。

some():对数组的每一项运行给定函数，如果该函数对任一项返回true，则返回true。

以上5个方法，不会改变原数组中的值，且都接受两个参数：

1.要在数组每一项上运行的函数。

2.（可选）运行该函数的作用域。

其中，作为参数1的函数，接受三个参数：

数组项的值、数组项的索引、以及数组对象本身。

例子：

```js

```

