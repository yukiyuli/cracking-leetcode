# 代码随想录算法训练 Day 44 | 动态规划

Topic: Dynamic programming

Question: 377. combination-sum Ⅳ, 518. coin-change II

Difficulty: Medium

- [x] Done

Completed: January 25, 2023

- [ ] Redo


## ****完全背包****

有N件物品和一个最多能背重量为W的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。每件物品都有无限个（也就是可以放入背包多次），求解将哪些物品装入背包里物品价值总和最大。

完全背包和01背包问题唯一不同的地方就是，每种物品有无限件。

同样leetcode上没有纯完全背包问题，都是需要完全背包的各种应用，需要转化成完全背包问题，所以这里还是以纯完全背包问题进行讲解理论和原理。

讲解使用以下例子：

背包最大重量为4。

物品为：

| 重量 | 价值 |
| --- | --- |
| 物品0 | 1 |
| 物品1 | 3 |
| 物品2 | 4 |

**每件商品都有无限个！**

问背包能背的物品最大价值是多少？

01背包和完全背包唯一不同就是体现在遍历顺序上，所以就不去做动规五部曲了，直接针对遍历顺序经行分析！

首先在回顾一下01背包的核心代码

```jsx
for(let i = 0; i < weight.length; i++) { // 遍历物品
    for(let j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

我们知道01背包内嵌的循环是从大到小遍历，为了保证每个物品仅被添加一次。

而完全背包的物品是可以添加多次的，所以要从小到大去遍历，即：

```jsx
// 先遍历物品，再遍历背包
for(let i = 0; i < weight.length; i++) { // 遍历物品
    for(let j = weight[i]; j <= bagWeight ; j++) { // 遍历背包容量
        dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

dp状态图如下：

![Untitled](https://user-images.githubusercontent.com/101588752/214669228-6071af20-f9ea-4598-b8a5-0e33ea114a52.png)

这里还有一个很重要的问题，为什么遍历物品在外层循环，遍历背包容量在内层循环？

在完全背包中，对于一维dp数组来说，其实两个for循环嵌套顺序是无所谓的！

因为dp[j] 是根据 下标j之前所对应的dp[j]计算出来的。 只要保证下标j之前的dp[j]都是经过计算的就可以了。

遍历物品在外层循环，遍历背包容量在内层循环，状态如图：

![Untitled 1](https://user-images.githubusercontent.com/101588752/214669269-29d8352a-d908-4221-92fc-9794f97af9e4.png)

遍历背包容量在外层循环，遍历物品在内层循环，状态如图：

![Untitled 2](https://user-images.githubusercontent.com/101588752/214669298-f2212a81-ad76-4096-a779-7fe836bb58ef.png)

看了这两个图，就会理解，完全背包中，两个for循环的先后循序，都不影响计算dp[j]所需要的值（这个值就是下标j之前所对应的dp[j]）。

先遍历背包在遍历物品，代码如下：

```jsx
// 先遍历背包，再遍历物品
for(let j = 0; j <= bagWeight; j++) { // 遍历背包容量
    for(let i = 0; i < weight.length; i++) { // 遍历物品
        if (j - weight[i] >= 0) dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
    }

```

JavaScript完整代码：

```jsx
// 先遍历物品，再遍历背包容量
function test_completePack1() {
    let weight = [1, 3, 5]
    let value = [15, 20, 30]
    let bagWeight = 4
    let dp = new Array(bagWeight + 1).fill(0)
    for(let i = 0; i <= weight.length; i++) {
        for(let j = weight[i]; j <= bagWeight; j++) {
            dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i])
        }
    }
    console.log(dp)
}

// 先遍历背包容量，再遍历物品
function test_completePack2() {
    let weight = [1, 3, 5]
    let value = [15, 20, 30]
    let bagWeight = 4
    let dp = new Array(bagWeight + 1).fill(0)
    for(let j = 0; j <= bagWeight; j++) {
        for(let i = 0; i < weight.length; i++) {
            if (j >= weight[i]) {
                dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i])
            }
        }
    }
    console.log(2, dp);
}
```

## ****518. 零钱兑换 II****

[leetcode](https://leetcode.cn/problems/coin-change-ii/)

### 思路

本题和纯完全背包不一样，纯完全背包是凑成背包最大价值是多少，而本题是要求凑成总金额的物品组合个数！

本题求的是组合，组合不强调元素之间的顺序，排列强调元素之间的顺序。

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[j]：凑成总金额j的货币组合数为dp[j]。

1. 确定递推公式

dp[j] 就是所有的dp[j - coins[i]]（考虑coins[i]的情况）相加。

所以递推公式：dp[j] += dp[j - coins[i]];

1. dp数组如何初始化

首先dp[0]一定要为1，dp[0] = 1是 递归公式的基础。如果dp[0] = 0 的话，后面所有推导出来的值都是0了。

下标非0的dp[j]初始化为0，这样累计加dp[j - coins[i]]的时候才不会影响真正的dp[j]

dp[0]=1还说明了一种情况：如果正好选了coins[i]后，也就是j-coins[i] == 0的情况表示这个硬币刚好能选，此时dp[0]为1表示只选coins[i]存在这样的一种选法。

1. 确定遍历顺序

纯完全背包求得装满背包的最大价值是多少，和凑成总和的元素有没有顺序没关系，即：有顺序也行，没有顺序也行！

而本题要求凑成总和的组合数，元素之间明确要求没有顺序。

所以纯完全背包是能凑成总和就行，不用管怎么凑的。

本题是求凑出来的方案个数，且每个方案个数是为组合数。

那么本题，两个for循环的先后顺序可就有说法了。

先来看 外层for循环遍历物品（钱币），内层for遍历背包（金钱总额）的情况。

代码如下：

```jsx
for (let i = 0; i < coins.length; i++) { // 遍历物品
    for (let j = coins[i]; j <= amount; j++) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}
```

假设：coins[0] = 1，coins[1] = 5。

那么就是先把1加入计算，然后再把5加入计算，得到的方法数量只有{1, 5}这种情况。而不会出现{5, 1}的情况。

所以这种遍历顺序中dp[j]里计算的是组合数！

如果把两个for交换顺序，代码如下：

```jsx
for (let j = 0; j <= amount; j++) { // 遍历背包容量
    for (let i = 0; i < coins.length; i++) { // 遍历物品
        if (j - coins[i] >= 0) dp[j] += dp[j - coins[i]];
    }
}
```

背包容量的每一个值，都是经过 1 和 5 的计算，包含了{1, 5} 和 {5, 1}两种情况。

此时dp[j]里算出来的就是排列数！

1. 举例推导dp数组

输入: amount = 5, coins = [1, 2, 5] ，dp状态图如下：最后红色框dp[amount]为最终结果。

![Untitled 3](https://user-images.githubusercontent.com/101588752/214669439-80426835-81e0-4665-bc63-03937893ee62.png)

JavaScript完整代码：

```jsx
/**
 * @param {number} amount
 * @param {number[]} coins
 * @return {number}
 */
var change = function (amount, coins) {
    let dp = Array(amount + 1).fill(0);
    dp[0] = 1;
    for (let i = 0; i < coins.length; i++) { // 遍历物品
        for (let j = coins[i]; j <= amount; j++) { // 遍历背包
            dp[j] += dp[j - coins[i]];
        }
    }
    return dp[amount];
};
```

## ****377.  组合总和 Ⅳ****

[leetcode](https://leetcode.cn/problems/combination-sum-iv/)

### 思路

本题求的其实是排列。

组合不强调顺序，(1,5)和(5,1)是同一个组合。

排列强调顺序，(1,5)和(5,1)是两个不同的排列。

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[i]: 凑成目标正整数为i的排列个数为dp[i]。

1. 确定递推公式

dp[i]（考虑nums[j]）可以由 dp[i - nums[j]]（不考虑nums[j]） 推导出来。

因为只要得到nums[j]，排列个数dp[i - nums[j]]，就是dp[i]的一部分。

求装满背包有几种方法，递推公式一般都是dp[i] += dp[i - nums[j]];

1. dp数组如何初始化

因为递推公式dp[i] += dp[i - nums[j]]的缘故，dp[0]要初始化为1，这样递归其他dp[i]的时候才会有数值基础。

至于非0下标的dp[i]始化为0，这样才不会影响dp[i]累加所有的dp[i - nums[j]]。

1. 确定遍历顺序

个数可以不限使用，说明这是一个完全背包。

得到的集合是排列，说明需要考虑元素之间的顺序。

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

如果把遍历nums（物品）放在外循环，遍历target的作为内循环的话，举一个例子：计算dp[4]的时候，结果集只有 {1,3} 这样的集合，不会有{3,1}这样的集合，因为nums遍历放在外层，3只能出现在1后面！

所以本题遍历顺序最终遍历顺序：target（背包）放在外循环，将nums（物品）放在内循环，内循环从前到后遍历。

1. 举例推导dp数组

我们再来用示例中的例子推导一下：

![Untitled 4](https://user-images.githubusercontent.com/101588752/214669521-2dfb803f-7005-4e91-a0d9-e697b88ac31d.png)

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var combinationSum4 = function (nums, target) {
    let dp = Array(target + 1).fill(0);
    dp[0] = 1;

    for (let i = 0; i <= target; i++) {  // 遍历背包
        for (let j = 0; j < nums.length; j++) {  // 遍历物品
            if (i >= nums[j]) {
                dp[i] += dp[i - nums[j]];
            }
        }
    }

    return dp[target];
};
```
