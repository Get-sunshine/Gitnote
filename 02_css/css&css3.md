# css重点内容

## 1.viewport

```HTML
<meta name="viewport" content="width=device-width, user-scalable=no,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0">
```

视口分为三个：

1.布局视口（layout viewport）

 一般移动设备的浏览器都默认设置了一个viewport 元标签，定义一个虚拟的layout viewport（布局视口），解决早期页面在手机上显示的问题。iOS, Android基本都将这个视口分辨率设置为 980px，所以pc上的网页基本能在手机上呈现，只不过元素看上去很小，一般默认可以通过手动缩放网页 。

2.视觉视口（visual viewport）

 是设备的屏幕区域，换句话说就是用户通过屏幕所看到的页面内容。但它所对应的并不是指屏幕区域里的物理像素，而是CSS 像素。 

3.理想视口（ideal viewport）

 是基于布局视口的。他其实就是变了尺寸的布局视口。理想视口就是把布局视口调整到合适的状态，让页面有最好的表面效果，这也是它名字的由来。 

设置理想视口，得添加meta标签。一般设置理想视口等于device-width（设备屏幕宽度）（width=device-width,initial-scale=1.0），设备多宽，理想视口就多宽，即布局视口就有多宽，覆盖浏览器自己设置的默认布局视口。

4.meta标签内的内容。

|    属性名     |          取值           |                   描述                   |
| :-----------: | :---------------------: | :--------------------------------------: |
|     width     | 正整数或者device-width  |           定义视口宽度，单位px           |
|    height     | 正整数或者device-height |      定义视口高度，单位px，一般不用      |
| initial-scale |       [0.0-10.0]        |              定义初始缩放值              |
| minimum-scale |       [0.0-10.0]        | 定义缩小的最小比例，小于等于最大放大比例 |
| maximum-scale |       [0.0-10.0]        | 定义放大的最大比例，大于等于缩小最小比例 |
| user-scalable |         yes/no          |    是否允许用户手动缩放页面，默认yes     |

5.物理像素比

物理像素是指屏幕显示的最小颗粒，是真实存在的。

CSS中1px不一定等于一个物理像素。

PC端1px为一个物理像素，但是移动端不一定。物理像素比是指1px占用的物理像素比个数，也称屏幕像素比。

## 2.盒子模型

盒子

content + padding+border+margin

标准盒子

width、height是content-width、content-height

IE盒子：

width=content-width+padding-width+border-width

height=content-height+padding-width+border-width

其中，盒子又分为块级盒子、行内盒子、行内块盒子

块级盒子，display:block;

独占一行，可以设置width、height，padding、border、margin，可以包含块级元素和行内元素。

从上到下，依次排列。



行内盒子，display:inline;

从左到右，排列一行，不可包含块级元素。

不可设置width、height，margin上下不影响布局、左右影响布局，padding上下不影响布局、左右影响布局，border上下不影响布局，左右影响布局。



行内块，display:inline-block；

既可设置width、height、padding、border、margin、也在一行内显示。

## 3.浮动

**文档流**

正常文档流，行内元素从左到右，在一行排列，宽度不够时，自动换行。块级元素，独占一行，从上到下，依次排列。

**浮动**

给一个元素添加float，元素浮动。浮动设置的初衷是为了让文字环绕图片。

**浮动元素特性**

1.浮动元素脱离文档流向左或向右浮动，直到遇到父元素或另一个浮动元素。

2.浮动元素的特性类似于inline-block，可以一行排列，可以设置宽、高等，默认高宽由内容撑开。

3.浮动元素不会覆盖在其后面的同级盒子中的文字。

4.浮动导致父元素高度塌陷。

**解决浮动带来的父元素高度塌陷**

1.clear清除浮动

 clear属性不允许被清除浮动的元素的左边/右边挨着浮动元素，底层原理是在被清除浮动的元素上边或者下边添加足够的清除空间 。

```html

<div class="box-wrapper">
     <div>宽高100px，浮动</div>
    <!-- clear方式一添加空白div -->
     <div style="clear:both;">宽高100px，清除浮动</div>
</div>
```

```html
<!-- clear方式二使用:after伪元素,这是较通用的方式 -->
.box-wrapper{
	content:'';
	display:block;
	clear:both;
}

<div class="box-wrapper">
     <div>宽高100px，浮动</div>
    
</div>
```

2.BFC

给父元素开启BFC，清除子元素浮动带来的高度塌陷。



## 4.定位

**包含块**

- 对于浮动元素，包含块定义为最近的块级祖先元素。（即父元素？）
- 根元素的包含块，由用户代理创建。在HTML中，根元素是html元素，有些浏览器也会将body作为根元素。大多数浏览器中，初始包含块是一个视窗大小的矩形。
- 对于非根定位元素
  - relative或static元素。包含块是由最近的块级框祖先框的内容边界构成。
  - absolute，包含块是最近的定位属性不为static的祖先元素。若祖先元素是块级框，则包含块设置为该祖先元素的内边距边界，即由边框界定的区域。若没有开启定位的祖先元素，包含块则是初始包含块。

**相对定位**

- 相对于在标准流中的位置（原位置）定位。
- 提升元素层级。
- 原来在文档流中的位置仍然保存（原位置是空白），不会被挤掉。

**绝对定位**

- 相对于最近的开启了定位的祖先元素定位，如果没有开启定位的祖先元素，则相对于初始包含块定位。
- 脱离文档流，提升层级，文档流中原位置不保存。

**固定定位**

- 相对于初始包含块定位。
- 脱离文档流，提升层级，文档流中原位置不保存。

**静态定位static**

- 浏览器默认的效果。



## 5.css隐藏元素的方式

```css
/* 元素隐藏，不占位 */
position: absolute;
left: -9999px;

/* 元素隐藏，不占位 */
display: none;

/* 元素隐藏，站位 */
visibility: hidden;

/* 元素隐藏，站位 */
opacity: 0; 

```

## 6.水平居中方式

- 若子元素是行内元素，给父元素添加text-align:center;

- 若该元素是块级元素，该元素设置margin:0 auto;

- 在父元素上使用flex布局

  ```css
  div{
      display:flex;
      justify-content:center;
  }
  ```

- 使用css3的transfrom,以及定位

  ```css
  div{
  	position:absolute;
      left:50%;
      transform:translateX(-50%);
  }
  ```

- 定位和margin，盒子需要有宽度

  ```css
  div{
      position:absolute;
      left:50%;
      width:50px;
      margin-left:-25px;
  }
  ```

- 定位和margin，盒子需要宽度

  ```css
  div{
      position:absolute;
      left:0px;
      right:0px;
      width:50px;
      margin:0 auto;
  }
  ```



## 7.垂直居中方式

- 单行文本垂直居中，line-height等于盒子高度。

- 元素是行内块

  ```css
  .parent::after, .son{
      display:inline-block;
      vertical-align:middle;
  }
  .parent::after{
      content:'';
      height:100%;
  }
  ```

- 使用flex

  ```css
  .parent{
      display:flex;
      align-items:center;
  }
  ```

- transform和定位

  ```css
  .son{
      position:absolute;
      top:50%;
      transform:translateY(-50%);
  }
  ```

- 定位和margin

  ```css
  .son{
      position:absolute;
      top:50%;
      height:固定高度;
      margin-top:-高度一半;
  }
  ```

- 定位和margin

  ```css
  .son{
      height:固定高度;
      position:absolute;
      top:0;
      bottom:0;
      margin:0 auto;
  }
  ```



## 8.BFC

BFC直译为块级格式化上下文，是一个独立的渲染区域，只有块级盒子才参与。它规定内部的盒子如何布局，并且这个区域与外部毫无关系。

**如何生成BFC**？

- 根元素
- float不为none
- overflow不为visible
- display的值为inline-block，table-cell（HTML表格单元的默认属性）
- 绝对定位元素（fixed也是绝对定位的一种）
- 弹性盒子，display:flex或者display:inline-flex;

**BFC的约束规则**

- 内部的盒子会在垂直方向上一个接一个的放置。
- 垂直方向上的距离由margin决定。
- 每个元素的左外边距与包含块的左边界相接触，即使是浮动元素也是如此。
- BFC的区域不会与float的元素区域重叠。
- BFC的高度包含浮动元素。
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素，反之亦然。

总之，BFC最显著的一个效果就是建立一个隔离的空间，断绝空间内外元素间相互的作用。



## 9.边距重叠

**什么是margin重叠**？

边距重叠是指两个或多个盒子（可能相邻也可能嵌套）的相邻边界（其间没有任何非空内容、补白、边框）重合在一起而形成一个单一边界。边距重叠一般只产生在标准文档流中相邻垂直边界。

**父子元素边距重叠**

解决办法：让父子的相邻边界不相邻或给父元素开启BFC。给父添加border、padding、before，给在子元素前添加非空兄弟元素，都行。

```less
.parent {
      width: 100px;
      height: 100px;
      background-color: pink;
      .son {
          margin-top: 10px;
      }
    }
```

**兄弟元素的边界重叠**

解决办法：让两个元素属于不同BFC、让两个元素不相邻或者让后一个兄弟元素浮动。

**空白元素的上下外边距重叠**

当一个元素内容为空，border、padding均没有时，此元素的margin-top和margin-bottom会相邻，发生重叠。

## 10.三栏布局

两边固定，中间自适应。

当中列要完整显示。

**定位实现三栏布局**

```css
*{
    margin: 0px;
    padding: 0px;
}
body{
    /*最小宽度的计算？ 2*left+right*/
    min-width: 600px;
}
div{
    height: 200px;
}
.left,.right{
    width: 200px;
    background-color: pink;
}
.middle{
    background-color: deeppink;
    padding: 0px 200px;
}
.left{
    position: absolute;
    left: 0px;
    top: 0px;
}
.right{
    position: absolute;
    right: 0px;
    top: 0px;
}
```

```html
<body style="border: solid 1px; position: relative;">
    <div class="left">
        left
    </div>
    <div class="middle">
        middle
    </div>
    <div class="right">
        right
    </div>
</body>
```

**浮动实现三列布局**

```css
*{
    margin: 0px;
    padding: 0px;
}
body{
    /*最小宽度的计算？ 2*left+right*/
    min-width: 600px;
}
div{
    height: 200px;
}
.left,.right{
    width: 200px;
    background-color: pink;
}
.left{
    float: left;
}
.right{
    float: right;
}
.middle{
    padding: 0 200px;
    text-align: center;
    background-color: deeppink;
}
```

```html
<div class="left">
    left
</div>
<div class="right">
    right
</div>
<div class="middle">
    middle
</div>
```

**圣杯布局**

步骤：

1.搭好dom结构

2.middle、left、right都浮动，设置好margin，到达基本位置

3.left、right设置相对定位，到达最终位置。

```css
*{
    margin: 0px;
    padding: 0px;
}
body{
    min-width: 600px;
}
.header,.footer{
    height: 20px;
    border: solid 1px pink;
    background-color: gray;
    text-align: center;
}
.content{
    padding: 0px 200px;
}
.content .middle{
    float: left;
    width: 100%;
    background-color: pink;

}
.content .left{
    position: relative;
    left: -200px;
    margin-left: -100%;
    float: left;
    width: 200px;
    background-color: yellow;

}
.content .right{

    float: left;
    width: 200px;
    background-color: yellow;
    margin-left: -200px;
    position: relative;
    right: -200px;

}
.clearfix:after{
    content: "";
    display: table;
    clear: both;
 }
```

```css
<div class="header">header</div>
<div class="content clearfix">
    <div class="middle">
        <p>middle</p>
        <p>middle</p>
        <p>middle</p>
        <p>middle</p>
    </div>
    <div class="left">
    	left
    </div>
    <div class="right">
    	right
    </div>
</div>
<div class="footer">footer</div>
```

**双飞翼布局**

步骤：

1.搭好dom结构。

2.content .middle .inner设置好padding （为什么是inner设置padding，而不是middle呢？思考）。

3.left、right、middle都浮动。

4.left、right调整到合适位置。

```css
*{
    margin: 0px;
    padding: 0px;
}
body{
    min-width: 600px;
}
.header,.footer{
    border: solid 1px black;
    background-color: wheat;
    text-align: center;
}
.content .left,.content .middle,.content .right{
    float: left;
    padding-bottom: 10000px;
    margin-bottom:-10000px;

}
/*双飞翼布局*/
.content{
    overflow: hidden;
}
.content .middle{
    width: 100%;
    background-color: deeppink;
}
/*双飞翼布局的关键之处*/
.content .middle .m_inner{
    /*background-color: black;*/
    padding: 0 200px;
}

.content .left,.content .right{
    width: 200px;
    text-align: center;
    background-color: pink;
}
.content .left{
    margin-left: -100%;
}
.content .right{
    margin-left: -200px;
}
```

```html
<div class="header">
    <h4>header</h4>
</div>
<div class="content">
    <div class="middle">
        <!--双飞翼布局的关键，不给content加padding，不给left和right用相对定位-->
        <div class="m_inner">
            <p>middle</p>
            <p>middle</p>
            <p>middle</p>
            <p>middle</p>
            <p>middle</p>
        </div>
    </div>
    <div class="left">
        left
    </div>
    <div class="right">
        right
    </div>
</div>
<div class="footer">
    <h4>header</h4>
</div>
```

**Flex实现三栏布局**

## 11.margin、padding的百分比

margin、padding可以使用百分比，百分比值是参照于容器的宽度的。

## 12.::before和:before

单冒号用于css3伪类，双冒号用于css3伪元素。

对于css2之前已有的伪元素，例如:before，单冒号和双冒号写法的作用是一样的。

## 13.rem和em

rem和em都是相对单位。

rem是相对于根元素html的font-size。

em是相对于父元素的font-size。

## 14.transition的坑

元素的transition属性在元素首次渲染还没完成的情况下是不会被触发的。

## 15.:nth-of-type和:nth-child

**p:nth-of-type()**

将同级的p（元素）分成一组，从1开始排列。

**p:nth-child()**

将同级的元素分成一组，从1开始排列，选择某个位置的元素，且该元素匹配给定标签。



## 16. 像素和DPR

像素（pixel），也称画素，是图像显示的基本单位。

一个像素就是计算机能够显示一种特定颜色的最小区域。若设备尺寸相同，但像素更密集时，屏幕能显示的画面的过渡更细致，网站看起来更明快。

ppi：每英寸可以显示的像素点的数量，即屏幕像素密度。

![1615354698309](E:\前端\GitNote\Gitnote\02_css\图片笔记\1615354698309.png)

像素分为两种：

设备像素、CSS像素。

设备像素：屏幕物理像素。

CSS像素：也称逻辑像素、css和JavaScript中使用的都是逻辑像素。

桌面端：1个css像素往往对应1个物理像素。

移动端：根据缩放比来看。

DPR：设备像素比。默认缩放100%的情况下，设备像素和CSS像素的比值。 DPR=设备像素/CSS像素（某一方向上）

![1615355436692](E:\前端\GitNote\Gitnote\02_css\图片笔记\1615355436692.png)

