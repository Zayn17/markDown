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



#### 使用内联样式

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

### 过滤器

概念：Vue.js 允许你自定义过滤器，**可被用作一些常见的文本格式化**。过滤器可以用在两个地方：**mustache 插值和 v-bind 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符指示；

#### 私有过滤器

HTML元素

```html
<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>
```

私有 `filters` 定义方式

```javascript
filters: { // 私有局部过滤器，只能在 当前 VM 对象所控制的 View 区域进行使用
    dataFormat(input, pattern = "") { // 在参数列表中 通过 pattern="" 来指定形参默认值，防止报错
      var dt = new Date(input);
      // 获取年月日
      var y = dt.getFullYear();
      var m = (dt.getMonth() + 1).toString().padStart(2, '0');
      var d = dt.getDate().toString().padStart(2, '0');
      // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
      // 否则，就返回  年-月-日 时：分：秒
      if (pattern.toLowerCase() === 'yyyy-mm-dd') {
        return `${y}-${m}-${d}`;
      } else {
        // 获取时分秒
        var hh = dt.getHours().toString().padStart(2, '0');
        var mm = dt.getMinutes().toString().padStart(2, '0');
        var ss = dt.getSeconds().toString().padStart(2, '0');
        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
      }
    }
  }
```

> 使用ES6中的字符串新方法 String.prototype.padStart(maxLength, fillString='') 或 String.prototype.padEnd(maxLength, fillString='')来填充字符串；

#### 全局过滤器

需添加在Vue的声明之前

```javascript

// 定义一个全局过滤器
  // 所谓的全局过滤器，就是所有的VM实例都共享的
Vue.filter('dataFormat', function (input, pattern = '') {
  var dt = new Date(input);
  // 获取年月日
  var y = dt.getFullYear();
  var m = (dt.getMonth() + 1).toString().padStart(2, '0');
  var d = dt.getDate().toString().padStart(2, '0');
  // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
  // 否则，就返回  年-月-日 时：分：秒
  if (pattern.toLowerCase() === 'yyyy-mm-dd') {
    return `${y}-${m}-${d}`;
  } else {
    // 获取时分秒
    var hh = dt.getHours().toString().padStart(2, '0');
    var mm = dt.getMinutes().toString().padStart(2, '0');
    var ss = dt.getSeconds().toString().padStart(2, '0');
    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
  }
});
```

### 键盘修饰符以及自定义键盘修饰符

1. 通过`Vue.config.keyCodes.名称 = 按键值`来自定义按键修饰符的别名

   HTML

   ```html
   <input type="text" v-model="name" @keyup.f2="add">
   ```

   JS

   ```javascript
   Vue.config.keyCodes.f2 = 113;
   ```

2. ```javascript
   Vue.config.keyCodes.f2 = 113;
   ```

3. 使用 Vue 提供的绝大多数常用的按键码 

   ```html
   <input v-on:keyup.enter="submit">
   ```

4. 直接使用按键码

   ```html
   <input v-on:keyup.13="submit">
   ```

### 自定义指令

#### 全局自定义指令

```javascript
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```

#### 局部自定义指令

```javascript
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

#### 钩子函数

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。

  ```javascript
  bind: function (el, binding) { 
      // 每当指令绑定到元素上的时候，会立即执行这个 bind 函数，只执行一次
      // 注意： 在每个 函数中，第一个参数，永远是 el ，表示 被绑定了指令的那个元素，这个 el 参数，是一个原生的JS对象
      // 在元素 刚绑定了指令的时候，还没有 插入到 DOM中去，这时候，调用 focus 方法没有作用
      //  因为，一个元素，只有插入DOM之后，才能获取焦点
      // el.focus()
      
       // 和样式相关的操作，一般都可以在 bind 执行
      // 样式，只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了一个内联的样式
      // 将来元素肯定会显示到页面中，这时候，浏览器的渲染引擎必然会解析样式，应用给这个元素
      el.style.color = binding.value
        },
  ```

  

- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

  ```javascript
        inserted: function (el) {
            // inserted 表示元素 插入到DOM中的时候，会执行 inserted 函数【触发1次】
          el.focus()
          // 和JS行为有关的操作，最好在 inserted 中去执行，放置 JS行为不生效
        },
  ```

- `update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

  ```javascript
        updated: function (el) {  
            // 当VNode更新的时候，会执行 updated， 可能会触发多次
        }
  ```

  

- `componentUpdated`：指令所在组件的 VNode **及其子 VNode** 全部更新后调用。

- `unbind`：只调用一次，指令与元素解绑时调用。

#### 钩子函数参数

- `el`：指令所绑定的元素，可以用来直接操作 DOM 。

- binding ：一个对象，包含以下属性：

  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。

- `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。

- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

##### 动态指令参数

 指令的参数可以是动态的。例如，在 `v-mydirective:[argument]="value"` 中，`argument` 参数可以根据组件实例数据进行更新！这使得自定义指令可以在应用中被灵活使用。 

- 通过指令值来更新数据

  ```html
  <div id="baseexample">
    <p>Scroll down the page</p>
    <p v-pin="200">Stick me 200px from the top of the page</p>
  </div>
  ```

  ```javascript
  Vue.directive('pin', {
    bind: function (el, binding, vnode) {
      el.style.position = 'fixed'
      el.style.top = binding.value + 'px'
    }
  })
  
  new Vue({
    el: '#baseexample'
  })
  ```

- 使用动态参数

  ```html
  <div id="dynamicexample">
    <h3>Scroll down inside this section ↓</h3>
    <p v-pin:[direction]="200">I am pinned onto the page at 200px to the left.</p>
  </div>
  ```

  ```javascript
  Vue.directive('pin', {
    bind: function (el, binding, vnode) {
      el.style.position = 'fixed'
      var s = (binding.arg == 'left' ? 'left' : 'top')
      el.style[s] = binding.value + 'px'
    }
  })
  
  new Vue({
    el: '#dynamicexample',
    data: function () {
      return {
        direction: 'left'
      }
    }
  })
  ```

#### 函数简写

 在很多时候，你可能想在 `bind` 和 `update` 时触发相同行为，而不关心其它的钩子。比如这样写: 

```javascript
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

```javascript
  directives: { // 自定义私有指令
        'fontweight': { // 设置字体粗细的
          bind: function (el, binding) {
            el.style.fontWeight = binding.value
          }
        },
        'fontsize': function (el, binding) { // 注意：这个 function 等同于 把 代码写到了 bind 和 update 中去
          el.style.fontSize = parseInt(binding.value) + 'px'
        }
      }
```



#### 对象字面量

 如果指令需要多个值，可以传入一个 JavaScript 对象字面量。记住，指令函数能够接受所有合法的 JavaScript 表达式。 

```html
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
```

```javascript
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})
```

### Vue实例的生命周期

什么是生命周期：从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期！

[生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)：就是生命周期事件的别名而已；

生命周期钩子 = 生命周期函数 = 生命周期事件

主要的生命周期函数分类：

 - 创建期间的生命周期函数：
      - beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
      - created：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
      - beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中
      - mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示

 - 运行期间的生命周期函数：
     - beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
     - updated：实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！

 - 销毁期间的生命周期函数：
     - beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
     - destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

   ![](F:\Vue\黑马vue\配套资料\day2\笔记\lifecycle.png)

### [vue-resource 实现 get, post, jsonp请求](https://github.com/pagekit/vue-resource)

除了 vue-resource 之外，还可以使用 `axios` 的第三方包实现实现数据的请求

#### JSONP的实现原理

 + 由于浏览器的安全性限制，不允许AJAX访问 协议不同、域名不同、端口号不同的 数据接口，浏览器认为这种访问不安全；

 + 可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式，称作JSONP（注意：根据JSONP的实现原理，知晓，JSONP只支持Get请求）；

   具体实现过程：

  - 先在客户端定义一个回调方法，预定义对数据的操作；
  - 再把这个回调方法的名称，通过URL传参的形式，提交到服务器的数据接口；
  - 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行；
  - 客户端拿到服务器返回的字符串之后，当作Script脚本去解析执行，这样就能够拿到JSONP的数据了；

#### JSONP实例

通过 Node.js ，来手动实现一个JSONP的请求例子

app.js

```javascript
const http = require('http');
    // 导入解析 URL 地址的核心模块
    const urlModule = require('url');

    const server = http.createServer();
    // 监听 服务器的 request 请求事件，处理每个请求
    server.on('request', (req, res) => {
      const url = req.url;

      // 解析客户端请求的URL地址
      var info = urlModule.parse(url, true);

      // 如果请求的 URL 地址是 /getjsonp ，则表示要获取JSONP类型的数据
      if (info.pathname === '/getjsonp') {
        // 获取客户端指定的回调函数的名称
        var cbName = info.query.callback;
        // 手动拼接要返回给客户端的数据对象
        var data = {
          name: 'zs',
          age: 22,
          gender: '男',
          hobby: ['吃饭', '睡觉', '运动']
        }
        // 拼接出一个方法的调用，在调用这个方法的时候，把要发送给客户端的数据，序列化为字符串，作为参数传递给这个调用的方法：
        var result = `${cbName}(${JSON.stringify(data)})`;
        // 将拼接好的方法的调用，返回给客户端去解析执行
        res.end(result);
      } else {
        res.end('404');
      }
    });

    server.listen(3000, () => {
      console.log('server running at http://127.0.0.1:3000');
    });
```

vue-resource 的配置步骤：

 + 直接在页面中，通过`script`标签，引入 `vue-resource` 的脚本文件；
 + 注意：引用的先后顺序是：先引用 `Vue` 的脚本文件，再引用 `vue-resource` 的脚本文件；

发送get请求：

```javascript
getInfo() { // get 方式获取数据
  this.$http.get('http://127.0.0.1:8899/api/getlunbo').then(res => {
    console.log(res.body);
  })
}
```

发送post请求：

```javascript
postInfo() {
  var url = 'http://127.0.0.1:8899/api/post';
  // post 方法接收三个参数：
  // 参数1： 要请求的URL地址
  // 参数2： 要发送的数据对象
  // 参数3： 指定post提交的编码类型为 application/x-www-form-urlencoded
  this.$http.post(url, { name: 'zs' }, { emulateJSON: true }).then(res => {
    console.log(res.body);
  });
}
```

发送JSONP请求获取数据：

```javascript
jsonpInfo() { // JSONP形式从服务器获取数据
  var url = 'http://127.0.0.1:8899/api/jsonp';
  this.$http.jsonp(url).then(res => {
    console.log(res.body);
  });
}
```

客户端

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
    <script>
        function showInfo(data){
            console.log(data)
        }
    </script>
    <script src="http://127.0.0.1:3000/getscript?callback=showInfo"></script>
</body>
</html>
```

服务器端

```javascript
// 导入 http 内置模块
const http = require('http')
const urlModule = require('url')
const server = http.createServer()

server.on('request',function(req,res){
    // const url = req.url
    const {pathname: url, query} = urlModule.parse(req.url, true)

    if(url === '/getscript'){
        var data = {
            name:'xjj',
            age: 18,
            gender: '女孩子'
        }
        var scriptStr = `${query.callback}(${JSON.stringify(data)})`
        res.end(scriptStr)
    }else {
        res.end('404')
    }
})

server.listen(3000, function(){
    console.log('server listen at http://127.0.0.1:3000')
})
```

