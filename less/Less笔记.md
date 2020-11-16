# less笔记

## 一、变量

### 1. 定义

```less
@link-color:#428bca;
a,link{
	color:@link-color;
}
```

### 2.插值

#### 2.1用在selector

```less
@my-selector:banner;
.@{my-selector}{
	font-weight:bold;
	line-height:40px;
	margin:0 auto;
}
```

#### 2.2用在URLs网址

```less
@images:'../img';
body{
    color:#444;
    background:url('@{images}/white-sand.png');
}
```

#### 2.3用在import

```less
@themes:'../../src/thems';
@import '@{themes}/tidal-wave.less';
```

#### 2.4用在properties

```less
@property:color;
.widget{
    @{property}:#0ee;
    background-@{property}:#999;
}
```

### 3.变量定义变量

```less
@primary:green;
@secondary:blue;
.section{
    @color:primary;
    .element{
        color:@@color;
    }
}
// 等同于
.section .element{
    color:green;
}
```

### 4.惰性计算

```less
// valid
.lazy-eval{
    width:@var;
}
@var:@a;
@a:9%;

// valid
.lazy-eval{
    width:@var;
    @a:9%;
}
@var:@a;
@a:100%;
// compile into
.lazy-eval{
    width:9%;
}
```

#### 4.1多次定义变量，最后一次有效，并总是从当前作用域开始向上搜索。

```less
// valid
@var:0;
.class{
    @var:1;
    .brass{
        @var:2;
        thress:@var;
        @var:3;
    }
    one:@var;
}
// compile into 
.class{
    one:1;
}
.class .brass{
    three:3;
}
```

### 5.将属性作为变量($property语法)

```less
// valid
.widget{
    color:#efefef;
    background-color:$color;
}
// compile into
.widget{
    color:#efefef;
    background-color:#efefef;
}
```

类似变量，多次定义属性，最后一次有效。

## 二、父选择器

### 1.&操作符

```less
// valid
a{
    color:blue;
    &:hover{
        color:green;
    }
}
// compile into
a{
    color:blue;
}
a:hover{
    color:green;
}
```

### 2.产生重复的类名

```less
.button{
    &-ok{
        background-image:url('ok.png');
    }
    &-cancel{
        background-image:url('cancel.png');
    }
    &-custom{
        background-image:url('custom.png');
    }
}
// compile into 
.button-ok {
  background-image: url("ok.png");
}
.button-cancel {
  background-image: url("cancel.png");
}
.button-custom {
  background-image: url("custom.png");
}
```

### 3.多个&

```less
// valid
.link {
  & + & {
    color: red;
  }

  & & {
    color: green;
  }

  && {
    color: blue;
  }

  &, &ish {
    color: cyan;
  }
}
// compile into
.link + .link {
  color: red;
}
.link .link {
  color: green;
}
.link.link {
  color: blue;
}
.link, .linkish {
  color: cyan;
}
```

&代表的是所有的父级选择器

```less
// valid
.grand {
  .parent {
    & > & {
      color: red;
    }

    & & {
      color: green;
    }

    && {
      color: blue;
    }

    &, &ish {
      color: cyan;
    }
  }
}
// result
.grand .parent > .grand .parent {
  color: red;
}
.grand .parent .grand .parent {
  color: green;
}
.grand .parent.grand .parent {
  color: blue;
}
.grand .parent,
.grand .parentish {
  color: cyan;
}
```

### 4.改变选择器顺序

```less
// valid
.header {
  .menu {
    border-radius: 5px;
    .no-borderradius & {
      background-image: url('images/button-background.png');
    }
  }
}
// result
.header .menu {
  border-radius: 5px;
}
.no-borderradius .header .menu {
  background-image: url('images/button-background.png');
}
```

### 5.爆炸组合？

```less
// valid
p, a, ul, li {
  border-top: 2px dotted #366;
  & + & {
    border-top: 0;
  }
}
// result
p,a,ul,li {
  border-top: 2px dotted #366;
}
p + p,p + a,p + ul,p + li,a + p,a + a,a + ul,a + li,ul + p,ul + a,ul + ul,ul + li,li + p,li + a,li + ul,li + li {
  border-top: 0;
}
```

## 三、Extend

### 1.extend

```less
// valid
nav ul{
    &:extend(.inline);
    background:blue;
}
.inline{
    color:red;
}
// result
nav ul{
    background:blue;
}
.inline,nav ul{
    color:red;
}
```

### 2.extend语法

extend 可以附加在选择器后面，也可以放入规则集。看起来像一个伪类，extend后的参数all可选。

```less
// valid
.a:extend(.b){}
// valid
.a{
    &:extend(.b);
}

// valid
.c:extend(.d all) {
  // extends all instances of ".d" e.g. ".x.d" or ".d.x"
}
// valid
.c:extend(.d) {
  // extends only instances where the selector will be output as just ".d"
}

//  one or more classes
.e:extend(.f) {}
.e:extend(.g) {}

// the above and the below do the same thing
.e:extend(.f, .g) {}
```

### 3.extend attached to selector

```less
// extend after the selector
pre:hover:extend(div pre)
// space between selector and extend is allowed
pre:hover :extend(div pre)
// multiple extends are allowed 
pre:hover:extend(div pre):extend(.bucket tr)  
// same
pre:hover:extend(div pre,.bucket tr)
// this is noe allowed,extend must be last
pre:hover:extend(div pre).nth-child(odd)

// If a ruleset contains multiple selectors, any of them can have the extend keyword. Multiple selectors with extend in one ruleset:

.big-division,
.big-bag:extend(.bag),
.big-bucket:extend(.bucket) {
  // body
}
```

### 4. Extend Inside Ruleset

```less
// valid
pre:hover,
.some-class{
    &:extend(div pre);
}
// is exactly the same as adding an extend after each selector
pre:hover:extend(div pre),
.some-class:extend(div pre){
    
}
```

### 5.Extending nested selectors

```less
// valid
.bucket{
    tr{
        color:blue;
    }
}
.some-class:extend(.bucket tr){}
// result
.bucket tr,
.some-class{
    
}
// Essentially the extend looks at the compiled css, not the original less
.bucket{
    tr &{
        color:blue;
    }
}
.some-class:extend(tr .bucket){}
// outputs
tr .bucket,
.some-class{
    color:blue;
}
```

### 6. Exact Matching with Extend

 Extend by default looks for exact match between selectors. 

```less
// valid
.a.class,
.class.a,
.class > .a {
  color: blue;
}
.test:extend(.class) {} // this will NOT match the any selectors above

//Selectors *.class and .class are equivalent, but extend will not match them
*.class {
  color: blue;
}
.noStar:extend(.class) {} // this will NOT match the *.class selector

// output
*.class{
    color:blue;
}

//  Selectors link:hover:visited and link:visited:hover match the same set of elements, but extend treats them as different:
link:hover:visited{
    color:blue;
}
.selector:extend(link:visited:hover){}
// outputs
link:hover:visited{}
```

### 7. nth Expression

```less
// 1. Nth-expressions 1n+3 and n+3 are equivalent, but extend will not match them
:nth-child(1n+3){
    color:blue;
}
.child:extend(:nth-child(n+3)){}

// outputs
:nth-child(1n+3){
    color:blue;
}
// 2.Quote type in attribute selector does not matter. All of the following are equivalent.
[title=identifier] {
  color: blue;
}
[title='identifier'] {
  color: blue;
}
[title="identifier"] {
  color: blue;
}
.noQuote:extend([title=identifier]) {}
.singleQuote:extend([title='identifier']) {}
.doubleQuote:extend([title="identifier"]) {}
// outputs
[title=identifier],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}
[title='identifier'],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}
[title="identifier"],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}

```

### 8. Extend "all"

 When you specify the all keyword last in an extend argument it tells Less to match that selector as part of another selector. The selector will be copied and the matched part of the selector only will then be replaced with the extend, making a new selector. 

```less
.a.b.test,
.test.c {
  color: orange;
}
.test {
  &:hover {
    color: green;
  }
}

.replacement:extend(.test all) {}
// outputs
.a.b.test,
.test.c,
.a.b.replacement,
.replacement.c {
  color: orange;
}
.test:hover,
.replacement:hover {
  color: green;
}
```

### 9.Selector Interpolation with Extend 

```less
//1.Selector with variable will not be matched
@variable:.bucket;
@{variable}{
    color:blue;
}
.some-class:extend(.bucket){} // does nothing,no match is found

//2. extend with variable in target selector matches nothing
.bucket{
    color:blue;
}
.some-class:extend(@{variable}){} // interpolated selector matches nothing
@variable:.bucket;
// Both of the above examples compile into
.bucket {
  color: blue;
}

//3.However, :extend attached to an interpolated selector works
.bucket{
    color:blue;
}
@{variable}:extend(.bucket){}
@variable:.selector;
//outputs
.bucket,.selector{
    color:blue;
}
```

### 10.Scoping / Extend Inside @media

```less
// 1.Currently, an :extend inside a @media declaration will only match selectors inside the same media declaration
@media print {
  .screenClass:extend(.selector) {} // extend inside media
  .selector { // this will be matched - it is in the same media
    color: black;
  }
}
.selector { // ruleset on top of style sheet - extend ignores it
  color: red;
}
@media screen {
  .selector {  // ruleset inside another media - extend ignores it
    color: blue;
  }
}
// outputs
@media print {
  .selector,
  .screenClass { /*  ruleset inside the same media was extended */
    color: black;
  }
}
.selector { /* ruleset on top of style sheet was ignored */
  color: red;
}
@media screen {
  .selector { /* ruleset inside another media was ignored */
    color: blue;
  }
}

//2.Note: extending does not match selectors inside a nested @media declaration
@media screen {
  .screenClass:extend(.selector) {} // extend inside media
  @media (min-width: 1023px) {
    .selector {  // ruleset inside nested media - extend ignores it
      color: blue;
    }
  }
}
// outputs
@media screen and (min-width: 1023px) {
  .selector { /* ruleset inside another nested media was ignored */
    color: blue;
  }
}

//3.Top level extend matches everything including selectors inside nested media
@media screen {
  .selector {  /* ruleset inside nested media - top level extend works */
    color: blue;
  }
  @media (min-width: 1023px) {
    .selector {  /* ruleset inside nested media - top level extend works */
      color: blue;
    }
  }
}

.topLevel:extend(.selector) {} /* top level extend matches everything */
// outputs
@media screen {
  .selector,
  .topLevel { /* ruleset inside media was extended */
    color: blue;
  }
}
@media screen and (min-width: 1023px) {
  .selector,
  .topLevel { /* ruleset inside nested media was extended */
    color: blue;
  }
}
```

### 11. Duplication Detection 重复检测

```less
// 目前没有重复检测手段
.alert-info,
.widget {
  /* declarations */
}

.alert:extend(.alert-info, .widget) {}
// outputs
.alert-info,
.widget,
.alert,
.alert {
  /* declarations */
}
```

## 四、Merge

 The `merge` feature allows for aggregating values from multiple properties into a comma or space separated list under a single property. `merge` is useful for properties such as background and transform. 

merge将多个属性的值聚合到单个属性下的逗号或者空格分隔的列表中。

```less
// comma
.mixin(){
    box-shadow+:inset 0 0 10px #555;
}
.myclass{
    .mixin();
    box-shadow+:0 0 20px black;
}
// outputs
.myclass{
    box-shadow:inset 0 0 10px #555,0 0 20px black;
}

// space
.mixin(){
    transform+_:scale(2);
}
.myclass{
    .mixin();
    transform+_:rotate(15deg);
}
// outputs
.myclass{
    transform:scale(2) rotate(15deg);
}
```

