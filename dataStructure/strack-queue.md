> ***维基百科***
- **堆栈**（英语：stack）又称为栈，是计算机科学中一种特殊的串列形式的抽象资料型别，其特殊之处在于只能允许在链接串列或阵列的一端（称为堆叠顶端指标，英语：top）进行加入数据（英语：push）和输出数据（英语：pop）的运算。另外栈也可以用**一维数组**或**连结串列的形式**来完成。
- 特点：先入后出，后入先出。 除头尾节点之外，每个元素有一个前驱，一个后继。
![图片描述][1]


从上面可知，有两种形式，数组形式和链表的形式。
1. 如果是数组（**Array**）的形式，那就很简单啦。压栈就是push，出栈就是pop。
2. 链表形式的，每个元素都有一个指向前的指针和一个指向后的指针......等等，那这不就是双向链表吗（[双向链表][2]），那栈顶就是链表的尾，栈底就是链表的头（head）咯。

- 下面是我以单链表形式写的栈。 （[这是我写的单链表文章][3]）

```
class Node {
  constructor (element) {
    this.element = element;
    this.next = null;
  }
}

class LinkedStack {
  constructor () {
    this.top = null;                   // 栈顶指针
    this.bottom = null;                // 栈底指针
    this.length = 0;
  }

  push (element) {
    let node = new Node(element);

    if (!this.top) {                  // 栈顶为空
      this.top = this.bottom = node;  // 栈顶为node
    } else {
      let front = this.top;
      this.top = front.next = node;   // 前一个的next 和 栈顶 都指向 node
    }
    this.length++;
    console.log('压栈成功啦！');
  }

  pop () {
    let node = this.bottom, front;
    if (!node) {                      // 栈底为空？ 那就是空栈咯。
      console.log('null stack');
    } else {
      while (node.next) {             // 当node.next为null，退出循环
        front = node;                 // 当node.next不为null，那么front指向这个node
        node = front.next;            // node 重新指向 front的下一个
      }
      this.top = front;               //node的next为null时，说明node为栈顶了，那么栈顶要指向前一个front
      front.next = null;              // front.next就为null了，不能指向node了，因为node出栈咯。
    }
    this.length--;
    console.log('出栈成功！');
  }

  print () {
    let arr = [this.bottom];
    let node = this.bottom;
    while (node.next) {
      node = node.next;
      arr.push(node);
    }
    arr.map( (x, index) => {
      console.log(`第${index + 1}个节点是:`);
      console.log(x);
    });
  }
}
```

[这是单链表形式的栈的源码地址][4] 。


----------

> ***维基百科***
- **队列**，又称为伫列（queue），是先进先出（FIFO, First-In-First-Out）的线性表。在具体应用中通常用链表或者数组来实现。队列只允许在后端（称为rear）进行插入操作，在前端（称为front）进行删除操作。
![图片描述][5]


有上面可知：
1. 队列也有链表式和数组形式。
2. 队列的特点是，从队尾入队，在队头出队，即**先进先出**。
数组形式就要数组（Array）的api就行了。链表式可以看看前几遍文章。这里就附上队列的[源码][6]


  [1]: https://sfault-image.b0.upaiyun.com/263/982/2639824088-5a268e5b7b10c
  [2]: https://segmentfault.com/a/1190000012281295
  [3]: https://segmentfault.com/a/1190000012265521
  [4]: https://github.com/bepromising/-dataStructure/blob/master/stack/linkedStack.js
  [5]: https://sfault-image.b0.upaiyun.com/299/779/299779926-5a269343045e5
  [6]: https://github.com/bepromising/-dataStructure/blob/master/queue/Queue.js