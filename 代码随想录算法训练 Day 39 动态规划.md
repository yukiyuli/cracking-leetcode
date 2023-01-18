# 代码随想录算法训练 Day 39 | 动态规划

Topic: Dynamic programming

Question: 509. 斐波那契数, 70. 爬楼梯, 746. 使用最小花费爬楼梯

Difficulty: Easy

- [x] Done

Completed: January 18, 2023

- [ ] Redo


## ****动态规划理论基础****

![Untitled](https://user-images.githubusercontent.com/101588752/213281513-449ddb6d-63a4-4078-bc22-3513a2688c57.png)


### ****什么是动态规划****

动态规划，英文：Dynamic Programming，简称DP，如果某一问题有很多重叠子问题，使用动态规划是最有效的。

所以动态规划中每一个状态一定是由上一个状态推导出来的，这一点就区分于贪心，贪心没有状态推导，而是从局部直接选最优的。

举一个背包问题的例子。

例如：有N件物品和一个最多能背重量为W 的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。每件物品只能用一次，求解将哪些物品装入背包里物品价值总和最大。

动态规划中dp[j]是由dp[j-weight[i]]推导出来的，然后取max(dp[j], dp[j - weight[i]] + value[i])。

但如果是贪心呢，每次拿物品选一个最大的或者最小的就完事了，和上一个状态没有关系。所以贪心解决不了动态规划的问题。

对于刷题来说，只要知道动规是由前一个状态推导出来的，而贪心是局部直接选最优就足够了。

### ****动态规划的解题步骤****

对于动态规划问题，可以拆解为五步曲：

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

### ****动态规划应该如何debug****

做动规的题目，写代码之前一定要把状态转移在dp数组的上具体情况模拟一遍，心中有数，确定最后推出的是想要的结果。

然后再写代码，如果代码没通过就打印dp数组，看看是不是和自己预先推导的哪里不一样。

如果打印出来和自己预先模拟推导是一样的，那么就是自己的递归公式、初始化或者遍历顺序有问题了。

如果和自己预先模拟推导的不一样，那么就是代码实现细节有问题。

## ****509. 斐波那契数****

[leetcode](https://leetcode.cn/problems/fibonacci-number/)

### ****动态规划：****

动规五部曲：

这里我们要用一个一维dp数组来保存递归的结果。

1. 确定dp数组以及下标的含义

dp[i]的定义为：第i个数的斐波那契数值是dp[i]；

1. 确定递推公式

题目已经把递推公式直接给我们了：状态转移方程 dp[i] = dp[i - 1] + dp[i - 2];

1. dp数组如何初始化

题目中把如何初始化也直接给我们了，如下**：**

```jsx
dp[0] = 0;
dp[1] = 1;
```

1. 确定遍历顺序

从递归公式dp[i] = dp[i - 1] + dp[i - 2];中可以看出，dp[i]是依赖 dp[i - 1] 和 dp[i - 2]，那么遍历的顺序一定是从前到后遍历的；

1. 举例推导dp数组

按照这个递推公式dp[i] = dp[i - 1] + dp[i - 2]，推导一下，当N为10的时候，dp数组应该是如下的数列：

0 1 1 2 3 5 8 13 21 34 55

如果代码写出来，发现结果不对，就把dp数组打印出来看看和我们推导的数列是不是一致的。

本题只依赖前两个元素的结果，所以只要两个变量代替dp数组记录状态过程，不需要记录整个序列。，将空间复杂度降到O(1)。

JavaScript完整代码：

```jsx
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
    if (n <= 1) return n;
    const dp = [];
    dp[0] = 0;
    dp[1] = 1;
    for (let i = 2; i <= n; i++) {
        let sum = dp[0] + dp[1];
        dp[0] = dp[1];
        dp[1] = sum;
    }
    return dp[1];
};
```

### ****递归解法****

JavaScript完整代码：

```jsx
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
    if (n < 2) return n;
    return fib(n - 1) + fib(n - 2);
};
```

## ****70. 爬楼梯****

[leetcode](https://leetcode.cn/problems/climbing-stairs/)

### 思路

动规五部曲：

定义一个一维数组来记录不同楼层的状态

1. 确定dp数组以及下标的含义

dp[i]： 爬到第i层楼梯，有dp[i]种方法

1. 确定递推公式

dp[i] 可以有两个方向推出来。

首先是dp[i - 1]，上i-1层楼梯，有dp[i - 1]种方法，那么再一步跳一个台阶不就是dp[i]了么。

还有就是dp[i - 2]，上i-2层楼梯，有dp[i - 2]种方法，那么再一步跳两个台阶不就是dp[i]了么。

那么dp[i]就是 dp[i - 1]与dp[i - 2]之和！

所以dp[i] = dp[i - 1] + dp[i - 2] 。

1. dp数组如何初始化

在回顾一下dp[i]的定义：爬到第i层楼梯，有dp[i]中方法。

那么i为0，dp[i]应该是多少呢，从dp数组定义的角度上来说，dp[0] = 0 也能说得通。需要注意的是：题目中说了n是一个正整数，题目根本就没说n有为0的情况。所以本题其实就不应该讨论dp[0]的初始化！

所以初始化应该是dp[1] = 1，dp[2] = 2，然后从i = 3开始递推，这样才符合dp[i]的定义。

1. 确定遍历顺序

从递推公式dp[i] = dp[i - 1] + dp[i - 2];中可以看出，遍历顺序一定是从前向后遍历的

1. 举例推导dp数组

举例当n为5的时候，dp table（dp数组）应该是这样的：

![Untitled 1](https://user-images.githubusercontent.com/101588752/213281555-bc0671e6-1d36-4349-bf85-7328bb44938e.png)

JavaScript完整代码：

```jsx
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function(n) {
    if (n <= 1) return n;
    const dp = [];
    dp[1] = 1;
    dp[2] = 2;
    for (let i = 3; i <= n; i++) {
        let sum = dp[1] + dp[2];
        dp[1] = dp[2];
        dp[2] = sum;
    }
    return dp[2];
};
```

## ****746. 使用最小花费爬楼梯****

[leetcode](https://leetcode.cn/problems/min-cost-climbing-stairs/)

### 思路

动规五部曲：

1. 确定dp数组以及下标的含义

本题只需要一个一维数组dp[i]就可以了。

dp[i]的定义：到达第i台阶所花费的最少体力为dp[i]。

2. 确定递推公式

可以有两个途径得到dp[i]，一个是dp[i-1] 一个是dp[i-2]。

dp[i - 1] 跳到 dp[i] 需要花费 dp[i - 1] + cost[i - 1]。

dp[i - 2] 跳到 dp[i] 需要花费 dp[i - 2] + cost[i - 2]。

那么究竟是选从dp[i - 1]跳还是从dp[i - 2]跳呢？

一定是选最小的，所以dp[i] = Math. min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);

3. dp数组如何初始化

看一下递归公式，dp[i]由dp[i - 1]，dp[i - 2]推出，那么只初始化dp[0]和dp[1]就够了，其他的最终都是dp[0]dp[1]推出。

题目描述中明确说了 “你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。” 也就是说 从 到达 第 0 个台阶是不花费的，但从 第0 个台阶 往上跳的话，需要花费 cost[0]。

所以初始化 dp[0] = 0，dp[1] = 0;

4. 确定遍历顺序

dp[i]由dp[i-1]dp[i-2]推出，所以是从前到后遍历cost数组就可以了。

> 但是稍稍有点难度的动态规划，其遍历顺序并不容易确定下来。 例如：01背包，都知道两个for循环，一个for遍历物品嵌套一个for遍历背包容量，那么为什么不是一个for遍历背包容量嵌套一个for遍历物品呢？ 以及在使用一维dp数组的时候遍历背包容量为什么要倒序呢？
> 
5. 举例推导dp数组

拿示例2：cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1] ，来模拟一下dp数组的状态变化，如下：

![https://code-thinking-1253855093.file.myqcloud.com/pics/20221026175104.png](https://code-thinking-1253855093.file.myqcloud.com/pics/20221026175104.png)

优化代码：因为dp[i]就是由前两位推出来的，那么也不用dp数组了。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} cost
 * @return {number}
 */
var minCostClimbingStairs = function (cost) {
    let dp0 = 0;
    let dp1 = 0;
    for (let i = 2; i <= cost.length; i++) {
        let dpi = Math.min(dp1 + cost[i - 1], dp0 + cost[i - 2]);
        dp0 = dp1; // 记录一下前两位
        dp1 = dpi;
    }
    return dp1;
};
```
