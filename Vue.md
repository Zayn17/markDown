###  Vue 基本知识

#### Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

- MVC 是后端的分层开发概念

- MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel

  ![](F:\GitHub\markDown\Vue\01.MVC和MVVM的关系图解.png)

#### Vue.js 基本代码结构

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <!-- 1. 导入Vue的包 -->
  <script src="./lib/vue-2.4.0.js"></script>
</head>

<body>
  <!-- 将来 new 的Vue实例，会控制这个 元素中的所有内容 -->
  <!-- Vue 实例所控制的这个元素区域，就是我们的 V  -->
  <div id="app">
    <p>{{ msg }}</p>
  </div>

  <script>
    // 2. 创建一个Vue的实例
    // 当我们导入包之后，在浏览器的内存中，就多了一个 Vue 构造函数
    //  注意：我们 new 出来的这个 vm 对象，就是我们 MVVM中的 VM调度者
    var vm = new Vue({
      el: '#app',  // 表示，当前我们 new 的这个 Vue 实例，要控制页面上的哪个区域
      // 这里的 data 就是 MVVM中的 M，专门用来保存 每个页面的数据的
      data: { // data 属性中，存放的是 el 中要用到的数据
        msg: '欢迎学习Vue' // 通过 Vue 提供的指令，很方便的就能把数据渲染到页面上，程序员不再手动操作DOM元素了【前端的Vue之类的框架，不提倡我们去手动操作DOM元素了】
      }
    })
  </script>
</body>

</html>
```

### Vue**指令**

#### v-clock

使用 v-cloak 能够解决 插值表达式闪烁的问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    [v-cloak] {
       display: none; 
    }
  </style>
</head>
<body>
    <div id="app">
        <p v-clock>{{msg}}</p>
    </div>
    <script src="./lib/vue-2.4.0.js"></script>
      <script>
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '123'
      }
    })

  </script>
</body>
</html>
```

#### v-text和v-html

**v-text** :

- *v-text* 是没有闪烁问题的
- *v-text*会覆盖元素中原本的内容，但是 插值表达式  只会替换自己的这个占位符，不会把 整个元素的内容清空

**v-html** :

- 可将文本当作HTML标签解析后输出

##### {{ }}、v-html 和 v-text 的不同

{{ }} :  将元素当成纯文本输出 

v-html :   v-html会将元素当成HTML标签解析后输出 

v-text :  v-text会将元素当成纯文本输出 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
    <div id="app">
        <div>{{msg}}</div>
    	<div v-text="msg"></div>
    	<div v-html="msg">1212112</div>

    </div>
    <script src="./lib/vue-2.4.0.js"></script>
      <script>
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '<h1>哈哈，我是一个大大的H1， 我大，我骄傲</h1>'
      }
    })

  </script>
</body>
</html>
```

#### v-on

**v-on**: 事件绑定机制，缩写是 **@**

```html
<body>
    <div id="app">
    	<input type="button" value="按钮" @click="show">
    </div>
    <script src="./lib/vue-2.4.0.js"></script>
      <script>
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '<h1>哈哈，我是一个大大的H1， 我大，我骄傲</h1>'
      },
      methods: { // 这个 methods属性中定义了当前Vue实例所有可用的方法
        show: function () {
          alert('Hello')
        }
      }
    })

  </script>
</body>
```

#### v-bind

`v-bind` : 属性绑定机制，缩写是 `:`

**使用方法**：

- 直接使用指令`v-bind`
- 使用简化指令`:`
- 在绑定的时候，拼接绑定内容：`:title="btnTitle + ', 这是追加的内容'"`

```html
<body>
  <div id="app">
    <div>{{msg2}}</div>
    <div v-text="msg2"></div>
    <div v-html="msg2">1212112</div>

    <!-- v-bind: 是 Vue中，提供的用于绑定属性的指令 -->
    <input type="button" value="按钮" v-bind:title="mytitle + '123'">
    <!-- 注意： v-bind: 指令可以被简写为 :要绑定的属性 -->
    <!-- v-bind 中，可以写合法的JS表达式 -->
  </div>
  <script src="./lib/vue-2.4.0.js"></script>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        mytitle: '这是一个自己定义的title'
      }
    })
  </script>
</body>
```

#### 事件修饰符

##### .stop

- 阻止冒泡

##### .prevent

- 阻止默认事件

##### .capture

- 实现捕获触发事件的机制

##### .self

- 只当事件在该元素本身（比如不是子元素）触发时触发回调

##### .once

- 事件只触发一次

```html
<body>
  <div id="app">

    <!-- 使用  .stop  阻止冒泡 -->
    <!-- <div class="inner" @click="div1Handler">
      <input type="button" value="戳他" @click.stop="btnHandler">
    </div> -->

    <!-- 使用 .prevent 阻止默认行为 -->
    <!-- <a href="http://www.baidu.com" @click.prevent="linkClick">有问题，先去百度</a> -->

    <!-- 使用  .capture 实现捕获触发事件的机制 -->
    <!-- <div class="inner" @click.capture="div1Handler">
      <input type="button" value="戳他" @click="btnHandler">
    </div> -->

    <!-- 使用 .self 实现只有点击当前元素时候，才会触发事件处理函数 -->
    <!-- <div class="inner" @click="div1Handler">
      <input type="button" value="戳他" @click="btnHandler">
    </div> -->

    <!-- 使用 .once 只触发一次事件处理函数 -->
    <!-- <a href="http://www.baidu.com" @click.prevent.once="linkClick">有问题，先去百度</a> -->


    <!-- 演示： .stop 和 .self 的区别 -->
    <!-- <div class="outer" @click="div2Handler">
      <div class="inner" @click="div1Handler">
        <input type="button" value="戳他" @click.stop="btnHandler">
      </div>
    </div> -->

    <!-- .self 只会阻止自己身上冒泡行为的触发，并不会真正阻止 冒泡的行为 -->
    <!-- <div class="outer" @click="div2Handler">
      <div class="inner" @click.self="div1Handler">
        <input type="button" value="戳他" @click="btnHandler">
      </div>
    </div> -->

  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        div1Handler() {
          console.log('这是触发了 inner div 的点击事件')
        },
        btnHandler() {
          console.log('这是触发了 btn 按钮 的点击事件')
        },
        linkClick() {
          console.log('触发了连接的点击事件')
        },
        div2Handler() {
          console.log('这是触发了 outer div 的点击事件')
        }
      }
    });
  </script>
</body>
```

#### v-model

- 实现 表单元素和 Model 中数据的双向数据绑定

- **注意** ：`v-model` 只能运用在 表单元素中

##### v-model 和 v-bind 的 不同

`v-bind` :  只能实现数据的**单向绑定**，从 M 自动绑定到 V， 无法实现数据的双向绑定

`v-model` : 实现 表单元素和 Model 中数据的**双向数据绑定**

```html
<body>
  <div id="app">
    <h4>{{ msg }}</h4>

    <!-- v-bind 只能实现数据的单向绑定，从 M 自动绑定到 V， 无法实现数据的双向绑定  -->
    <!-- <input type="text" v-bind:value="msg" style="width:100%;"> -->

    <!-- 使用  v-model 指令，可以实现 表单元素和 Model 中数据的双向数据绑定 -->
    <!-- 注意： v-model 只能运用在 表单元素中 -->
    <!-- input(radio, text, address, email....)   select    checkbox   textarea   -->
    <input type="text" style="width:100%;" v-model="msg">
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '大家都是好学生，爱敲代码，爱学习，爱思考，简直是完美，没瑕疵！'
      },
      methods: {
      }
    });
  </script>
</body>
```



#### v-for 和 key 属性

1. 迭代数组

```html
<ul>
  <li v-for="(item, i) in list">索引：{{i}} --- 姓名：{{item.name}} --- 年龄：{{item.age}}</li>
</ul>
```

2. 迭代对象中的属性

```html
	<!-- 循环遍历对象身上的属性 -->
    <div v-for="(val, key, i) in userInfo">{{val}} --- {{key}} --- {{i}}</div>
```

3. 迭代数字

```html
<p v-for="i in 10">这是第 {{i}} 个P标签</p>
```



> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，key 现在是必须的。



当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。



为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

```html
<p v-for="item in list" :key="item.id">
      <input type="checkbox">{{item.id}} --- {{item.name}}
    </p>
```



#### `v-if` 和 `v-show`

`v-if` ：每次都会重新删除或创建元素

 `v-show`： 每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式

```html
    <h3 v-if="flag">这是用v-if控制的元素</h3>
    <h3 v-show="flag">这是用v-show控制的元素</h3>
```



> 一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好。
>
> 

### 在Vue中使用样式

#### 使用class样式

1. 数组
```html
<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>
```

2. 数组中使用三元表达式
```html
<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
```

3. 数组中嵌套对象
```html
<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
```

4. 直接使用对象
```html
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```



### 使用内联样式

1. 直接在元素上通过 `:style` 的形式，书写样式对象
```html
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
 + 在data上定义样式：
```js
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```html
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```

3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象
 + 在data上定义样式：
```javascript
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```html
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```