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