# 代码随想录算法训练 Day 43 | 动态规划

Completed: January 24, 2023
Difficulty: Medium
Done: Yes
Redo: No
Topic: Dynamic programming

## ****1049. 最后一块石头的重量 II****

[leetcode](https://leetcode.cn/problems/last-stone-weight-ii/)

### 思路

本题其实就是尽量让石头分成重量相同的两堆，相撞之后剩下的石头最小，这样就化解成01背包问题了。

本题物品的重量为stones[i]，物品的价值也为stones[i]。

对应着01背包里的物品重量weight[i]和，物品价值value[i]。

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[j]表示容量（这里说容量更形象，其实就是重量）为j的背包，最多可以背最大重量为dp[j]。

01背包中，dp[j]的含义，容量为j的背包，最多可以装的价值为 dp[j]。

相对于 01背包，本题中，石头的重量是 stones[i]，石头的价值也是 stones[i] ，可以 “最多可以装的价值为 dp[j]” == “最多可以背的重量为dp[j]”

1. 确定递推公式

01背包的递推公式为：dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);

本题则是：dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);

所以递推公式：dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);

1. dp数组如何初始化

既然 dp[j]中的j表示容量，那么最大容量（重量）就是所有石头的重量和。

因为提示中给出1 <= stones.length <= 30，1 <= stones[i] <= 100，所以最大重量就是30 * 100 。

而我们要求的target其实只是最大重量的一半，所以dp数组开到1500大小就可以了。

接下来就是始化dp[j]，因为重量都不会是负数，所以dp[j]都初始化为0就可以了，这样在递归公式dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);中dp[j]才不会初始值所覆盖。

代码如下：

```jsx
let sum = 0;
for (let i = 0; i < stones.length; i++) sum += stones[i];

let target = Math.floor(sum / 2);
let dp = Array(target + 1).fill(0);
```

1. 确定遍历顺序

在01背包问题，一维dp数组（滚动数组）中就已经说明：如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历！

所以遍历顺序为：

```jsx
for (int i = 0; i < stones.length; i++) { // 遍历物品
    for (int j = target; j >= stones[i]; j--) { // 遍历背包
        dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
    }
}
```

1. 举例推导dp数组

举例，输入：[2,4,1,1]，此时target = (2 + 4 + 1 + 1)/2 = 4 ，dp数组状态图如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2043%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%2063866f92bccb4a588a467cd928ae5391/Untitled.png)

最后dp[target]里是容量为target的背包所能背的最大重量。

那么分成两堆石头，一堆石头的总重量是dp[target]，另一堆就是sum - dp[target]。

在计算target的时候，target = sum / 2 因为是向下取整，所以sum - dp[target] 一定是大于等于dp[target]的。

那么相撞之后剩下的最小石头重量就是 (sum - dp[target]) - dp[target]。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} stones
 * @return {number}
 */
var lastStoneWeightII = function (stones) {
    let sum = 0;
    for (let i = 0; i < stones.length; i++) sum += stones[i];

    let target = Math.floor(sum / 2);
    let dp = Array(target + 1).fill(0);

    // 开始 01背包
    for (let i = 0; i < stones.length; i++) {
        for (let j = target; j >= stones[i]; j--) { // 每一个元素一定是不可重复放入，所以从大到小遍历
            dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
        }
    }
    return sum - dp[target] - dp[target];
};
```

## 4****94. 目标和****

[leetcode](https://leetcode.cn/problems/target-sum/)

### 思路

本题既然为target，那么就一定有 left组合 - right组合 = target。

left + right = sum，而sum是固定的。right = sum - left

公式来了， left - (sum - left) = target 推导出 left = (target + sum)/2 。

target是固定的，sum是固定的，left就可以求出来。

此时问题就是在集合nums中找出和为left的组合。

### ****动态规划：****

本题要如何转化为01背包问题呢？

假设加法的总和为x，那么减法对应的总和就是sum - x。

所以我们要求的是 x - (sum - x) = target

x = (target + sum) / 2

此时问题就转化为，装满容量为x的背包，有几种方法。

这里的x，就是bagSize，也就是我们后面要求的背包容量。

此时需要考虑(target + sum) / 2 向下取整有没有影响？

例如sum 是5，S是2的话其实就是无解的，所以：

```jsx
if ((target + sum) % 2 == 1) return 0; // 此时没有方案
```

同时如果S的绝对值已经大于sum，那么也是没有方案的。

```jsx
if (Math.abs(target) > sum) return 0; // 此时没有方案
```

为什么是01背包呢？

因为每个物品（题目中的1）只用一次！

这次和之前遇到的背包问题不一样了，之前都是求容量为j的背包，最多能装多少。

本题则是装满有几种方法。其实这就是一个组合问题了。

动规五部曲：

1. 确定dp数组以及下标的含义

dp[j] 表示：填满j（包括j）这么大容积的包，有dp[j]种方法。

其实也可以使用二维dp数组来求解本题，dp[i][j]：使用 下标为[0, i]的nums[i]能够凑满j（包括j）这么大容量的包，有dp[i][j]种方法。

本题统一使用一维数组进行讲解， 二维降为一维（滚动数组），其实就是上一层拷贝下来。

1. 确定递推公式

只要搞到nums[i]），凑成dp[j]就有dp[j - nums[i]] 种方法。

例如：dp[j]，j 为5，

- 已经有一个1（nums[i]） 的话，有 dp[4]种方法 凑成 容量为5的背包。
- 已经有一个2（nums[i]） 的话，有 dp[3]种方法 凑成 容量为5的背包。
- 已经有一个3（nums[i]） 的话，有 dp[2]中方法 凑成 容量为5的背包
- 已经有一个4（nums[i]） 的话，有 dp[1]中方法 凑成 容量为5的背包
- 已经有一个5 （nums[i]）的话，有 dp[0]中方法 凑成 容量为5的背包

那么凑整dp[5]有多少方法呢，也就是把 所有的 dp[j - nums[i]] 累加起来。

所以求组合类问题的公式，都是类似这种：

```jsx
dp[j] += dp[j - nums[i]]
```

1. dp数组如何初始化

从递推公式可以看出，在初始化的时候dp[0] 一定要初始化为1，因为dp[0]是在公式中一切递推结果的起源，如果dp[0]是0的话，递推结果将都是0。

如果数组[0] ，target = 0，那么 bagSize = (target + sum) / 2 = 0。 dp[0]也应该是1， 也就是说给数组里的元素 0 前面无论放加法还是减法，都是 1 种方法。

所以本题我们应该初始化 dp[0] 为 1。

 但如果是数组[0,0,0,0,0] target = 0 呢？

其实 此时最终的dp[0] = 32，也就是这五个零 子集的所有组合情况，但此dp[0]非彼dp[0]，dp[0]能算出32，其基础是因为dp[0] = 1 累加起来的。

dp[j]其他下标对应的数值也应该初始化为0，从递推公式也可以看出，dp[j]要保证是0的初始值，才能正确的由dp[j - nums[i]]推导出来。

1. 确定遍历顺序

我们讲过对于01背包问题一维dp的遍历，nums放在外循环，target在内循环，且内循环倒序。

1. 举例推导dp数组

输入：nums: [1, 1, 1, 1, 1], S: 3

bagSize = (S + sum) / 2 = (3 + 5) / 2 = 4

dp数组状态变化如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2043%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%2063866f92bccb4a588a467cd928ae5391/Untitled%201.png)

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var findTargetSumWays = function (nums, target) {
    let sum = 0;
    for (let i = 0; i < nums.length; i++) sum += nums[i];
    if (Math.abs(target) > sum) return 0; // 此时没有方案
    if ((target + sum) % 2 == 1) return 0; // 此时没有方案
    const bagSize = (target + sum) / 2;
    let dp = new Array(bagSize + 1).fill(0);
    dp[0] = 1;
    for (let i = 0; i < nums.length; i++) {
        for (let j = bagSize; j >= nums[i]; j--) {
            dp[j] += dp[j - nums[i]];
        }
    }
    return dp[bagSize];
};
```

## ****474. 一和零****

[leetcode](https://leetcode.cn/problems/ones-and-zeroes/)

### 思路

先捋清几种背包的关系。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2043%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%2063866f92bccb4a588a467cd928ae5391/Untitled%202.png)

本题会被误以为是多重背包，但其实并不是，多重背包是每个物品，数量不同的情况。

本题中strs 数组里的元素就是物品，每个物品都是一个！而m 和 n相当于是一个背包，两个维度的背包。

本题其实是01背包问题！不过这个背包有两个维度，一个是m 一个是n，而不同长度的字符串就是不同大小的待装物品。

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[i][j]：最多有i个0和j个1的strs的最大子集的大小为dp[i][j]。

1. 确定递推公式

dp[i][j] 可以由前一个strs里的字符串推导出来，strs里的字符串有zeroNum个0，oneNum个1。

dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。

然后我们在遍历的过程中，取dp[i][j]的最大值。

所以递推公式：dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);

字符串的zeroNum和oneNum相当于物品的重量（weight[i]），字符串本身的个数相当于物品的价值（value[i]）。

1. dp数组如何初始化

01背包的dp数组初始化为0就可以。因为物品价值不会是负数，初始为0，保证递推的时候dp[i][j]不会被初始值覆盖。

1. 确定遍历顺序

在[关于01背包问题（滚动数组）](https://www.notion.so/Day-42-cb0272a9d75f44d893003a6b02ab8dab)中，我们讲到了01背包为什么一定是外层for循环遍历物品，内层for循环遍历背包容量且从后向前遍历！本题物品就是strs里的字符串，背包容量就是题目描述中的m和n。

代码如下：

```jsx
for (let str of strs) { // 遍历物品
        let oneNum = 0, zeroNum = 0;
        for (let c of str) {
            if (c === '0') zeroNum++;
            else oneNum++;
        }
        for (let i = m; i >= zeroNum; i--) { // 遍历背包容量且从后向前遍历！
            for (let j = n; j >= oneNum; j--) {
                dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
            }
        }
    }
```

遍历背包容量的两层for循环先后循序没讲究，都是物品重量的一个维度，先遍历哪个都行！

1. 举例推导dp数组

以输入：["10","0001","111001","1","0"]，m = 3，n = 3为例

最后dp数组的状态如下所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2043%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%2063866f92bccb4a588a467cd928ae5391/Untitled%203.png)

JavaScript完整代码：

```jsx
/**
 * @param {string[]} strs
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var findMaxForm = function (strs, m, n) {
    const dp = Array.from(Array(m + 1), () => Array(n + 1).fill(0)); // 默认初始化0
    for (let str of strs) { // 遍历物品
        let oneNum = 0, zeroNum = 0;
        for (let c of str) {
            if (c === '0') zeroNum++;
            else oneNum++;
        }
        for (let i = m; i >= zeroNum; i--) { // 遍历背包容量且从后向前遍历！
            for (let j = n; j >= oneNum; j--) {
                dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
            }
        }
    }
    return dp[m][n];
};
```