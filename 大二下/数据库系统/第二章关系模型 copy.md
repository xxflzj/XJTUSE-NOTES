[TOC]

# [XJTUSE DATABASE]——第二章 关系模型

![image-20230503174030680](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20230503174030680.png)

## 一、基本概念

### 关系模型三要素

**基本结构**：关系/表

**基本操作**：Relation Operator 

> 基本的：**选择，投影，并，差，笛卡尔积**
>
> 扩展的：**交、连接、除**

 **完整性约束**：实体完整性、参照完整性和用户自定义的完整性

### 如何定义关系

:one: **首先定义“列”的取值范围“域(Domain)”**

> 一组值的集合，这组值具有**相同的数据类型**
>
> > 如整数的集合、字符串的集合、全体学生的集合
> >
> > 再如, 由8位数字组成的数字串的集合，由0到100组成的整数集合
>
> 集合中元素的个数称为域的**基数(Cardinality)**
>
> 不同的列可来自同一个域，称其中的每一列为一个属性，不同的属性要给予不同的属性名。

:two: **再定义“元组”及所有可能组合成的元组：笛卡尔积**

一组域D1 , D2 ,…, Dn的笛卡尔积为:

> $D1×D2×…×Dn = \{ (d1 , d2 , … , dn) | di∈Di , i=1,…,n \}$

笛卡尔积的每个元素(d1 , d2 , … , dn)称作一个n元组（行）

若Di的基数为mi，则笛卡尔积的基数，即元组个数为$ m_1\times m_2\times ...\times m_n$

:three: **关系**

一组域D1 , D2 ,…, Dn的笛卡尔积的子集。

**笛卡尔积中具有某一方面意义的那些元组（行）被称作一个关系(Relation)。**

由于关系的不同列可能来自同一个域，为区分，需要为每一列起一个名字，该名字即为**属性名**。

关系可用$R(A_1:D_1 , A_2:D_2 , … , A_n:D_n )$表示，可简记为$R(A_1 , A_2 , … , A_n )$，这种描述又被称为关系模式(Schema)或**表标题(head)**

>  R是关系的名字, Ai 是属性, Di 是属性所对应的域, n是关系的元或目(degree), 关系中元组的数目称为关系的基数(Cardinality)

例如下图的关系为3元(目)关系，描述为：

家庭(丈夫:男人，妻子:女人,子女:儿童)或家庭(丈夫，妻子, 子女)

![image-20220118180130030](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220118180130030.png)

### 重要概念

**关系模式与关系** 

>  同一关系模式下，可有很多的关系。
>
>  关系模式是关系的结构, 关系是关系模式在某一时刻的数据。
>
>  关系模式是稳定的；而关系是某一时刻的值，是随时间可能变化的。**

#### 候选码(Candidate Key)/候选键

**关系中的一个属性组，其值能唯一标识一个元组**，若从该属性组中去掉任何一个属性，它就不具有这一性质了，这样的属性组称作候选码。

> 例如：“学生(S#, Sname, Sage, Sclass)”，S#就是一个候选码，在此关系中，任何两个元组的S#是一定不同的，而这两个元组的Sname, Sage, Sclass都可能相同(同名、同龄、同班)，所以S#是候选码。

> 再如：“选课(S#, C#, Sname, Cname, Grade)”，(S#,C#)联合起来是一个候选码

有时，**关系中有很多组候选码**

#### 主码(Primary Key)/主键

> 当有多个候选码时，可以**选定一个**作为主码。
>
> DBMS以主码为主要线索管理关系中的各个元组。
>
> 例如可选定属性S#作为“学生”表的主码，也可以选定属性组(Sname, Saddress)作为“学生”表的主码。选定EmpID为Employee的主码。

#### 主属性与非主属性

包含在任何一个候选码中的属性被称作**主属性**，而其他属性被称作**非主属性**

> 如 “选课”中的S# , C#为主属性，而Sname则为非主属性；

最简单的，候选码只包含一个属性

最极端的，所有属性构成这个关系的候选码，称为全码(All-Key)。

> 比如：关系“教师授课”(T#,C#)中的候选码(T#,C#)就是全码。

####  外码(Foreign Key)/外键

关系R中的一个属性组，它不是R的候选码，但它**与另一个关系S的候选码**相对应，则称这个属性组为R的外码或外键。

例如“合同”关系中的客户号不是候选码，但却是外码。因它与“客户”关系中的候选码“客户号” 相对应。

![image-20220119153110846](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119153110846.png)

两个关系通常是靠外码连接起来的。

![image-20220119153339265](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119153339265.png)

### 关系模型中的完整性

#### 实体完整性

关系的主码中的属性值不能为空值；

空值：不知道或无意义的值；

意义：关系中的元组对应到现实世界相互之间可区分的一个个个体，这些个体是通过主码来唯一标识的；若主码为空，则出现不可标识的个体，这是不容许的

![image-20220119153711368](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119153711368.png)

#### 参照完整性

如果关系R1的外码Fk与关系R2的主码Pk相对应，则R1中的每一个元组的Fk值或者等于R2 中某个元组的Pk 值，或者为空值。

意义：如果关系R1的某个元组t1参照了关系R2的某个元组t2，则t2必须存在

例如关系Student在D#上的取值有两种可能:

> 空值，表示该学生尚未分到任何系中。
>
> 若非空值，则必须是Dept关系中某个元组的D#值，表示该学生不可能分到一个不存在的系中

![image-20220119153833097](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119153833097.png)

#### 用户自定义完整性

**用户针对具体的应用环境定义的完整性约束条件**

**如S#要求是10位整数，其中前四位为年度，当前年度与他们的差必须在4以内**

![image-20220119153935686](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119153935686.png)

## 二、关系代数运算

**关系代数操作：集合操作和纯关系操作**

![image-20220119154043076](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119154043076.png)

**某些关系代数操作，如并、差、交等，需满足“并相容性”**

> 参与运算的两个关系及其相关属性之间有一定的对应性、可比性或意义关联性
>
> 定义：关系R与关系S存在相容性，当且仅当： 
>
> (1) 关系R和关系S的**属性数目必须相同**；
>
> (2) 对于任意i，关系R的第i个属性的域必须和关系S的第i个属性的域相同
>
> ​           假设：R(A1, A2, … , An) ,  S(B1, B2, … ,Bm)
>
> ​           R和S满足并相容性：n = m 并且 Domain(Ai) = Domain(Bi)

### 关系代数基本操作

#### 并Union

定义：假设关系R和关系S是并相容的，则关系R与关系S的并运算结果也是一  个关系，记作：R ∪S, 它由或者出现在关系R中，或者出现在S中的元组构成。

数学描述： $R\cup S =\{ t | t\in R \or t\in S \}$ ，其中t是元组

并运算是**将两个关系的元组合并成一个关系，在合并时去掉重复的元组。**

 R ∪S 与 S ∪R 运算的结果是同一个关系

示例如下：查询或者参加体育队或者参加文艺队所有学生的信息

![image-20220119160107221](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119160107221.png)

#### 差Difference

定义：假设关系R 和关系S是并相容的，则关系R 与关系S 的差运算结果也是一个关系，记作：R - S, 它由**出现在关系R中但不出现在关系S**中的元组构成。

数学描述：  $R- S =\{ t | t\in R \or t\notin S \}$ ，其中t是元组

R - S 与 S - R 是不同的 

示例：

> 查询只参加体育队而未参加文艺队的学生信息
>
> 查询只参加文艺队而未参加体育队的学生信息  

![image-20220119160408042](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119160408042.png)

#### 广义笛卡尔积 Cartesian Product

定义：关系R (<a1 , a2, …, an >) 与关系S(<b1, b2, …, bm >) 的广义笛卡尔积 (简称广义积,或 积 或笛卡尔积) 运算结果也是一个关系，记作： R x S, 它由关系R中的元组与关系S的元组进行所有可能的拼接(或串接)构成。

数学描述：$ R \times S =\{ <a_1 , a_2, …, a_n, b_1, b_2, …, b_m> | <a_1 , a_2, …, a_n > \in R \and <b_1, b_2, …, b_m> \in S \}$

示例：

![image-20220119160624882](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119160624882.png)

#### 选择Select

定义：给定一个关系R, 同时给定一个选择的条件condition(简记con), 选择运算结果也是一个关系，记作$\sigma _{con}(R)$ , 它从关系R中选择出满足给定条件condition的元组构成。

 数学描述：$\sigma _{con}(R)=\{t|t\in R \and con(t)='true'\}$

 设$R(A_1 ,A_2 , … ,A_n)$, t是R的元组, t 的分量记为$t[A_i],$ 或简写为Ai

 条件con由逻辑运算符连接比较表达式组成

 逻辑运算符：$\and\quad \or \quad\neg$  或写为 and , or, not

 比较表达式：$X \theta Y$, 其中X, Y 是t的分量、常量或简单函数, $\theta$是比较运算符, $\theta \in\{ > , \ge , < , \le , = , ≠\}$

示例

![image-20220119162326012](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119162326012.png)

#### 投影Project

定义：给定一个关系R, 投影运算结果也是一个关系，记作 PA(R) , 它从关系R中选出属性包含在A中的列构成。

数学描述：$ \Pi_{A_{i1}, A_{i2}, … ,A_{ik}}(R) = \{ <t[A_{i1}], t[A_{i2}],…,t[A_{iK}]> | t\in R \}$

 设$R(A_1 ,A_2 , … ,A_n), \  { A_{i1}, A_{i2}, … ,A_{ik} } \subseteq { A1 ,A2 , … ,An }$

 $t[A_i]$表示元组t中相应于属性Ai的分量

 投影运算可以对原关系的列在投影后重新排列

投影操作**从给定关系中选出某些列组成新的关系, 而选择操作是从给定关系中选出某些行组成新的关系**

示例

![image-20220119161353820](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119161353820.png)

 **投影与选择一起使用**

> 选择选出某行，投影选出某列

![image-20220119162446195](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119162446195.png)

### 关系代数扩展操作

#### 交Intersection

定义：假设关系R和关系S是并相容的，则关系R与关系S的交运算结果也是一个关系，记作：R ∩S, 它由同时出现在关系R和关系S中的元组构成。

数学描述：$ R\cap S ={ t | t\in R \and t\in S } $，其中t是元组

 R∩S 和 S∩R 运算的结果是同一个关系

交运算可以通过差运算来实现：$R \cap S = R - (R - S) = S - (S - R) $

示例：

![image-20220119162827325](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220119162827325.png)

#### $\theta$-连接($\theta$-Join, theta-Join)

定义：给定关系R和关系S, R与S的$\theta$连接运算结果也是一个关系，记作$\underset{A\theta B}{R\Join S}$，它由**关系R和关系S的笛卡尔积中, 选取R中属性A与S中属性B之间满足$\theta$条件的元组**构成。

数学描述
$$
\underset{A\theta B}{R\Join S}=\sigma_{t[A]\ \theta\  s[B]}(R\times S)
$$

>  设$R(A_1 ,A_2 , … ,A_m), A\in \{ A_1 ,A_2 , … ,A_m \} $ 
>
>  $ S(B_1 ,B_2 , … ,B_n), B\in \{ B_1 ,B_2 , … ,B_n \}$
>
>  t是关系R中的元组，s是关系S中的元组
>
>  属性A和属性B具有可比性
>
>  $\theta$是比较运算符, $\theta \in\{ > , \ge , < , \le , = , ≠\}$

**示例**

> 员工表Worker(W#, Wname, Wsex, Wage, Degree) , 
>
> 职位限定表Position(Type, Limited_Degree)
>
> 竞聘的岗位必须由不低于其最低学历要求的人员担任，找出所有员工的姓名及其可能竞聘职位的名称

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125112811417.png" alt="image-20220125112811417" style="zoom:80%;" />

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125112832923.png" alt="image-20220125112832923" style="zoom:80%;" />

#### 等值连接(Equi-Join)

定义：给定关系R和关系S, R与S的等值连接运算结果也是一个关系，记作 $\underset{A= B}{R\Join S}$ ，它由关系R和关系S的笛卡尔积中选取**R中属性A与S中属性B上值相等**的元组所构成。

 数学描述： 
$$
\underset{A= B}{R\Join S}=\sigma_{t[A]\ =\  s[B]}(R\times S)
$$
当$\theta$-连接中运算符为“＝”时，就是等值连接，**等值连接是$\theta$-连接的一个特例；**

 广义积的元组组合并不是都有意义的，另广义积的元组组合数目也非常庞大，因此采用$\theta$-连接/等值连接运算可大幅度**降低中间结果的保存量，提高速度。**

#### 自然连接

定义：给定关系R和关系S, R与S的自然连接运算结果也是一个关系，记作 $R\Join S$ ，它由关系R和关系S的笛卡尔积中选取**相同属性组B上值相等**的元组所构成。

 数学描述
$$
{R\Join S}=\sigma_{t[A]\ =\  s[B]}(R\times S)
$$

> **自然连接是一种特殊的等值连接。**
>
> 要求关系R和关系S**必须有相同的属性组B**(如R,S共有一个属性B1,则B是B1 , 如R, S共有一组属性B1, B2, …, Bn，则B是这些共有的所有属性)
>
> R, S属性相同，值必须相等才能连接，即：
>
> R.B1 = S.B1  and R.B2 = S.B2  … and R.Bn = S.Bn才能连接
>
> 要在结果中**去掉重复的属性列**(因结果中R.Bi 始终是等于S.Bi 所以可只保留一列即可)。

示例：

> 学生选课表SC(S#, C#, Score) , 
>
> 课程表Course (C#, Cname, Chours, Credit, T#)

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125114249993.png" alt="image-20220125114249993" style="zoom: 67%;" />

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125114345968.png" alt="image-20220125114345968" style="zoom:80%;" />

### 基本思路

> :one: 检索是否涉及多个表，如不涉及，则可直接采用并、差、交、选择与投影，只要注意条件书写正确与否即可。
>
> :two: 如涉及多个表，则检查：
>
> >  能否使用自然连接，将多个表连接起来(多数情况是这样的)
> >
> >  如不能，能否使用等值或不等值连接(q-连接)
> >
> >  还不能，则使用广义笛卡尔积，注意相关条件的书写
>
> :three: 连接完后，可以继续使用选择、投影等运算，即所谓数据库的“选投联”操作。

### 关系代数复杂扩展操作

#### 除

<mark></mark>

除法运算经常用于求解“查询… 全部的/所有的…”问题 

前提条件：给定关系$R(A_1 ,A_2 , … ,A_m)$为m度关系，关系$S(B_1 ,B_2 , … ,B_n)$为n度关系。如果可以进行关系R与关系S的除运算，当且仅当：属性集$\{ B_1 ,B_2 , … , B_n \}$是属性集$\{ A1 ,A2 , … ,Am \}$的真子集，即m>n。

定义：关系R 和关系S的除运算结果也是一个关系，记作$R\div S$，分两部分来定义。

:one: 先定义$R\div S$的结果**应有哪些属性**

设属性集$\{C_1,C_2, … ,C_k \} = \{A_1,A_2, … ,A_m \}\ – \ \{B_1,B_2, … ,B_n \}$, 则有k=n–m

则$R\div S$结果关系是<mark>**k度(m-n度)**</mark>关系，由$\{C_1,C_2, … ,C_k \}$属性构成

:two: 再定义$R\div S$的**元组怎样形成**

再设关系$R (<a_1, …, a_m>)$和关系$S (<b_1, …, b_n >)$, 那么$R\div S$结果关系为元组 $<c_1, …, c_k>$的集合，元组 $<c_1, …, c_k>$满足下述条件：

它与S中每一个元组$<b_1, …, b_n >$组合形成的一个新元组都是R中的某一个元组$<a_1, …, a_m>$ 。(其中，a1, …, am ,b1, …, bn, c1, …, ck分别是**属性**A1 , … ,Am,B1 , … ,Bn C1 , … ,Ck 的**值**)

 数学描述

![image-20220125115926595](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125115926595.png)

<mark>**其实就是挑出R中含有S中属性全部值的的相关元组的投影**</mark>

示例：

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125120156871.png" alt="image-20220125120156871" style="zoom:67%;" />

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125120322936.png" alt="image-20220125120322936" style="zoom:67%;" />

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125120715193.png" alt="image-20220125120715193" style="zoom:80%;" />

#### 外连接

定义：两个关系R与S进行连接时，如果关系R(或S)中的元组在S(或R)中找不到相匹配的元组，则为了避免该元组信息丢失，从而将该元组与S(或R)中假定存在的**全为空值的元组形成连接**，放置在结果关系中，这种连接称之为外连接(Outer Join)。

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125121653925.png" alt="image-20220125121653925" style="zoom:67%;float:left" />

![image-20220125121748104](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125121748104.png)

## 三、关系演算

**按照谓词变量的不同，可分为关系元组演算和关系域演算**

>  **关系元组演算是以元组变量作为谓词变量的基本对象。**
>
>  **关系域演算是以域变量作为谓词变量的基本对象。**

### 关系元组演算

关系元组演算公式的基本形式：`{ t | P(t) }`

上式表示：所有使谓词 *P* 为真的元组 t 的集合

>  t 是元组变量
>
>  $t \in r $ 表示元组 t 在关系 r 中
>
>  t [A] 表示元组 t 的分量，即 t 在属性 A 上的值
>
>  P 是与谓词逻辑相似的公式, P(t)表示以元组 t 为变量的公式

#### 示例

:one: 检索出年龄小于20岁并且是男同学的所有学生。

![image-20220125174457934](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125174457934.png)

:two: 再例如：检索出年龄小于20岁或者03系的所有男学生。

![image-20220125174510968](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220125174510968.png)

:three: 在元组演算公式构造过程中，如果需要，可以使用括号，通过括号改变运算的优先次序，即：**括号内的运算优先计算**

检索出不是03系的所有学生

![image-20220125174547736](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220125174547736.png)

:four: 检索不是(小于20岁的男同学)的所有同学

![image-20220125174736170](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125174736170.png)

:five: “检索出年龄不是最小的所有同学”

![image-20220125174754442](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125174754442.png)

关系代数表达式![image-20220125174829264](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220125174829264.png)                               

:six: 检索软件学院的所有同学

![image-20220125174941405](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125174941405.png)

:seven: 检索出比张三年龄小的所有同学 

![image-20220125175023446](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125175023446.png)

![image-20220125175119882](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125175119882.png)

#### 四个复杂例子

![image-20220125175411275](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220125175411275.png)

### 用元组演算公式实现关系代数

关系代数有五种基本操作：并、差、广义积、选择、投影操作，还有：交、theta-连接操作。

:one: 并运算：$R \cup S =  \{ t | t\in R \or t\in S \}$

:two: 差运算：$R - S =  \{ t | t\in R \and  \neg t\in S \}$

:three: 交运算：$R \cap  S = \{ t | t\in R \and  t\in S \}$

:four: 广义笛卡尔积	$R(A) \times S(B) = \{ t | \exists (u\in R)\exists (s\in S)(t[A] = u[A] \and  t[B] = s[B])\}$

:five: 选择运算： $\sigma _{con(R) }= { t | t\in R \and  F(con) }$

:six: 投影运算：$ \Pi _A( R ) = \{ t[A] | t\in R \}$

### 关系域演算

关系域演算公式的基本形式： { < x1 ,  x2 , … , xn > | P ( x1 ,  x2 , … , xn ) }

其中， xi 代表域变量或常量， P为以xi为变量的公式。

示例：

:one: 检索出不是03系的所有学生

![image-20220126095115823](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220126095115823.png)

:two: 检索成绩不及格的同学姓名、课程及其成绩

<img src="https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095150570.png" alt="image-20220126095150570" style="zoom:150%;" />

![image-20220126095226495](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095226495.png)

不产生无限关系和无穷验证的运算被称为是安全的 

> 关系代数是一种集合运算，是安全的。
>
> > 集合本身是有限的，有限元素集合的有限次运算仍旧是有限的。 
>
> **关系演算不一定是安全的**

![image-20220126095442941](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095442941.png)

## 四、习题解答和解析

![image-20220126095606435](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095606435.png)

![image-20220126095614454](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220126095614454.png)

![image-20220126095625744](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095625744.png)

![image-20220126095632150](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095632150.png)

![image-20220126095637192](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095637192.png)

![image-20220126095641039](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095641039.png)

![image-20220126095645918](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095645918.png)

![image-20220126095703545](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095703545.png)

![image-20220126095708957](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220126095708957.png)

![image-20220126095717692](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095717692.png)

## 五、补充习题

![image-20220126095745766](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095745766.png)

![image-20220126095752161](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095752161.png)

![image-20220126095756780](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220126095756780.png)

![image-20220126095801632](https://raw.githubusercontent.com/yijunquan-afk/img-bed-1/main/image-20220126095801632.png)

![image-20220126095808185](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095808185.png)

![image-20220126095812442](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095812442.png)

![image-20220126095817977](https://gitee.com/yi-junquan/image_gitee/raw/master/images/image-20220126095817977.png)



