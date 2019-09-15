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

