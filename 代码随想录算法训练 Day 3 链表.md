# 代码随想录算法训练 Day 3 | 链表

Completed: December 10, 2022
Difficulty: Easy
Done: Yes
Redo: No
Topic: Linked List

## 链表理论基础

### 什么是链表？

链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null（空指针的意思）。

链表的入口节点称为链表的头结点也就是head。

如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled.png)

## ****链表的类型****

### ****单链表****

单向链表（又名单链表、线性链表）是链表的一种，其特点是链表的链接方向是单向的，对链表的访问要通过从头部开始，依序往下读取。

### ****双链表****

单链表中的指针域只能指向节点的下一个节点。

双链表：每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点。

双链表既可以向前查询也可以向后查询。

如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%201.png)

### ****循环链表****

循环链表，顾名思义，就是链表首尾相连。

循环链表可以用来解决约瑟夫环问题。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%202.png)

## ****链表的存储方式****

数组是在内存中是连续分布的，但是链表在内存中可不是连续分布的。

链表是通过指针域的指针链接在内存中各个节点。

所以链表中的节点在内存中不是连续分布的 ，而是散乱分布在内存中的某地址上，分配机制取决于操作系统的内存管理。

如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%203.png)

这个链表起始节点为2， 终止节点为7， 各个节点分布在内存的不同地址空间上，通过指针串联在一起。

## ****链表的定义****

JavaScript的定义链表节点方式，如下所示：

```jsx
class ListNode {
  val;
  next = null;
  constructor(value) {
    this.val = value;
    this.next = null;
  }

```

## **链表的操作**

### ****删除节点****

删除D节点，如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%204.png)

只要将C节点的next指针 指向E节点就可以了。

那有同学说了，D节点不是依然存留在内存里么？只不过是没有在这个链表里而已。

是这样的，所以在C++里最好是再手动释放这个D节点，释放这块内存。

其他语言例如Java、Python，就有自己的内存回收机制，就不用自己手动释放了。

### ****添加节点****

如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%205.png)

可以看出链表的增添和删除都是O(1)操作，也不会影响到其他节点。

但是要注意，要是删除第五个节点，需要从头节点查找到第四个节点通过next指针进行删除操作，查找的时间复杂度是O(n)。

## ****性能分析****

再把链表的特性和数组的特性进行一个对比，如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%206.png)

数组在定义的时候，长度就是固定的，如果想改动数组的长度，就需要重新定义一个新的数组。

链表的长度可以是不固定的，并且可以动态增删， 适合数据量不固定，频繁增删，较少查询的场景。

## ****203.移除链表元素****

[leetcode link](https://leetcode.cn/problems/remove-linked-list-elements/)

### 思路

以链表 1 4 2 4 来举例，移除元素4。

因为单链表的特殊性，只能指向下一个节点，刚刚删除的是链表的中第二个，和第四个节点。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%207.png)

如果删除的是头结点，有两种方式：

- **直接使用原来的链表来进行删除操作。**
- **设置一个虚拟头结点在进行删除操作。**

第一种方式：直接使用原来的链表来进行移除。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%208.png)

移除头结点和移除其他节点的操作是不一样的，因为链表的其他节点都是通过前一个节点来移除当前节点，而头结点没有前一个节点。

所以头结点如何移除呢，其实只要将头结点向后移动一位就可以，这样就从链表中移除了一个头结点。但是在单链表中移除头结点和移除其他节点的操作方式是不一样，在写代码时需要单独写一段逻辑来处理移除头结点的情况。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%209.png)

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%2010.png)

第二种方式：设置一个虚拟头结点。

这样原链表的所有节点就都可以按照统一的方式进行移除了。

如何设置一个虚拟头。依然还是在这个链表中，移除元素1。给链表添加一个虚拟头结点为新的头结点，此时要移除这个旧头结点元素1。移除元素1的方式还是指向下一个节点。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%2011.png)

> 注意：return头结点的时候，不要忘了dummyNode. next; ，这才是新的头结点。
> 

JavaScript代码：

方法一：直接删除

```jsx
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function(head, val) {
	//1. 该结点为头结点
	while(head!== null && head.val === val) head = head.next;
	//2. 该结点不是头结点
	let cur = head;// 方便找到要删除结点node的前一个prev，让prev指向node.next
	while(cur!== null && cur.next!== null)// 避免对空指针进行操作
	{
			if(cur.next.val === val){
				cur.next = cur.next.next;
			}
			else // 不满足val，继续指向下一个节点
			cur = cur.next;
	}
	return head// 返回操作完的头指针
};
```

方法二：虚拟头结点

```jsx
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function(head, val) {
	const dummyHead = new ListNode(0);//创建一个虚拟头结点，将dummy节点的next指向head，temp指向dummy
	dummyHead.next = head;
	let temp = dummyHead;
	while (temp.next !== null) {//当temp的next不为null 不断循环节点
		if (temp.next.val == val) {
			temp.next = temp.next.next;//当temp的next值是要删除的 则删除该节点
		} else {
			temp = temp.next;//移动temp指针
		}
	}
	return dummyHead.next;
};
```

## ****707.设计链表****

[leetcode link](https://leetcode.cn/problems/design-linked-list/)

### 思路：

所有的操作都设置一个虚拟头节点。

1. **获取第n个节点**

为什么定义一个指针来遍历链表？

因为操作完链表需要返回头节点，如果直接操作头节点，头节点的值被改，就无法返回头节点.

![IMG_0663.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/IMG_0663.jpg)

1. **插入头节点**

![IMG_0663 2.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/IMG_0663_2.jpg)

1. **插入第n个节点**

![IMG_0664.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/IMG_0664.jpg)

1. **删除第n个节点**

![IMG_0663 3.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/IMG_0663_3.jpg)

完整代码：

```jsx
class LinkNode {
    constructor(val, next = null) {
        /**节点值 */
        this.val = val;
        /**下一节点 */
        this.next = next;
    }
}

var MyLinkedList = function() {
        /**虚拟头节点 */
        this.dummyhead = new LinkNode('head');
        /**尾节点 */
        this.end = this.dummyhead;
        /**节点数 */
        this.size = 0;

};

/** 
 * @param {number} index
 * @return {number}
 */
MyLinkedList.prototype.get = function(index) {
        if (index >= this.size || index < 0) {
            return -1;
        } else if (index === this.size - 1) {
            return this.end.val;
        } else {
            let cur = this.dummyhead.next;
            while (index-- > 0) {
                cur = cur.next;
            }
            return cur.val;
        }
};

/** 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtHead = function(val) {
        const newnode=new LinkNode(val, this.dummyhead.next)
        this.dummyhead.next = newnode;
        if (this.size === 0) this.end = newnode;
        this.size++;
};

/** 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtTail = function(val) {
        //将值为 val 的节点追加到链表的最后一个元素
        this.end = this.end.next = new ListNode(val);
        this.size++;
};

/** 
 * @param {number} index 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtIndex = function(index, val) {
        // 如果 index 小于等于0，则在头部插入节点。
        if(index <= 0) {
            this.addAtHead(val);
        // 如果 index 等于链表的长度，则该节点将附加到链表的末尾。
        } else if (index === this.size) {
            this.addAtTail(val);
        } else if (index < this.size) {
            let cur = this.dummyhead;
            while (index-- > 0) {
                cur = cur.next;
            }
            cur.next = new LinkNode(val, cur.next);
            this.size++;
        }
};

/** 
 * @param {number} index
 * @return {void}
 */
MyLinkedList.prototype.deleteAtIndex = function(index) {
        if (this.size === 0 || index >= this.size || index < 0) return -1;
        let cur = this.dummyhead;
        while (index-- > 0) {
            cur = cur.next;
        }
        // 处理尾节点
        if (cur.next === this.end) this.end = cur;
        cur.next = cur.next.next;
        this.size--;
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * var obj = new MyLinkedList()
 * var param_1 = obj.get(index)
 * obj.addAtHead(val)
 * obj.addAtTail(val)
 * obj.addAtIndex(index,val)
 * obj.deleteAtIndex(index)
 */
```

## ****206.反转链表****

[leetcode link](https://leetcode.cn/problems/reverse-linked-list/)

### 思路

改变链表的next指针的指向，直接将链表反转 ，而不用重新定义一个新的链表，如图所示:

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%203%20%E9%93%BE%E8%A1%A8%20863c6e12719044cb9ae658dd34fb9529/Untitled%2012.png)

之前链表的头节点是元素1， 反转之后头结点就是元素5 ，这里并没有添加或者删除节点，仅仅是改变next指针的方向。

首先定义一个cur指针，指向头结点，再定义一个pre指针，初始化为null。

然后开始反转，首先要把 cur->next 节点用tmp指针保存一下，也就是保存一下这个节点。

为什么要保存一下这个节点呢，因为接下来要改变 cur->next 的指向了，将cur->next 指向pre ，此时已经反转了第一个节点了。

接下来，就是循环走如下代码逻辑了，继续移动pre和cur指针。

最后，cur 指针已经指向了null，循环结束，链表也反转完毕了。 此时我们return pre指针就可以了，pre指针就指向了新的头结点。

如动画所示：（纠正：动画应该是先移动pre，在移动cur）

![https://tva1.sinaimg.cn/large/008eGmZEly1gnrf1oboupg30gy0c44qp.gif](https://tva1.sinaimg.cn/large/008eGmZEly1gnrf1oboupg30gy0c44qp.gif)

JavaScript代码：

```jsx
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
if (!head || !head.next) return head;
let pre=null, cur=head;
//当cur=null时，循环结束
while(cur){
    let temp=cur.next;
    cur.next=pre;
		//一定要先移动pre，再移动cur
    pre=cur;
    cur=temp;
}
return pre;
};
```