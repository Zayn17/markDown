## jQuery

### jQuery基本概念

#### 什么是jQuery

​	jQuery就是一个js库，使用jQuery的话，会比使用JavaScript更简单。

#### 为什么要学习jQuery

![](F:\markDown\media\567.png)

#### 怎样使用jQuery

1. 引入jQuery文件

   ```javascript
   <script src="jquery-1.12.4.js"></script>
   ```

2. 入口函数

   ```javascript
       $(document).ready(function () {
       });
   ```

3. 功能实现

   ```javascript
       $("#btn").click(function () {
           $("div").hide(1000);
       })
   ```

   

### jQuery详细解释

#### 版本介绍

##### 下载jQuery

官网下载地址：http://jquery.com/download/

##### 大版本分类

jquery大版本分为1.x和2.x和3.x

**区别：2.x3.x版本不再支持IE6/7/8，在中国，用的最多的还是1.x版本**

##### 同一版本分类

jQuery每一个版本又分为压缩版和未压缩版：

-  **jquery.js**：未压缩版本（开发版本），代码可读性高，推荐在开发和学习阶段使用，方便查看源代码。
-  **jquery.min.js**：压缩版本，去除了注释、换行、空格、并且将一些变量替换成了a,b,c之类的简单字符，基本没有可读性，推荐在项目生产环境使用，因为文件较小，减少网络压力。

##### 关于jQuery3.0

jquery3.0现在发布了，这个版本自从2014年10月就开始测试了，我们的目标是创建一个更苗条、更快的jquery版本（并且能向后兼容）。我们已经移除了IE旧版本的解决方案，并且带来了一些较为现代的web API，但这是有道理的。3.0是2.x分支的延续，但是有一些突破性的改变。但是1.12和2.2分支将会在同一时间继续获得关键性的支持补丁。但是他们不会再有任何新的功能和重大的修订。**jQuery3是jQuery的未来，如果你需要兼容IE6-8，你可以继续使用1.12版本。**

#### 入口函数

##### jQuery入口函数的两种写法

1. ```javascript
   $(document).ready(function() {
   	
   });
   ```

2. ```javascript
   $(function() {
   	
   });
   ```

   

##### 对比JavaScript的入口函数与jQuery的入口函数,执行时机

1. JavaScript的入口函数要等到页面中所有资源**（包括图片、文件）**加载完成才开始执行。
2. jQuery的入口函数只会等待**文档树加载完成**就开始执行，并不会等待图片、文件的加载。

#### 了解jQuery的$符号

##### $是什么

​	$是一个函数，$()；参数不一样，功能不一样。

​	$常用的几种情况：

```javascript
$(function() {});//参数是function，说明是入口函数
$(“#btnSetConent”);
  //参数是字符串，并且以#开头，是一个标签选择，查找id=“btnSetContent”的元素
$(“div”);//查找所有的div元素
$(document).ready(funciton(){})//将document转换成jQuery对象

```

​	**$ === jQuery,也就是说能用$的地方，完全可以用jQuery，$仅仅是简写形式。**

#### jQuery对象与DOM对象之间的转换

##### 什么是DOM对象（js对象）？

​	使用JavaScript中的方法获取页面中的元素返回的对象就是dom对象。

​	**dom对象只可以使用dom对象的方法和属性**



##### 什么是jquery对象？

​	jquery对象就是使用jquery的方法获取页面中的元素返回的对象就是jQuery对象。

​	**jquery对象只能使用jquery对象的方法**



##### 深入了解jQuery对象

​	jQuery对象其实就是DOM对象的包装集（包装了DOM对象的集合（伪数组））



##### jQuery对象和DOM对象的相互转换

1. jquery对象转DOM对象

   ```javascript
   var $li = $(“li”);
   //第一种方法（推荐使用）
   $li[0]
   //第二种方法
   $li.get(0)
   ```

   

2. DOM对象转jquery对象（联想记忆：我有钱[美元]，所以我的功能就更强大）

   ```
   var $obj = $(domObj);
   // $(document).ready(function(){});就是典型的DOM对象转jQuery对象
   ```



##### 区分jQuery和JavaScript

​	JavaScript是一门编程语言，jquery是用JavaScript实现的一个JavaScript库，目的是简化我们的开发。



### jQuery选择器

#### jQuery选择器概述

##### 什么是jQuery选择器？

​	jQuery选择器是jQuery为我们提供的一组方法，让我们更加方便的获取到页面中的元素。注意：jQuery选择器返回的是jQuery对象。

​	jQuery选择器有很多，基本兼容了CSS1到CSS3所有的选择器，并且jQuery还添加了很多更加复杂的选择器。【查看jQuery文档】

​	jQuery选择器虽然很多，但是选择器之间可以相互替代，就是说获取一个元素，你会有很多种方法获取到。所以我们平时真正能用到的只是少数的最常用的选择器。

#### 基本选择器

| 名称       | 用法               | 描述                                                         |
| ---------- | ------------------ | ------------------------------------------------------------ |
| ID选择器   | $(“#id”);          | 获取指定ID的元素                                             |
| 类选择器   | $(“.class”);       | 获取同一类class的元素                                        |
| 标签选择器 | $(“div”);          | 获取同一类标签的所有元素                                     |
| 并集选择器 | $(“div, p, li”);   | 使用逗号分隔，只要符合条件之一就可。获取所有的div、p、li元素 |
| 交集选择器 | $(“div.redClass”); | 注意选择器1和选择器2之间没有空格，class为redClass的div元素，注意区分后代选择器。 |



#### 层级选择器

| **名称**       | **用法**    | **描述**                                                    |
| -------------- | ----------- | ----------------------------------------------------------- |
| **子代选择器** | $(“ul>li”); | 使用>号，获取儿子层级的元素，注意，并不会获取孙子层级的元素 |
| **后代选择器** | $(“ul li”); | 使用空格，代表后代选择器，获取ul下的所有li元素，包括孙子等  |



#### 过滤选择器

|                  | **用法**                                             | **描述**                                                    |
| ---------------- | ---------------------------------------------------- | ----------------------------------------------------------- |
| **:eq（index）** | **$(“li:eq(2)”).css(“color”,<br/>”red”);**           | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始。 |
| **:odd**         | **$(“li:odd”).css(“color”,<br/>”red”);**             | 获取到的li元素中，选择索引号为奇数的元素                    |
| **:even**        | **$(“li:even”).css(“color”,<br/>”red”);**            | 获取到的li元素中，选择索引号为偶数的元素                    |
| **:lt(n)**       | **$("tr:lt(2)").css("background-color","#B2E0FF");** | 获取前两行的元素                                            |

#### 筛选选择器（方法）

筛选选择器的功能与过滤选择器有点类似，但是用法不一样，筛选选择器主要是方法。

|                        | **用法**                    | **说明**                         |
| ---------------------- | --------------------------- | -------------------------------- |
| **children(selector)** | $(“ul”).children(“li”)      | 相当于$(“ul>li”)，子类选择器     |
| **find(selector)**     | $(“ul”).find(“li”);         | 相当于$(“ul li”),后代选择器      |
| **siblings(selector)** | $(“#first”).siblings(“li”); | 查找兄弟节点，不包括自己本身。   |
| **parent()**           | $(“#first”).parent();       | 查找父亲                         |
| **eq(index)**          | $(“li”).eq(2);              | 相当于$(“li:eq(2)”),index从0开始 |
| **next()**             | $(“li”).next()              | 找下一个兄弟                     |
| **prev()**             | $(“li”).prev()              | 找上一次兄弟                     |



### jquery操作样式

#### css操作

功能：设置或者修改样式，操作的是style属性。

##### 设置单个样式

```javascript
//name：需要设置的样式名称
//value：对应的样式值
css(name, value);
//使用案例
$("#one").css("background","gray");//将背景色修改为灰色
```

##### 设置多个样式

```javascript
//参数是一个对象，对象中包含了需要设置的样式名和样式值
css(obj);
//使用案例
$("#one").css({
    "background":"gray",
    "width":"400px",
    "height":"200px"
});
```

##### 获取样式

```javascript
//name:需要获取的样式名称
css(name);
//案例
$("div").css("background-color");
```

注意：获取样式操作只会返回第一个元素对应的样式值。

隐式迭代：

1. 设置操作的时候，如果是多个元素，那么给所有的元素设置相同的值
2.  获取操作的时候，如果是多个元素，那么只会返回第一个元素的值。



#### attr操作

##### 设置单个属性

```javascript
        // 设置单个属性
        // attr(name, value)
        $("img").attr("alt", "呜啦啦");
        $("img").attr("title", "错错错");
```

##### 设置多个属性

```javascript
//设置多个属性
    /*$("img").attr({
      alt:"图破了",
      title:"错错错",
      aa:"bb"
    })*/
```

##### 获得属性

```

```



#### class操作

##### 添加样式类

```javascript
//name：需要添加的样式类名，注意参数不要带点.
addClass(name);
//例子,给所有的div添加one的样式。
$(“div”).addClass(“one”);
```

##### 移除样式类

```javascript
//name:需要移除的样式类名
removeClass(“name”);
//例子，移除div中one的样式类名
$(“div”).removeClass(“one”);
```

##### 判断是否有样式类

```javascript
//name:用于判断的样式类名，返回值为true false
hasClass(name)
//例子，判断第一个div是否有one的样式类
$(“div”).hasClass(“one”);
```

##### 切换样式类

```javascript
//name:需要切换的样式类名，如果有，移除该样式，如果没有，添加该样式。
toggleClass(name);
//例子
$(“div”).toggleClass(“one”);
```

##### prop方法

**对于布尔类型的属性**，不要attr方法，应该用prop方法 prop用法跟attr方法一样。





### jQuery方法

#### jQuery事件方法与JS事件方法

| 方法                     | jQuery       | 原生JS        |
| :----------------------- | ------------ | ------------- |
| 点击事件                 | click()      | onclick()     |
| 鼠标经过事件             | mouseover()  | onmouseover() |
| 鼠标离开事件(与over配套) | mouseout()   | onmouseout()  |
| 鼠标进入事件             | mouseenter() |               |
| 鼠标离开事件             | mouseleave() |               |
|                          |              |               |
|                          |              |               |
|                          |              |               |

##### mouseover与mouseenter的差别

mouseenter(): 事件只有在鼠标指针进入被选元素时被触发

mouseover(): 事件在鼠标指针进入被选元素或任意子元素时都会被触发

测试代码

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        h3,h4,h5{
            text-align: center;
        }

    </style>
    <script src="jquery-1.12.4.js"></script>
    <script>
        var x = 0;
        var y = 0;
        $(function () {
            $("#over").mouseover(function () {
                $("#over h5").text("次数：" + (x+=1));
            });
            $("#enter").mouseenter(function () {
                $("#enter h5").text("次数：" + (y+=1));
            })
        });

    </script>
</head>
<body>
<h3>mouseover事件， 事件在鼠标指针进入被选元素或任意子元素时都会被触发</h3>
<div id="over">
    <h4 style="background-color: aquamarine">触发mouseover事件</h4>
    <h4 style="background-color: #fffb9a">我是movseover的子代</h4>
    <h5>次数：0</h5>
</div>
<h3>mouseenter(): 事件只有在鼠标指针进入被选元素时被触发</h3>
<div id="enter">
    <h4 style="background-color: aquamarine">触发mouseenter事件</h4>
    <h4 style="background-color: #fffb9a">我是mouseenter的子代</h4>
    <h5>次数：0</h5>
</div>
</body>
</html>
```



#### jQuery效果方法

| 方法         | jQuery                           |
| ------------ | -------------------------------- |
| 隐藏被选元素 | hide([speed], [callback])        |
| 显示被选元素 | show([speed], [callback])        |
| 淡入         | fadeIn([speed], [callback])      |
| 淡出         | fadeOut([speed], [callback])     |
| 滑入         | slideDown([speed], [callback])   |
| 滑出         | sildeUp([speed], [callback])     |
| 滑入/滑出    | slideToggle([speed], [callback]) |
| 淡入/淡出    | fadeToggle([speed], [callback])  |
| 停止动画     | stop()                           |

##### show方法

speed: 动画的持续时间，可以是毫秒值，还可以是固定字符串

- fast: 200ms
- normal: 400ms
- slow: 600ms

```javascript
      $("div").show(1000, function () {
        console.log("哈哈，动画执行完成啦");
      })
      
```



##### stop方法

```javascript
$(selector).stop(stopAll,goToEnd)
```

- *stopAll* —— 可选。规定是否停止被选元素的所有加入队列的动画。
- *goToEnd* —— 可选。规定是否允许完成当前的动画。该参数只能在设置了 stopAll 参数时使用。

```javascript
$(function () {
    $("input").eq(0).click(function () {
        $("div").slideDown(4000).slideUp(4000);
    });
    $("input").eq(1).click(function () {
        // stop: 停止当前正在执行的动画
        // clearQueue: 是否清除动画队列   true/false
        // jumpToEnd：是否跳跃到当前动画的最终效果 true/false
//        $("div").stop(clearQueue, jumpToEnd);
        $("div").stop(true,true);
    });
});
```



##### animate —— 自定义动画

- 第一个参数：对象，里面可以传需要动画的样式
- 第二个参数：speed 动画的执行时间
- 第三个参数：动画的执行效果
- 第四个参数：回调函数

```javascript
$("input").eq(0).click(function () {
    $("#box1").animate({left:800}, 8000);

    // swing: 秋千 摇摆
    $("#box2").animate({left:1000}, 8000, "swing");

    // linear: 线性 匀速
    $("#box3").animate({left:800},8000,"linear", function () {
        console.log("哇哈哈");
    })
})
```



##### 动画队列

```javascript
  $(function () {
    $("#btn").click(function () {
      
      //把这些动画存储到一个动画队列里面
      $("div").animate({left:800})
        .animate({top:400})	
        .animate({width:300,height:300})
        .animate({top:0})
        .animate({left:0})
        .animate({width:100,height:100})
    })
  });
```



#### jQuery杂项方法

| 方法                                 | jQuery  |
| ------------------------------------ | ------- |
| 返回当前元素在所有兄弟元素里面的索引 | index() |



### 节点操作

#### 创建节点 

##### append

append() 方法在被选元素的结尾插入指定内容。

```javascript
// 法1
$(selector).append(content)
// 法2
$(selector).append(function(index,html))
```

- content: 必需。规定要插入的内容（可包含 HTML 标签）。
  - 可能的内容：
  - HTML 元素
  - jQuery 对象
  - DOM 元素
- function(*index,html*): 可选。规定返回待插入内容的函数。
  - *index* - 返回集合中元素的 index 位置。
  - *html* - 返回被选元素的当前 HTML。

```javascript
    $(function () {
//        var box = document.getElementById("box");
//        var a = document.createElement("a");
//        box.appendChild(a);
//        a.setAttribute("href", "http://baidu.com");
//        a.setAttribute("target","_blank");
//        a.innerHTML = "百度";
        $("#box").append('<a href="http://www.baidu.com" target="_blank">百度</a>');
    });
```



##### appendTo

appendTo() 方法在被选元素的结尾插入 HTML 元素。

```javascript
$(content).appendTo(selector)
```

- content: 必需。规定要插入的内容（必须包含 HTML 标签？？？？）。
  **注意：**如果 *content* 是已存在的元素，它将从当前位置被移除，并在被选元素的结尾被插入。
- *selector*: 必需。规定把内容追加到哪个元素上。

```javascript
$(document).ready(function(){
	  $("button").click(function(){
		$("h1").appendTo("p");
	  });
});
```



##### prepend

prepend() 方法在被选元素的开头（仍位于内部）插入指定内容。

```
// 法1
$(selector).prepend(content)
// 法2
$(selector).prepend(function(index,html))
```

- content: 必需。规定要插入的内容（可包含 HTML 标签）。
  - 可能的内容：
  - HTML 元素
  - jQuery 对象
  - DOM 元素
- function(*index,html*): 可选。规定返回待插入内容的函数。
  - *index* - 返回集合中元素的 index 位置。
  - *html* - 返回被选元素的当前 HTML。



##### prependTo

prependTo() 方法在被选元素的开头（仍位于内部）插入指定内容。

```javascript
$(content).prependTo(selector)
```

- content: 必需。规定要插入的内容（必须包含 HTML 标签 ？？？）。
  **注意：**如果 *content* 是已存在的元素，它将从当前位置被移除，并在被选元素的结尾被插入。
- *selector*: 必需。规定把内容追加到哪个元素上。



#### 清空和删除节点

##### empty —— 清空节点

```javascript
$("div").empty();
```

##### remove —— 删除节点

```javascript
$("div").remove();
```



#### 复制节点

#### clone()

- false:不传参数也是深度复制,不会复制事件
- true:也是深复制，会复制事件

```
$(".des").clone(true).appendTo("div");
```

