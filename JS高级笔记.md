## jJavaScript高级 笔记

#### Little trick

console.dir(XX) 显示XX的结构

### 创建对象的方式

1. 字面量的方式
2. 调用系统的构造函数
3. 自定义构造函数的方式
4. 工厂模式

```javascript
        // 实例对象
        var per1 = {
            name: "卡卡西",
            age: 20,
            eat: function () {
                console.log("哇哈哈");
            }
        }

        // 调用系统的构造函数
        var per2 = new Object();
        per2.name = "大蛇丸";
        per2.age = 20;
        per2.eat = function () {
            console.log("我吃冰淇淋");
        };

        // 自定义构造函数
        function Person(name, age, sex) {
            this.name = name;
            this.age = age;
            this.sex = sex;
            this.play = function () {
                console.log("瓦吉吉哇");
            };
        }
        var per = new Person("W", 18, "MALE");
        console.log(per instanceof Person); // true

        // 工厂模式
        function createObject(name, age) {
            var obj = new Object();
            obj.name = name;
            obj.age = age;
            obj.eat = function () {
              console.log("123456");
            };
        }
        var per1 = createObject("Cody", 20);
```



#### 实例对象和构造函数之间的关系

1. 实例对象是通过构造函数来创建 —— 创建的过程叫实例化

2. 如何判断对象是不是这个数据类型？

   - 通过构造器的方式 实例对象.构造器 == 构造函数的名字

     ```javascript
     console.log(dog.constructor == Animal);
     ```

   - 对象 instanceof 构造函数名字（尽可能使用第二种方式来识别）

     ```javascript
     console.log(dog instanceof Animal);
     ```



### 原型（prototype）

**原型:**  __ proto__ 或者是prototype,都是原型对象
实例对象中有个属性, __ proto__,也是对象,叫原型,不是标准的属性,浏览器使用的(部分浏览器支持，部分不支持如IE)
构造函数中有一个属性,prototype,也是对象,叫原型,是标准属性,程序员使用
原型的作用:共享数据,节省内存空间

```javascript
 function Person(name, age) {
            this.name = name;
            this.age = age;
        }
        // 通过原型来添加方法，解决数据共享，节省内存空间
        Person.prototype.eat = function () {
            console.log("哇哈哈");
        }
        var p1 = new Person("小明", 20);
        var p2 =new Person("小红", 30);
        console.log(p1.eat == p2.eat);      // true
```

```javascript
console.log(per.__proto__.constructor==Person.prototype.constructor); // true
```



#### 构造函数，实例对象和原型对象之间的关系

```javascript
    function Person(age,sex) {
      this.age=age;
      this.sex=sex;
    }
    Person.prototype.eat=function () {
      console.log("123");
    };
    var per=new Person(20,"男");
    per.eat();
```

![1568464306249](C:\Users\杨志豪\AppData\Roaming\Typora\typora-user-images\1568464306249.png)



#### 原型的简单写法

```javascript
        Student.prototype.height = "188";
        Student.prototype.weight = "55kg";
        Student.prototype.study = function () {
            console.log("学习");
        };
// 原型的简单写法
        Student.prototype = {
            // 构造器必须添加，勿忘！！
            constructor: Student,
            height: "188",
            weight: "55kg",
            study: function () {
                console.log("学习");
            }
        };
```



#### 原型中的方法是可以相互调用的

```javascript
        function Animal(name, age) {
            this.name = name;
            this.age = age;
        }
        Animal.prototype.eat  = function () {
          console.log("I want an apple");
            this.play();
        };
        Animal.prototype.play = function () {
          console.log("玩球");
            this.sleep();
        };
        Animal.prototype.sleep = function () {
          console.log("睡觉了");
        };
```



#### 实例对象的属性和方法的查找方式

实例对象调用属性或方法时,先查找实例对象本身是否含有该属性或方法,若不存在,则从实例对象的__ proto__属性指向的原型对象prototype中查找,找到则直接调用,若查找不到则报错.

实例对象本身 → 原型对象

```javascript
    function Person(age,sex) {
      this.age=age;//年龄
      this.sex=sex;
      this.eat=function () {
        console.log("构造函数中的吃");
      };
    }
    Person.prototype.sex="女";
    Person.prototype.eat=function () {
      console.log("原型对象中的吃");
    };


    var per=new Person(20,"男");
    console.log(per.sex);//男
    per.eat();
    console.dir(per);
```



#### 为内置对象添加原型方法

为系统的对象的原型中添加方法,相当于在改变源码

```
String.prototype.myReverse = function(){
	...
};

```



#### 原型链

​	原型链：是一种关系，实例对象和原型对象之间的关系，关系是通过原型（__ proto __）来联系的

#### 原型的指向是否可以改变

​	实例对象的原型__ proto__指向的是该对象所在的构造函数的原型对象。

​	构造函数的原型对象（prototype）指向如果改变了，实例的对象的原型（__ proto__）指向也会发生改变

#### 原型最终指向了哪里

per实例对象的__ proto__ ------->Person.prototype的__ proto__ ---->Object.prototype的__ proto__是null

#### 原型指向改变如何添加方法和访问

​	如果原型指向改变了，那么就应该在原型指向之后添加原型方法

#### 实例对象的属性和原型对象的属性重名问题

​	实例对象访问这个属性，应该先从实例对象中寻找，如果存在则直接使用，如果不存在，则从指向的原型对象中寻找，找到了直接使用。若都不存在，则返回undefined

### 继承

 面向对象的特性:封装,继承,多态
 **封装**:就是包装
  	一个值存储在一个变量中--封装
 	一坨重复代码放在一个函数中--封装
	 一系列的属性放在一个对象中--封装
	 一些功能类似的函数(方法)放在一个对象中--封装
 	好多相类似的对象放在一个js文件中---封装

**继承**: 首先继承是一种关系,类(class)与类之间的关系,JS中没有类,但是可以通过构造函数模拟类,然后通过原型来实现继承
 继承也是为了数据共享,js中的继承也是为了实现数据共享
 原型作用之一:数据共享,节省内存空间
 原型作用之二:为了实现继承

**多态**:一个对象有不同的行为,或者是同一个行为针对不同的对象,产生不同的结果,要想有多态,就要先有继承,js中可以模拟多态,但是不会去使用,也不会模拟。

#### 原型继承

```javascript
        function Person(name, age, sex) {
            this.name = name;
            this.sex =sex;
            this.age = age;
        }
        Person.prototype.eat = function () {
            console.log("人可以吃东西");
        };
        function Student(score) {
            this.score = score;
        }
        Student.prototype = new Person("哇哈哈", 10, "男");
        Student.prototype.study = function () {
          console.log("学习很累");
        };
        var stu = new Student(100);
        console.log(stu.name);
        stu.eat();
        stu.study();
```

#### 借用构造函数继承

​	借用构造函数：构造函数名字.call(当前对象，属性，属性,......)
​	解决属性继承，并且值不重复的问题
​	缺陷：父级类别中的方法不能继承

​	

```javascript
        function Person(name, age, sex, weight) {
            this.name = name;
            this.age = age;
            this.sex = sex;
            this.weight = weight;
        }
        Person.prototype.sayHi = function () {
            console.log("您好");
        };
        function Student(name, age,sex, weight, score) {
            Person.call(this, name, age, sex, weight);
            this.score = score;
        }
        var stu1 = new Student("小明", 10, "男", "10kg","100");
```



#### 组合继承

​	组合继承：原型继承+借用构造函数继承

​	优点： 属性和方法都被继承了

```javascript
        function Person(name, age, sex) {
            this.name = name;
            this.age = age;
            this.sex =sex;
        }
        Person.prototype.sayHi = function () {
            console.log("加油，加油");
        };
        function Student(name, age, sex, score) {
            Person.call(this, name, age, sex);
            this.score = score;
        }
        Student.prototype = new Person();
        Student.prototype.eat = function () {
            console.log("吃东西");
        };
        var stu = new Student("小黑", 20, "男", "100分");
        console.log(stu.name, stu.age, stu.sex, stu.score);
        stu.sayHi();
        stu.eat();
```

#### 拷贝继承

​	把一个对象中的属性或者方法直接复制到另一个对象中

```javascript
        function Person() {
        }
        Person.prototype.age = 10;
        Person.prototype.sex = "男";
        Person.prototype.height = 100;
        Person.prototype.play = function () {
            console.log("玩的好开心");
        };
        var obj2 = {};
        // Person的构造中有原型prototype,prototype就是一个对象,
        // 那么里面,age,sex,height,play都是该对象中的属性和方法
        for(var key in Person.prototype){
            obj2[key] = Person.prototype[key];
        }
        console.dir(obj2);
        obj2.play();
```

#### 总结

**原型的作用**

1. 数据共享，节省内存空间

2. 继承，为了节省内存空间

**继承**

1. 原型继承：改变原型的指向

2. 借用构造函数继承：主要解决属性的问题

3. 组合继承：原型继承＋组合构造函数继承

   既能解决属性问题，又能解决方法问题

4. 拷贝继承：把对象中需要共享的属性或者方法，直接遍历的方式复制到另一个对象中

### 函数

#### 函数声明与函数表达式

```
//函数声明
function f1(){
	console.log("我是函数");
}
f1();
//函数表达式
var ff = function(){
	console.log("我也是一个函数");
}
ff();
```

#### 函数声明和函数表达式的区别

​	函数声明如果放在if-else的语句中，再IE8的浏览器中会出现问题。以后宁愿用函数表达式，都不用函数声明

#### 函数中的this的指向

1. 普通函数中的this是**window**
2. 对象.方法中的this是**当前的实例对象**
3. 定时器方法中的this是**window**
4. 构造函数中的this是**实例对象**
5. 原型对象方法中的this是**实例对象**
BOM中顶级对象是window，浏览器中所有的东西都是window的


```javascript
        "use strict"; // 严格模式
//        普通函数
        function f1() {
            console.log(this);
        }
        f1();
//         定时器中this
        setInterval(function () {
            console.log(this);
        }, 1000);
        // 构造函数
        function Person() {
            console.log(this);
//            对象的方法
            this.sayHi = function () {
                console.log(this);
            };
        }
//        原型中的方法
        Person.prototype.eat = function () {
          console.log(this);
        };
        var per = new Person();
        per.sayHi();
        per.eat();
```

#### 函数的不同的调用方式

1. 普通函数 —— 直接调用

   ```javascript
   function f1(){
   	console.log("哇哈哈1");
   }
   f1();
   ```

2. 构造函数 —— 通过new来调用，创建对象

   ```
   function F1(){
   	console.log("哇哈哈2");
   }
   var f = new F1();
   ```

3. 对象的方法

   ```
   function Person(){
   	this.play = function(){
   	console.log("玩代码");
   	};
   }
   var per =new Person();
   per.play();
   ```


#### 函数与对象

​	函数是对象，对象不一定是函数	

​	对象：有**__ proto__**原型

​	函数：有**prototype**原型

​	如果既有__ proto__ 和prototype, 说明是函数，也是对象。

​	所有函数实际上都是Function的构造函数创建出来的实例对象

#### 数组的函数调用

​	数据可以存储任何类型的数据

```javascript
        var arr = [
            function () {
                console.log("1");
            },
            function () {
                console.log("2");
            },
            function () {
                console.log("3");
            },
            function () {
                console.log("4");
            },
            function () {
                console.log("5");
            }
        ];
        arr.forEach(function (ele) {
            ele();
        });
```

#### apply和call

​	作用：可以改变this的指向

- apply的使用方法

  函数名字.apply(对象,[参数1,参数2,...]);
  方法名字.apply(对象,[参数1,参数2,...]);

- call的使用方法
  
  函数名字.call(对象,参数1,参数2,...);
  方法名字.call(对象,参数1,参数2,...);

第一个参数如果为空，或者是传入的是null，那边调用该方法的函数对象中的this就是默认的window

只要是想使用别的对象的方法,并且希望这个方法是当前对象的,那么就可以使用apply或者是call的方法改变this的指向

```
	function f1(x, y) {
		console.log("结果是:" + (x+y)+this);
		return "10000";
	}
	f1(10, 20);
	// apply和call都可以让函数或者方法来调用，传入的参数和函数自己调用的写法不同，但是效果一样
	f1.apply(null, [100,200]);
	f1.call(null, 100, 200);
	
	function f1(x,y) {
		console.log("这个函数是window对象的一个方法:" + (x+y) + this.sex);
	}
	window.f1(10,20);
	var obj = {
 		age:10,
  		sex:"男"
	};
	window.f1.apply(obj,[10,20]);
	window.f1.call(obj, 10,20);
```

#### bind

bind是用来复制方法，并同时改变this指向

**bind与apply和call的差异：**

- apply和call是在调用的时候改变this指向

- bind方法，是赋值一份的时候，改变this 指向

**使用的语法：**

- 函数名字.bind(对象，参数1，参数2，...)；----> 返回值是复制之后的这个函数
- 方法名字.bind(对象，参数1，参数2，...)；----> 返回值是复制之后的这个函数

```javascript
        function Person(age) {
            this.age = age;
        }
        Person.prototype.play = function () {
            console.log(this + "====>" + this.age);
        };
        function Student(age) {
            this.age = age;
        }
        var per = new Person(10);
        var stu = new Student(20);
        var ff =per.play.bind(stu);
        ff();

```



#### 函数的几个属性

1. name属性：函数的名字，name属性是只读，不能修改
2. arguments的属性：实参
3. length属性：函数定义的时候形参的个数
4. caller属性：调用者（如：f1函数在f2函数中调用，所以，此时调用者就是f2）

```javascript
        function f1(x, y) {
            console.log(f1.name);
            console.log(f1.arguments.length);
            console.log(f1.length);
            console.log(f1.caller);
        }
        f1(10,20,30,40);
        console.dir(f1);
        function f2() {
            console.log("f2函数的代码");
            f1(1,2);
        }
        f2();
```

**函数作为参数使用**

​	函数作为参数的时候，如果是命名函数，那么**只传入命名函数的名字，没有括号**

```javascript
        function f1(fn) {
            console.log("f1的函数");
            fn(); // 此时fn是当成一个函数来使用的
        }
        // fn是参数, 最后作为函数使用，函数是可以作为参数来使用
        // 传入匿名函数
        f1(function () {
            console.log("我是匿名函数");
        });
        function f2() {
            console.log("f2的函数");
        }
        f1(f2);

        function f1(fn) {
            setInterval(function () {
                console.log("定时器开始");
                fn();
                console.log("定时器结束");
            }, 1000);
        }
        f1(function () {
            console.log("好困啊,好累啊, 就是想睡觉");
        })
```

#### 函数作为返回值使用

```javascript
       // 判断这个对象和传入类型是不是同一个类型
        function getFunc(type) {
            return function (obj) {
                return Object.prototype.toString.call(obj) === type;
            }
        }
        var ff =getFunc("[object Array]");
        var result = ff([10, 20, 30]);
        console.log(result);

        var ff1 = getFunc("[object Object]");
        var dt = new Date();
        var result1 = ff1(dt);
        console.log(result1);
```

#### 作用域和作用域链及预解析

**变量**：局部变量和全局变量

- 函数中定义的变量是局部变量，函数使用结束后，局部变量就会被自动的释放

**作用域**：变量的使用范围

- 局部作用域

- 全局作用域

  **js没有块级作用域，一对括号中的变量，这个变量可以在大括号外面使用**

**作用域链**：变量的使用，从里向外，层层的搜索，若搜索到则直接使用。搜索到0级作用域（最外面的一层）的时候，如果还是没有找到这个变量，则报错。

```javascript
        var num = 10;   // 作用域链 级别：0
        var num2 = 20;
        var str = "Abv";
        function f1() {
            var num2 = 20;
            function f2() {
                var num3 = 30;
                console.log(num);
            }
            f2();
        }
        f1();
```

**预解析**：在浏览器解析代码之前，把变量的声明和函数的声明提前到该作用域的最上面

```javascript
        // 变量声明提前, 定义没有提前，所以结果为undefined
        console.log(num);
        var num = 100;

        // 函数的声明提前, 成功调用
        f1();
        function f1() {
            console.log("这个函数，执行了");
        }
```

#### 闭包

**闭包**：函数A中，有一个函数B，函数B中可以访问函数A中定义的变量或者是数据，此时形成了闭包（此话不严谨）。闭包后，里面的局部局部变量的使用作用域链就会被延长

**闭包的作用**：缓存数据，延长作用域链

**闭包的优点和缺点**：缓存数据

**闭包的模式**：

- 函数模式的闭包：在一个函数中有一个函数

  ```javascript
          function f1() {
              var num = 10;
              function f2() {
                  console.log(num);
              }
              f2();
          }
          f1();
  ```

  

- 对象模式的闭包：函数中有一个对象

  ```javascript
          function f3() {
              var num = 10;
              var obj = {
                age:num
              };
              console.log(obj.age);
          }
          f3();
  ```

**小案例**

```
        // 普通函数
        function f1() {
            var num = 10;
            num++;
            return num;
        }
        console.log(f1());
        console.log(f1());
        console.log(f1());

        // 函数模式的闭包
        function f2() {
            var num = 10;
            return function () {
                num++;
                return num;
            }
        }
        var ff = f2();
        console.log(ff());  // 11
        console.log(ff());  // 12
        console.log(ff());  // 13
```



#### 沙箱

**沙箱**：相当于黑盒，在一个虚拟的环境中模拟真实世界,做实验,实验结果和真实世界的结果是一样,但是不会影响真实世界

```javascript
//        沙箱——小环境
        (function () {
            var num = 10;
            console.log(num);
        })();
```

#### 递归

递归：函数中调用函数自己，此时就是递归，递归一定要有结束的条件

```javascript
        var i = 0;
        function f1() {
            i++;
            if(i < 5){
                f1();
            }
            console.log("wahahahahahahaha");
        }
        f1();
```

### 拷贝

#### 浅拷贝

**浅拷贝**：拷贝就是复制，相当于把一个对象中的所有内容，复制一份给另一个对象，直接复制，或者说，就是把一个对象的地址给了另一个对象，他们指向相同，两个对象之间有共同的属性或者方法，都可以使用（被复制方改变后，另一方也开斯改变）

```javascript
        var obj1 = {
            age: 10,
            sex: "男",
            car:["奔驰","宝马","特斯拉", "奥托"]
        };
        // 另一个对象
        var obj2= {};
        // 浅拷贝
        function extend(a, b) {
            for(var key in a){
                b[key] = a[key];
            }
        }
        extend(obj1, obj2);
        console.dir(obj1);
        console.dir(obj2);
```



#### 深拷贝

**深拷贝**：把一个对象中的所有属性或者方法，一个个的找到，并且在另一个对象中开辟相应的空间，一个一个的存储到另一个对象中。（针对复杂的object类型的数据）

```javascript
    <script>
        var obj1= {
            age:10,
            sex: "man",
            car:["1", "@","a"],
            dog:{
                name:"大黄",
                age:5                                                                                                                                                                                                 
                color:"white"
            }
        };
        var obj2 = {};
        // 深拷贝
        function extend(a, b) {
            for(var key in a){
                var item = a[key];
                if(item instanceof Array){
                    b[key] = [];
                    extend(item, b[key]);
                }else if(item instanceof Object){
                    b[key] = {};
                    extend(item, b[key]);
                } else {
                    b[key] = item;
                }
            }
        }
        extend(obj1, obj2);
        console.dir(obj1);
        console.dir(obj2);
```

### 遍历DOM树

```javascript
<body>
<h1>遍历 DOM 树</h1>
<p style="color: green;">Tip: 可以在遍历的回调函数中任意定制需求</p>
<div>
  <ul>
    <li>123</li>
    <li>456</li>
    <li>789</li>
  </ul>
  <div>
    <div>
      <span>haha</span>
    </div>
  </div>
</div>
<div id="demo_node">
  <ul>
    <li>123</li>
  </ul>
  <p>hello</p>
  <h2>world</h2>
  <div>
    <p>dsa</p>
    <h3>
      <span>dsads</span>
    </h3>
  </div>
</div>
<script>

  //获取页面中的根节点--根标签
  var root=document.documentElement;//html
  //函数遍历DOM树
  //根据根节点,调用fn的函数,显示的是根节点的名字
  function forDOM(root1) {
    //调用f1,显示的是节点的名字
   // f1(root1);
    //获取根节点中所有的子节点
    var children=root1.children;
    //调用遍历所有子节点的函数
    forChildren(children);
  }
  //给我所有的子节点,我把这个子节点中的所有的子节点显示出来
  function forChildren(children) {
    //遍历所有的子节点
    for(var i=0;i<children.length;i++){
      //每个子节点
      var child=children[i];
      //显示每个子节点的名字
      f1(child);
      //判断child下面有没有子节点,如果还有子节点,那么就继续的遍历
      child.children&&forDOM(child);
    }
  }
  //函数调用,传入根节点
  forDOM(root);
  function f1(node) {
    console.log("节点的名字:"+node.nodeName);
  }

  //节点:nodeName,nodeType,nodeValue
</script>
</body>
```

### 正则表达式

**正则表达式**：也叫规则表达式，按照一定的规则组成的一个表达式，这个表达式的作用主要是匹配字符串的.

**正则表达式的作用**：匹配字符串

**正则表达式的组成**：是由元字符或者是限定符组成的一个式子

#### 贪婪匹配和懒惰匹配

在正则表达式中，**贪婪匹配**会匹配到符合正则表达式匹配模式的字符串的最长可能部分，并将其作为匹配项返回。另一种方案称为**懒惰匹配**，它会匹配到满足正则表达式的字符串的最小可能部分。

#### 元字符

- **.** :  通配符，匹配除了\n以外的任意一个字符

  ```javascript
  let exampleStr = "Let's have fun with regular expressions!";
  let unRegex = /.un/; // 修改这一行
  let result = unRegex.test(exampleStr);
  ```

  

- **[]** ：字符集，定义一组需要匹配的字符串
    ```javascript
    let quoteSample = "Beware of bugs in the above code; I have only proved it correct, not tried it.";
    let vowelRegex = /[aeiou]/ig; // 修改这一行
    let result = quoteSample.match(vowelRegex); // 修改这一行
    ```

- **-** : 连字符，定义匹配的范围

  - [0-9] ：表示0到9之间的任意的一个数字

  - [a-z]： 表示小写的字母中的任意的一个

  - [A-Z]： 表示大写的字母中的任意的一个

  - [0-9a-zA-Z] : 表示所有的字母的任意一个
  
    ```javascript
    let quoteSample = "The quick brown fox jumps over the lazy dog.";
    let alphabetRegex = /[a-z]/ig; // 修改这一行
    let result = quoteSample.match(alphabetRegex); // 修改这一行
    ```
  
    

- **I** ：或者

  - [0-9]|[a-z] : 表示的是要么是一个数字，要么是一个小写的字母

    ```javascript
    let petString = "James has a pet cat.";
    let petRegex = /dog|fish|cat|bird/;
    let result = petRegex.test(petString);
    ```

    

- **()** : 分组，提升优先级

#### 限定符（元字符）

​	限定符：限定前面表达式出现的次数

- ***** : 匹配出现零	次或多次的字符
  
  - [a-z] [0-9]* 小写字母中的任意一个，后面要么没有数字的，要么是多个数字的
  
    ```javascript
    let chewieQuote = "Aaaaaaaaaaaaaaaarrrgh!";
    let chewieRegex = /Aa*/; // 修改这一行
    let result = chewieQuote.match(chewieRegex);
    ```
  
    
  
- **+** : 匹配出现1次或多次的字符

  ```javascript
  let difficultSpelling = "Mississippi";
  let myRegex = /s+/ig; // 修改这一行
  let result = difficultSpelling.match(myRegex);
  ```

  

- **?** ：前面的表达式出现了0次到1次，最少是0次，最多1次，另一个含义：阻止贪婪模式

  - 可以使用问号`?`指定可能存在的元素

  ```javascript
  let text = "<h1>Winter is coming</h1>";
  let myRegex = /<.*?>/; // 修改这一行
  let result = text.match(myRegex);
  // 结果为<h1>
  
  // 检查全部或无
  let favWord = "favorite";
  let favRegex = /favou?rite/; // 修改这一行
  let result = favRegex.test(favWord);
  ```

  

- **{}** : 数量说明符，根据明确前面的表达式出现的次数
  
  - {0,} : 前面的表达式出现了0次到多次，和 ***** 一样
  - {1,} : 前面的表达式出现了1次到多次，和 **+** 一样
  - {5,10} : 表示前面的表达式出现了5次到10次
  - {4} 前面的表达式出现了4次
- {,10} **错误写法**
  
  ```javascript
  let ohStr = "Ohhh no";
  let ohRegex = /Oh{3,6}\sno/; // 修改这一行
  let result = ohRegex.test(ohStr);
  
  // 指定下限	 
  let haStr = "Hazzzzah";
  let haRegex = /Haz{4,}ah/; // 修改这一行
  let result = haRegex.test(haStr);
  // 指定匹配的确切数量
  let timStr = "Timmmmber";
  let timRegex = /Tim{4}ber/; // 修改这一行
  let result = timRegex.test(timStr);
  ```
  
  
#### 定位符


- **^** : 表示的是以什么开始，或者创建否定字符集

  - [^0-9] : 取反，非数字
  
  - [^a-z] : 非小写字母
  
  - ^[0-9] [a-z] 相当于严格模式
  
    ```javascript
    let quoteSample = "3 blind mice.";
    let myRegex = /[^0-9aeiou]/g; // 修改这一行
    let result = quoteSample.match(myRegex); // 修改这一行
    ```
  
    ```javascript
    let rickyAndCal = "Cal and Ricky both like racing.";
    let calRegex = /^Cal/; // 修改这一行
    let result = calRegex.test(rickyAndCal);
    ```
  
    
  
- **$** : 表示以什么结束

  - [0-9] [a-z]$ : 必须以小写字母结束
  
  ```javascript
  let caboose = "The last car on a train is the caboose";
  let lastRegex = /caboose$/; // 修改这一行
  let result = lastRegex.test(caboose);
  ```
  
  
  
- \d : 数字中的任意一个 \d == [0-9]

  ```javascript
  let numString = "Your sandwich will be $5.00";
  let numRegex = /\d/g; // 修改这一行
  let result = numString.match(numRegex).length;
  ```

  

- \D : 非数字中的一个 \D == [ ^0-9]

  ```javascript
  let numString = "Your sandwich will be $5.00";
  let noNumRegex = /\D/g; // 修改这一行
  let result = numString.match(noNumRegex).length;
  ```

  

- \s : 空白字符中的一个

  ```javascript
  let sample = "Whitespace is important in separating words";
  let countWhiteSpace = /\s/g; // 修改这一行
  let result = sample.match(countWhiteSpace);
  ```

  

- \S : 非空白字符中的一个

  ```javascript
  let sample = "Whitespace is important in separating words";
  let countNonWhiteSpace = /\S/g; // 修改这一行
  let result = sample.match(countNonWhiteSpace);
  ```

  

- \w: 非特殊符号的一个 \w == `[A-Za-z0-9_]`

  ```javascript
  let quoteSample = "The five boxing wizards jump quickly.";
  let alphabetRegexV2 = /\w/g; // 修改这一行
  let result = quoteSample.match(alphabetRegexV2).length;
  ```

  

- \W：匹配除了字母和数字的所有符号

  ```javascript
  let quoteSample = "The five boxing wizards jump quickly.";
  let nonAlphabetRegex = /\W/g; // 修改这一行
  let result = quoteSample.match(nonAlphabetRegex).length;
  ```

  

- \b : 单词的边界

  

#### 全局模式匹配与忽略大小写

- g : 全局模式匹配

  ```javascript
  var array = str.match(/\d{5}/g);
  ```


- i : 忽略大小写

  ```javascript
  let myString = "freeCodeCamp";
  let fccRegex = /freeCodecamp/i; 
  let result = fccRegex.test(myString);
  ```

- ig可以放在一起

  ```javascript
  let twinkleStar = "Twinkle, twinkle, little star";
  let starRegex = /Twinkle/gi; // 修改这一行
  let result = twinkleStar.match(starRegex); // 修改这一行
  ```

  

#### 创建正则表达式对象

1. 通过构造函数创建对象

   ```javascript
           var reg = new RegExp(/\d{5}/);
           var str = "我的电话是10086";
           var flag = reg.test(str);
           console.log(flag);
   ```

   

2. 字面量的方式构建对象

   ```javascript
           var reg = /\d{1,5}/;
           var flag = reg.test("whaha: 1123");
           console.log(flag);
   ```


#### 方法

- test 测试方法

  ```javascript
  let myString = "Hello, World!";
  let myRegex = /Hello/;
  let result = myRegex.test(myString); 
  ```

- match  提取找到的实际匹配项

  ```javascript
  let extractStr = "Extract the word 'coding' from this string.";
  let codingRegex = /coding/; // 修改这一行
  let result = extractStr.match(codingRegex); // 修改这一行
  ```

- replace 搜索并替换字符串中的文本。`.replace()`的输入首先是你想要搜索的正则表达式匹配模式，第二个参数是用于替换匹配的字符串或用于执行某些操作的函数。

  ```javascript
  let wrongText = "The sky is silver.";
  let silverRegex = /silver/;
  wrongText.replace(silverRegex, "blue");
  // Returns "The sky is blue."
  ```

  可以使用美元符号（`$`）访问替换字符串中的捕获组。

  ```javascript
  "Code Camp".replace(/(\w+)\s(\w+)/, '$2 $1');
  // Returns "Camp Code"
  ```

  
#### 先行断言

**正向先行断言**：会查看并确保搜索匹配模式中的元素存在，但实际上并不匹配。更实际用途是检查一个字符串中的两个或更多匹配模式。

**用法**：`(?=...)`，其中`...`就是需要存在但不会被匹配的部分。将返回匹配模式的其余部分。

**例子**：这里有一个简单的密码检查器，密码规则是 3 到 6 个字符且至少包含一个数字

```javascript
let password = "abc123";
let checkPass = /(?=\w{3,6})(?=\D*\d)/;
checkPass.test(password); // Returns true
```

**反向先行断言**： 会查看并确保搜索匹配模式中的元素不存在。

**用法**：`(?!...)`，其中`...`是你希望不存在的匹配模式。如果负向先行断言部分不存在，将返回匹配模式的其余部分。



#### 使用捕获组重用模式

使用**捕获组**搜寻重复的子字符串，括号`(`和`)`可以用来匹配重复的子字符串。

要指定重复字符串将出现的位置，可以使用反斜杠（`\`）后接一个数字。这个数字从 1 开始，随着你使用的每个捕获组的增加而增加。这里有一个示例，`\1`可以匹配第一个组。

下面的示例匹配任意两个被空格分割的单词：

```javascript
let repeatStr = "regex regex";
let repeatRegex = /(\w+)\s\1/;
repeatRegex.test(repeatStr); // Returns true
repeatStr.match(repeatRegex); // Returns ["regex regex", "regex"]
```

在正则表达式`reRegex`中使用`捕获组`，以匹配在字符串中仅重复三次的数字，每一个都由空格分隔。

```javascript
let repeatNum = "42 42 42";
let reRegex = /^(\d+)\s\1\s\1$/; // 修改这一行
let result = reRegex.test(repeatNum);
```



### 伪数组和数组

伪数组和数组的区别

- 真数组的长度是可变的，伪数组的长度是不可变的

- 真数组可以使用数组中的方法，伪数组不可以使用数组中的方法

- arguments也是假数组，虽然长度可变，但是无法调用数组中的方法

  ```javascript
      // 数组
     var arr=[10,20,30];
     arr[3]=100;
     console.log(arr.length);
     //对象---假的数组
     var obj={
       0:10,
       1:20,
       2:30,
       length:3
     };
     
  ```

  



