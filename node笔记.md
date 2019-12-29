### Node 中的 JavaScript

### 没有DOM和BOM

在 Node 中，采用 *EcmaScript* 进行编码，没有 *BOM、DOM*

  + EcmaScript
    * 变量
    * 方法
    * 数据类型
    * 内置对象
    * Array
    * Object
    * Date
    * Math

### 读取文件

浏览器中的 JavaScript 是没有文件操作的能力的，但是 Node 中的 JavaScript 具有文件操作的能力。

1. 使用require方法加载 fs 模块

   ```javascript
   // fs 是 file-system 的简写，就是文件系统的意思
   // 在 Node 中如果想要进行文件操作，就必须引入 fs 这个核心模块
   // 在 fs 这个核心模块中，就提供了所有的文件操作相关的 API
   // 例如：fs.readFile 就是用来读取文件的
   var fs = require('fs')
   ```

   

2. 读取文件

   - 第一个参数就是要读取的文件路径
   - 第二个参数是一个回调函数
     - 成功
       - data — 数据	error — undefined
     - 失败
       - data — undefined	error — 错误对象

   ```javascript
   fs.readFile('./data/a.txt', function (error, data) {
     // <Buffer 68 65 6c 6c 6f 20 6e 6f 64 65 6a 73 0d 0a>
     // 文件中存储的其实都是二进制数据 0 1
     // 这里为什么看到的不是 0 和 1 呢？原因是二进制转为 16 进制了
     // 所以我们可以通过 toString 方法把其转为我们能认识的字符
     // 在这里就可以通过判断 error 来确认是否有错误发生
     if (error) {
       console.log('读取文件失败了')
     } else {
       console.log(data.toString())
     }
   })
   
   ```

   ​	

### 写文件

1. 使用require方法加载 fs 模块

2. 写文件

   - 第一个参数：文件路径
   - 第二个参数：文件内容
   - 第三个参数：回调函数
     - 成功
       - error — undefined
     - 失败
       - error— 错误对象

   ```javascript
   var fs = require('fs')
   
   fs.writeFile('./data/你好.md', '大家好，给大家介绍一下，我是Node.js', function (error) {
     if (error) {
       console.log('写入失败')
     } else {
       console.log('写入成功了')
     }
   })
   
   ```


### 构建 Web 服务器

   1. 加载 http 核心模块
   
      ```javascript
      var http = require('http')
      ```

      

   2. 使用 http.createServer() 方法创建一个 Web 服务器
   
      ```javascript
      //    返回一个 Server 实例
      var server = http.createServer()
      ```

      

   3. 服务器提供服务

      - request 请求事件处理函数，需要接收两个参数：

        - Request 请求对象

          请求对象可以用来获取客户端的一些请求信息，例如请求路径

          - write()

            write 可以用来给客户端发送响应数据，write 可以使用多次

          - end()

            end 来结束响应，可同时发送响应数据

        - Response 响应对象

          响应对象可以用来给客户端发送响应消息
        
      ```javascript
      //    发请求
      //    接收请求
      //    处理请求
      //    给个反馈（发送响应）
      //    注册 request 请求事件
      //    当客户端请求过来，就会自动触发服务器的 request 请求事件，然后执行第二个参数：回调处理函数
      server.on('request', function (request, response) {
        console.log('收到请求了，请求路径是：' + req.url)
        console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)
      
        // res.write('hello')
        // res.write(' world')
        // res.end()
      
        // 上面的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
        // res.end('hello nodejs')
      
        // 根据不同的请求路径发送不同的响应结果
        // 1. 获取请求路径
        //    req.url 获取到的是端口号之后的那一部分路径
        //    也就是说所有的 url 都是以 / 开头的
        // 2. 判断路径处理响应
      
        var url = req.url
      
        if (url === '/') {
          res.end('index page')
        } else if (url === '/login') {
          res.end('login page')
        } else if (url === '/products') {
          var products = [{
              name: '苹果 X',
              price: 8888
            },
            {
              name: '菠萝 X',
              price: 5000
            },
            {
              name: '小辣椒 X',
              price: 1999
            }
          ]
      
          // 响应内容只能是二进制数据或者字符串
          //  数字
          //  对象
          //  数组
          //  布尔值
          res.end(JSON.stringify(products))
        } else {
          res.end('404 Not Found.')
        }
      })
      ```

   4. 绑定端口号，启动服务器
   
      ```javascript
      server.listen(3000, function () {
        console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
      })
      ```
   
      


### 模块系统

* 在 Node 中没有全局作用域的概念
  
    * 在 Node 中，只能通过 require 方法来加载执行多个 JavaScript 脚本文件
    
    * require 加载只能是执行其中的代码，文件与文件之间由于是模块作用域，所以不会有污染的问题
      - 模块完全是封闭的
      - 外部无法访问内部
      - 内部也无法访问外部
      
    * 模块作用域固然带来了一些好处，可以加载执行多个文件，可以完全避免变量命名冲突污染的问题
    
    * 但是某些情况下，模块与模块是需要进行通信的
    
    * 在每个模块中，都提供了一个对象：`exports`
    
    * 该对象默认是一个空对象
    
    * 你要做的就是把需要被外部访问使用的成员手动的挂载到 `exports` 接口对象中
    
    * 然后谁来 `require` 这个模块，谁就可以得到模块内部的 `exports` 接口对象
    
    * 还有其它的一些规则，具体后面讲，以及如何在项目中去使用这种编程方式，会通过后面的案例来处理
    
    * a.js
    
      ```javascript
      var bExports = require('./b')
      var fs = require('fs')
      
      console.log(bExports.foo)
      
      console.log(bExports.add(10, 30))
      
      console.log(bExports.age)
      
      bExports.readFile('./a.js')
      
      fs.readFile('./a.js', function (err, data) {
        if (err) {
          console.log('读取文件失败')
        } else {
          console.log(data.toString())
        }
      })
      ```
    
    * b.js
    
      ```javascript
      var foo = 'bbb'
      
      // console.log(exports)
      
      exports.foo = 'hello'
      
      exports.add = function (x, y) {
        return x + y
      }
      
      exports.readFile = function (path, callback) {
        console.log('文件路径：', path)
      }
      
      var age = 18
      
      exports.age = age
      
      function add(x, y) {
        return x - y
      }
      ```



#### 核心模块
* 核心模块是由 Node 提供的一个个的具名的模块，它们都有自己特殊的名称标识，例如
	
	- fs 文件操作模块
	- http 网络服务构建模块
	- os 操作系统信息模块
	   - path 路径处理模块
	   - 。。。。
	
	  * 所有核心模块在使用的时候都必须手动的先使用 `require` 方法来加载，然后才可以使用，例如：
	    - `var fs = require('fs')`
	
	  ```javascript
	  // 用来获取机器信息的
	  var os = require('os')
	  
	  // 用来操作路径的
	  var path = require('path')
	  
	  // 获取当前机器的 CPU 信息
	  console.log(os.cpus())
	  
	  // memory 内存
	  console.log(os.totalmem())
	  
	  // 获取一个路径中的扩展名部分
	  // extname extension name
	  console.log(path.extname('c:/a/b/c/d/hello.txt'))
	  ```


​    
### http

  + require

  + 端口号
    * ip 地址定位计算机
    
    * 端口号定位具体的应用程序
    
      ```javascript
      server.on('request', function (req, res) {
        console.log('收到请求了，请求路径是：' + req.url)
        console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)
    
        res.end('hello nodejs')
      })
      ```
    
  + Content-Type
    * 服务器最好把每次响应的数据是什么内容类型都告诉客户端，而且要正确的告诉
    * 不同的资源对应的 Content-Type 是不一样，具体参照：http://tool.oschina.net/commons
    * 对于文本类型的数据，最好都加上编码，目的是为了防止中文解析乱码问题
    
  + 通过网络发送文件
    * 发送的并不是文件，本质上来讲发送是文件的内容
    * 当浏览器收到服务器响应内容之后，就会根据你的 Content-Type 进行对应的解析处理
    
    ```javascript
    
    server.on('request', function (req, res) {
      // 在服务端默认发送的数据，其实是 utf8 编码的内容
      // 但是浏览器不知道你是 utf8 编码的内容
      // 浏览器在不知道服务器响应内容的编码的情况下会按照当前操作系统的默认编码去解析
      // 中文操作系统默认是 gbk
      // 解决方法就是正确的告诉浏览器我给你发送的内容是什么编码的
      // 在 http 协议中，Content-Type 就是用来告知对方我给你发送的数据内容是什么类型
      // res.setHeader('Content-Type', 'text/plain; charset=utf-8')
      // res.end('hello 世界')
    
      var url = req.url
    
      if (url === '/plain') {
        // text/plain 就是普通文本
        res.setHeader('Content-Type', 'text/plain; charset=utf-8')
        res.end('hello 世界')
      } else if (url === '/html') {
        // 如果你发送的是 html 格式的字符串，则也要告诉浏览器我给你发送是 text/html 格式的内容
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end('<p>hello html <a href="">点我</a></p>')
      }
    })
    
    ```
    
    
