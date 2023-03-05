# 代码随想录算法训练 Day 50 | 动态规划

Topic: Dynamic programming

Question: 139. word-break

Difficulty: Medium

- [x] Done

Completed: January 29, 2023

- [ ] Redo


## ****139. 单词拆分****

[leetcode](https://leetcode.cn/problems/word-break/)

### 思路

单词就是物品，字符串s就是背包，单词能否组成字符串s，就是问物品能不能把背包装满。

拆分时可以重复使用字典中的单词，说明就是一个完全背包！

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[i] : 字符串长度为i的话，dp[i]为true，表示可以拆分为一个或多个在字典中出现的单词。

2. 确定递推公式

如果确定dp[j] 是true，且 [j, i] 这个区间的子串出现在字典里，那么dp[i]一定是true。（j < i ）。

所以递推公式是 if([j, i] 这个区间的子串出现在字典里 && dp[j]是true) 那么 dp[i] = true。

3. dp数组如何初始化

从递推公式中可以看出，dp[i] 的状态依靠 dp[j]是否为true，那么dp[0]就是递推的根基，dp[0]一定要为true，否则递推下去后面都都是false了。

下标非0的dp[i]初始化为false，只要没有被覆盖说明都是不可拆分为一个或多个在字典中出现的单词。

4. 确定遍历顺序

题目中说是拆分为一个或多个在字典中出现的单词，所以这是完全背包。

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

题其求的是排列数。 拿 s = "applepenapple", wordDict = ["apple", "pen"] 举例。

"apple", "pen" 是物品，那么我们要求物品的组合一定是 "apple" + "pen" + "apple" 才能组成 "applepenapple"。

"apple" + "apple" + "pen" 或者 "pen" + "apple" + "apple" 是不可以的，那么我们就是强调物品之间顺序。

所以说，本题一定是 先遍历 背包，再遍历物品。

5. 举例推导dp数组

以输入: s = "leetcode", wordDict = ["leet", "code"]为例，dp状态如图：

![Untitled](https://user-images.githubusercontent.com/101588752/215352836-b28efbb3-a98b-4fe3-bee6-626f0d9f55c5.png)

JavaScript完整代码：

```jsx
/**
 * @param {string} s
 * @param {string[]} wordDict
 * @return {boolean}
 */
var wordBreak = function(s, wordDict) {
    let dp = Array(s.length + 1).fill(false);
    dp[0] = true;
        for (let i = 1; i <= s.length; i++) {   // 遍历背包
            for (let j = 0; j < i; j++) {       // 遍历物品
                const word = s.slice(j, i); 
                if (wordDict.includes(word) && dp[j] === true) {
                    dp[i] = true;
                }
            }
        }
        return dp[s.length];
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

2. 确定递推公式

凑足总额为j - coins[i]的最少个数为dp[j - coins[i]]，那么只需要加上一个钱币coins[i]即dp[j - coins[i]] + 1就是dp[j]（考虑coins[i]），所以dp[j] 要取所有 dp[j - coins[i]] + 1 中最小的。

递推公式：dp[j] = min(dp[j - coins[i]] + 1, dp[j]);

3. dp数组如何初始化

首先凑足总金额为0所需钱币的个数一定是0，那么dp[0] = 0;

考虑到递推公式的特性，dp[j]必须初始化为一个最大的数，否则就会在min(dp[j - coins[i]] + 1, dp[j])比较的过程中被初始值覆盖。所以下标非0的元素都是应该是最大值。

代码如下：

```jsx
    let dp = Array(amount + 1).fill(Infinity);
    dp[0] = 0;
```

4. 确定遍历顺序

本题求钱币最小个数，那么钱币有顺序和没有顺序都可以，都不影响钱币的最小个数。

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

所以本题的两个for循环的关系是：外层for循环遍历物品，内层for遍历背包或者外层for遍历背包，内层for循环遍历物品都是可以的！

本题钱币数量可以无限使用，那么是完全背包。所以遍历的内循环是正序

5. 举例推导dp数组

以输入：coins = [1, 2, 5], amount = 5为例

![Untitled 1](https://user-images.githubusercontent.com/101588752/215352897-37c12a88-9b24-4d02-a02b-2573b89bf21d.png)

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

## ****多重背包****

有N种物品和一个容量为V 的背包。第i种物品最多有Mi件可用，每件耗费的空间是Ci ，价值是Wi 。求解将哪些物品装入背包可使这些物品的耗费的空间 总和不超过背包容量，且价值总和最大。

多重背包和01背包是非常像的， 为什么和01背包像呢？

每件物品最多有Mi件可用，把Mi件摊开，其实就是一个01背包问题了。

例如：

背包最大重量为10。

物品为：

|  | 重量 | 价值 | 数量 |
| --- | --- | --- | --- |
| 物品0 | 1 | 15 | 2 |
| 物品1 | 3 | 20 | 3 |
| 物品2 | 4 | 30 | 2 |

问背包能背的物品最大价值是多少？

和如下情况有区别么？

|  | 重量 | 价值 | 数量 |
| --- | --- | --- | --- |
| 物品0 | 1 | 15 | 1 |
| 物品0 | 1 | 15 | 1 |
| 物品1 | 3 | 20 | 1 |
| 物品1 | 3 | 20 | 1 |
| 物品1 | 3 | 20 | 1 |
| 物品2 | 4 | 30 | 1 |
| 物品2 | 4 | 30 | 1 |

毫无区别，这就转成了一个01背包问题了，且每个物品只用一次。

这种方式来实现多重背包的代码如下：

```jsx
function testMultiPack() {
  const bagSize = 10;
  const weightArr[] = [1, 3, 4],
    valueArr[] = [15, 20, 30],
    amountArr[] = [2, 3, 2];
  for (let i = 0, length = amountArr.length; i < length; i++) {
    while (amountArr[i] > 1) {
      weightArr.push(weightArr[i]);
      valueArr.push(valueArr[i]);
      amountArr[i]--;
    }
  }
  const goodsNum = weightArr.length;
  const dp[] = new Array(bagSize + 1).fill(0);
  // 遍历物品
  for (let i = 0; i < goodsNum; i++) {
    // 遍历背包容量
    for (let j = bagSize; j >= weightArr[i]; j--) {
      dp[j] = Math.max(dp[j], dp[j - weightArr[i]] + valueArr[i]);
    }
  }
  console.log(dp);
}
testMultiPack();
```

也有另一种实现方式，就是把每种商品遍历的个数放在01背包里面在遍历一遍。

代码如下：

```jsx
function testMultiPack() {
  const bagSize = 10;
  const weightArr[] = [1, 3, 4],
    valueArr[] = [15, 20, 30],
    amountArr[] = [2, 3, 2];
  const goodsNum = weightArr.length;
  const dp[] = new Array(bagSize + 1).fill(0);
  // 遍历物品
  for (let i = 0; i < goodsNum; i++) {
    // 遍历物品个数
    for (let j = 0; j < amountArr[i]; j++) {
      // 遍历背包容量
      for (let k = bagSize; k >= weightArr[i]; k--) {
        dp[k] = Math.max(dp[k], dp[k - weightArr[i]] + valueArr[i]);
      }
    }
  }
  console.log(dp);
}
testMultiPack();
```
