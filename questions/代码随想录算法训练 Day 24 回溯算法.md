# 代码随想录算法训练 Day 24 | 回溯算法

Topic: Backtrace

Difficulty: Medium

Question: 第77题. 组合

- [x] Done

Completed: January 4, 2023

- [ ] Redo


## **回溯算法理论基础**

![Untitled](https://user-images.githubusercontent.com/101588752/210672840-1c5f9080-56fd-45d7-98de-d6cb567a965a.png)

### 什么是回溯法

回溯法也可以叫做回溯搜索法，它是一种搜索的方式。

回溯是递归的副产品，只要有递归就会有回溯。

### 回溯法的效率

溯的本质是穷举，穷举所有可能，然后选出我们想要的答案，如果想让回溯法高效一些，可以加一些剪枝的操作，但也改不了回溯法就是穷举的本质。

那么既然回溯法并不高效为什么还要用它呢？

因为没得选，一些问题能暴力搜出来就不错了，撑死了再剪枝一下，还没有更高效的解法。

### 回溯法解决的问题

回溯法，一般可以解决如下几种问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等

> 组合和排列的区别：                                                                                                                                                     组合是不强调元素顺序的，排列是强调元素顺序。                                                                                                   例如：{1, 2} 和 {2, 1} 在组合上，就是一个集合，因为不强调顺序，而要是排列的话，{1, 2} 和 {2, 1} 就是两个集合了。
> 

### ****如何理解回溯法****

回溯法解决的问题都可以抽象为树形结构，因为回溯法解决的都是在集合中递归查找子集，集合的大小就构成了树的宽度，递归的深度，都构成的树的深度。

递归就要有终止条件，所以必然是一棵高度有限的树（N叉树）。

### ****回溯法模板****

回溯三部曲：

- 回溯函数模板返回值以及参数

回溯算法中函数返回值一般为void。

参数，因为回溯算法需要的参数可不像二叉树递归的时候那么容易一次性确定下来，所以一般是先写逻辑，然后需要什么参数，就填什么参数。

回溯函数伪代码如下：

```jsx
const backtracking = (参数) =>
```

- 回溯函数终止条件

一般来说搜到叶子节点了，也就找到了满足条件的一条答案，把这个答案存放起来，并结束本层递归。

所以回溯函数终止条件伪代码如下：

```jsx
if (终止条件) {
    存放结果;
    return;
}
```

- 回溯搜索的遍历过程

回溯法一般是在集合中递归搜索，集合的大小构成了树的宽度，递归的深度构成的树的深度。

如图：

![Untitled 1](https://user-images.githubusercontent.com/101588752/210672870-e1315696-af13-4746-a2db-aaf3815da93c.png)

回溯函数遍历过程伪代码如下：

```jsx
for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
    处理节点;
    backtracking(路径，选择列表); // 递归
    回溯，撤销处理结果
}
```

for循环就是遍历集合区间，可以理解一个节点有多少个孩子，这个for循环就执行多少次。

backtracking这里自己调用自己，实现递归。

图中看出for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历，这样就把这棵树全遍历完了，一般来说，搜索叶子节点就是找的其中一个结果了。

分析完过程，回溯算法模板框架如下：

```jsx
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

## ****第77题. 组合****

[leetcode](https://leetcode.cn/problems/combinations/)

### 思路：

直接的解法当然是使用for循环，例如示例中k为2，很容易想到 用两个for循环，这样就可以输出 和示例中一样的结果。这个方法如果k不是很大，用几个for循环可以实现。但如果n为100，k为50呢，那就50层for循环，是不是开始窒息？此时就会发现虽然想暴力搜索，但是用for循环嵌套连暴力都写不出来！

这个时候到了回溯出场了，虽然回溯法也是暴力，但至少能写出来，不像for循环嵌套k层让人绝望。

回溯法就用递归来解决嵌套层数的问题。递归来做层叠嵌套（可以理解是开k层for循环），每一次的递归中嵌套一个for循环，那么递归就可以用于解决多层嵌套循环的问题了。

例如：n=100，k=50的情况下，就是递归50层。

这时需要把组合问题抽象为如下树形结构：

![Untitled 2](https://user-images.githubusercontent.com/101588752/210672903-0daa94d4-7e9d-4b2d-b2f7-e7dedc946acb.png)

如图所示，这棵树一开始集合是 1，2，3，4， 从左向右取数，取过的数，不再重复取。

第一次取1，集合变为2，3，4 ，因为k为2，我们只需要再取一个数就可以了，分别取2，3，4，得到集合[1,2] [1,3] [1,4]，以此类推。

每次从集合中选取元素，可选择的范围随着选择的进行而收缩，调整可选择的范围。

图中可以发现n相当于树的宽度，k相当于树的深度。

图中每次搜索到了叶子节点，我们就找到了一个结果。相当于只需要把达到叶子节点的结果收集起来，就可以求得 n个数中k个数的组合集合。

### 回溯三步曲

- 递归函数的返回值以及参数

在这里要定义两个全局变量，一个用来存放符合条件单一结果，一个用来存放符合条件结果的集合。

代码如下：

```jsx
let result = []; // 存放符合条件结果的集合
let path = []; // 用来存放符合条件结果
```

然后还需要一个参数，为int型变量startIndex，这个参数用来记录本层递归的中，集合从哪里开始遍历（集合就是[1,...,n] ）。

> 为什么要有这个startIndex呢？startIndex 就是防止出现重复的组合。
> 

从下图中红线部分可以看出，在集合[1,2,3,4]取1之后，下一层递归，就要在[2,3,4]中取数了，那么下一层递归如何知道从[2,3,4]中取数呢，靠的就是startIndex。

![Untitled 3](https://user-images.githubusercontent.com/101588752/210672919-ab05c86b-5364-48e4-b383-95f8daa684ff.png)

代码如下：

```jsx
const backtracking = (n, k, startIndex) =>
```

- 回溯函数终止条件

path这个数组的大小如果达到k，说明我们找到了一个子集大小为k的组合了，在图中path存的就是根节点到叶子节点的路径。

如图红色部分：

![Untitled 4](https://user-images.githubusercontent.com/101588752/210672935-f5863638-ff37-4092-9ac9-f30991d82ff6.png)

此时用result二维数组，把path保存起来，并终止本层递归。

代码如下：

```jsx
if (path.length === k) {
	return result.push([...path]); // 拷贝一份track，推入result 结束当前递归
}
```

- 单层搜索的过程

回溯法的搜索过程就是一个树型结构的遍历过程，在如下图中，可以看出for循环用来横向遍历，递归的过程是纵向遍历。

![Untitled 5](https://user-images.githubusercontent.com/101588752/210672948-1911df7b-b194-401a-b4c0-56d7cc9854e8.png)

for循环每次从startIndex开始遍历，然后用path保存取到的节点i。

代码如下：

```jsx
for (let i = startIndex; i <= n; i++) {
		 path.push(i); // 处理节点 
		 backtracking(n, k, i + 1); // 递归
		 path.pop(); // 回溯，撤销处理的节点
}
```

可以看出backtracking（递归函数）通过不断调用自己一直往深处遍历，总会遇到叶子节点，遇到了叶子节点就要返回。

backtracking的下面部分就是回溯的操作了，撤销本次处理的结果。

JavaScript完整代码：

```jsx
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function (n, k) {
    let result = []; // 存放符合条件结果的集合
    let path = []; // 用来存放符合条件结果
    const backtracking = (n, k, startIndex) => {
        if (path.length === k) {
            return result.push([...path]); // 拷贝一份track，推入result 结束当前递归
        }
        for (let i = startIndex; i <= n; i++) {
            path.push(i); // 处理节点 
            backtracking(n, k, i + 1); // 递归
            path.pop(); // 回溯，撤销处理的节点
        }
    }
    backtracking(n, k, 1);
    return result;
};
```

### ****剪枝优化****

思路：

举一个例子，n = 4，k = 4的话，那么第一层for循环的时候，从元素2开始的遍历都没有意义了。 在第二层for循环，从元素3开始的遍历都没有意义了。

如图所示：

![Untitled 6](https://user-images.githubusercontent.com/101588752/210672968-233fab0d-3652-49db-b66c-eac4827f2fb5.png)

图中每一个节点（图中为矩形），就代表本层的一个for循环，那么每一层的for循环从第二个数开始遍历的话，都没有意义，都是无效遍历。

所以，可以剪枝的地方就在递归中每一层的for循环所选择的起始位置。如果for循环选择的起始位置之后的元素个数 已经不足 我们需要的元素个数了，那么就没有必要搜索了。

优化过程如下：

1. 已经选择的元素个数：path.length;
2. 还需要的元素个数为: k - path.length;
3. 在集合n中至多要从该起始位置 : n - (k - path.length) + 1，开始遍历

为什么有个+1呢，因为包括起始位置，我们要是一个左闭的集合。

举个例子，n = 4，k = 3， 目前已经选取的元素为0（path.length=0），n - (k - 0) + 1 即 4 - ( 3 - 0) + 1 = 2。

从2开始搜索都是合理的，可以是组合[2, 3, 4]。

所以优化之后的for循环是：

```jsx
for (int i = startIndex; i <= n - (k - path.length) + 1; i++) // i为本次搜索的起始位置
```

优化后整体代码如下：

```jsx
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function (n, k) {
    let result = []; // 存放符合条件结果的集合
    let path = []; // 用来存放符合条件结果
    const backtracking = (n, k, startIndex) => {
        if (path.length === k) {
            return result.push([...path]); // 拷贝一份track，推入result 结束当前递归
        }
        for (let i = startIndex; i <= n - (k - path.length) + 1; i++) {
            path.push(i); // 处理节点 
            backtracking(n, k, i + 1); // 递归
            path.pop(); // 回溯，撤销处理的节点
        }
    }
    backtracking(n, k, 1);
    return result;
};
```
