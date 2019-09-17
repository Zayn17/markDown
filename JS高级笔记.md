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