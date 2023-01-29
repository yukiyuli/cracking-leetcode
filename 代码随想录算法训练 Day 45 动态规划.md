# 代码随想录算法训练 Day 45 | 动态规划

Completed: January 29, 2023
Difficulty: Medium
Done: Yes
Question: 279. 完全平方数, 322. 零钱兑换, 70. 爬楼梯
Redo: No
Topic: Dynamic programming

## ****70. 爬楼梯****

[leetcode](https://leetcode.cn/problems/climbing-stairs/)

### 思路

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[i]：爬到有i个台阶的楼顶，有dp[i]种方法。

1. 确定递推公式

求装满背包有几种方法，递推公式一般都是dp[i] += dp[i - nums[j]];

本题呢，dp[i]有几种来源，dp[i - 1]，dp[i - 2]，dp[i - 3] 等等，即：dp[i - j]

那么递推公式为：dp[i] += dp[i - j]

1. dp数组如何初始化

既然递归公式是 dp[i] += dp[i - j]，那么dp[0] 一定为1，dp[0]是递归中一切数值的基础所在，如果dp[0]是0的话，其他数值都是0了。

下标非0的dp[i]初始化为0，因为dp[i]是靠dp[i-j]累计上来的，dp[i]本身为0这样才不会影响结果

1. 确定遍历顺序

这是背包里求排列问题，即：**1、2 步 和 2、1 步都是上三个台阶，但是这两种方法不一样！**

所以需将target放在外循环，将nums放在内循环。

每一步可以走多次，这是完全背包，内循环需要从前向后遍历。

1. 举例推导dp数组

和**[动态规划：377. 组合总和 Ⅳ](https://www.notion.so/Day-44-c85f5f15441b4773b8f64a815c8eeaaa)** 几乎是一样的

JavaScript完整代码：

```jsx
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function(n) {
    const dp = new Array(n + 1).fill(0);
    const m = 2;
    dp[0] = 1;
    for(let i = 1; i <= n; i++){
        for(let j = 1; j <= m; j++){
            if(i >= j) {
	    	dp[i] += dp[i - j];
	     }
        }
    }
    return dp[n];
};
```

## ****322. 零钱兑换****

[leetcode](https://leetcode.cn/problems/coin-change/)

### 思路

题目中说每种硬币的数量是无限的，可以看出是典型的完全背包问题。

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[j]：凑足总额为j所需钱币的最少个数为dp[j]。

1. 确定递推公式

凑足总额为j - coins[i]的最少个数为dp[j - coins[i]]，那么只需要加上一个钱币coins[i]即dp[j - coins[i]] + 1就是dp[j]（考虑coins[i]），所以dp[j] 要取所有 dp[j - coins[i]] + 1 中最小的。

递推公式：dp[j] = min(dp[j - coins[i]] + 1, dp[j]);

1. dp数组如何初始化

首先凑足总金额为0所需钱币的个数一定是0，那么dp[0] = 0;

考虑到递推公式的特性，dp[j]必须初始化为一个最大的数，否则就会在min(dp[j - coins[i]] + 1, dp[j])比较的过程中被初始值覆盖。所以下标非0的元素都是应该是最大值。

代码如下：

```jsx
    let dp = Array(amount + 1).fill(Infinity);
    dp[0] = 0;
```

1. 确定遍历顺序

本题求钱币最小个数，那么钱币有顺序和没有顺序都可以，都不影响钱币的最小个数。

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

所以本题的两个for循环的关系是：外层for循环遍历物品，内层for遍历背包或者外层for遍历背包，内层for循环遍历物品都是可以的！

本题钱币数量可以无限使用，那么是完全背包。所以遍历的内循环是正序

1. 举例推导dp数组

以输入：coins = [1, 2, 5], amount = 5为例

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2045%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%208a38fb89400d424d80661c8cb8fbd74b/Untitled.png)

JavaScript完整代码：

```jsx
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function (coins, amount) {
    let dp = Array(amount + 1).fill(Infinity);
    dp[0] = 0;
    for (let i = 0; i < coins.length; i++) { // 遍历物品
        for (let j = coins[i]; j <= amount; j++) { // 遍历背包
            if (dp[j - coins[i]] != Infinity) { // 如果dp[j - coins[i]]是初始值则跳过
                dp[j] = Math.min(dp[j - coins[i]] + 1, dp[j]);
            }
        }
    }
    if (dp[amount] === Infinity) return -1;
    return dp[amount];
};
```

## ****279. 完全平方数****

[leetcode](https://leetcode.cn/problems/perfect-squares/)

### 思路

把题目翻译一下：完全平方数就是物品（可以无限件使用），凑个正整数n就是背包，问凑满这个背包最少有多少物品？这就变成了完全背包。

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[j]：和为j的完全平方数的最少数量为dp[j]。

1. 确定递推公式

dp[j] 可以由dp[j - i * i]推出， dp[j - i * i] + 1 便可以凑成dp[j]。

此时我们要选择最小的dp[j]，所以递推公式：dp[j] = min(dp[j - i * i] + 1, dp[j]);

1. dp数组如何初始化

dp[0]表示 和为0的完全平方数的最小数量，那么dp[0]一定是0。

从递归公式dp[j] = min(dp[j - i * i] + 1, dp[j]);中可以看出每次dp[j]都要选最小的，所以非0下标的dp[j]一定要初始为最大值，这样dp[j]在递推的时候才不会被初始值覆盖。

1. 确定遍历顺序

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

本题是求最小数，所以本题外层for遍历背包，内层for遍历物品，还是外层for遍历物品，内层for遍历背包，都是可以的！

1. 举例推导dp数组

已输入n为5例，dp状态图如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2045%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%208a38fb89400d424d80661c8cb8fbd74b/Untitled%201.png)

dp[0] = 0 dp[1] = min(dp[0] + 1) = 1 dp[2] = min(dp[1] + 1) = 2 dp[3] = min(dp[2] + 1) = 3 dp[4] = min(dp[3] + 1, dp[0] + 1) = 1 dp[5] = min(dp[4] + 1, dp[1] + 1) = 2

最后的dp[n]为最终结果。

JavaScript完整代码：

```jsx
// 先遍历物品，再遍历背包
var numSquares1 = function(n) {
    let dp = new Array(n + 1).fill(Infinity)
    dp[0] = 0

    for(let i = 1; i**2 <= n; i++) {
        let val = i**2
        for(let j = val; j <= n; j++) {
            dp[j] = Math.min(dp[j], dp[j - val] + 1)
        }
    }
    return dp[n]
};

// 先遍历背包，再遍历物品
var numSquares2 = function(n) {
    let dp = new Array(n + 1).fill(Infinity)
    dp[0] = 0

    for(let i = 1; i <= n; i++) {
        for(let j = 1; j * j <= i; j++) {
            dp[i] = Math.min(dp[i - j * j] + 1, dp[i])
        }
    }

    return dp[n]
};
```