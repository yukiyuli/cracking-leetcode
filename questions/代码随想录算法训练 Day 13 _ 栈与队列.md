# 代码随想录算法训练 Day 13 | 栈与队列

Topic: Queue, Stack

Question: 239. 滑动窗口最大值, 347. 前 K 个高频元素

Difficulty: Hard

- [x] Done

Completed: December 25, 2022

- [ ] Redo


## ****239. 滑动窗口最大值****

[leetcode link](https://leetcode.cn/problems/sliding-window-maximum/)

### 思路

维护单调递减队列，当进入滑动窗口的元素大于等于队尾的元素时，不断从队尾出队，直到进入滑动窗口的元素小于队尾的元素，才可以入队，以保证单调递减的性质，当队头元素已经在滑动窗口外了，移除对头元素，当i大于等于k-1的时候，单调递减队头就是滑动窗口的最大值。

设计单调队列的时候，pop和push操作要保持如下规则：

1. pop(value)：如果窗口移除的元素value等于单调队列的出口元素，那么队列弹出元素，否则不用任何操作。
2. push(value)：如果push的元素value大于入口元素的数值，那么就将队列入口的元素弹出，直到push元素的数值小于等于队列入口元素的数值为止。

动画如下：

![https://code-thinking.cdn.bcebos.com/gifs/239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC-2.gif](https://code-thinking.cdn.bcebos.com/gifs/239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC-2.gif)

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function(nums, k) {
    const queue = [];
    if (!nums.length) return [];
    for (let i = 0; i < k; i++) {
        // 循环比较当前元素和queue队尾对应的元素大小，若当前元素大，将队尾元素出队
        while (queue.length && nums[i] >= nums[queue[queue.length - 1]]) {
            queue.pop();
        }
        queue.push(i)
    }
    // 这时候queue的队头元素对应的值，肯定是初始窗口中最大的
    const res = [nums[queue[0]]];
    for (let i = k; i < nums.length; i++) {
        // 窗口开始滑动，做之前相同的操作
        while (queue.length && nums[i] >= nums[queue[queue.length - 1]]) {
            queue.pop();
        }
        queue.push(i)
     // 窗口滑动，将不应该在窗口中的元素弹出
     while (queue[0] <= i - k) queue.shift();
     // 这时候queue的队头元素对应的值，肯定是当前窗口中最大的
     res.push(nums[queue[0]]);
    }
    return res;
};
```

## ****347. 前 K 个高频元素****

[leetcode link](https://leetcode.cn/problems/top-k-frequent-elements/)

### 思路

用map统计元素出现的频率，key存储元素，value存储频率，然后对频率进行排序。这里可以使用一种 容器适配器就是**优先级队列**。

> 什么是优先级队列呢？                                                                                                                            其实**就是一个披着队列外衣的堆**，因为优先级队列对外接口只是从队头取元素，从队尾添加元素，再无其他取元素的方式，看起来就是一个队列。                                                                                  而且优先级队列内部元素是自动依照元素的权值排列。那么它是如何有序排列的呢？                    缺省情况下priority_queue利用max-heap（大顶堆）完成对元素的排序，这个大顶堆是以vector为表现形式的complete binary tree（完全二叉树）。
> 

使用小顶堆得出k个最大元素，因为要统计最大前k个元素，只有小顶堆每次将最小的元素弹出，最后小顶堆里积累的才是前k个最大元素。

寻找前k个最大元素流程如图所示：（图中的频率只有三个，所以正好构成一个大小为3的小顶堆，如果频率更多一些，则用这个小顶堆进行扫描）

![Untitled](https://user-images.githubusercontent.com/101588752/209517344-66da77ca-f863-4121-9d75-8e42083827c6.png)

具体步骤如下：

- 遍历数据，统计每个元素的频率，并将元素值（ key ）与出现的频率（ value ）保存到 map 中。
- 遍历 map ，将前 k 个数，构造一个小顶堆。
- 从 k 位开始，继续遍历 map ，每一个数据出现频率都和小顶堆的堆顶元素出现频率进行比较，如果小于堆顶元素，则不做任何处理，继续遍历下一元素；如果大于堆顶元素，则将这个元素替换掉堆顶元素，然后再堆化成一个小顶堆。
- 遍历完成后，堆中的数据就是前 k 大的数据。

> 什么是堆？
> 
> 
> **堆是一棵完全二叉树，树中每个结点的值都不小于（或不大于）其左右孩子的值。** 如果父亲结点是大于等于左右孩子就是大顶堆，小于等于左右孩子就是小顶堆。
> 
> 所以大家经常说的大顶堆（堆头是最大元素），小顶堆（堆头是最小元素），如果懒得自己实现的话，就直接用priority_queue（优先级队列）就可以了，底层实现都是一样的，从小到大排就是小顶堆，从大到小排就是大顶堆
> 

> 为什么是使用小顶堆，而不是大顶堆？                                                                                                 定义一个大小为k的大顶堆，在每次移动更新大顶堆的时候，每次弹出都把最大的元素弹出去了，那么怎么保留下来前K个高频元素呢。而且使用大顶堆就要把所有元素都进行排序，其实只要排序k个元素就可以了。
> 

JavaScript完整代码：

```
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var topKFrequent = function(nums, k) {
    let map = new Map(), heap = [,]
    nums.map((num) => {
        if(map.has(num)) map.set(num, map.get(num)+1)
        else map.set(num, 1)
    })
    
    // 如果元素数量小于等于 k
    if(map.size <= k) {
        return [...map.keys()]
    }
    
    // 如果元素数量大于 k，遍历map，构建小顶堆
    let i = 0
    map.forEach((value, key) => {
        if(i < k) {
            // 取前k个建堆, 插入堆
            heap.push(key)
            // 原地建立前 k 堆
            if(i === k-1) buildHeap(heap, map, k)
        } else if(map.get(heap[1]) < value) {
            // 替换并堆化
            heap[1] = key
            // 自上而下式堆化第一个元素
            heapify(heap, map, k, 1)
        }
        i++
    })
    // 删除heap中第一个元素
    heap.shift()
    return heap
};

// 原地建堆，从后往前，自上而下式建小顶堆
let buildHeap = (heap, map, k) => {
    if(k === 1) return
    // 从最后一个非叶子节点开始，自上而下式堆化
    for(let i = Math.floor(k/2); i>=1 ; i--) {
        heapify(heap, map, k, i)
    }
}

// 堆化
let heapify = (heap, map, k, i) => {
    // 自上而下式堆化
    while(true) {
        let minIndex = i
        if(2*i <= k && map.get(heap[2*i]) < map.get(heap[i])) {
            minIndex = 2*i
        }
        if(2*i+1 <= k && map.get(heap[2*i+1]) < map.get(heap[minIndex])) {
            minIndex = 2*i+1
        }
        if(minIndex !== i) {
            swap(heap, i, minIndex)
            i = minIndex
        } else {
            break
        }
    }
}

// 交换
let swap = (arr, i , j) => {
    let temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}
```

## 总结

### ****栈经典题目****

### ****括号匹配问题****

在[20. 有效括号](https://www.notion.so/Day-11-f9fb703b23e942b9a751a8daa5677240)是使用栈解决的经典问题。

有三种不匹配的情况，

1. 第一种情况，字符串里左方向的括号多余了 ，所以不匹配。
2. 第二种情况，括号没有多余，但是 括号的类型没有匹配上。
3. 第三种情况，字符串里右方向的括号多余了，所以不匹配。

> 还有一些技巧，在匹配左括号的时候，右括号先入栈，就只需要比较当前元素和栈顶相不相等就可以了，比左括号先入栈代码实现要简单的多了！
> 

### ****字符串去重问题****

在[1047. 删除字符串中的所有相邻重复项](https://www.notion.so/Day-11-f9fb703b23e942b9a751a8daa5677240)中讲解了字符串去重问题。

思路就是可以把字符串顺序放到一个栈中，然后如果相同的话 栈就弹出，这样最后栈里剩下的元素都是相邻不相同的元素了。

### ****逆波兰表达式问题****

在[150. 逆波兰表达式求值](https://www.notion.so/Day-11-f9fb703b23e942b9a751a8daa5677240)中讲解了求逆波兰表达式。

本题中每一个子表达式要得出一个结果，然后拿这个结果再进行运算。

### **队列的经典题目**

### ****滑动窗口最大值问题****

在[239. 滑动窗口最大值](https://www.notion.so/Day-13-2a741b19ae104eafa6f49e8c10916ada)中讲解了一种数据结构：单调队列。

主要思想是队列没有必要维护窗口里的所有元素，只需要维护有可能成为窗口里最大值的元素就可以了，同时保证队列里的元素数值是由大到小的。

那么这个维护元素单调递减的队列就叫做单调队列，即单调递减或单调递增的队列。

设计单调队列的时候，pop，和push操作要保持如下规则：

1. pop(value)：如果窗口移除的元素value等于单调队列的出口元素，那么队列弹出元素，否则不用任何操作
2. push(value)：如果push的元素value大于入口元素的数值，那么就将队列出口的元素弹出，直到push元素的数值小于等于队列入口元素的数值为止

保持如上规则，每次窗口移动的时候，只要问que.front()就可以返回当前窗口的最大值。

要明确的是，题解中单调队列里的pop和push接口，仅适用于本题。

单调队列不是一成不变的，而是不同场景不同写法，总之要保证队列里单调递减或递增的原则，所以叫做单调队列。

### ****求前 K 个高频元素****

在[347. 前 K 个高频元素](https://www.notion.so/Day-13-2a741b19ae104eafa6f49e8c10916ada)中讲解了求前 K 个高频元素。

通过求前 K 个高频元素，引出另一种队列就是**优先级队列**。

本题就要使用优先级队列来对部分频率进行排序。注意这里是对部分数据进行排序而不需要对所有数据排序！
