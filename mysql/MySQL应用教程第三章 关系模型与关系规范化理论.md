# 第三章 关系模型与关系规范化理论

关系数据库建立在数学理论的基础上，关系模型是目前商品化数据库产品的主流数据模型。

对于关系型数据库来说，设计任务就是构造哪些关系模式，每个关系模式包含哪些属性。这是数据库逻辑结构设计问题。数据库设计需要理论指导。

关系数据库的规范化理论是数据库逻辑结构设计的理论指南。提供了判断关系模式优劣的理论标准，预测可能出现的问题，是数据库设计人员的有力工具，使数据库的设计工作有据可依。

## 3.1 关系模型及其定义

### 3.1.1 关系模型概述

关系模型中，只有关系这一种单一的数据结构，从用户的角度来说关系模型的逻辑结构就是一张二维表。其中，关于数据库结构的数据称为元数据。例如，表名、列名和列所属的表、表和列的属性等都是元数据。

#### 1.关系中基本术语

##### （1）元组

元组（即记录），关系表中的每一行对应一个元组，组成元组的元素称为分量。一个实体或实体之间的一个联系均使用一个元组来表示。

|   姓名   | 性别 | 专业 |
| :------: | :--: | :--: |
| 欧阳宝贝 |  女  | 工商 |
|   孙凯   |  男  | 商务 |

一行就是一个元组，“欧阳宝贝，女，工商”，就是一个元组，由三个分量组成。

##### （2）属性

关系中每列对应一个域，域可以相同。为了加以区分，给每个列一个命名，这个命名就是**属性**。

属性包含型和值，型：字段名和属性值域。值：属性具体取值。

关系中的字段名标识列。同一个关系中的字段名（列名）不能相同。一个关系通常具有多个属性，属性用于表示实体的特征。

##### （3）候选码

关系中能唯一标识一个元组的某一属性或属性组。

##### （4）主码

若一个关系有多个候选码，则选一个为主码（即主键、主关键字。）

![1605520691486](E:\前端\GitNote\Gitnote\mysql\图片笔记\1605520691486.png)

当包含两个或更多个的键称为复合码（键）。

主码不仅可以标识唯一的行，还可以建立与别的表之间的联系。

注意事项：

1） 建议简单的关键字为主码。 例如，学号和身份证号，建议使用学号作为主码。

2） 不建议使用符合主键，会给表的维护带来不便。

3） 若没有已有的字段（或者字段组合）可做为主码，数据库设计人员可添加一个没有实际意义的字段作为该表的主码。

4） 若使用添加的没有实际意义的字段作为主键，即代理键。则建议主键的值由数据库管理系统或者应用程序自动生成，避免人工录入时人为操作产生的错误。

##### （5）全码

最简单的情况，候选码只包含一个属性；最极端的情况，关系模式的所有属性是这个关系模式的候选码，称为全码。全码是候选码的特例。

##### （6）主属性和非主属性

主属性：候选码中的属性。非主属性：不包含在任何候选码中的属性。

##### （7）代理键

代理键是具有DBMS分配的唯一标识符，该标识符已经作为主键添加到表中。每次创建行时由DBMS分配代理键的唯一值，通常是较短的数字，该值永远不变。该值对于用户没有任何意义。

#### 2.数据库中关系的类型

关系数据库中的关系可以有3中类型：基本关系（基本表或基表）、查询表和视图表。

基本表：实际存在的表，是实际存储数据的逻辑表示。

查询表：查询结果表或查询中生成的临时表。

视图表：由基本表或其他视图表导出的表，是虚表，不对应实际存储的数据。

#### 3.关系的性质

1. 关系中的元组存储了某个实体或实体某个部分的数据。
2. 关系中元组的位置具有顺序无关性，即元组的顺序可以任意交换。
3. 同一属性的数据具有同质性，即同一列中的分量是同一类型的数据，它们来自同一个域。
4. 同一关系的字段名具有不可重复性，即同一关系中不同属性的数据可以出自同一个域，但不同的属性要给予不同的字段名。
5. 关系具有元组无冗余性，即关系中的任意两个元组不能完全相同。
6. 关系中列的位置具有顺序无关性，即列的次序可以任意交换、重新组织。
7. 关系中每个分量必须取原子值，即每个分量都必须是不可分的数据项。

#### 4.关系模式

![1605605949477](E:\前端\GitNote\Gitnote\mysql\图片笔记\1605605949477.png)

![1605606604060](E:\前端\GitNote\Gitnote\mysql\图片笔记\1605606604060.png)

### 3.1.2 关系操作

关系模型与其他数据模型相比，最具特色的是关系操作语言。

#### 1.关系操作的基本内容

关系操作包括：**数据查询**、**数据维护**和**数据控制**。

- **数据查询**指数据检索、统计、排序、分组，以及用户对信息的需求等功能。
- **数据维护**指数据增加、删除、修改等数据自身更新的功能。
- **数据控制**是为了保证数据的安全性和完整性而采用的数据存储控制及并发控制等功能。

数据查询和数据维护功能使用关系代数中的8种操作来表示，即并、差、交、广义的笛卡尔积、选择、投影、连接和除。选择、投影、并、差、笛卡尔积是5种基本操作，其他操作可以由基本操作导出。

#### 2.关系操作语言的种类

关系操作语言可分为三类：

1）关系代数语言：用对关系的运算来表达查询要求的语言。ISBL（Information System Base Language）是关系代数语言的代表。

2）关系演算语言：用查询得到的元组应满足的谓词条件来表达查询要求的语言。可以分为元组关系演算语言和域关系演算语言。

3） 具有关系代数和关系演算双重特点的语言。SQL（Structure Query language）是介于关系代数和关系演算之间的语言，包括数据定义、数据操作和数据控制三种功能，是关系数据库的标准语言。

### 3.1.3 关系的完整性

关系模型的完整性：对关系的某种约束条件。

关系模型允许定义三类完整性约束：实体完整性、参照完整性、用户自定义的完整性。

其中前两者是关系模型必须满足的完整性约束条件，称为两个不变性，由关系系统自动支持。用户自定义的完整性是应用领域需要遵守的约束条件，体现了具体领域中的语义约束。

#### 1.实体完整性

规则：**若属性A是基本关系B的主属性，则属性A不能为空**。

例：学生关系，“学生（学号，姓名，性别，专业号，年龄）”，“学号”是主码，则“学号”不能取空值。

若主码由多个属性组成，则所有这些属性都不能取空值。

例，学生选课关系，“选修（学号，课程号，成绩）”，“学号、课程号”是主码，则两个属性都不能取空值。

实体完整性规则说明：

1）该规则是针对基本关系而言的。

2）信息世界中的实体是可区分的，即它们具有某种唯一性标识。

3）关系模型中以主码作为唯一标识。

4）主码中的属性，即主属性不能取空。

#### 2.参照完整性

实体之间是存在某种联系的，在关系模型中实体及实体间的联系都是用关系描述的，故存在关系与关系之间的引用。

![1605776612203](E:\前端\GitNote\Gitnote\mysql\图片笔记\1605776612203.png)

![1606098945516](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606098945516.png)

![1606099035930](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606099035930.png)

![1606102111363](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606102111363.png)

参照完整性：**若属性（或属性组）F是基本关系R的外码，它与基本关系S的主码Ks相对应（基本关系R和S有可能是同一关系），则对于R中每个元组在F上的值必须为以下值之一。**

**1） 取空值（F的每个属性值均为空值）。**

**2） 等于S中某个元组的主码值。**

![1606102465151](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606102465151.png)

![1606102502676](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606102502676.png)

#### 3.用户定义的完整性

任何关系数据库系统都应该支持实体完整性和参照完整性。除此之外，不同的关系数据库系统根据其应用环境的不同，还需要支持一些特殊的约束条件。用户自定义的完整性就是针对某一具体关系数据库的约束条件的，它反映某一具体应用所涉及的数据必须满足的语义要求。

例如：某个属性必须取唯一值、属性值之间应满足一定的关系、某属性的取值范围在一定区间之内等。

关系模型应提供定义和检验这类完整性的机制，以便用统一的系统方法处理它们，而不要应用程序承担这一功能。

## 3.2 关系代数

关系代数：一种抽象的查询语言，是关系数据操作语言的一种传统表达方式，是用对关系的运算来表达查询的。

关系代数建立在集合代数基础上，所以先定义几个关系术语中的数学定义。

### 3.2.1 关系的数学定义

#### 1.域

域是一组具有相同数据类型的值的集合。

![1606111441404](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606111441404.png)

#### 2.笛卡尔积

![1606111471598](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606111471598.png)

![1606111636674](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606111636674.png)

#### 3.关系

D1xD2x...xDn的子集叫做在域D1，D2,...,Dn上的关系，表示为R(D1,D2,...,Dn)。

R：关系的名字。n:关系的目或度。 n=1时，称该关系为单目关系。n=2时，二目关系。关系是笛卡尔积的有限子集，所以，关系也是一个二维表。

![1606112861346](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606112861346.png)

### 3.2.2 关系代数概述

关系代数：一种抽象的查询语言，是关系数据操作语言的一种传统表达方式，是用对关系的运算来表达结果的。

任何一种运算都是将一定的运算符作用于一定的运算对象上，的到预期的结果，因此，运算对象、运算符、运算结果是运算的三大要素。

关系代数中：运算对象--》关系，运算结果---》关系。

关系代数中使用的运算符：集合运算符、专门的关系运算符、比较运算符、逻辑运算符。

![1606113580817](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606113580817.png)

![1606113642499](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606113642499.png)

#### 3.2.3 传统的集合运算

传统的集合运算：是二目运算，包括并、交、差、广义的笛卡尔积。

设关系R和关系S具有相同的目n（即两个关系都具有n个属性），且相应的属性取自同一个域，则可以定义并、差、交、广义笛卡尔积。

##### 1.并

![1606115266874](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606115266874.png)

##### 2.差

![1606115305878](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606115305878.png)

##### 3.交

![1606115329920](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606115329920.png)

##### 4.广义笛卡尔积

![1606115421479](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606115421479.png)

#### 3.2.4 专门的关系运算

##### 1.选择

![1606206426974](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606206426974.png)

![1606206445015](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606206445015.png)

![1606206479646](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606206479646.png)

![1606206493741](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606206493741.png)

##### 2.投影(Projection) 

关系R上的投影是从R中选择若干字段名组成新的关系。记作:

![1606206849648](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606206849648.png)

A:R中的字段名。

投影操作是从列的角度进行的运算。投影之后不仅取消了原关系中的某些列，而且还可取消某些元组。因为取消了某些字段名后，就可能出现重复行，应取消这些完全相同的行。

![1606206958154](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606206958154.png)

![1606207085538](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606207085538.png)

##### 3.连接

![1606207257819](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606207257819.png)

![1606207560338](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606207560338.png)

![1606207573149](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606207573149.png)

##### 4.除运算（Division）

![1606207965791](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606207965791.png)

![1606208021226](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606208021226.png)

![1606208408063](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606208408063.png)

![1606208427269](E:\前端\GitNote\Gitnote\mysql\图片笔记\1606208427269.png)

## 3.3. 数据库关系模式的规范化

关系数据库的设计主要是关系模式的设计，关系模式设计的好坏直接影响到数据库设计的成败。

设计数据库时要考虑减少冗余数据和避免数据经常变化，减少额外的维护。

规范化的核心思想：表中每个决定因子都必须是候选键;若不满足，可以将表分解成两个或多个满足条件的表。

因此，规范化就是检查并修改表使其结构良好的过程，即表中每个决定因子都必须是**候选键**。这个过程之所以叫规范化，是因为表中容易出现的问题可以归人不同的范式( Normal Form)类型。

将关系模式规范化，使之达到较高的范式是设计好关系模式的唯一途径， 否则，设计的关系数据库会产生系列的问题。

### 3.3.1 问题的提出

关系模式的完整表示是一个五元组: R(U,D,Dom,F)
R:关系名，代表一个关系模式。
U:关系模式R的属性集合(属性组)。
D:属性集合U的数据域。
Dom:属性到域的映射关系。
F:属性集合U上的一组数据依赖的集合。
D和Dom对设计关系模式的作用不大，在讨论关系规范化理论时简化为三元组: R(U,F)。

