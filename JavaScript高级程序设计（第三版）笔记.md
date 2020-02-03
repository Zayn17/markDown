## 第10章	DOM

 DOM（**文档对象模型**）是针对 HTML 和 XML 文档的一个 API（应用程序编程接口）。 DOM 描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。   

### 10.1	节点类型

**文档节点**是每个文档的根节点。

文档元素是文档的**最外层元素**，文档中的其他所有元素都包含在文档元素中。每个文档只能有一个文档元素。  

#### 10.1.1	Node类型

DOM1 级定义了一个 Node 接口，该接口将由 DOM 中的所有节点类型实现。这个 Node 接口在JavaScript 中是作为 Node 类型实现的；**除了 IE 之外**，在其他所有浏览器中都可以访问到这个类型。  

每个节点都有一个 *nodeType* 属性，用于表明节点的类型。  

确定节点类型

```javascript
if (someNode.nodeType == Node.ELEMENT_NODE){ // 在IE中无效
	alert("Node is an element.");
}
if (someNode.nodeType == 1){ // 适用于所有浏览器
	alert("Node is an element.");
}
```

1. nodeName 和 nodeValue 属性  

- 要了解节点的具体信息，可以使用 *nodeName* 和 *nodeValue* 这两个属性。这两个属性的值完全取决于节点的类型。  

- 对于元素节点， *nodeName* 中保存的始终都是元素的标签名，而 *nodeValue* 的值则始终为 null。  

2. 节点关系

- 每个节点都有一个 *childNodes* 属性，其中保存着一个 NodeList 对象。 NodeList 是一种**类数组对象**，用于保存一组有序的节点，可以通过位置来访问这些节点。

- NodeList 对象的独特之处在于，它实际上是基于 DOM 结构动态执行查询的结果，因此 DOM 结构的变化能够自动反映在 NodeList 对象中。

- 实例

  ```javascript
  var firstChild = someNode.childNodes[0];
  var secondChild = someNode.childNodes.item(1);
  var count = someNode.childNodes.length;
  ```

-  每个节点都有一个 *parentNode* 属性，该属性指向文档树中的父节点。  

- 包含在*childNodes* 列表中的每个节点相互之间都是同胞节点。通过使用列表中每个节点的  *previousSibling* 和 *nextSibling* 属性，可以访问同一列表中的其他节点。  

- 父节点的 *firstChild* 和 *lastChild* 属性分别指向其 childNodes 列表中的第一个和最后一个节点。 

- *hasChildNodes()* 也是一个非常有用的方法，这个方法在节点包含一或多个子节点的情况下返回 true；应该说，这是比查询 childNodes列表的 length 属性更简单的方法。  

- ![](F:\GitHub\markDown\JavaScript高级程序设计\Node关系图.png)

3. 操作节点
因为关系指针都是只读的，所以 DOM 提供了一些操作节点的方法。
- appendChild()

  - appendChild()，用于向 childNodes 列表的末尾添加一个节点。添加节点后， childNodes 的新增节点、父节点及以前的最后一个子节点的关系指针都会相应地得到更新。
  - 更新完成后， appendChild() 返回新增的节点。  
  -   如果传入到 appendChild() 中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置转移到新位置。  
  
-   insertBefore()  

  - 把节点放在 childNodes 列表中某个特定的位置上  

  - 这个方法接受两个参数：要插入的节点和作为参照的节点。

  - 插入节点后，被插入的节点会变成参照节点的前一个同胞节点（ previousSibling），同时被方法返回。  

  - ```javascript
    var returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
    ```

-   replaceChild()  

  - 接受的两个参数是：要插入的节点和要替换的节点。  

  - 要替换的节点将由这个方法返回并从文档树中被移除，同时由要插入的节点占据其位置。
  
- ```javascript
    //替换第一个子节点
    var returnedNode = someNode.replaceChild(newNode, someNode.firstChild);
    ```
  
-   removeChild()  

  - 接受一个参数，即要移除的节点。
  - 被移除的节点将成为方法的返回值  

- cloneNode()

  - 用于创建调用这个方法的节点
    的一个完全相同的副本。 
  - 接受一个布尔值参数，表示是否执行深复制。
    -  在参数为 true的情况下，执行深复制，也就是复制节点及其整个子节点树；
    -  在参数为 false 的情况下，执行浅复制，即只复制节点本身
  - 复制后返回的节点副本属于文档所有，但并没有为它指定父节点。因此，这个节点副本就成为了一个“孤儿。  
- normalize()
    - 这个方法唯一的作用就是处理文档树中的文本节点。
    - 如果找到了空文本节点，则删除它。
    - 如果找到相邻的文本节点，则将它们合并为一个文本节点。



#### 10.1.2 Document类型

## 第12章	DOM2 和 DOM3

### 12.1	DOM 变化

DOM2 级和 3 级的目的在于**扩展 DOM API**，以满足操作 XML 的所有需求，同时提供更好的错误处理及特性检测能力。从某种意义上讲，实现这一目的很大程度意味着对命名空间的支持。

“DOM2 级核心”没有引入新类型，它只是在 DOM1 级的基础上通过增加新方法和新属性来增强了既有类型。

“DOM3级核心”同样增强了既有类型，但也引入了一些新类型。  

#### 12.1.1	针对XML命名空间的变化

> 详细见书，目前来看对自己没有帮助

HTML 不支持 XML 命名空间，但 XHTML 支持 XML 命名空间。
命名空间要使用 xmlns 特性来指定。 XHTML 的命名空间是 http://www.w3.org/1999/xhtml，在任何格式良好 XHTML 页面中，都应该将其包含在<html>元素中

```html
<html xmlns="http://www.w3.org/1999/xhtml">
	<head>
		<title>Example XHTML page</title>
	</head>
	<body>
		Hello world!
	</body>
</html>
```

要想明确地为 XML 命名空间创建前缀，可以使用 xmlns 后跟冒号，再后跟前缀.有时候为了避免不同语言间的冲突，也需要使用命名空间来限定特性  

```html
<xhtml:html xmlns:xhtml="http://www.w3.org/1999/xhtml">
	<xhtml:head>
		<xhtml:title>Example XHTML page</xhtml:title>
	</xhtml:head>
	<xhtml:body xhtml:class="home">
		Hello world!
	</xhtml:body>
</xhtml:html>
```

1. Node类型的变化

   在 DOM2 级中， Node 类型包含下列特定于命名空间的属性。  

   节点使用了命名空间前缀时，其 nodeName 等于 prefix+":"+ localName。  

   - localName：不带命名空间前缀的节点名称
   - namespaceURI：命名空间 URI 或者（在未指定的情况下是） null。
   - prefix：命名空间前缀或者（在未指定的情况下是） null。

   ......

2. Document 类型的变化

3. Element 类型的变化

4. NamedNodeMap 类型的变化



#### 12.1.2	其他方面的变化

1. DocumentType 类型的变化  

   DocumentType 类型新增了 3 个属性

   - publicId

   - systemId

   - internalSubset

   其中，前两个属性表示的是文档类型声明中的两个信息段，**这两个信息段在 DOM1 级中是没有办法访问到的。**
   
   ```html
   <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
   "http://www.w3.org/TR/html4/strict.dtd" [<!ELEMENT name (#PCDATA)>]>
   ```
   
   对这个文档类型声明而言， publicId 是"-//W3C//DTD HTML 4.01//EN"，而 systemId 是"http://www.w3.org/TR/html4/strict.dtd"。  访问 document.doctype.internalSubset 将得到"<!ELEMENT name (#PCDATA)>"。  
   
2. Document  类型的变化。

   Document 类型的变化中唯一与命名空间无关的方法是 **importNode()**。
   
   - 用途
   
     从一个文档中取得一个节点，然后将其导入到另一个文档，使其成为这个文档结构的一部分。
   
   - 注意
   
     每个节点都有一个 ownerDocument 属性，表示所属的文档。如果调用 appendChild()时传入的节点属于不同的文档（ ownerDocument 属性的值不一样），则会导致错误。但在调用 importNode()时传入不同文档的节点则会返回一个新节点，这个新节点的所有权归当前文档所有。  
   
     ```javascript
     var newNode = document.importNode(oldNode, true); //导入节点及其所有子节点
     document.body.appendChild(newNode);
     ```
   
3. Node 类型的变化

   Node 类型中唯一与命名空间无关的变化，就是添加了 **isSupported()方法。**
   
   与 DOM1 级为document.implementation 引入的 hasFeature()方法类似， isSupported()方法用于确定当前节点具有什么能力。这个方法也接受相同的两个参数：**特性名**和**特性版本号**。如果浏览器实现了相应特性，而且能够基于给定节点执行该特性， isSupported()就返回 true。  
   
   ```javascript
   if (document.body.isSupported("HTML", "2.0")){
   //执行只有"DOM2 级 HTML"才支持的操作
   }
   ```



### 12.2	样式

要确定浏览器是否支持 DOM2 级定义的 CSS 能力，可以使用下列代码。  

```javascript
var supportsDOM2CSS = document.implementation.hasFeature("CSS", "2.0");
var supportsDOM2CSS2 = document.implementation.hasFeature("CSS2", "2.0");
```

#### 12.2.1	访问元素的样式

对于使用**短划线**（分隔不同的词汇，例如 background-image）的 CSS 属性名，必须将其转换成驼峰大小写形式，才能通过 JavaScript 来访问。  例子如下👇

| CSS属性          | JavaScript属性        |
| ---------------- | --------------------- |
| background-image | style.backgroundImage |
| color            | style.color           |
| display          | style.display         |
| fontFamily       | style.fontFamily      |

**注意**:

在**标准模式**下，所有度量值都必须指定一个度量单位。在**混杂模式**下，可以将style.width 设置为"20"，浏览器会假设它是"20px"；但在标准模式下，将style.width 设置为"20"会导致被忽略—— 因为没有度量单位。**在实践中，最好始终都指定度量单位。**

1. DOM 样式属性和方法

   > 见书P314

2. 计算的样式

   > 见书 P315



#### 12.2.2	操作样式表

**CSSStyleSheet** 类型表示的是样式表，包括通过<link>元素包含的样式表和在<style>元素中定义的样式表。 

 有读者可能记得，这两个元素本身分别是由 **HTMLLinkElement** 和 **HTMLStyleElement** 类型表示的。

但是， **CSSStyleSheet** 类型相对更加通用一些，它只表示样式表，而不管这些样式表在 HTML中是如何定义的。

此外，上述两个针对元素的类型允许修改 HTML 特性，但 CSSStyleSheet 对象则是**一套只读的接口**（有一个属性例外）。  

> 见书 P317



#### 12.2.3	元素大小

1. 偏移量

   - offsetHeight：元素在垂直方向上占用的空间大小，以像素计。包括元素的高度、（可见的）水平滚动条的高度、上边框高度和下边框高度。
   - offsetWidth：元素在水平方向上占用的空间大小，以像素计。包括元素的宽度、（可见的）垂直滚动条的宽度、左边框宽度和右边框宽度。
   - offsetLeft：元素的左外边框至包含元素的左内边框之间的像素距离。
   - offsetTop：元素的上外边框至包含元素的上内边框之间的像素距离。
   - 其中， **offsetLeft 和 offsetTop 属性与包含元素有关**，包含元素的引用保存在 offsetParent属性中。 offsetParent 属性不一定与 parentNode 的值相等。  

   ![](F:\GitHub\markDown\JavaScript高级程序设计\偏移量.png)
   
   ```javascript
   // 取得元素的左偏移量
   function getElementLeft(element){
   var actualLeft = element.offsetLeft;
   var current = element.offsetParent;
   while (current !== null){
   actualLeft += current.offsetLeft;
   current = current.offsetParent;
   }
   return actualLeft;
   }
   // 取得元素的右偏移量
   function getElementTop(element){
   var actualTop = element.offsetTop;
   var current = element.offsetParent;
   while (current !== null){
   actualTop += current. offsetTop;
   current = current.offsetParent;
   }
   return actualTop;
   }
   ```
   
   
   
2. 客户区大小

   元素的**客户区大小**（ client dimension），指的是元素内容及其内边距所占据的空间大小。有关客户区大小的属性有两个： clientWidth 和 clientHeight。

   - clientWidth 属性是元素内容区宽度加上左右内边距宽度
   - clientHeight 属性是元素内容区高度加上上下内边距高度。
   - ![](F:\GitHub\markDown\JavaScript高级程序设计\客户区.png)  

   ```javascript
   function getViewport(){
   // 首先检查 document.compatMode 属性，以确定浏览器是否运行在混杂模式。 Safari 3.1之前的版本不支持这个属性，因此就会自动执行 else 语句。 
   	if (document.compatMode == "BackCompat"){
   		return {
   			width: document.body.clientWidth,
   			height: document.body.clientHeight
   		};
   	} else {
   		return {
   		width: document.documentElement.clientWidth,
   		height: document.documentElement.clientHeight
   		};
   	}
   }
   ```

   

3. 滚动大小

   滚动大小（ scroll dimension），指的是包含滚动内容的元素的大小。有些元素（例如<html>元素），即使没有执行任何代码也能自动地添加滚动条；但另外一些元素，则需要通过 CSS 的overflow 属性进行设置才能滚动。
   
   - scrollHeight：在没有滚动条的情况下，元素内容的总高度。
   
   - scrollWidth：在没有滚动条的情况下，元素内容的总宽度。
   
   - scrollLeft：被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置。
   
   - scrollTop：被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置。
   
     scrollWidth 和 scrollHeight 主要用于确定元素内容的实际大小。例如，通常认为<html>元素是在 Web 浏览器的视口中滚动的元素（ IE6 之前版本运行在混杂模式下时是<body>元素）。因此，**带有垂直滚动条的页面总高度就是 document.documentElement.scrollHeight。**  
   
     ![](F:\GitHub\markDown\JavaScript高级程序设计\滚动大小.png)
   
   在确定文档的总高度时（包括基于视口的最小高度时），必须取得 scrollWidth/clientWidth 和scrollHeight/clientHeight 中的最大值，才能保证在跨浏览器的环境下得到精确的结果。
   
   ```javascript
   var docHeight = Math.max(document.documentElement.scrollHeight,
   document.documentElement.clientHeight);
   var docWidth = Math.max(document.documentElement.scrollWidth,
   document.documentElement.clientWidth);
   // 注意，对于运行在混杂模式下的 IE，则需要用 document.body 代替 document.documentElement
   ```
   
4. 确定元素大小

   > 见书 P325

   IE、 Firefox 3+、 Safari 4+、 Opera 9.5 及 Chrome 为每个元素都提供了一个 **getBoundingClientRect()方法**。
   
   这个方法返回会一个矩形对象，包含 4 个属性： left、 top、 right 和 bottom。这些属性给出了元素在页面中相对于视口的位置。但是，浏览器的实现稍有不同。 IE8 及更早版本认为文档的左上角坐
   标是(2, 2)，而其他浏览器包括 IE9 则将传统的(0,0)作为起点坐标。因此，就需要在一开始检查一下位于(0,0)处的元素的位置，在 IE8 及更早版本中，会返回(2,2)，而在其他浏览器中会返回(0,0)。  
   
   ```javascript
    function getBoundingClientRect(element){
   	var scrollTop = document.documentElement.scrollTop;
   	var scrollLeft = document.documentElement.scrollLeft;
   	if (element.getBoundingClientRect){
   		if (typeof arguments.callee.offset != "number"){
     			var temp = document.createElement("div");
    			temp.style.cssText = "position:absolute;left:0;top:0;";
   			document.body.appendChild(temp);
   			arguments.callee.offset = -temp.getBoundingClientRect().top - scrollTop;
   			document.body.removeChild(temp);
   		    temp = null;
     	 	}
   		var rect = element.getBoundingClientRect();
   		var offset = arguments.callee.offset;
   		return {
   			left: rect.left + offset,
   			right: rect.right + offset,
   			top: rect.top + offset,
   			bottom: rect.bottom + offset
   		};
   	} else {
   		var actualLeft = getElementLeft(element);
   		var actualTop = getElementTop(element);
   		return {
   			left: actualLeft - scrollLeft,
   			right: actualLeft + element.offsetWidth - scrollLeft,
   			top: actualTop - scrollTop,
			bottom: actualTop + element.offsetHeight - scrollTop
   		}
   	}
   }
   ```
   



### 12.3	遍历

#### 12.3.1	NodeIterator

> 见书	P328

#### 12.3.2	TreeWalker

> 见书	P330

### 12.4	范围

DOM2 级在 Document 类型中定义了 createRange()方法。在兼容 DOM 的浏览器中，这个方法属于 document 对象。使用 hasFeature()或者直接检测该方法，都可以确定浏览器是否支持范围。  

```javascript
var supportsRange = document.implementation.hasFeature("Range", "2.0");
var alsoSupportsRange = (typeof document.createRange == "function");
```

如果浏览器支持范围，那么就可以使用 createRange()来创建 DOM 范围，如下所示：  

```
var range = document.createRange();
```

> 见书	P332



## 第13章	事件

### 13.1时间流

**事件流**描述的是从页面中接收事件的顺序。

#### 13.1.1	事件冒泡

**IE 的事件流**叫做**事件冒泡**（ event bubbling），即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。  

**例子**

```html
<!DOCTYPE html>
	<html>
		<head>
			<title>Event Bubbling Example</title>
		</head>
		<body>
			<div id="myDiv">Click Me</div>
		</body>
	</html>
```

如果你单击了页面中的<div>元素，那么这个 click 事件会按照如下顺序传播:
1. < div >
2. < body >
3. < html >
4. document

也就是说， click 事件首先在<div>元素上发生，而这个元素就是我们单击的元素。然后， click事件沿 DOM 树向上传播，在每一级节点上都会发生，直至传播到 document 对象。图 13-1 展示了事件冒泡的过程。  

![](F:\GitHub\markDown\JavaScript高级程序设计\事件冒泡.png)

**所有现代浏览器都支持事件冒泡**，但在具体实现上还是有一些差别。 IE5.5 及更早版本中的事件冒泡会跳过<html>元素（从<body>直接跳到 document）。 IE9、 Firefox、 Chrome 和 Safari 则将事件一直冒泡到 window 对象。  

#### 13.1.2	事件捕获

- **事件捕获的思想**是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。

- 事件捕获的**用意**在于在事件到达预定目标之前捕获它。

仍以前面的 HTML 页面作为演示事件捕获的例子，那么单击<div>元素就会以下列顺序触发 click 事件。  

1. document
2. < html >
3. < body >
4. < div >

在事件捕获过程中， document 对象首先接收到 click 事件，然后事件沿 DOM 树依次向下，一直传播到事件的实际目标，即<div>元素。图 13-2 展示了事件捕获的过程。  

![](F:\GitHub\markDown\JavaScript高级程序设计\事件捕获.png)

虽然事件捕获是 Netscape Communicator 唯一支持的事件流模型，但 IE9、 Safari、 Chrome、 Opera和 Firefox 目前也**都支持这种事件流模型**。尽管“DOM2 级事件”规范要求事件应该从 document 对象开始传播，但这些浏览器都是从 window 对象开始捕获事件的。  

#### 13.1.3	DOM事件流

DOM2级事件”规定的事件流包括三个阶段

- 事件捕获阶段
- 处于目标阶段
- 事件冒泡阶段

首先发生的是**事件捕获**，为截获事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。以前面简单的 HTML 页面为例，单击<div>元素会按照图13-3所示顺序触发事件。  

![](F:\GitHub\markDown\JavaScript高级程序设计\DOM事件流.png)

在 DOM 事件流中，实际的目标（ <div>元素）在捕获阶段不会接收到事件。这意味着在捕获阶段，事件从 document 到<html>再到<body>后就停止了。

下一个阶段是“处于目标”阶段， 于是事件在<div>上发生，并在事件处理（后面将会讨论这个概念）中被看成冒泡阶段的一部分。

然后，冒泡阶段发生，事件又传播回文档。  

### 13.2	事件处理程序

- **事件**就是用户或浏览器自身执行的某种动作。诸如 click、 load 和 mouseover，都是事件的名字。

- 而响应某个事件的函数就叫做**事件处理程序**（或事件侦听器）。

- 事件处理程序的名字**以"on"开头**，因此click 事件的事件处理程序就是 onclick， load 事件的事件处理程序就是 onload。  



#### 13.2.1	HTML事件处理程序

事件处理程序中的代码在执行时，有权访问全局作用域中的任何代码。  

```javascript
<script type="text/javascript">
	function showMessage(){
		alert("Hello world!");
	}
</script>
<input type="button" value="Click Me" onclick="showMessage()" />
```

> 见书 P348，了解事件对象和扩展作用域

HTML 中指定事件处理程序的**缺点**

- 存在一个时差问题。因为用户可能会在HTML 元素一出现在页面上就触发相应的事件，但当时的事件处理程序有可能尚不具备执行条件。  
- 这样扩展事件处理程序的作用域链在不同浏览器中会导致不同结果。不同 JavaScript引擎遵循的标识符解析规则略有差异，很可能会在访问非限定对象成员时出错。  

- **HTML 与 JavaScript 代码紧密耦合。**如果要更换事件处理程序，就要改动两个地方： HTML 代码和 JavaScript 代码。而这正是许多开发人员摒弃 HTML 事件处理程序，转而使用 JavaScript 指定事件处理程序的原因所在。 



#### 13.2.2	DOM0 级事件处理程序

通过 JavaScript 指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。  

每个元素（包括 window 和 document）都有自己的事件处理程序属性，这些属性通常全部小写，例如 onclick。将这种属性的值设置为一个函数，就可以指定事件处理程序。

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(){
	alert("Clicked");
};
```

使用 DOM0 级方法指定的事件处理程序被认为是**元素的方法**。因此，这时候的事件处理程序是在元素的作用域中运行；换句话说，**程序中的 this 引用当前元素**。来看一个例子。  

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(){
alert(this.id); //"myBtn"
};
```

以这种方式添加的事件处理程序会**在事件流的冒泡阶段被处理**。    

#### 13.2.3	DOM2 级事件处理程序

“DOM2 级事件” 定义了两个方法，用于处理指定和删除事件处理程序的操作： 
- addEventListener()

- removeEventListener()。

所有 DOM 节点中都包含这两个方法，并且它们都接受 3 个参数

- 要处理的事件名
- 作为事件处理程序的函数
- 一个布尔值。这个布尔值参数如果是 true，表示在捕获阶段调用事件处理程序；如果是 false，表示在冒泡阶段调用事件处理程序。  

使用 DOM2 级方法添加事件处理程序的主要好处是可以添加多个事件处理程序。  

```javascript
var btn = document.getElementById("myBtn");
btn.addEventListener("click", function(){
	alert(this.id);
}, false);
btn.addEventListener("click", function(){
	alert("Hello world!");
}, false);
```

通过 addEventListener() 添加的事件处理程序**只能使用** removeEventListener() 来移除；移除时传入的参数与添加处理程序时使用的参数相同。这也意味着通过 addEventListener()添加的**匿名函数**将**无法移除**。  

大多数情况下，**都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览器。**最好只在需要在事件到达目标之前截获它的时候将事件处理程序添加到捕获阶段。如果不是特别需要，我们不建议在事件捕获阶段注册事件处理程序。

#### 13.2.4	IE事件处理程序

IE 实现了与 DOM 中类似的两个方法

- attachEvent()
- detachEvent()

这两个方法接受相同的两个参数

- 事件处理程序名称
- 事件处理程序函数

由于 IE8 及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序**都会被添加到冒泡阶段**。  

```javascript
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
	alert("Clicked");
});
```

在 IE 中使用 attachEvent()与使用 DOM0 级方法的**主要区别**在于**事件处理程序的作用域**。在使用 DOM0 级方法的情况下，事件处理程序会**在其所属元素的作用域**内运行；在使用 attachEvent()方法的情况下，事件处理程序会在**全局作用域**中运行，因此 this 等于 window。  

```javascript
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
	alert(this === window); //true
});
```

与 addEventListener()类似， attachEvent()方法也可以用来为一个元素**添加多个事件处理程序**。来看下面的例子  

```javascript
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
	alert("Clicked");
});
btn.attachEvent("onclick", function(){
	alert("Hello world!");
});
```

这里调用了两次 attachEvent()，为同一个按钮添加了两个不同的事件处理程序。不过，与 DOM方法不同的是，**这些事件处理程序不是以添加它们的顺序执行，而是以相反的顺序被触发。**单击这个例子中的按钮，首先看到的是"Hello world!"，然后才是"Clicked"。使用 attachEvent()添加的事件可以通过 detachEvent()来移除，条件是必须提供相同的参数。
与 DOM 方法一样，这也意味着**添加的匿名函数将不能被移除**。不过，只要能够将对相同函数的引用传给 detachEvent()，就可以移除相应的事件处理程序。  



#### 13.2.5	跨浏览器的事件处理程序

为了以跨浏览器的方式处理事件，不少开发人员会**使用能够隔离浏览器差异的 JavaScript 库**，还有一些开发人员会自己**开发最合适的事件处理的方法**。  

> 见书	P354



### 13.3	事件对象

- 在触发 DOM 上的某个事件时，会产生一个**事件对象 event**，这个对象中包含着所有与事件有关的信息。
- 包括导致事件的元素、事件的类型以及其他与特定事件相关的信息。
- 例如，鼠标操作导致的事件对象中，会包含鼠标位置的信息，而键盘操作导致的事件对象中，会包含与按下的键有关的信息。**所有浏览器都支持 event 对象，但支持方式不同。**  



#### 13.3.1	DOM中的事件对象

兼容 DOM 的浏览器会将一个 **event 对象**传入到事件处理程序中。无论指定事件处理程序时使用什么方法（ DOM0 级或 DOM2 级），都会传入 event 对象。  

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
	alert(event.type); //"click"
};
btn.addEventListener("click", function(event){
	alert(event.type); //"click"
}, false);
```

![](F:\GitHub\markDown\JavaScript高级程序设计\event属性和方法1.png)

![](F:\GitHub\markDown\JavaScript高级程序设计\event属性和方法2.png)

在事件处理程序内部，**对象 this 始终等于 currentTarget 的值**，而 target 则只包含事件的实际目标。如果直接将事件处理程序指定给了目标元素，则 this、 currentTarget 和 target 包含相同的值。  

```javascript
// 第一个例子
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
	alert(event.currentTarget === this); //true
	alert(event.target === this); //true
};

// 第二个例子
document.body.onclick = function(event){
	alert(event.currentTarget === document.body); //true
	alert(this === document.body); //true
	alert(event.target === document.getElementById("myBtn")); //true
};
```

在需要通过一个函数处理多个事件时，可以使用 type 属性。  

```javascript
var btn = document.getElementById("myBtn");
var handler = function(event){
	switch(event.type){
		case "click":
			alert("Clicked");
			break;
		case "mouseover":
			event.target.style.backgroundColor = "red";
			break;
		case "mouseout":
			event.target.style.backgroundColor = "";
			break;
		}
	};
	btn.onclick = handler;
	btn.onmouseover = handler;
	btn.onmouseout = handler;
```

要阻止特定事件的默认行为，可以使用 **preventDefault()**方法。  

**注意**： 只有 cancelable 属性设置为 true 的事件，才可以使用 preventDefault()来取消其默认行为。  

```javascript
var link = document.getElementById("myLink");
link.onclick = function(event){
	event.preventDefault();
};
```

**stopPropagation()方法**用于立即停止事件在 DOM 层次中的传播，即取消进一步的事件捕获或冒泡。

例如，直接添加到一个按钮的事件处理程序可以调用 stopPropagation()，从而避免触发注册在 document.body 上面的事件处理程序，如下面的例子所示。  

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
	alert("Clicked");
	event.stopPropagation();
};
document.body.onclick = function(event){
	alert("Body clicked");
};
```

事件对象的 eventPhase 属性，可以用来确定事件当前正位于事件流的哪个阶段。
- 在捕获阶段调用的事件处理程序，那么 eventPhase 等于 1

- 如果事件处理程序处于目标对象上，则 eventPhase 等于 2

- 如果是在冒泡阶段调用的事件处理程序， eventPhase 等于 3。

**注意：**尽管“处于目标”发生在冒泡阶段，但 eventPhase 仍然一直等于 2。  

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
	alert(event.eventPhase); //2
};
document.body.addEventListener("click", function(event){
	alert(event.eventPhase); //1
}, true);
document.body.onclick = function(event){
	alert(event.eventPhase); //3
};
```

注意：只有在事件处理程序执行期间， **event** 对象才会存在；一旦事件处理程序执行完成， **event** 对象就会被销毁。



#### 13.3.2	IE的事件对象

与访问 DOM 中的 event 对象不同，要访问 IE 中的 event 对象有几种不同的方式，取决于指定事件处理程序的方法。

在使用 DOM0 级方法添加事件处理程序时， **event 对象作为 window 对象的一个**
**属性存在**。

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(){
	var event = window.event;
	alert(event.type); //"click"
};
```

  如果事件处理程序是使用 attachEvent()添加的，那么就会有一个 event 对象作为参数被传入事件处理程序函数中。

```javascript
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(event){
	alert(event.type); //"click"
});
```

