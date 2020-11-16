# JavaScript高级程序设计

## JavaScript

一个完整的JavaScript实现由三部分组成：

- 核心（ECMAScript）
- 文档对象模型（DOM）
- 浏览器对象模型（BOM）

## 3.第3章 基本概念

### 3.1 语法

#### 3.1.1 区分大小写

#### 3.1.2 标识符

#### 3.1.3 注释

#### 3.1.4 严格模式

#### 3.1.5 语句

### 3.2 关键字和保留字

### 3.3 变量

ECMAScript的变量是松散类型的，即一个变量可以保存任何类型的值。定义变量使用var 关键字，后跟变量名。例：

```js
var message
```

直接初始化变量。

```js
var message = 'hi'
```

上述的初始化变量，不会将message标记为字符串类型，仅仅是给message赋值。可以修改message的值的同时修改值类型。

```js
var message = 'hi'
message = 123 // 有效，但不推荐
```

注意,使用var操作符定义的变量将成为定义该变量作用域中的局部变量。即，在函数中使用var定义变量，函数退出后，变量被销毁。

```js
function test(){
	var a=1 // 局部变量
}
test()
alert(a) // error
```

省略var操作符，创建全局变量。

```js
function test(){
	a='hi' // 全局变量，不推荐这样写。严格模式时，会抛出错误。
}
test()
alert(a) // 'hi'
```

一条语句定义多个变量,逗号分隔变量，变量可初始化，可不初始化。

```js
var message = 'hi',found = false,age = 29
```

### 3.4 数据类型

ECMAScript中五种基本数据类型：Undefined、NULL、Boolean、Number、String。

一种复杂数据类型：Object。

#### 3.4.1 typeof操作符

检测给定变量的数据类型--typeof操作符，返回一个字符串。

- "undefined"------若这个值未定义。
- "boolean"-------若这个值是布尔值。
- "string"------若这个值是字符串。
- "number"------若这个值是数值。
- "object"-----若这个值是对象或者null。
- "function"----若这个值是函数

例子：

```js
var message = 'some string'
alert(typeof message) // "string"
alert(typeof(message))// "string"
alert(typeof 95) // "number"
```

typeof 操作符的操作数可以是一个变量，也可以是数值字面量。圆括号，可以使用，也可以不用。

#### 3.4.2 Undefined类型

Undefined类型只有一个值，即undefined。

未初始化的变量，值默认就是undefined。

```js
var message
alert(message == undefined) // true
```

声明变量，赋值为undefined。

```js
var message = undefined
alert (message == undefined) // true
```

对未初始化的变量执行typeof操作符会返回undefined，对未声明的变量执行typeof操作符同样会返回undefined。

```js
var message // 默认值为undefined
// var age
alert(typeof message) // "undefined"
alert(typeof age) // "undefined"
```

#### 3.4.3 Null类型

Null类型只有一个值，即null。

从逻辑角度来看，null表示一个空对象指针，如果定义的变量准备在将来用以保存一个对象，最好将其初始化为null值。

实际上，undefined值是派生于null的。ECMA-262规定对它们的相等性应该返回true。

```js
alert(null==undefined) // true
```

#### 3.4.4 Boolean类型

该类型只有两个值，true和false。true不一定是1，false不一定是0。

尽管Boolean类型只有两个值，但是可以对其他任何数据类型的值调用Boolean()转型函数，返回一个Boolean值。

```js
var message='hi'
var messageAsBoolean=Boolean(message)
```

转换规则：

| 数据类型  | 转换为true的值              | 转换为false的值 |
| --------- | --------------------------- | --------------- |
| Boolean   | true                        | false           |
| String    | 非空字符串                  | "" （空字符串） |
| Number    | 任何非0数字值（包含无穷大） | 0和NaN          |
| Object    | 任何对象                    | null            |
| Undefined | 不适用                      | undefined       |

自动执行的Boolean转换：

```js
var message='hi'
if(message){
	alert('Value is true!')
}
```
#### 3.4.5 Number类型

这种类型使用IEEE754来表示整数和浮点数值。ECMA-262定义了不同的数值字面量格式。

十进制整数：

```js
var intNum=55 // 整数
```

八进制整数，八进制字面值第一位必须是0，然后是八进制数字序列（0-7）。若字面值中的数值超出范围，则前导0被忽略，后面的数值将被当做十进制数解析。

```js
var octalNum1 = 070 // 八进制的56
var octalNum2 = 079 // 无效八进制数值--解析为79
var octaNum3 = 08 // 无效的八进制数值--解析为8
```

八进制字面值在严格模式下无效。

十六进制，十六进制字面值的前两位必须是0x,后跟任何十六进制数字（0-f），其中如字母f可以小写，可以大写。

```js
var hexNum1=0xA // 十六进制的10
var hexNum2=ox1f // 十六进制的31
```

进行算术计算时，所有以八进制和十六进制表示的数值都将被转换为十进制数值。

JS中可以保存正零和负零。+0和-0被认为是相等的。

##### 1.浮点数值

浮点数值，即数值中必须包含一个小数点，且小数点后必须至少有一位数字。小数点前可以没有数字，但不推荐。

```js
var floatNum1=1.1
var floatNum2=0.1
var floatNum3=.1 // 有效，不推荐。
```

小数点后没有任何数字，会转换为整数。若浮点数本身就是一个整数（1.0），也会转换为整数。

```js
var floatNum1=1. // 小数点后没有数字--解析为1
var floatNum2=10.0 // 整数--解析为10
```

对于极大、极小的数字，使用e表示法（科学计数法）表示的浮点数值表示。e表示法表示的数字的数值：e前面的数值乘以10的指数次幂。

e前面的数值可以是整数也可以是浮点数，中间是大写或小写的字母E，后面是10的幂中的指数。

```js
var floatNum =3.125e7 // 即31250000
```

极小的数值：

```js
var num=3e-17 
```

默认，将小数点后带有6个零以上的浮点数值转换为以e表示法表示的数值。

浮点数值最高精度是17位小数，但算术计算时精确度远远不如整数。例，0.1加0.2，结果不是0.3，而是0.30000000000000004

```js
if(a+b==0.3){ // 不要做这样的测试
    alert("You got 0.3.")
}
```

若a和b是0.05和0.25或者是0.15和0.15就可以。为什么？

##### 2.数值范围

ECMAScript中最大值保存在：Number.MIN_VALUE中，最小值保存在：Number.MAX_VALUE中。

若计算结果超出数值范围，则数值被转换为特殊的Infinity值。若为负，则-Infinity（负无穷），若为正，则+Infinity（正无穷）。

Infinity值不能参与运算，使用isFinite()函数确定一个值是否位于最大最小值之间，函数返回true或false。

```js
var res=Number.MAX_VALUE+Number.MAX_VALUE
alert(isFinite(res)) // false
```

##### 3.NaN

NaN,即Not a number,非数值，是一个特殊的数值，用于表示一个本来要返回数值的操作数未返回数值的情况。例如：任何数值除以0会返回NaN。

任何涉及NaN的操作都会返回NaN。

NaN与任何值都不相等，包括NaN本身。

```js
alert(NaN==NaN) // false
```

判断非数值，isNaN()函数。isNaN()接受一个参数后，尝试将这个值转换为数值，任何不能被转换为数值的值都将导致这个函数返回true。

```js
alert(isNaN(NaN)) // true
alert(isNaN(10)) // false 10是整数
alert(isNaN('10')) // false 可以被转换为10
alert(isNaN('blue')) // true 不能转换为数值
alert(isNaN(true)) // false 可以被转换为数值1
```

##### 4.数值转换 xx

#### 3.4.6 String类型 xx

#### 3.4.7 Object类型

对象其实就是一组数据和功能的集合。

对象可以通过执行new 操作符后跟要创建的对象类型名称来创建。创建Object类型的实例并为其添加属性和（或方法），就可以创建自定义对象。

```js
var o = new Object()
```

若不给构造函数传参，则可以省略圆括号。

```js
var o = new Object  // 有效，不推荐。
```

Object的每个实例都具有下列属性和方法：

- constructor：保存着用于创建当前对象的函数。
- hasOwnProperty(propertyName):检查给定属性在当前对象实例中，是否存在。例：o.hasOwnProperty('name')
- isPrototypeOf(object):检查传入的对象是否是另一个对象的原型。
- propertyIsEnumerable(propertyName):检查给定属性是否能够使用for-in语句枚举。
- toLocaleString():返回对象的字符串表示，该字符串与执行环境地区对应。
- toString():返回对象的字符串表示。
- valueOf():返回对象的字符串、数值或布尔值表示。通常与toString方法的返回值相同。

在ECMAScript中Object是所有对象的基础，故，所有对象都具有这些基本的属性和方法。

### 3.5 操作符

### 3.6 语句

### 3.7  函数

#### 3.7.1理解参数

使用function关键字声明函数，后跟一组参数和函数体。

```js
function sayHi(name,message){
    alert('Hi '+name+' '+message)
}
```

通过函数名调用函数。

```
sayHi('Nick','How are you!')
```

通过return返回值

```js
function add(number1,number2){
	return number1+number2
}
```

调用函数，接受返回值。

```js
var result = add(1,2)
```

return语句后的代码不会被执行。return可以不带任何返回值，在这种情况之下，函数返回undefined值。一般用在需要提前停止函数执行，又不需要返回值的情况下。

ECMAScript函数不介意传递进函数的参数数量，也不介意参数的数据类型。即，如果定义的函数只接收两个参数，但在调用时，可以传递一个、两个、三个甚至不传参数也行。

在函数内部，参数是用一个数组来表示的。可以用arguments对象来访问这个参数数组。

arguments对象是一个类数组（类似数组），可以用[ ]语法，也有长度。arguments对象由传入的参数决定。

arguments可以和命名参数一起使用。

arguments的值与对应命名参数的值始终保持同步，但是这是单向的，即修改arguments的值会影响对应命名参数的值，但是，修改命名参数的值，对应arguments的值不会同步。  （好像不对，，自己动手输出的时候，，是互相影响的。。）

但是两个值对应的内存空间是相互独立的，仅仅是值会同步。

```js
function sum(num1,num2){
    arguments[1]=10
    return num1+num2
}
console.log(sum(1,2)) // 11
```

```js
function sum(num1,num2) {
        num2=10
        return num1+arguments[1]
    }
    console.log(sum(1,2)) // 3 不是3 应该是11
```

没有传递值的命名参数，默认为undefined。

严格模式下，arguments对象的值与对应命名参数的同步特性被取消。

ECMAScript中的所有参数传递都是值（无论是地址值还是实际值），不可能通过引用传递参数。

#### 3.7.2没有重载

在其他语言中，如Java，可以实现函数的重载。即函数可以同名，只要函数的参数类型或（和）数量不同就行。

但在ECMAScript中，若出现两个同名函数，则后面的函数定义有效。

利用arguments检测传入参数类型和数量做出不同反应，模仿重载。

## 4.第4章 变量、作用域和内存问题

### 4.1基本类型和引用类型的值

ECMAScript变量包含两种不同数据类型的值：基本类型值、引用类型值。

**基本类型值**：简单的数据段。（Undefined、Null、Boolean、Number、String）

**引用类型值**：可能由多个值构成的对象。

基本数据类型按值访问，可以操作保存在变量中实际的值。

引用数据类型按引用访问，是操作对象的引用而不是实际值。

#### 4.1.1动态的属性

对于引用类型的值，可以添加其属性和方法，也可以改变、删除其属性和方法。

```js
var person = new Object()
person.name='Nick'
alert(person.name) // "Nick"
```

给基本类型的值添加属性和方法是没有意义的。

#### 4.1.2复制变量值

基本类型值的复制：

```js
var num1=5
var num2=num1
```

num1和num2是相互独立的，互不影响。

**复制前：**

|      |                 |
| ---- | --------------- |
|      |                 |
|      |                 |
| num1 | 5（Number类型） |

**复制后:**

|      |               |
| ---- | ------------- |
|      |               |
| num2 | 5(Number类型) |
| num1 | 5(Number类型) |



引用类型值的复制：

```js
var obj1=new Object()
var obj2=obj1
obj2.name='Nick'
console.log(obj1.name) // "Nick"
```

引用类型变量在栈中存储了指向堆中对象的地址值，变量值复制之后，两个变量指向同一个对象，互相影响。

![1596161179625](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596161179625.png)

#### 4.1.3 传递参数

ECMAScript中所有函数的参数都是按值传递的。即，把函数外部的值复制给函数内部的参数。

传递基本类型值时，被传递的值复制给一个局部变量（即命名参数，或者用ES的话来说，就是arguments对象中的一个元素）。传递引用类型值时，被传递的值的地址复制给一个局部变量（命名参数），即局部变量和外部变量引用同一个对象。

```js
 function add(num){ // num参数就是函数的局部变量。
     num+=10
     return num
 }
var count=10
var res=add(count)
console.log(count) // 10
console.log(res) // 20
```

```js
function setName(obj){
    obj.name='Nick'
}
var person=new Object()
setName(person)
console.log(person.name)
```

如何证明参数是按值传递的？

```js
function setName(obj){
    obj.name='Nick'
    obj = new Object()
    obj.name='Jack'
}
var person = new Object()
setName(person)
console.log(person.name) // Nick
```

如果是按照引用传值的话，可以这么说：函数内部的obj就是person。当obj指向一个新对象时，person也指向一个新对象。故person.name应当是Jack，而现在是Nick，就说明参数是按值传递的，obj复制的是指向person 对象的地址。

#### 4.1.4检测类型

使用typeof检测基本数据类型很有用，可以检测字符串、数值、布尔值、undefined。但是typeof对null和Object都返回"object"。

```js
var a = 'abc'
var b = 1
var c = null
var d = undefined
var e = true
console.log( typeof a) // string
console.log(typeof b) // number
console.log(typeof c) // object
console.log(typeof d) // undefined
console.log(typeof e) // boolean
```

检测引用类型，使用instanceof操作符(根据原型链来识别的)。

若变量是给定引用类型的实例，则返回true，否则返回false。

```js
alert(person instanceof Object) // 变量person是Object吗？
alert(colors instanceof Array) // 变量colors是Array吗？
alert(pattern instanceof RegExp) // 变量pattern是RegExp吗？
```

所有引用类型的值都是Object的实例，因此检测引用类型值和Object构造函数比较时，都会返回true。

使用instanceof检测基本数据类型时，始终都返回false。

```js
var person = 1
console.log(person instanceof Number) // false
```

![1604026044978](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604026044978.png)

```js
function fn(){}
console.log(typeof fn) // function
```

### 4.2执行环境及作用域

执行环境，简称环境，定义了变量和函数有权访问的其他数据，决定了它们各自的行为。

每个执行环境都有一个变量对象与之关联，此对象中保存了环境中定义的全部变量和函数，我们无法访问此对象，但解析器处理数据时会在后台使用它。

全局执行环境是最外围的环境。根据ECMAScript实现所在宿主环境不同，表示全局环境的对象也不同。web浏览器中，window对象是全局环境，全局变量和函数都是作为window对象的属性和方法创建的。某个环境中的代码执行完毕之后，环境被销毁，其中的变量和函数也被销毁。

函数都有环境。代码在环境中执行时，会创建变量对象的作用域链。作用域链前端是当前执行代码所在环境变量对象。若此环境是函数，则将其活动对象作为变量对象。活动对象最开始只有arguments对象。

作用域中下一个变量对象来自包含（外部）环境，再下一个来自下一个包含环境。一直延伸到全局环境，全局环境关联的变量对象始终是作用域链中最后一个对象。

标识符解析沿作用域链一级一级搜索，从作用域链前端开始，一直回溯，直到找到对应标识符。

```js
 var color = 'blue'
 function changeColor(){
     if(color==='blue'){
         color='red'
         return
     }
     color='blue'
 }
changeColor()
console.log(color)
```

函数参数被当做变量对待，访问规则也如同函数(执行环境)中其他变量。

#### 4.2.1 延长作用域链

执行环境的类型总共只有两种，全区和局部（函数）。

#### 4.2.2没有块级作用域

##### 1.**声明变量**

var定义的变量自动添加到最近的执行环境，没有使用var则添加到全局环境。

```js
function add(num1,num2){
    var sum=num1+num2
    return sum
}
var res=add(1,2)
console.log(res) // 3
console.log(sum) // sum is not defined
```

```js
function add(num1,num2){
    sum=num1+num2
    return sum
}
var res=add(1,2)
console.log(res) // 3
console.log(sum) // 3
```

在严格模式下，初始化未经声明的变量会报错。

##### 2.查询标识符

当在某个环境中为了读取或写入而引用一个标识符时，必须通过搜索来确定标识符具体代表什么。

搜索的过程从作用域链的前端开始，向上逐级查询。如果在局部环境中找到了匹配的标识符，就停止搜索。如果没有找到就一直向上级搜索，直到全局环境。如果在全局环境中没有找到该标识符，则表示该标识符未声明。

例子：

```js
var color ='blue'
function getColor(){
    return color
}
console.log(getColor()) // blue
```

```js
var color = 'blue'
function getColor(){
    var color='red'
    return color
}
console.log(getColor()) // red
```

为了引用变量color，需要进行两步搜索操作。首先在getColor()变量对象中搜索，看是否有一个名为color的标识符。如果没有找到，则向上级，在此处即到全局变量对象中寻找标识符。

#### 4.3 垃圾收集

##### 4.3.1 标记清除

javascript中最常用的垃圾收集方式是**标记清除**。

垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记（可以使用任何标记方式）。然后，它会去掉环境中的变量以及被环境中的变量引用的变量的标记。而在此之后再被加上标记的变量 将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾收集器完成内存清除工作，销毁那些带标记的值并回收它们所占用的内存空间。

##### 4.3.2 引用计数

另一种不太常见的垃圾收集方式叫做引用计数。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是1.如果同一个值又被赋值给另一个变量，则该值的引用次数加一。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减一。当这个值的引用次数变成0时，则表示没有办法再访问这个值了，因而就可以将其占用的内存空间回收。故，当垃圾收集器下次再运行时，它就会释放那些引用次数为零的值所占用的内存。

一个严重的问题：循环引用。

```js
function problem(){
    var objectA=new Object()
    var objectB=new Object()
    objectA.aObject=objectB
    objectB.anotherObject=objectA
}
```

这个例子中，objectA和objectB通过属性相互引用，故，这两个对象的引用次数都是2.在函数执行完毕之后，objectA和objectB还是将继续存在，因为它们的引用次数不为0。假如这个函数被重复多次引用，就会导致大量的内存得不到回收。

##### 4.3.3 性能问题

##### 4.3.4 管理内存

![1604051154363](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604051154363.png)

```js
function createPerson(name){
    vae localPerson = new Object()
    localPerson.name=name
    return localPerson
}
var globalPerson = createPerson('Nick')
// 解除引用
globalPerson = null
```

![1604051336437](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604051336437.png)

## 5.第5章 引用类型

### 5.1Object类型

创建Object实例有两种方式。

使用new操作符。

```js
var obj = new Object()
obj.name = 'Nick'
obj.age = 20
```

对象字面量,目的在于简化创建具有大量属性的对象：

```js
var person = {
    name:'Nick',
    age:18
}
```

访问对象属性时，可以使用点表示法，也可以使用方括号语法。

```js
console.log(person.name)
console.log(person['name'])
```

方括号语法优点：

可以通过变量访问属性。

```js
var propertyName = 'name'
console.log(person[propertyName])
```

属性名可以包含导致语法错误的字符，或者属性名是关键字或保留字。此时，不能通过点语法访问属性。

```js
console.log(person['first name'])
console.log(person['var'])
```

### 5.2 Array类型

ES中数组的每一项可以保存任意类型的数据。例如：数组第一个位置可以保存字符串、第二个位置可以保存数值、第三个位置可以保存对象、等等。ES数组的大小是可以动态调整的，即可以随着数据的添加自动增长，以容纳新增数据。

创建数据的基本方式有两种：

使用Array()构造函数：

```js
var arr = new Array();
```

```js
var arr = new Array(20) // 创建一个长度为20的数组
var colors = new Array('red','blue','green')// 创建一个包含指定元素的数组
```

在使用构造函数时，也可以省略new操作符，结果与使用new操作符是一样的。

```js
var arr = Array()
var colors = Array('color','red','blue')
```

使用数组字面量的方式：

```js
var arr = ['1','1','2']//创建一个包含三个元素的数组
var arr1 = [] // 创建一个空数组
```

数组的length属性很有特点，不只是可读的。可以改变length属性，从数组的末尾移除项或添加新项。

```js
var arr = ['red','green','blue'] // 创建一个包含3个元素的数组
arr.length=2
console.log(arr[2]) // undefined
```

如果将length属性增大，则新增的每一项都会取得undefined值。

```js
var colors = ['red','green','blue']
colors.length=4
console.log(colors[3]) // undefined
```

利用length属性向数组添加元素。

```js
var colors = ['red','green','blue']
colors[colors.length]='black'
colors[colors.length]='brown'
```

#### 5.2.1 检测数组

对于一个网页或者一个全局作用域而言，使用instanceof操作符就能得到一个满意的结果。

```js
if(arr instanceof Array){
   	// 对数组的某些操作。。。
   }
```

如果一个网页中包含多个框架，即具有多个全局作用域。则使用ES5的isArray()方法。

```js
if(Array.isArray(value)){
   // 对数组的某些操作
   }
```

#### 5.2.2 转换方法

所有对象都具有toLocaleString()、toString()和valueOf()方法。

toString()方法：返回由数组中每个值的字符串形式拼接而成的一个用逗号分隔的字符串。

valueOf()方法：返回的还是数组。

```js
var colors = ['red','green','blue']
alert(colors.toString()) // red,blue,green
alert(colors.valueOf()) // red,blue,green
alert(colors) // red,blue,green
```

toLocaleString()也会返回和toString()、valueOf()方法一样的结果，但不总是如此。

调用数组的toLocaleString()方法时，它也会创建一个数组值的逗号分隔的字符串。而不同之处在于，为了取得数组元素中每一项的值，调用的是数组中每一项的toLocaleString()方法，而不是toString()方法。

例如：

```js
var person1 = {
    toLocaleString(){
        return 'Nikolaos'
    },
    toString(){
        return 'Nicholas'
    }
}
var person2 = {
    toLocaleString(){
        return 'Grigorios'
    },
    toString(){
        return 'Greg'
    }
}
var people = [person1,person2]
alert(people)  // Nicholas,Greg
alert(people.toString()) // Nicholas,Greg
alert(people.toLocaleString()) // Nikolaos,Grigorios
```

数组的join()方法，使用不同的分隔符构建数组元素的字符串。此方法只接受一个参数，即用作分隔作用的字符串，返回值是包含所有数组项的字符串。

```js
var colors = ['red','green','blue']
console.log(colors.join(' ')) // red green blue
console.log(colors.join('||')) // red||green||blue
```

如果不给join传入参数或者传入undefined，则使用逗号作为分隔符。

```js
var colors = ['red','green','blue']
console.log(colors.join()) // red,green,blue
console.log(colors.join(undefined)) // red,green,blue
```

#### 5.2.3 栈方法

push():此方法接收任意数量的参数，并将其逐个添加到数组末尾，并返回修改后的数组长度。

pop():从数组末尾移除最后一项，减少数组的length值，然后返回被移除的项。

```js
var colors = []
var count = colors.push('red','green') // 推入两项
alert(count) // 2
count = colors.push('black')
alert(count) // 3
var item = colors.pop() // 取得最后一项
alert(item) // black
alert(colors.length) //2
```

#### 5.2.4 队列方法

shift():移除数组中的第一个项，并返回该项，同时数组长度减1。

```js
var colors = new Array()
colors.push('red','blue')
var result = colors.shift()
console.log(result) // red
```

unshift():在数组前端添加任意个项并返回新数组的长度。

```js
var colors = []
var count = colors.unshift('red','blue')
console.log(count) //2 
console.log(colors.pop()) // blue
```

#### 5.2.5 重排序方法

reverse():反转数组项的顺序。返回值：顺序改变后的数组。

```js
var arr=[1,2,3,4,5]
arr.reverse()
console.log(arr) // [5,4,3,2,1]
```

sort():默认按照升序排序数组项。即最小的值位于最前面，最大的值排在最后。为了实现排序，sort()方法会调用每个数组项的toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值，sort()方法比较的也是字符串。返回值：顺序改变后的数组。

但是这种排序方式，在很多情况下都不是最佳方案。例如下面的情况：

```js
var arr = [0,1,5,10,15]
arr.sort()
console.log(arr) // [0,1,10,15,5]
```

为了解决这个问题，sort()方法可以接收一个比较函数作为参数。

```js
// 升序比较函数  降序比较函数将value1和value2的比较值更改就行。
function compare(value1,value2){
    if(value1<value2){
       	return -1
       }
    else if(value1>value2){
               return 1 
                }
    else {
        return 0
    }
}
// 上述比较函数适用于大多数的数据类型，那哪些类型不适用呢？
// 再次使用sort方法，使用比较函数作为参数。
var values = [0,1,5,10,15]
values.sort(compare)
console.log(values) // [0,1,5,10,15]
```

对于数值类型或者其valueOf()方法会返回数值类型的对象类型，可以使用一个更简单的比较函数。这个函数只要用第二个值减第一个值即可。

```js
// 升序比较函数  如果想要降序比较函数，就使用value2-value1
// 书上没有给例子，，，怎么去创建一个？
function compare(value1,value2){
    return value1-value2
}
```

#### 5.2.6 操作方法

ES为操作已经包含在数组中的项提供了很多方法。其中，concat()方法可以基于当前数组中的所有项创建一个新数组。

concat()：先创建当前数组的一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。如果没有给concat()方法传参，它只是复制当前数组并返回副本。如果传递给concat()方法的是一或多个数组，则该方法会将这些数组中的每一项都添加到结果数组中。如果传递的值不是数组，这些值就会被简单地添加到结果数组的末尾。

```js
var colors = ['red','green','blue']
var colors2 = colors.concat('yellow',['black','brown'])
console.log(colors) // ["red","green","blue"]
console.log(colors2) // ["red","green","blue","black","brown"]
```

slice():基于当前数组中的一个或多个项创建一个新数组。接受一或两个参数，即要返回项的起始和结束位置。若只有一个参数，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。若有两个参数，则放回起始位置和结束位置之间的项，但是不包括结束位置的项。注意，slice()方法不会影响原数组。

```js
var colors = ['red','green','blue','yellow','purple']
var colors2 = colors.slice(1) // 从第二个位置开始的所有项
var colors3 = colors.slice(1,4) // 从第二个位置到第五个位置的项，不包含第五个。
console.log(colors2) // ['green','blue','yellow','purple']
console.log(colors3) // ['green','blue','yellow']
console.log(colors) // ['red','green','blue','yellow','purple']
```

注意：slice()方法参数中有负数的情况。如果参数中有负数，则用数组的长度加上该负数来确定相应的位置。

长度为5的数组，调用slice(-2,-1)与调用slice(3,4)的结果是一样的。如果结束位置小于起始位置，则返回空数组。

splice():这个方法可能是最强大的数组方法了，它有很多种用法。主要作用是向数组的中部插入项，使用方式有以下3种，

删除：可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的项数。splice(0,2)，删除前两项。

插入：提供3个参数：起始位置、0（其实是要删除的项数）及要插入的项。若要插入多个项，则再传入第四个、第五个参数，或者任意多个项。例：splice(2,0,'red','green')。从位置2开始插入字符’red‘和’green‘。

替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项。需指定3个参数：起始位置、要删除的项数以及要插入的任意数量的项。splice(2,1,'red','green')。删除从位置2开始的1项数组项，再从位置2开始插入‘red’和‘green’。

splice()方法返回一个数组，该数组包含从原始数组中删除的项（如果没有删除任何项，则返回一个空数组。）

```js
var colors = ['red','green','blue']
var removed = colors.splice(0,1) // 删除第一项
console.log(colors) // ['green','blue']  
console.log(removed) // ["red"]

removed = colors.splice(1,0,'yellow','orange')
console.log(colors) // ["green","yellow","orange","blue"]
console.log(removed) // []

removed = colors.splice(1,1,'red','purple') // 删除第二项，再从第二项的位置插入red、purple
console.log(colors) // ["green","red","purple","orange","blue"]
console.log(removed) // ["yellow"]
```

#### 5.2.7 位置方法

ES5为数组实例添加了两个位置方法：indexOf()和lastIndexOf()。两个方法都接收两个参数：要查找的项和（可选）表示查找起点位置的索引。

indexOf()方法从数组的开头（位置0）开始向后查找，lastIndexOf()方法从数组末尾开始向前查找。

两个方法都返回要查找的项在数组中的**位置**，没找到则返回-1。在比较传入的参数(要查找的项)和数组项时，使用的是**全等操作符（===）**。

```js
let numbers =[1,2,3,4,5,4,3,2,1]
console.log(numbers.indexOf(4))  // 3
console.log(numbers.lastIndexOf(4)) // 5
console.log(numbers.indexOf(4,4)) // 5
console.log(numbers.lastIndexOf(4,4)) // 3

let person ={
    name:'Nick'
}
let people = [{name:'Nick'}]
let morePeople = [person]
console.log(people.indexOf(person))  //-1
console.log(morePeople.indexOf(person)) // 0
```

#### 5.2.8 迭代方法

ES5为数组定义了5个迭代方法。每个方法接收两个参数：在每项上运行的函数和（可选的）运行该函数的作用域对象--即影响this的值。

传入方法的函数接受三个参数：数组项的值、该项在数组中的位置和数组对象本身。（参数似乎都是可选的。）

every()：对数组中的每一项运行给定函数，若该函数对每一项都返回true，则方法放回true。

filter():对数组每一项运行给定函数，返回该函数会返回true的项组成的数组。

forEach():对数组每一项运行给定函数。该方法没有返回值。

map():对数组每一项运行给定函数，返回每次函数调用结果组成的数组。

some():对数组每一项运行给定函数，若该函数对数组任意一项返回true，则some()方法返回true。

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]
let everyRes = numbers.every((value,index,array) => {
     return (value > 2)
 })
 console.log(everyRes); // false
 let someRes = numbers.some((value,index,array) => {
     return (value > 2)
  })
 console.log(someRes); // true
```

```js
let numbers = [1,2,3,4,5,4,3,2,1]
let filterRes = numbers.filter((value,index,array)=>{
    return (value > 2)
})
console.log(filterRes) // [3,4,5,3]
```

```js
let numbers = [1,2,3,4]
let mapRes = numbers.map((value,index,array)=>{
    return value * 2
})
console.log(mapRes) // [2,4,6,8]
```

```js
let numbers = [1,2,3,4]
let numbersCopy=[]
let forEachRes = numbers.forEach((value,index,array)=>{
	numbersCopy.push(value*2)
})
console.log(numbersCopy) // [2,4,6,8]
```

#### 5.2.9 缩小方法

ES5新增两个缩小数组的方法：reduce()和reduceRight()。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值。reduce()方法从数组第一项开始，逐个遍历到最后，而reduceRight()从数组最后一项开始，向前遍历到第一项。

参数：1，在每一项上调用的函数 2.（可选）作为缩小基础的初始值。

在每一项上调用的函数接收4个参数：前一个值、当前值、项的索引值、和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数是数组第二项。

例：使用reduce()方法执行数组中所有值之和。

```js
let values = [1,2,3,4,5]
let sum = values.reduce((prev,cur,index,array)=>{
    return prev+cur
})
console.log(sum) // 15
```

第一次执行回调函数，prev是1，cur是2。第二次，prev是3（1+2），cur是3（数组第3项）。这个过程会持续到把数组中的每一项都访问一遍，最后返回结果。

reduceRight()作用类似，只不过方向相反而已。

```js
let value = [1,2,3,4,5]
let sum = value.reduceRight((prev,current,index,array)=>{
    return prev+current
})
console.log(sum) // 15
```

使用reduce()和reduceRight(),主要取决于从哪头开始遍历数组，从此之外，它们完全相同。

### 5.3 Date类型

ES中的Date类型是在早期Java中的java.util.Date类基础上构建的。因此，Date类型使用呢自UTC1970年1月1日午夜（零时）开始经过的毫秒数来保存日期。

创建一个日期对象：

```js
let now = new Date();
```

调用Date构造函数而不传递参数时，新创建的对象自动获得当前日期和时间。

若想创建特定的日期和时间对象，必须传入毫秒数。ES提供了两个方法：Date.parse()和Date.UTC()，简化这个操作。

Date.parse():接收一个表示日期的字符串参数，然后尝试根据这个字符串返回相应日期的毫秒数。这个方法的行为因实现而异，通常是因地区而异。将地区设置为美国的浏览器通常支持下列日期格式：

![](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604908167196.png)

例如：2004年5月25日

```js
let someDate = new Date(Date.parse('May 25, 2004'))
```

![](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604910464632.png)

![1604910571023](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604910571023.png)



ES5添加了Date.now()方法，返回表示调用这个方法时的日期和时间的毫秒数。

```js
let start = Date.now() // 取得开始时间
// 调用函数，dosomething()
let stop = Date.now() // 取得结束时间
let result = stop - start // 取得时间差。
```

![1604910834356](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604910834356.png)

#### 5.3.1 继承的方法

Date类型也重写了toLocaleString()、toString()和valueOf()方法。Date类型的这些方法的返回值有些不同：

toLocaleString():按照与浏览器设置的地区相适应的格式返回日期和时间。

toString():通常返回带有时区信息的日期和时间，其中时间一般以军用时间（即小时是0到23表示）表示。

![1604911166206](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604911166206.png)

![1604911235272](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604911235272.png)

#### 5.3.2 日期格式化方法

![1604911316574](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604911316574.png)

#### 5.3.3 日期/时间组件方法

直接取得或设置日期值中特定部分的方法。

![1604911457630](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604911457630.png)

![1604911474842](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604911474842.png)

### 5.4 RegExp类型

ES通过RegExp类型来支持正则表达式。

创建正则表达式：

**字面量形式**

```js
var expression = /pattern/flags // 创建正则表达式采用这种语法。
```

模式（pattern）部分可以是任何简单或复杂的正则表达式，可以包含字符类、限定类、分组、向前查找以及反向引用。每个正则表达式可带有一个或多个标志（flags）。

3个标志：

g:全局模式（global），即模式将被应用于所有字符串，而非在发现第一个匹配项时立即停止。

i:不区分大小写模式，即在确定匹配项时忽略模式与字符串的大小写。

m:多行模式，即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项。

```js
let pattern1 = /at/g // 匹配字符串中所有‘at’实例
let pattern2 = /[bc]at/i //匹配第一个“bat"或”cat“,不区分大小写
let pattern3 = /.at/gi //匹配所有以“at”结尾3个字符的结合，不区分大小写
```

与其他语言正则类似，模式中使用的所有元字符都必须转义。正则中的元字符包含：

```js
( [ { \ ^ $ | ) ? * + . ] }
```

想要匹配字符串中包含的这些字符，就必须对它们转义。

```js
let pattern1 = /[bc]at/i // 匹配第一个"bat"或"cat"，不区分大小写。

let pattern2 = /\[bc\]at/i // 匹配第一个"[bc]at",不区分大小写。

let pattern3 = /.at/gi //匹配所有以"at"结尾的3个字符的组合，不区分大小写

let pattern4 = /\.at/gi //匹配所有".at",不区分大小写。
```

**RegExp构造函数**：

接收两个参数：1.要匹配的字符串模式 2.可选的标志字符串。

```js
let pattern1 = /[bc]at/i  //匹配第一个"bat"或"cat"，不区分大小写

let pattern2 = new RegExp('[bc]at','i') //功能同上
```

![1604916159656](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604916159656.png)



#### 5.4.1 RegExp实例属性

RegExp的每个实例都具有下列属性，通过属性可以取得有关模式的各种信息。

```js
global //布尔值，是否设置g标志。
ignoreCase //布尔值，是否设置i标志。
lastIndex //整数，表示开始搜索下一个匹配项的字符位置，从0算起。
multiline //布尔值，是否设置m标志。
source //正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。
```

例如：

![1604997310675](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604997310675.png)

#### 5.4.2 RegExp的实例方法

#### **此处未完、、、、、**





### 5.5 Function类型

函数实际上是对象，每个函数都是Function类型的实例，且具有属性和方法。函数名是指向函数对象的指针，不会与某个函数绑定。

**创建函数的方式：**

**函数声明：**

```js
function sum(num1,num2){
    return num1+num2
}
```

**函数表达式：**

```js
var sum = function(num1,num2){
    return num1+num2
}
```

**不推荐**使用Function构造函数定义函数。

![1604998237029](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1604998237029.png)

函数名仅是指向函数的指针，一个函数可以有多个函数名。

```js
function add(num1,num2){
    return num1+num2
}

var anotherAdd = add
console.log(add(1,2))
console.log(anotherAdd(1,2))
add=null
console.log(anotherAdd(1,2))
```

#### 5.5.1没有重载（深入理解）

函数名是指针，这点与没有重载息息相关。

后定义的同名函数，覆盖前面的定义。

```js
function addSomeNumber(num){
    return num+100
}
function addSomeNumber(num){
    return num+200
}
console.log(addSomeNumber(1)) // 201
```

#### 5.5.2函数声明与函数表达式

两者最大的区别：函数声明会有提升。

解析器会率先读取函数声明，使其在执行任何代码之前可用（可以访问）；函数表达式则等到解析器执行到它所在代码行，才会被解释执行。

```js
console.log(sum(1,2)) // 3
function sum(num1,num2){
	return num1+num2
}

console.log(sum1(1,3)) // sum1 is not a function
var sum1 = function(num1,num2){
    return num1+num2
}
```

#### 5.5.3作为值的函数

函数可以作为参数传递，也可以作为函数返回值返回。

```js
function callSomeFunction(someFunction,someArguments){
    return someFunction(someArguments)
}
function someFunction(){
    return function(){
        。。。
    }
}
function add10(num){
    return num+10
}
let res = callSomeFunction(add10,10)
console.log(res) //20
```

#### 5.5.4 函数内部属性

函数内部有两个特殊的(对象)属性：this和arguments。

**arguments：**类数组对象，包含传入函数的所有参数。它有一个callee属性，该属性是一个指针，指向拥有这个arguments对象的函数。

例子：

```js
function factorial(num){
    if(num<=1){
       	return 1
       }
    return num*factorial(num-1)
}
```

实现一个阶乘函数，但此函数执行与函数名耦合在一起。使用arguments.callee解耦。

```js
function factorial(num){
	if(num<=1){
       	return 1
       }
    return num*arguments.callee(num-1)
}
```

**this:**引用函数据以执行的环境对象----或者说是this值。 谁调用函数，this就指向谁。。？

![1605000151320](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1605000151320.png)

ES5规范化了另一个函数对象**内部属性：caller**。此属性保存**调用当前函数的函数的引用**，若在全局作用域中调用当前函数，则它的值为null。

```js
function outer(){
    return inner()
}
function inner(){
    return inner.caller
    // console.log(inner.caller)  inner.caller指向outer函数。
}
console.log(outer())
```

可以使用arguments让函数与函数名解耦

```js
function outer(){
    return inner()
}
function inner(){
    return arguments.callee.caller
    // console.log(inner.caller)
}
console.log(outer())
```

严格模式，访问callee会报错。

#### 5.5.5函数属性和方法

在ES中，函数是对象，具有属性和方法。函数有两个属性：length和prototype。

length：函数命名参数的个数。

prototype：对ES中的引用对象而言，prototype是保存它们所有实例方法的真正所在，在ES5中，prototype属性是不可枚举的。

函数包含两个非继承的方法：call()和apply()。作用：在特定作用域中调用函数，即设置函数体内this对象的值。

apply():接受两个参数。1.运行该函数的作用域。2.参数数组。其中，参数数组可以是Array实例，也可是arguments对象。

```js
function sum(num1,num2){
    return num1+num2
}
function applySum1(num1,num2){
    return sum.apply(this,arguments)
}
function applySum2(num1,num2){
    return sum.apply(this,[num1,num2])
}
console.log(applySum1(1,2),applySum2(2,3)) //3 5  这些代码里的this都是window
```

call():接受1个及以上参数。第一个参数：运行该函数的作用域。其余参数，用逗号分隔。

```js
function sum(num1,num2){
    return num1+num2
}
function callSum(num1,num2){
    return sum.call(this,num1,num2)
}
console.log(callSum(2,3)) //3 5
```

call()和apply()真正的用武之地：扩充函数赖以运行的作用域。

```js
color='red'
var o={
    color:'blue'
}
function sayColor(){
    console.log(this.color)
}
sayColor() // 'red'
sayColor.call(this) // 'red'
sayColor.call(window) // 'red'
sayColor.call(o) // "blue"
```

ES5中的方法：bind()。该方法创建函数的实例，实例的this值绑定到传给给bind()函数的参数。

```js
var color = 'red'
var o = {
    color:'blue'
}
function sayColor(){
    console.log(this.color)
}
var bindFunction = sayColor.bind(o)
bindFunction() // "blue"
```

继承来的方法：toLocaleString()、toString()、valueOf()

toLocaleString(),返回函数的代码。

toString()，返回函数的代码。

valueOf(),返回函数的代码。

## 6.第6章 面向对象的程序设计

ECMA-262将对象定义为：无序属性的集合，其属性包含基本值、对象或函数。

每个对象都基于一个引用类型创建。

### 6.1理解对象

采用对象字面量方式构造对象比较简洁。

对象的属性在创建时，都具有一些特征值，JS通过这些特征值定义属性的行为。

#### 6.1.1属性类型

ES5在定义只有内部才用的特性时，描述了属性的各种特征。该规范将这些特征值放在两对方括号中，JS无法直接访问，这是为了实现JS引擎设计的。

ES中有两种属性：数据属性和访问器属性。

##### **1.数据属性**

数据属性包含一个值的位置，在此位置可以读取和写入值。数据属性有4个描述其行为的特性。

- [[Configurable]]:表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。直接定义在对象上的属性的这个特性默认为true。
- [[Enumerable]]:表示能否通过for-in循环返回属性。直接定义在对象的属性的这个特性默认为true。
- [[Writable]]:表示能否修改属性的值。直接定义在对象上的属性的这个特性默认为true。
- [[Value]]：包含这个属性的数据值。读取属性值时，在这个位置读；写入属性值时，新值保存在这个位置。这个特性默认为undefined。

使用ES5的Object.defineProperty(),修改属性默认特性。此方法接受三个参数：1.属性所属对象，2.属性名，3.描述符对象。其中，描述符对象属性必须是configurable、enumerable、writable、value。可设置其中一个或多个值，修改其对应特性。

```js
var person={}
Object.defineProperty(person,'name',{
    writable:false,
    value:'Nick'
})
console.log(person.name) // Nick
person.name='Tony'
console.log(person.name) // Nick
```

##### 2.访问器属性

此属性不包含数据值：包含getter和setter函数（不过，这两个函数都不是必须的）。读取访问器属性时，调用getter，返回有效的值；写入访问器属性时，调用setter并传入新值，此函数决定如何处理函数。

访问器属性有4个特性：

- [[Configurable]]:表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。直接定义在对象上的属性的这个特性默认为true。
- [[Enumerable]]:表示能否通过for-in循环返回属性。直接定义在对象上的属性的这个特性默认为true。
- [[Get]]:读取属性时调用的函数。默认undefined。
- [[Set]]:写入属性时调用的函数。默认undefined。

访问器属性使用Object.defineProperty()定义。

```js
var book={
    _year:2004,
    edition:1
}
Object.defineProperty(book,'year',{
    get:function(){
        return this._year
    },
    set:function(newValue){
        if(newValue>2004){
            this._year=newValue
            this.edition+=newValue-2004
        }
    }
})
book.year=2005
console.log(book.edition)
```

_year中的下划线，表示只能通过对象方法访问的属性。而访问器属性year则包含一个getter函数和setter函数。getter函数返回  _year的值，setter函数设置edition的值。

getter和setter不一定都指定，可以只指定一个。但只指定getter，则属性不能写。只指定setter，则属性不能读。

#### 6.1.2定义多个属性

ES5制定了通过描述符一次定义多个属性的方法。Object.defineProperties()。

此方法接受两个对象参数：1.要添加和修改其属性的对象。2.与参数一中要添加和修改的属性一一对应的对象。

```js
var book={}
Object.defineProperties(book,{
    _year:{
        value:2004
    },
    edition:{
        value:1
    },
    year:{
        get:function(){
            return this._year
        },
        set:function(newValue){
        	if(newValue>2004){
               	this._year=newValue
                this.edition+=newValue-2004
               }
    	}
    }
})
```

此处定义了两个数据属性：_year和edition，一个访问器属性：year。

#### 6.1.3读取属性的特性

使用ES5的Object.getOwnPropertyDescriptor()方法，获取给定属性的描述符。

参数：1.对象（属性所在的）2.属性名。 

返回值：对象。

```js
var descriptor = Object.getOwnPropertyDescriptor(book,'_year')
console.log(descriptor.value)
console.log(descriptor.configurable)
```

在JS中，可以对任何对象，包括DOM和BOM使用Object.getOwnPropetyDescriptor()方法。

### 6.2创建对象

Object构造函数或对象字面量都可以创建单个对象。

缺点：创建多个对象时，产生大量重复代码。

#### 6.2.1工厂模式

用函数封装以特定接口创建对象的细节。

```js
function createPerson(name,age,job){
    var o = new Object()
    o.name=name
    o.age=age
    o.job=job
    o.sayName=function(){
        console.log(this.name)
    }
    return o
}
var person1 = createPerson('Nick',18,'H5')
```

工厂模式解决了创建多个相似对象的问题，但没有解决对象识别的问题（即怎样知道一个对象的类型。）

#### 6.2.2构造函数模式

ES中的构造函数可用来创建对象。如Object、Array这类原生构造函数，在运行时，会自动出现在浏览器中。

可以自定义构造函数，从而定义自定义对象类型的属性和方法。

```js
function Person(name,age,job){
    this.name=name,
        this.age=age,
        this.job=job,
        this.sayName=function(){
        console.log(this.name)
    }
}
var person1 = new Person('Nick',18,'H5')
var person2=new Person('Tony',20,'Java')
```

构造函数名首字母大写，如Person。

构造函数，需使用new操作符才能创建新实例。new的过程经历四个步骤：

1. 创建新对象。
2. 将构造函数的作用域赋给新对象（因此，函数中的this就指向这个对象。）
3. 执行构造函数的代码。（即添加属性等操作。。）
4. 返回新对象。

person1和person2分别保存着Person不同的实例。

这两个对象都有一个constructor（构造函数）属性，指向它们的构造函数。此处，即Person构造函数。

```js
console.log(person1.constructor)
console.log(person2.constructor)
```

创建自定义构造函数的意义：将它的实例标识为一种特定的类型。

person1、person2即是Person的实例，也是Object的实例，因为所有对象均继承自Object。可以使用instanceof 检测类型。

```js
console.log(person1 instanceof Person) // true
console.log(person2 instanceof Person) // true
console.log(person1 instanceof Object) // true
console.log(person2 instanceof Object) // true
```

##### 1.将构造函数当成函数

构造函数与普通函数唯一的区别是调用方式不同。

任何函数通过new操作符调用就是作为构造函数，不通过new操作符调用就是普通函数。

所以，构造函数可通过new操作符调用，可当做普通函数调用，可指定函数作用域调用。

```js
// new
var person = new Person()
// 普通函数方式
Person()
// 指定作用域
var o ={}
Person.call(o,参数1，参数2,...)
```

##### 2.构造函数的问题

person1和person2中的方法是不同的Function实例，但是完成的确是同一个任务。因此，没有必要创建多个完成相同任务的函数实例。

```js
function Person(name,age,job){
	this.name=name,
    this.age=age,
    this.job=job,
    this.sayName=sayName
}
function sayName(){
    console.log(this.name)
}
```

将sayName函数定义为全局函数，则person1和person2引用的是同一个函数。但是，当外部函数定义过多时，就没有封装性可言了。

#### 6.2.3原型模式

我们创建的每个函数都有一个prototype（原型）属性，该属性是一个指针，指向一个对象。此对象包含某个特定类型的所有实例共享的属性和方法。

即prototype指向的对象就是调用构造函数创建的那个对象实例的原型对象。

将信息添加到原型对象中，所有实例就可以用了。

```js
function Person(){
    
}
Person.prototype.name='Nick'
Person.prototype.age='20'
Person.prototype.job='java'
Person.prototype.sayName=function (){
    console.log(this.name)
}
var person1 = new Person()
person1.sayName() // "Nick"
var person2 = new Person()
person2.sayName() // "Nick"
console.log(person1.name==person2.name) // true
```

##### 1.理解原型对象

只要创建新函数，就会为该函数创建一个prototype属性，该属性指向函数原型对象。

默认，所有原型对象都自动获得constructor（构造器）属性，此属性包含指向prototype属性所在函数的指针。

如：Person.prototype.constructor指向Person。

创建自定义构造函数后，原型对象默认只会取得constructor属性；其他方法则从Object继承。

当调用构造函数创建一个新实例后，此实例内部包含一个指针（内部属性），指向构造函数的原型对象。ES5中将这个指针称为[[Prototype]],在脚本中，没有标准的方式访问[[Prototype]]，但大多数浏览器在每个对象上支持一个属性 `__proto__`用以访问。

![1596266982630](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596266982630.png)

可以通过isPrototypeOf()方法来确定对象之间是否存在上述关系。即[[Prototype]]指向调用isPrototypeOf()方法的对象时，会返回true。

```js
console.log(Person.prototype.isPrototypeOf(person1)) // true
```

ES5的新方法：Object.getPrototypeOf()，此方法返回[[Prototype]]的值。

```
console.log(Object.getPrototypeOf(person1)) // 打印Person.prototype指向的对象。
```

当代码读取某个对象的某个属性时，会先在对象实例中寻找，找到则返回该属性对应的值，未找到则向原型对象找。

原型对象的constructor属性也是所有实例共享的。

可以通过对象实例访问存储在原型中的值，但不可以通过实例重写原型中的值。

若在实例中定义与原型中同名的属性，则实例中的属性屏蔽原型中的属性。

```js
function Person(){
    
}
Person.prototype.name='Nick'
var person1 = new Person()
person1.name='Tony'
console.log(person1.name) // "Tony" 屏蔽 "Nick"值。
```

在对象实例添加属性，只会屏蔽原型中的同名属性，不会修改。即使实例同名属性设置为null，也只是在实例中设置这个属性。

可以使用delete操作符，删除实例属性，然后就可以访问原型中的属性了。

使用hasOwnProperty()方法检测属性存在于实例还是原型中。给定属性存在于实例时，会返回true。

```js
function Person(){}
Person.prototype.name='Nick'
var person1 = new Person()
console.log(person1.hasOwnProperty(name)) // false
```

##### 2.原型与in操作符

两种方式使用in操作符：for-in循环和单独使用in操作符。

单独使用in操作符。通过对象能够访问属性时返回true，无论属性在实例还是在原型上。

```js
function Person(){}
Person.prototype.name='Nick'
var person1 = new Person()
console.log('name' in person1) // true 原型中的
person1.age = 20
console.log('age' in person1) // true 实例中的
```

如何确定属性是原型中的属性？

```js
function hasPrototypeProperty(object,name){
    return !object.hasOwnProperty(name)&&(name in object)
}
```

只要hasOwnProperty()函数返回false，in操作符返回true。就可确定，该属性来自原型。

for-in循环：返回的是所有能通过对象访问的、可枚举的属性。包括实例本身的属性和原型上的属性。若原型中的不可枚举属性被实例中的同名属性屏蔽了，则该属性可使用for-in循环返回。因为根据规定，所有开发人员定义的属性都是可枚举的。

使用ES5的Object.keys()取得对象上所有可枚举的实例属性。

Object.keys()：接受一个参数，即，要取得属性的对象，返回一个包含所有可枚举属性的字符串数组。

```js
function Person(){}
var person1 = new Person()
person1.name='Nick'
person1.age=20
var keyArr=Object.keys(person1)
console.log(keyArr) // ["name","age"]
```

若想要得到所有实例属性，无论它是否可枚举，可以使用Object.getOwnPropertyNames()方法。

参数：实例对象。此方法返回实例所有属性组成的字符串数组。

```js
var keys = Object.getOwnPropertyNames(Person.prototype)
```

##### 3.更简单的原型语法

用对象字面量重写原型对象。

```js
function Person(){}
Person.prototype={
    name:'Nick',
    age:20,
    sayName:function (){
        console.log(this.name)
    }
}
```

以上方式导致Person的默认原型对象被重写，故Person.prototype.constructor属性不再指向Person。

此时新原型的constructor属性指向Object函数，不再指向Person。

如果constructor的值很重要，可以手动设置。

```js
function Person(){}
Person.prototype={
    constructor:Person,
    name:'Nick',
    age:20,
    sayName:function(){
        console.log(this.name)
    }
}
```

此时的constructor则是一个可以枚举的属性。原生的constructor则是不可枚举属性。

##### 4.原型的动态性

使用原生原型。先创建实例，后修改原型上的属性是没有任何问题的。

```js
function Person(){}
var person=new Person()
Person.prototype.name='Nick'
consoel.log(person.name) // "Nick"
```

使用重设原型对象。先创建实例，后修改原型上的属性，会出现一些问题。

```js
function Person(){}
var person = new Person()
Person.prototype.name='Nick'
Person.prototype={ // 切断Person构造函数与原生原型对象的联系
    constructor:Person,
    name:'Tony'
}
console.log(person.name) // "Nick"
```

这样的实例的 `__proto__`（[[Prototype]]）指向的是原生原型对象，重写后的原型对象对其不起作用。

![1596285265328](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596285265328.png)

##### 5.原生对象的原型

所有原生引用类型（Object、Array、String等），都在其构造函数的原型上定义了方法。可以在其原型对象上添加方法。但不推荐。

##### 6.原型对象的问题

原型模式的缺点：

1.大量实例默认属性都相同。

2.由实例共享原型对象特性引起的问题。

对于包含引用类型值的属性来说，问题比较突出。

```js
function Person() { }
Person.prototype = {
    constructor:Person,
    name: 'Tony',
    habby:['A','B'],
    say:function(){
        console.log(this.name)
    }
}
var person1=new Person()
var person2 = new Person()
person1.habby.push('C')
console.log(person2.habby) // ["A", "B", "C"]
```

person1和person2共享一个原型对象，当person1修改了原型对象中的数组值时，会在person2中反应出来。

#### 6.2.4组合使用构造函数模式和原型模式

创建自定义类型最常见的方式，组合使用构造函数模式和原型模式。构造函数定义实例属性，原型模式定义方法和共享的属性。

```js
function Person(name,age,job){
    this.name=name
    this.age=age
    this.job=job
}
Person.prototype={
    constructor:Person,
    sayHi:function (){
        console.log(this.name)
    }
}
var person1 = new Person('Nick',20,'Java')
var person2 = new Person('Tony',18,'H5')
```

#### 6.2.5动态原型模式

动态原型模式，将所有信息封装在构造函数中，仅在必要的时候在构造函数中初始化原型。保留了使用构造函数和原型的优点。

```js
function Person(name,age,job) {
    this.name=name
    this.age=age
    this.job=job
    if(typeof this.sayName!='function'){
        Person.prototype.sayName=function(){
            console.log(this.name)
        }
    }
}
```

if判断处的代码，只有在初次调用构造函数，且sayName属性不存在的时候才会调用。调用后完成原型的初始化，所有实例都可共享原型上的属性。

#### 6.2.6寄生构造函数模式

在前几种模式不适用的情况下，使用这个模式。基本思想：先创建一个函数，在函数内部创建对象，然后返回该对象。即与工厂模式一样，但是通过new操作符调用。

```js
function Person(name,age,job) {
    var o = new Object()
    o.name=name
    o.age=age
    o.job=job
    o.sayName=function(){
        console.log(this.name)
    }
    return o
}
var person1 = new Person('Nick',20,'Java')
person1.sayName() // "Nick"
```

此模式可以在特殊情况下用来为对象创建构造函数。例如，想创建一个具有额外方法的特殊数组，因为不能直接修改Array构造函数，因此可以使用这个模式。

```js
function SpecialArray(){
    var values = new Array()
    values.push.apply(values,arguments)
    values.toPipedString=function(){
        return this.join('|')
    }
    return values
}
var colors=new SpecialArray('red','blue','green')
console.log(colors.toPipedString()) // red|blue|green
```

函数内部返回的对象与函数外部的对象没什么不同，这个对象与构造函数、构造函数的原型没什么联系。也不能使用instanceof判断对象类型。因此，在有更好的选择下，不要使用这个方法。

#### 6.2.7稳妥构造函数模式

道格拉斯·克罗克富德发明了JS中稳妥对象这个概念。稳妥对象，即没有公共属性，其方法中不使用this的对象。

稳妥构造函数模式与寄生构造函数模式有些类似，但有两点不同：

1.函数内部创建的对象实例不引用this。

2.构造函数不通过new操作符使用。

重写构造函数：

```js
function Person(name,age){
    var o = new Object()
   // 在此处定义私有变量和函数
    
    // 
    o.sayName=function(){
        console.log(name)
    }
    return o
}
var person1 = Person('Nick',20)
person1.sayName()  // "Nick"
```

person1中保存的是一个稳妥对象，除了使用sayName()方法之外，没有其他方法可以访问到传入构造函数中的原始数据。

使用这种方式返回的对象，与构造函数没什么关系，因此使用instanceof操作符判断对象类型没有什么意义。

### 6.3继承

ES中只支持实现继承，且继承主要是通过原型链实现的。

#### 6.3.1原型链

ES中描述了原型链的概念，并将其作为实现继承的主要方法。

每个构造函数都有一个原型对象，每个原型对象都包含一个指针指向构造函数，每个构造函数创建的实例都有一个内部指针指向原型对象。

如果构造函数的原型对象是另一个构造函数的实例，会发生什么呢？

实现原型链的一种基本模式：

```js
function SuperType(){
    this.property=true
}
SuperType.prototype.getSuperValue=function(){
    console.log(this.property)
}
function SubType(){
    this.subProperty=false
}
SubType.prototype=new SuperType()
SubType.prototype.getSubValue=function(){
    console.log(this.subProperty)
}
var instance=new SubType()
instance.getSuperValue() // true
instance.getSubValue() // false
```

上述例子中，instance是SubType的实例，但是使用了SuperType的属性和方法。实现了继承。即，让SubType的原型变为SuperType的实例。

##### 1.别忘记默认的原型

所有引用类型默认继承Object，而创建构造函数时出现的默认原型也是引用类型。所以，每个默认原型都是Object的实例，故默认原型中有个内部指针[[Prototype]]指向（Object.prototype）。Object构造函数的原型上，保存了所有实例共享的属性和方法。例如每个自定义类型都会继承toString()、valueOf()方法。

![1596350983127](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596350983127.png)

##### 2.确定原型和实例的关系

通过两种方式确定原型和实例的关系。

1.instanceof操作符

```js
console.log(instance instanceof Object) // true
console.log(instance instanceof SuperType) // true
console.log(instance instanceof SubType) // true
```

由于原型链的关系，这儿都会返回true。

2.使用isPrototypeOf()方法

```js
console.log(Object.prototype.isPrototypeOf(instance)) // true
console.log(SuperType.prototype.isPrototypeOf(instance)) // true
console.log(SubType.prototype.isPrototypeOf(instance)) // true
```

上述的原型，都instance的原型链中出现过，所有都会返回true。

##### 3.谨慎的定义方法

子类型有时候需要重写超类型中的某个方法，或添加超类型中没有的方法。但是给原型添加方法的代码一定要放在替换原型的语句之后。

```js
SubType.prototype=new SuperType()
// 添加方法
SubType.prototype.getSubValue=function(){
    return true
}
// 重写超类中的方法
SubType.prototype.getSuperValue=function(){
    return false
}
```

在使用原型链实现继承时，不要使用对象字面量的方式创建原型方法，这样会重写原型链。

```js
SubType.prototype=new SuperType()
// 这样会再次重写为一个新原型
SubType.prototype={
    sayHi:function(){
        console.log('Hi')
    }
}
```

##### 4.原型链的问题

1.包含引用类型值的属性被所有实例共享带来的问题。一个实例修改此属性，在所有实例中体现出来。

```js
function SuperType(){
    this.colors=['red','blue']
}
function SubType(){}
SubType.prototype=new SuperType()
var instance = new SubType()
var instance1 = new SubType()
instance.colors.push('green')
console.log(instance1.colors) // ["red", "blue", "green"]
```

2.在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。

由于以上两点，实践中很少单独使用原型链。

#### 6.3.2借用构造函数

在解决包含引用类型值的属性带来的问题时，开发人员使用一种叫做借用构造函数的技术。（也称伪造对象或经典继承）

其实质是在子构造函数内部调用父构造函数。

```js
function SuperType() {
    this.colors = ['red', 'blue']
}
function SubType() {
    SuperType.call(this)
}
var instance = new SubType()
var instance1 = new SubType()
instance.colors.push('green')
console.log(instance.colors) // ["red", "blue", "green"]
console.log(instance1.colors) // ["red", "blue"]
```

没有使用原型链，产生的实例不会相互影响。SubType的每个实例，都有colors的副本。

##### 1.传递参数

相对于原型链，借用构造函数有一个优势，可以在子类型构造函数中向超类型构造函数传递参数。

```js
 function SuperType(name) {
     this.name = name
 }
function SubType() {
    SuperType.call(this,'Nick')
    this.age=20
}
var instance = new SubType()
var instance1 = new SubType()
console.log(instance.name) // "Nick"
console.log(instance1.age) // 20
```

为了确保SuperType构造函数不会重写子类型的属性，应该在调用父类构造函数之后，再添加应该在子类型中定义的属性。

##### 2.借用构造函数的问题

如果仅仅是借用构造函数，将无法避免构造函数模式存在的问题-------方法都在构造函数中定义，每次实例化时都创建实现相同功能的新方法，函数无法复用。并且，因为没有使用原型链，在超类原型中定义的方法，子类实例不可以使用。因此，借用构造函数技术很少单独使用。

#### 6.3.3组合继承

组合继承，也称伪经典继承。指的是将原型链和借用构造函数技术组合到一起。

实际思想：使用原型链实现对原型上的属性和方法的继承，通过借用构造函数实现对超类构造函数的实例属性的继承。

```js
function Super(name){
    this.name=name
    this.colors=['red','blue']
}
Super.prototype.sayName=function(){
    console.log(this.name)
}
function Sub(name,age){
    // 继承实例上的属性
    Super.call(this,name)
    this.age=age
}
// 继承原型方法
Sub.prototype=new Super()
// Sub.prototype.constructor=Sub
// 添加原型方法
Sub.prototype.sayAge=function(){
    console.log(this.age)
}
var instance = new Sub('Nick',20)
```

这样的情况之下，SubType的不同实例具有的属性值相同，但是却是相互独立的。但是这些实例可以使用同一个sayName方法，此方法来自于Super.prototype.

#### 6.3.4原型式继承

道格拉斯·克罗克福德实现一种继承方法：原型式继承。

```js
function object(o){
    function F(){}
    F.prototype=o
    return new F()
}
```

ES5的Object.create()方法规范了原型式继承。这个方法接受两个参数。返回值是一个对象实例。

1.用作新对象实例原型的对象。

2.（可选）用作新对象的属性组成的对象。

```js
var person={
    name:'Nick',
    friends:['Tony','Alice']
}
var person1=Object.create(person)
person1.name='Nick1' // 定义实例自己的属性
person1.friends.push('Lucy') // 操作原型属性
console.log(person.friends) // ["Tony", "Alice", "Lucy"]
```

 第二个参数与Object.defineProperties()的第二个参数格式相同，每个属性都是通过自己的描述符定义的
 以这种方式指定的任何属性都会覆盖原型对象上的同名属性。

```js

var person={
	name:'Nick'
}
var person1 = Object.create(person,{
	name:{
		value:'Tony'
	}
})
console.log(person1.name) // Tony 原Nick被覆盖了
```

原型中包含引用类型值的属性仍然是被所有实例共享的。

#### 6.3.5寄生式继承

寄生式继承与原型式继承紧密相关，也是克罗克福德推而广之的。创建一个仅用于封装继承过程的函数。

```js
function createAnother(original){
    var o = Object.create(original) // 创建新对象，
    o.sayHi = function(){ // 给对象添加方法，增强这个对象
        console.log('Hi')
    }
    return o // 返回这个对象
}
```

在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。

使用寄生式继承为对象添加函数，会由于不能做到函数复用而降低效率。

#### 6.3.6寄生组合式继承

组合继承是JS中最常用的继承方式，但是也有不足。最大的问题是：无论什么情况下，都会调用两次超类型构造函数，一次在创建子类型原型时，一次在子类型构造函数内部。

例子：

```js
function Super(name){
    this.name=name
    this.colors=['red','blue']
}
Super.prototype.sayName=function(){
    console.log(this.name)
}
function Sub(name,age){
    Super.call(this,name) // 第二次调用Super构造函数
    this.age=age
}
Sub.prototype=new Super() // 第一次调用Super构造函数
Sub.prototype.constructor=Sub
Sub.prototype.sayAge=function(){
    console.log(this.age)
}
```

寄生组合式继承，本质是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

寄生组合式继承基本模式：

```js
function inheritPrototype(subType,superType){
    var prototype = Object.create(superType.prototype)  // 创建对象
    prototype.constructor = subType
    subType.prototype=prototype
}
```

这样就可以替换前面例子中的为子类型原型赋值的语句了。

```js
function Super(name){
    this.name=name
    this.colors=['red','blue']
}
Super.prototype.sayName=function(){
    console.log(this.name)
}
function Sub(name,age){
    Super.call(this,name) // 第二次调用Super构造函数
    this.age=age
}
//Sub.prototype=new Super() // 第一次调用Super构造函数
//Sub.prototype.constructor=Sub
inheritPrototype(Sub,Super)
Sub.prototype.sayAge=function(){
    console.log(this.age)
}
```

这样，代码中就只调用了一次Super()构造函数。

开发人员普遍认为，寄生组合式继承是引用类型最理想的继承方式。

## 7.第7章 函数表达式

定义函数有两种方式：1.函数声明 2.函数表达式

函数声明有个特征，函数声明提升。即，在定义函数的代码之前，可以调用函数。（var声明的变量也有这样的特征）。

函数表达式：

```js
// 方式
var fn1 = function(arg1,arg2...){...}

```

### 7.1递归

### 7.2闭包

闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包常见的方式，就是在一个函数内部创建另一个函数。

```js
function createComparisonFunction(propertyName){
        return function (object1,object2){
            var value1 = object1[propertyName]
            var value2 = object2[propertyName]
           // ...一些操作
        }
    }
```

在内部的匿名函数中，可以访问到外部函数的变量propertyName。即使，内部函数被返回，而且在其他地方被调用，也仍然可以访问变量propertyName。因为内部函数的作用域链中包含外部函数的作用域。

函数第一次被调用时，会创建一个执行环境以及相应的作用域链，并把作用域链赋值给一个特殊的内部属性（[[Scope]]）

。然后，使用this、arguments和其他命名参数来初始化活动对象（函数的变量对象是活动对象）。在作用域链中，外部函数的活动对象处于第二位，外部函数的外部函数的活动对象处于第三位，以此类推，直到作为作用链终点的全局执行环境。

![1596439362340](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596439362340.png)

![1596439396021](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596439396021.png)

无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般来讲，当函数执行完毕之后，局部活动对象就会被销毁，内存中仅保存全局作用域（即全局环境的变量对象）。但是，闭包略有不同。

![1596439862751](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596439862751.png)

![1596439922152](E:\前端\GitNote\Gitnote\03_Javascript\图片笔记\1596439922152.png)

闭包会携带包含它的函数的作用域，因此它所占的内存比一般函数大。因此，在绝对必要的情况下再使用闭包。

#### 7.2.1闭包与变量

闭包包含的是整个变量对象，而不是某个特殊的变量。闭包只能取得包含函数中任何变量的最后一个值。

```js
function createFunction(){
    var result = new Array()
    for(var i=0;i<10;i++){
        result[i]=function (){
            return i
        }
    }
    return result
}
```

这个函数返回一个函数数组，看起来数组中的每个函数都返回不同的数字值。但是，实际上每个函数都返回数字10.

因为每个函数的作用链中保存的都是同一个外部函数中的变量对象。这个变量对象中的i值就是10.

```js
function createFunction() {
    var result = new Array()
    for (var i = 0; i < 10; i++) {
        result[i] = function (num) {
            return function(){
                return num
            }
        }(i)
    }
    return result
}
var res = createFunction()

console.log(res[1]())
```

上面的代码创建了一个匿名的立即执行函数，该函数返回一个闭包。因此，每个闭包的作用域中所存储的外部活动对象就变的不一样，每个外部活动对象都是存储了不同i值的活动对象。（因为每个闭包函数的外部函数都是传入了不同值的匿名函数）

#### 7.2.2关于this对象

this对象是在运行时基于函数的执行环境绑定的：在全局函数中，this等于window。

匿名函数的执行具有全局性，因此其this对象通常指向window。

```js
var name ='Window'
var object = {
    name:'Object',
    sayName:function(){
        return function(){
            console.log(this.name)
        }
    }
}
object.sayName()() // "Window"
```

返回的匿名函数中的this指向的是window对象。为什么匿名函数不会取得它包含作用域的this对象呢？

每个函数在被调用时，其活动对象都会自动取得两个特殊变量：this和arguments。内部函数在搜索这个两个变量时，只会搜索到其活动对象为止，不可能访问到外部函数中的这两个变量。

```js
var name ='Window'
var object = {
    name:'Object',
    sayName:function(){
        var that = this
        return function(){
            console.log(that.name)
        }
    }
}
object.sayName()() // "Object"
```

将包含函数的this存储在that变量中，就可以使用闭包来访问that，进而访问包含函数的this对象。

arguments也是类似的方法。

#### 7.2.3内存泄漏

看下例代码：

```js
function assignHandler(){
    var element = document.getElementById('someElement')
    element.onclick=function(){
        console.log(element.id)
    }
}
```

匿名函数的作用域链中，包含了整个外部函数的活动对象。因此element变量无法销毁，一直占用内存。（且element是一个HTML元素。）

更改代码：

```js
function assignHandler(){
    var element = document.getElementById('someElement')
    var id = element.id
    element.onclick=function(){
        console.log(id)
    }
    element=null
}
```

上述代码，将element的id存储在一个变量中，且在闭包函数中引用此变量。但是闭包函数的作用域链中，仍然包含整个外部函数的活动对象，element仍然占用内存。将element置为空，让垃圾回收机制解除element占用的内存。

### 7.3模仿块级作用域

JS中是没有块级作用域的概念的。在块级语句中定义的变量，实际就是在包含函数中创建变量。

```js
function alertFunc(count){
    for(var i=0;i<count;i++){
        console.log(i)
    }
    console.log(i)
}
```

运行函数，两个console.log()都能正常输出。

使用var重新声明同一个变量也没关系。即，多次声明同一个变量，JS对后续声明视而不见，但是会执行后续的变量初始化。

```js
function alertFunc(count){
    for(var i=0;i<count;i++){
        console.log(i)
    }
    var i
    console.log(i)
}
```

代码正常输出。但是不可以重新初始化变量，这样会改变变量的值。

使用匿名函数模仿块级作用域（通常称为私有作用域）避免这样的问题。

```js
(function (){
	// 块级作用域
})()
```

上述代码定义，并调用了一个匿名函数。将函数包含在一对圆括号中，表示它实际上是一个函数表达式。后面跟的圆括号表示立即调用这个函数。

但是不能这样写,会报错，因为function开头，JS认为其是一个函数声明，函数声明后面不能跟圆括号，但是函数表达式可以。

```js
function (){
    // 块级作用域
}()
```

无论在什么时候，只要临时需要一些变量的时候，就可以使用私有作用域。

```js
function private(){
    (function (){
        for(var i=0;i<10;i++){
            console.log(i)
        }
    })()
    console.log(i)  // 产生一个错误
}
```

这种技术经常用在全局作用域中被用在函数外部，从而限制向全局作用域添加过多的变量和函数。

通过创建私有作用域，每个开发人员既可以使用自己的变量，也不用担心污染全局作用域。

### 7.4私有变量

严格来说，JS中是没有私有成员这个概念的，所有对象的属性都是公开的。但是有一个私有变量的概念。定义在函数中的变量被称为私有变量，在函数外部是无法访问这些变量的。

私有变量包括：命名参数、局部变量、函数内部定义的函数。

把有权访问私有变量和私有函数的公有方法叫做特权方法。

第一种创建特权方法的方式：在构造函数中定义特权方法。

```js
function MyObject(){
    // 私有变量和私有函数
    var privateVariable=10
    function privateFunction(){
        return false
    }
    // 特权方法
    this.publicMethod=function(){
        privateVariavle++
        return privateFunction()
    }
}
```

特权方法作为一个闭包，有权访问构造函数中的私有变量和私有函数。而MyObject()构造函数的实例，除了通过公有方法之外，无法直接访问构造函数的私有变量和私有方法。

利用私有和特权成员，隐藏那些不应该被直接修改的数据。

```js
function Person(name){
    this.getName=function(){
        return name
    }
    this.setName=function(value){
        name = value
    }
}
```

每次调用构造函数，创建实例，都会创建不同的内部函数及name。

内部的两个函数是闭包，有权访问构造函数中定义的变量。

### 7.5静态私有变量

通过在私有作用域中定义私有变量和函数，同样可以创建特权方法。

基本模式：

```js
(function(){
    // 私有变量和私有函数
    var privateVariable = 10;
    function privateFunc(){
        return false
    }
    // 构造函数
    MyObject=function(){

    }
    // 公有，特权方法
    MyObject.prototype.publicMethod=function(){
        privateVariable++
        return privateFunc()
    }
})()
```

注意：这个模式在定义构造函数的时候没有使用函数声明，使用函数表达式。这使MyObject成为了全局的变量，能够在私有作用域外访问到。但是，在严格模式下，给未经声明的变量赋值会导致错误。

```js
(function () {
    var name = ''
    // 构造函数
    Person = function (value) {
        name = value
    }
    // 特权方法
    Person.prototype.getName = function () {
        return name
    }
    Person.prototype.setName = function (value) {
        name = value
    }
})()
```

特权方法是在原型上定义的，所有的实例使用同样的特权方法。

Person构造函数，getName()函数，setName()函数都能访问name。

在这种模式之下，变量name就变成了一个静态的、所有实例共享的属性。

当在一个实例上修改了name的值，或者新建了一个实例，name属性就被修改了，所有实例使用的name值也同样被修改。

所有实例都会返回相同的name属性值。