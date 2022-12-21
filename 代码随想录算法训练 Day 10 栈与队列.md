# 代码随想录算法训练 Day 10 | 栈与队列

Topic: Queue, Stack

Question: 225. 用队列实现栈, 232.用栈实现队列

Difficulty: Easy

- [x] Done

- [ ] Redo

Completed: December 20, 2022




## ****一、栈****

> 栈是一种具有 **「后入先出」(Last-in-First-Out，LIFO)** 特点的抽象数据结构。
> 

![Untitled](https://user-images.githubusercontent.com/101588752/208820533-2543b7f2-c084-419a-accf-efc098c58035.png)

由于栈后入先出的特点，每次只能操作栈顶的元素，任何不在栈顶的元素，都无法访问。要访问下面的元素，先得拿掉上面的元素。所以它是一种高效的数据结构。

### **栈实现**

栈通常需要实现下面常用功能：

- push（插入新元素，并让新元素成为栈顶元素）
- pop（栈顶元素出栈，并返回栈顶元素）
- peek（想知道栈最后添加的是哪个，用这个方法。返回栈顶元素，不出栈。是个辅助方法）
- clear（清空栈）
- isEmpty（若栈为空，返回 true，否则返回 false）
- size（返回栈元素个数）

### ****内存区域****

栈也是存放数据的一种内存区域。程序运行的时候，需要内存空间存放数据。一般来说，系统会划分出两种不同的内存空间：一种叫做stack（栈），另一种叫做heap（堆）。

它们的主要区别是：stack是有结构的，每个区块按照一定次序存放，可以明确知道每个区块的大小；heap是没有结构的，数据可以任意存放。因此，stack的寻址速度要快于heap。

其他的区别还有，一般来说，每个线程分配一个stack，每个进程分配一个heap，也就是说，stack是线程独占的，heap是线程共用的。此外，stack创建的时候，大小是确定的，数据超过这个大小，就发生stack overflow错误，而heap的大小是不确定的，需要的话可以不断增加。

根据上面这些区别，数据存放的规则是：只要是局部的、占用空间确定的数据，一般都存放在stack里面，否则就放在heap里面。

### **队列（Queue）**

队列 是一个先进先出 的数据结构，排在前面的事件，优先被主线程读取。主线程的读取过程基本上是自动的，只要执行栈一清空，"任务队列"上第一位的事件就自动进入主线程

### **栈实现**

列队通常需要实现下面常用功能：：

- enqueue(element):向队列尾部添加一个（或是多个）元素。
- dequeue():移除队列的第一个元素，并返回被移除的元素。
- front():返回队列的第一个元素——最先被添加的也是最先被移除的元素。队列不做任何变动。
- isEmpty():检查队列内是否有元素，如果有返回true，没有返回false。
- size():返回队列的长度。
- print():打印队列的元素。

## ****232.用栈实现队列****

[leetcode link](https://leetcode.cn/problems/implement-queue-using-stacks/)

### 思路：

用两个栈**一个输入栈，一个输出栈**来实现列队。

下面动画模拟以下队列的执行过程：

执行语句：

queue.push(1);

queue.push(2);

queue.pop(); **注意此时的输出栈的操作**

queue.push(3);

queue.push(4);

queue.pop();

queue.pop();**注意此时的输出栈的操作**

queue.pop();

queue.empty();

![https://code-thinking.cdn.bcebos.com/gifs/232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97%E7%89%88%E6%9C%AC2.gif](https://code-thinking.cdn.bcebos.com/gifs/232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97%E7%89%88%E6%9C%AC2.gif)

在push数据的时候，只要数据放进输入栈就好，**但在pop的时候，操作就复杂一些，输出栈如果为空，就把进栈数据全部导入进来（注意是全部导入）**，再从出栈弹出数据，如果输出栈不为空，则直接从出栈弹出数据就可以了。

最后，**如果进栈和出栈都为空的话，说明模拟的队列为空了。**

JavaScript完整代码：

```jsx
var MyQueue = function() {
  //准备两个栈
   this.stackIn = [];
   this.stackOut = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function(x) {
    //push的时候加入输入栈
    this.stackIn.push(x);

};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function() {
    const size = this.stackOut.length;
    //push的时候判断输出栈是否为空
    if(size) {
       return this.stackOut.pop();//不为空则输出栈出栈
    }
    //输出栈为空，则把输入栈所有的元素加入输出栈
    while(this.stackIn.length) {
       this.stackOut.push(this.stackIn.pop());
   }
   return this.stackOut.pop();

};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function() {
    //查看队头的元素 复用pop方法，然后在让元素push进输出栈
    const x = this.pop();
    this.stackOut.push(x);
    return x;
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function() {
    return !this.stackIn.length && !this.stackOut.length
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * var obj = new MyQueue()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.peek()
 * var param_4 = obj.empty()
 */
```

## ****225. 用队列实现栈****

[leetcode link](https://leetcode.cn/problems/implement-stack-using-queues/)

### 思路

本题其实用一个列队就可以实现栈。入栈的时候直接push进队列就行，出栈的时候将除了最后一个元素外的元素全部加入到队尾。

如何知道先弹出那些元素？其实弹出的元素就是size-1，要把最后一个元素前的所有元素弹出，然后重复加入到列队最后，然后就可以把最后一个元素弹出了。

```
var MyStack = function() {
    this.queue = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyStack.prototype.push = function(x) {
     this.queue.push(x);
};

/**
 * @return {number}
 */
MyStack.prototype.pop = function() {
    //将除了最后一个元素的所有元素弹出，再加入到列队
    let size = this.queue.length;
    while(size-- > 1) {
        this.queue.push(this.queue.shift());
    }
    return this.queue.shift();
};

/**
 * @return {number}
 */
MyStack.prototype.top = function() {
    const x = this.pop();
    this.queue.push(x);
    return x;
};

/**
 * @return {boolean}
 */
MyStack.prototype.empty = function() {
    return !this.queue.length;
};

/**
 * Your MyStack object will be instantiated and called as such:
 * var obj = new MyStack()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.empty()
 */
```
