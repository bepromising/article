> ***维基百科***
- 双向链表也叫双链表，是链表的一种，它的每个数据结点中都有两个指针，分别指向直接后继和直接前驱。所以，从双向链表中的任意一个结点开始，都可以很方便地访问它的前驱结点和后继结点。一般我们都构造双向循环链表。
![图片描述][1]


----------
又上面可知，双向链表与单链表的区别在于，双向链表的每个节点都有一个指向**前一个的指针**（previous）。
想了解单链表，可以看看我上一遍写的**[单链表][2]**。


1.先创建节点的类。

```
class Node {
  constructor (element) {
    this.elememt = element;
    this.previous = null;  // 这个是指向前一个的指针。
    this.next = null;
  }
}
```
2.再创建双向链表的类。

```
class DoubleLinkedList {
  constructor () {
    this.head = null;
    this.length = 0;
  }
}
```
接下来给双向联表添加一些方法（一下方法都是在双向链表类里面写的）。
- **Append**

```
/**
   * 
   * @param element 用来实例节点的数据,什么数据类型都行. 
   */
  append (element) {
    let node = new Node(element), current;

    if (!this.length) {                 // 长度为0，则是新的链表。
      this.head = node;                 // 链表的头指向新的node。
    } else {
      current = this.head;              // current获得指向第一个节点的指针。

      while (current.next) {           // 当前node.next存在。
        current = current.next;        // 如果当前节点（current）的next不为null，那么current.next这个指针就给了current。
      }                                // current的下一个为null的时候，退出循环。
      current.next = node;             // current.next为null退出循环（即已到了最后一个节点）,那么它的next为node
      node.previous = current;         // 新加入的node的前一个就是current。
    }
    this.length++;                     // 长度加一，别忘咯。
    console.log('Append successfully!');
}
```
- **InsertNode**

```
/**
   * @param element
   * @param {Number} position 要插入的某个位置. 
   */
  insertNode (element, position) {
    if (position >= 0 && position <= this.length) {
    
      let node = new Node(element),
        front,
        index = 0,
        current = this.head;

      if (!position) {                 // 如果插入的位置为 0 。
        this.head = node;
        node.next = current;           // node的后一个是之前head所指的，即是current。
      } else {
      
        while (index++ < position) {
          front = current;             // 当前变为前一个。
          current = current.next;      // 下一个就变为当前
        }
        
        front.next = current.previous = node; // 前一个的next 和 当前的previous 是node。
        node.previous = front;         // 插入的node的前一个为front。
        node.next = current;           // 插入的node的后一个是current。
      }
      
      this.length++;
      console.log('Insert successfully!');
    } else {
      throw new Error('插入的位置有误啊!');
    }
}
```
- **RemoveNode**

```
/**
   * @param {Number} position 
   */
  removeNode (position) {
    if (position > -1 && position < this.length) {
    
      let current = this.head, front, index = 0;

      if (!position) {                           // 位置为 0 。
        this.head = current.next;
        current.next.previous = this.previous;
      } else {
      
        while (index++ < position) {
          front = current;
          current = current.next;
        }
        
        if (current.next) {                      // 这里判断当前node的下一个是否为 null。（例如要删除最后一个是node.next是null的）
          current.next.previous = front;         // 当前node的下一个的previous为front。（有点绕口）
        }
        
        front.next = current.next;               // 前一个的下一个为current的下一个。 (...绕口)
      }
      this.length--;
      console.log('Remove successfully!');
    } else {
      throw new Error('移除的位置有误啊！');
    }
}
```
- **Print**

```
print () {
    let arr = [this.head];
    let node = this.head;
    
    while (node.next) {
      node = node.next;
      arr.push(node);
    }
    
    arr.map( (x, index) => console.log(`第${index + 1}个节点是`, x));
}
```
![图片描述][3]

附上[个人源码][4]
----------

> ***循环链表 ----（维基百科）***
- **循环链表**是一种链式存储结构，它的最后一个结点指向头结点，形成一个环。因此，从循环链表中的任何一个结点出发都能找到任何其他结点。循环链表的操作和单链表的操作基本一致，差别仅仅在于算法中的循环条件有所不同。

**1.单向循环链表**
![图片描述][5]


**2.双向循环链表**
![图片描述][6]


- 单向循环链表是在单链表基础上，将**最后一个节点的next指针指向链表头（head）**。
- 双向循环链表是在双向链表基础上，将**最后一个节点的next指针指向链表头（head）**。


----------
就写到这里咯。你问我怎么不实现循环链表？这个就...你们实现吧，程序猿嘛，要多动手，多动手。
谢谢大家。


  [1]: https://sfault-image.b0.upaiyun.com/273/761/2737617547-5a2543f712e86
  [2]: https://segmentfault.com/a/1190000012265521
  [3]: https://sfault-image.b0.upaiyun.com/182/889/1828890815-5a254b0598334
  [4]: https://github.com/bepromising/-dataStructure/blob/master/linkedList/doubleLinkedList.js
  [5]: https://sfault-image.b0.upaiyun.com/409/664/4096649838-5a254d267aba3
  [6]: https://sfault-image.b0.upaiyun.com/723/482/723482499-5a254d765966c