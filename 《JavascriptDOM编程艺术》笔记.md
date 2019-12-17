### 第9章	CSS-DOM

#### 9.1	三位一体的网页

网页是由以下三层信息构成的一个共同体：

- 结构层：由HTML或XHTML之类的标记语言负责创建
- 表示层：由CSS负责完成
- 行为层：由Javascript语言和DOM负责完成



#### 9.2	style属性

##### 9.2.1	获取样式

​	style属性包含着元素的样式，查询这个属性将**返回一个对象**而不是一个简单的字符串。样式都存放在这个style对象的属性里。

```
element.style.property
```

​	当需要引用一个中间带减号的CSS属性时，DOM要求用驼峰命名法。如:CSS属性*font-family*变为DOM属性*fontFamily*。

​	在某些浏览器里，color属性以RGB（红，绿，蓝）格式的颜色值返回。绝大部分样式属性的返回值与他们的设置值都采用同样的计量单位。

​	**局限性：**style属性只能返回内嵌样式。



##### 9.2.2	设置样式

```
element.style.property = value
```

style对象的属性的值必须放在引号里，单引号或双引号均可。如果忘记引号，JavaScript会把等号右边的值解释为一个变量。



#### 9.3	何时改用DOM脚本设置样式

在绝大多数场合，还是应该使用CSS去声明样式，不应该利用DOM为文档设置重要的样式。

**注意：**虽然利用表格做页面布局不是好主意，但利用表格来显示表格数据却是理所当然的。

何时应该使用CSS来设置样式，何时应该使用DOM来设置样式？

- 这个问题最简单的解决方案是什么
- 哪种解决方案会得到更多的浏览器的支持



#### 9.4	className属性

className属性是一个可读/可写的属性，凡是元素节点都有这个属性

```
element.className
// 设置属性
element.className = value
```

**不足：**通过className属性设置某个元素的class属性将替换（而不是追加）该元素原有的class设置。

**解决：** 利用字符串拼接操作，把新的class设置追加到className属性上去（**请注意**：添加的class第一个字符是空格）

只要用可能，就应选择更新className属性，而不是去直接更新style对象的有关属性。**确保网页的表示层和行为层分离得更加彻底。**



### 第10章	用JavaScript实现动画效果

#### 10.1	动画基本知识

​	动画是让元素的位置随着时间而不断地变化

##### 10.1.1	位置

position

- static	——	position的默认属性
- fixed
- relative 
- absolute



##### 10.1.2	时间

setTimeout 让某个函数再经过一段时间之后才执行

```
variable = setTimeout("function", interval)
```

- function —— 需要运行的函数
- interval —— 需要经过多少时间运行

clearTimeout取消某个正在排队等待执行的函数

```
clearTimeout(variable)
```

-	variable —— 保存着某个setTimeout函数调用返回值的变量



##### 10.1.3	时间递增量

**注意：**只有使用了DOM脚本或style属性为元素分配了位置后，这里的parseInt才有作用

​	真感觉没啥



### 第11章	HTML5

部分内容已发在博客