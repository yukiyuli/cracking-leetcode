# 代码随想录算法训练 Day 4 | 链表

Completed: December 12, 2022
Difficulty: Medium
Done: Yes
Redo: No
Topic: Linked List

## ****24. 两两交换链表中的节点****

[leetcode link](https://leetcode.cn/problems/swap-nodes-in-pairs/)

### 思路

老规矩，还是使用虚拟头节点。并让cur指向虚拟头节点。

```jsx
    let dummyHead=new ListNode(0,head);
    let cur=dummyHead;
```

接下来交换相邻两个元素，注意顺序，并定义两个临时变量，存储节点。

![IMG_0667.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0667.jpg)

什么时候终止遍历？

```jsx
// 偶数
cur.next=null

// 奇数
cur,next.next=null
```

![IMG_0667 2.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0667_2.jpg)

一定要先写偶数链表，不然会出现空指针。

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
var swapPairs = function(head) {
    let dummyHead=new ListNode(0,head);
    let cur=dummyHead;
  //当链表是偶数时，cur.next=null遍历结束，一定要先偶数，不然会出现空指针
	//当链表是奇数时，cur,next.next=null遍历结束
    while(cur.next && cur.next.next){
        let temp=cur.next; // 记录临时节点
        let temp1=cur.next.next.next; // 记录临时节点
        cur.next=cur.next.next; // 步骤一
        cur.next.next=temp; // 步骤二
        cur.next.next.next=temp1; // 步骤三
        cur=cur.next.next; // cur向后移动两位，准备下一轮交换
    }
    return dummyHead.next;
};
```

## ****19. 删除链表的倒数第N个节点****

[leetcode link](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

### 思路：

- 老规矩，先设置一个虚拟头节点。
- 定义两个快慢指针，fast，slow都指向虚拟头结点，如图：
    
    ![IMG_0669.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0669.jpg)
    
- fast首先走n + 1步，因为只有这样同时移动的时候slow才能指向删除节点的上一个节点（方便做删除操作），如图：
    
    ![IMG_0669 2.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0669_2.jpg)
    
- fast和slow同时移动，直到fast指向末尾，slow指针必须指向要删除节点的前一个节点，如图：

![IMG_0669 3.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0669_3.jpg)

完整代码：

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
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let dummyHead=new ListNode(0, head),
    slow=fast=dummyHead;
    while(n--&& fast!==null){
        fast=fast.next;
    }
    //fast要再走一步，因为需要让slow指向删除节点的上一个节点
    fast=fast.next;
    while(fast !==null){
        fast=fast.next;
        slow=slow.next;
    }
    //slow指向删除节点的上一个节点
    slow.next=slow.next.next;
    return dummyHead.next;
};
```

## ****面试题 02.07. 链表相交****

[leetcode link](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

### 思路

这里计算的是两个链表首个交点节点的指针，交点比较的是指针相同而不是数值相同。

1. 定义两个指针，curA指向链表A的头部，curB指向链表B的头部。
    
    ![IMG_0672.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0672.jpg)
    
2. 求出两个链表的长度，并求出两个链表长度的差值，然后让curA移动到，和curB 末尾对齐的位置。
    
    ![IMG_0672 2.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0672_2.jpg)
    
3. 比较curA和curB是否相同，如果不相同，同时向后移动curA和curB，如果遇到curA == curB，则找到交点。否则循环退出返回空指针。

JavaScript代码：

```jsx
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function(headA, headB) {
    let len = 0, cur = head;
    while(cur) {
       len++;
       cur = cur.next;
    }
    return len;
}

var getIntersectionNode = function(headA, headB) {
    let curA = headA,curB = headB,
    lenA = getListLen(headA),   // 求链表A的长度
    lenB = getListLen(headB);   // 求链表B的长度
    if(lenA < lenB) {       // 让curA为最长链表的头，lenA为其长度
    
        // 交换变量注意加 “分号” ，两个数组交换变量在同一个作用域下时
        // 如果不加分号，下面两条代码等同于一条代码: [curA, curB] = [lenB, lenA]
        
        [curA, curB] = [curB, curA];
        [lenA, lenB] = [lenB, lenA];
    }
    let i = lenA - lenB;   // 求长度差
    while(i-- > 0) {       // 让curA和curB在同一起点上（末尾位置对齐）
        curA = curA.next;
    }
    while(curA && curA !== curB) {  // 遍历curA 和 curB，遇到相同则直接返回
        curA = curA.next;
        curB = curB.next;
    }
    return curA;
};
```

## ****142. 环形链表II****

[leetcode link](https://leetcode.cn/problems/linked-list-cycle-ii/)

### 1. 判断链表是否有环

可以使用快慢指针法，分别定义 fast 和 slow 指针，从头结点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，如果 fast 和 slow指针在途中相遇 ，说明这个链表有环。

> 为什么fast 走两个节点，slow走一个节点，有环的话，一定会在环内相遇呢，而不是永远的错开呢？           首先：**fast指针一定先进入环中，如果fast指针和slow指针相遇的话，一定是在环中相遇。**                     其次：fast是走两步，slow是走一步，**其实相对于slow来说，fast是一个节点一个节点的靠近slow的**
，所以fast一定可以和slow重合。
> 

动画如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1goo4xglk9yg30fs0b6u0x.gif](https://tva1.sinaimg.cn/large/008eGmZEly1goo4xglk9yg30fs0b6u0x.gif)

### 2. ****找到这个环的入口****

假设从头结点到环形入口节点 的节点数为x。 环形入口节点到 fast指针与slow指针相遇节点 节点数为y。 从相遇节点 再到环形入口节点节点数为 z。 如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/Untitled.png)

相遇时slow指针走过的节点数为: `x + y`， fast指针走过的节点数：`x + y + n (y + z)`，n为fast指针在环内走了n圈才遇到slow指针， （y+z）为 一圈内节点的个数A。

因为fast指针是一步走两个节点，slow指针一步走一个节点， 所以 fast指针走过的节点数 = slow指针走过的节点数 * 2：`(x + y) * 2 = x + y + n (y + z)，`两边消掉一个（x+y）: `x + y = n (y + z)`

因为要找环形的入口，那么要求的是x，因为x表示 头结点到 环形入口节点的的距离。

所以要求x ，将x单独放在左面：`x = n (y + z) - y` ,

再从n(y+z)中提出一个 （y+z）来，整理公式之后为如下公式：`x = (n - 1) (y + z) + z` 注意这里n一定是大于等于1的，因为 fast指针至少要多走一圈才能相遇slow指针。

这个公式说明什么呢？

先拿n为1的情况来举例，意味着fast指针在环形里转了一圈之后，就遇到了 slow指针了。

当 n为1的时候，公式就化解为 `x = z`，

这就意味着，**从头结点出发一个指针，从相遇节点也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是环形入口的节点**。

也就是在相遇节点处，定义一个指针index1，在头结点处定一个指针index2。

让index1和index2同时移动，每次移动一个节点， 那么他们相遇的地方就是 环形入口的节点。

动画如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1goo58gauidg30fw0bi4qr.gif](https://tva1.sinaimg.cn/large/008eGmZEly1goo58gauidg30fw0bi4qr.gif)

> 疑问：slow为什么在第一圈的时候被快指针追上？
> 

![IMG_0674.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0674.jpg)

JavaScript代码：

```jsx
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var detectCycle = function(head) {
		// 先判断是否是环形链表
    if(!head || !head.next) return null;
    let slow =head.next, fast = head.next.next;
    while(fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if(fast == slow) {
            slow = head;
            while (fast !== slow) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        }
    }
    return null;
};
```

## 链表总结

### 移除链表元素

使用虚拟头节点，从虚拟头节点开始便利，删除每一个节点是让指针指向要删的节点的前一个节点，让指针指向后一个节点，并删除当前节点。当便利结束，返回虚拟头节点的后一个节点。

### 设计链表

使用虚拟头节点，在初始化链表的时候设置虚拟头节点。

获取表的index个节点，应该取到正对的那个节点，因此cur应该设置为head而不是dummyHead，并且由于index从0开始，所以index不能>size-1

在链表第index个节点前插入和删除都需要便利到目标节点的前一个节点，因此cur应该从dummyHead开始。而插入一个节点index可以是size，因为实际会取到最后一个节点并且在最后一个节点后边插入。删除节点index不能是size，因为没有size这个节点。

### 反转链表

使用双指针法。

首先定义一个cur指针，指向头结点，再定义一个pre指针，初始化为null。

然后开始反转，首先要把 cur->next 节点用temp指针保存一下，也就是保存一下这个节点。将cur.next指向pre，此时已经反转了第一个节点。然后pre=cur, cur=temp。

### ****两两交换链表中的节点****

使用虚拟头节点。并让cur指向虚拟头节点。接下来交换相邻两个元素，并定义两个临时变量，存储节点。

![IMG_0667.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/IMG_0667.jpg)

> 注意：                                                                                                                                                                 1. 交换顺序                                                                                                                                                                     2. 终止遍历情况一定要先偶数链表，不然会出现空指针
> 

### ****删除链表的倒数第N个节点****

使用虚拟头节点。定义两个快慢指针，fast，slow都指向虚拟头结点。fast首先走n + 1步，这样同时移动的时候slow才能指向删除节点的上一个节点（方便做删除操作）。fast和slow同时移动，直到fast指向末尾。

> 注意：slow指针必须指向要删除节点的前一个节点
> 

### ****链表相交****

这里计算的是两个链表首个交点节点的指针，交点比较的是指针相同而不是数值相同。

定义两个指针分别指向两个链表的头部。比较两个链表的长度，并求出两个链表长度的差值，让两个链表尾部对齐。两个指针同时向后遍历，直到找到两个指针相同的节点并返回。否则循环退出返回空指针。

### ****环形链表II****

1. 先判断是否有环。

使用快慢指针法，分别定义 fast 和 slow 指针，从头结点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，如果 fast 和 slow指针在途中相遇 ，说明这个链表有环。

1. 判断环的入口

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%204%20%E9%93%BE%E8%A1%A8%20a3602bc0283d4452b670d1749ace3e86/Untitled.png)

相遇时slow指针走过的节点数为: `x + y`， fast指针走过的节点数：`x + y + n (y + z)`，因为fast指针是一步走两个节点，slow指针一步走一个节点， 所以 fast指针走过的节点数是slow指针走2倍。假设n=1，公式就化解为 `x = z`，也就是一个指针从头走，一个从交点走，两个指针第一次相遇点为入口。