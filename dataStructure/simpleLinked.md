> ***维基百科***

- **链表**是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针。
- 链表中最简单的一种是**单向链表**，它包含两个域，一个信息域和一个指针域。这个链接指向列表中的下一个节点，而最后一个节点则指向一个空值。
- 一个单向链表包含两个值: *当前节点的值和一个指向下一个节点的链接*。
- 一般查找一个节点的时候需要从第一个节点开始每次访问下一个节点，一直访问到需要的位置。
![2017031911565346.png][1]
   


----------
  从上面可以得知：
- 单链表的每一个节点里面有一个信息域（element）和一个指向下一个节点的指针（next）。
- 查找一个节点是从第一个节点（head）找起。

那先创建节点的类吧：

```
class Node {
  constructor (element) { //传入数据
    this.element = element;
    this.next = null; //next是指向下一个节点的指针。
  }
}
```
接着创建单链表的类：

```
class SingleLinkedList {
  constructor () {
    this.head = null; // head指向第一个节点的指针。
    this.length = 0;
  }
｝
```
**给单链表增加一些方法**（以下的方法都是写在单链表类里面的）

- **Append** (加入一个节点)

```
/**
   * @param element 一个数据，可以是任何的数据类型.
   */
append (element) {
    let node = new Node(element), // 实例化一个节点。
        current;                 

    if (!this.head) {             // 如果为空则为空链表
      this.head = node;           // 链表头（head）指向第一个节点node。
    } else { // 不是空链表
      current = this.head;       // current也指向了第一个节点（用来从头部开始进行操作，且为了不改变head的指向）
      while (current.next) {     // 循环，直到某个节点的next为null
        current = current.next;  // 如果当前节点（current）的next不为null，那么current.next这个指针就给了current。
      }
      current.next = node;       //current.next为null退出循环（即已到了最后一个节点）,那么它的next为node
    }
    this.length++;               //加入节点后，要把单链表长度加一。
    console.log('Append successfully');
}
```
- **InsertNode**（在某位置加入一个节点）

```
/**
   * @param element 一个数据，可以是任何的数据类型.
   * @param {Number} position 要插入的某个位置. 
   */
  insertNode (element, position) {
    if (position >= 0 && position <= this.length) { // 判断插入的位置。
      let node = new Node(element),
        current = this.head,          // current是指向第一个借点咯。
        previous,                     // 指向前一个节点的指针。
        index = 0;

      if (position === 0) {
        node.next = current;         //此时head的指向的节点，变为了node指向的节点
        this.head = node;            // 而head当然指向node咯
      } else {
        while (index++ < position) { // index是否是要插入的位置，不是就+1。
          previous = current;        // 不是当前位置，那么current这个指针就交给previous。
          current = current.next;    // 这个跟append方法一样的。
        }                            // 是当前位置啦，就退出循环。
        previous.next = node;        // 前一个节点的next指向node（插入的节点）
        node.next = current;         // node.next就是current啦。
      }
      this.length++;
      console.log('Insert successfully');
    } else {
      throw new Error('这个单链表中不能从这个位置加入节点');
    }
  }
```
- **RemoveNode** （删除一个节点）

```
/** 
   * @param {Number} position 要删除节点的位置
   */
  removeNode (position) {
    if (position > -1 && position < this.length) { //判断是否存在这position。
      let current = this.head,         // 同上面一样，用current来循环。
        previous,
        index = 0;

      if (position === 0) {
        this.head = current.next;     //head变为指向下一个节点。
      } else {
        while (index++ < position) {  //判断是否为当前的位置。
          previous = current;         // current就变为前一个节点 （指针变化）。
          current = current.next;     
        }                             // 确定要删除节点的位置，退出循环。
        previous.next = current.next; // 当前节点为current，那么移除它，这时前一个的节点就和后一个节点连在一起咯。
      }
      this.length--;                  // 长度减一。
      console.log('Remove successfully');
    } else {
      throw new Error('单链表没这个节点哦。');
    }
}
```
- **Print** (输出单链表的每个节点)

```
print () {
    let arr = [this.head];  // 我用了数组包住了每个节点。
    let node = this.head;
    while (node.next) {     // 下一个节点是否存在。
      node = node.next;
      arr.push(node);
    }
    arr.map( (x, index) => {
      console.log(`第${index + 1}个节点是:`);
      console.log(x);
    });
}
```
- **运行结果：**
![图片描述][2]


----------
[个人源码地址][3]

单链表就写到这里咯，接下来还会写其他的数据结构（本人也在学习当中）。第一次写文章，有错漏之处，希望指出。代码也有可以精简的地方，不过这样我觉让人看得明白些（大神轻喷）。不过也总算踏出了写文章的第一步，还是很开心的。^_^


  [1]: https://sfault-image.b0.upaiyun.com/774/028/774028031-5a240dd42c0f5
  [2]: https://sfault-image.b0.upaiyun.com/230/145/2301458111-5a24354b8425a
  [3]: https://github.com/bepromising/-dataStructure/blob/master/linkedList/singleLinkedList.js