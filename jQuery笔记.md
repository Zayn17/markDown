## jQuery

### jQuery基本概念

#### 什么是jQuery

​	jQuery就是一个js库，使用jQuery的话，会比使用JavaScript更简单。

#### 为什么要学习jQuery

![img](file:///C:/Users/杨志豪/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

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

![img](file:///C:/Users/杨志豪/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

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