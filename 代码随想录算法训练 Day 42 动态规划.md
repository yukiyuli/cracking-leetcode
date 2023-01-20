# 代码随想录算法训练 Day 42 | 动态规划

Completed: January 20, 2023
Difficulty: Medium
Done: Yes
Question: 416. 分割等和子集
Redo: No
Topic: Dynamic programming

## ****01 背包****

有n件物品和一个最多能背重量为w的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。每件物品只能用一次，求解将哪些物品装入背包里物品价值总和最大。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled.png)

举一个例子：

背包最大重量为4。

物品为：

|  | 重量 | 价值 |
| --- | --- | --- |
| 物品0 | 1 | 15 |
| 物品1 | 3 | 20 |
| 物品2 | 4 | 30 |

问背包能背的物品最大价值是多少？

## ****二维dp数组01背包****

动规五部曲

1. 确定dp数组以及下标的含义

使用二维数组，即dp[i][j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少
。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%201.png)

1. 确定递推公式

可以有两个方向推出来dp[i][j]，

- **不放物品i**：由dp[i - 1][j]推出，即背包容量为j，里面不放物品i的最大价值，此时dp[i][j]就是dp[i - 1][j]。(其实就是当物品i的重量大于背包j的重量时，物品i无法放进背包中，所以被背包内的价值依然和前面相同。)
- **放物品i**：由dp[i - 1][j - weight[i]]推出，dp[i - 1][j - weight[i]] 为背包容量为j - weight[i]的时候不放物品i的最大价值，那么dp[i - 1][j - weight[i]] + value[i] （物品i的价值），就是背包放物品i得到的最大价值

所以递归公式： dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

1. dp数组如何初始化

初始化，一定要和dp数组的定义吻合。

首先从dp[i][j]的定义出发，如果背包容量j为0的话，即dp[i][0]，无论是选取哪些物品，背包价值总和一定为0。如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%202.png)

在看其他情况：

状态转移方程 dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]); 可以看出i 是由 i-1 推导出来，那么i为0的时候就一定要初始化。

dp[0][j]，即：i为0，存放编号0的物品的时候，各个容量的背包所能存放的最大价值。

那么很明显当 j < weight[0]的时候，dp[0][j] 应该是 0，因为背包容量比编号0的物品重量还小。

当j >= weight[0]时，dp[0][j] 应该是value[0]，因为背包容量放足够放编号0物品。

代码初始化如下：

```jsx
for (let j = 0 ; j < weight[0]; j++) {  // 当然这一步，如果把dp数组预先初始化为0了，这一步就可以省略，但很多同学应该没有想清楚这一点。
    dp[0][j] = 0;
}
// 正序遍历
for (let j = weight[0]; j <= bagweight; j++) {
    dp[0][j] = value[0];
}
```

此时dp数组初始化情况如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%203.png)

dp[0][j] 和 dp[i][0] 都已经初始化了，那么其他下标应该初始化多少呢？

其实从递归公式： dp[i][j] = Math. max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]); 可以看出dp[i][j] 是由左上方数值推导出来了，那么 其他下标初始为什么数值都可以，因为都会被覆盖。

**初始-1，初始-2，初始100，都可以！**

但只不过一开始就统一把dp数组统一初始为0，更方便一些。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%204.png)

最后初始化代码如下：

```jsx
// 初始化 dp
let dp = Array(weight.length).fill().map(() => Array(size + 1).fill(0));
for (let j = weight[0]; j <= bagweight; j++) {
    dp[0][j] = value[0];
}
```

1. 确定遍历顺序

在如下图中，可以看出，有两个遍历的维度：物品与背包重量，先遍历物品或先遍历背包重量都可以，但是先遍历物品更好理解。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%205.png)

以下是先遍历物品的代码：

```jsx
// weight数组的大小 就是物品个数
for(let i = 1; i < weight.length; i++) { // 遍历物品
    for(let j = 0; j <= bagweight; j++) { // 遍历背包容量
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

    }
}
```

dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]); 递归公式中可以看出dp[i][j]是靠dp[i-1][j]和dp[i - 1][j - weight[i]]推导出来的。

dp[i-1][j]和dp[i - 1][j - weight[i]] 都在dp[i][j]的左上角方向（包括正上方向），那么先遍历物品，再遍历背包的过程如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%206.png)

以下遍历背包重量的代码：

```jsx
// weight数组的大小 就是物品个数
for(let j = 0; j <= bagweight; j++) { // 遍历背包容量
    for(int i = 1; i < weight.length; i++) { // 遍历物品
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
    }
}
```

再来看看先遍历背包，再遍历物品过程如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%207.png)

1. 举例推导dp数组

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%208.png)

JavaScript完整代码：

```jsx
function testWeightBagProblem (weight, value, size) {
    // 定义 dp 数组
    const len = weight.length,
          dp = Array(len).fill().map(() => Array(size + 1).fill(0));

    // 初始化
    for(let j = weight[0]; j <= size; j++) {
        dp[0][j] = value[0];
    }

    // weight 数组的长度len 就是物品个数
    for(let i = 1; i < len; i++) { // 遍历e物品
        for(let j = 0; j <= size; j++) { // 遍历背包容量
            if(j < weight[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
        }
    }

    console.table(dp)

    return dp[len - 1][size];
}

function test () {
    console.log(testWeightBagProblem([1, 3, 4, 5], [15, 20, 30, 55], 6));
}

test();
```

## ****一维dp数组（滚动数组）****

在使用二维数组的时候，递推公式：dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

其实可以发现如果把dp[i - 1]那一层拷贝到dp[i]上，表达式完全可以是：dp[i][j] = Math. max(dp[i][j], dp[i][j - weight[i]] + value[i]);

与其把dp[i - 1]这一层拷贝到dp[i]上，不如只用一个一维数组了，只用dp[j]（一维数组，也可以理解是一个滚动数组）。

动规五部曲

1. 确定dp数组的定义

在一维dp数组中，dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]。

1. 一维dp数组的递推公式

dp[j]为 容量为j的背包所背的最大价值，dp[j]可以通过dp[j - weight[i]]推导出来，dp[j - weight[i]]表示容量为j - weight[i]的背包所背的最大价值。

dp[j - weight[i]] + value[i] 表示 容量为 j - 物品i重量 的背包 加上 物品i的价值。（也就是容量为j的背包，放入物品i了之后的价值即：dp[j]）

此时dp[j]有两个选择，一个是取自己dp[j] 相当于 二维dp数组中的dp[i-1][j]，即不放物品i，一个是取dp[j - weight[i]] + value[i]，即放物品i，指定是取最大的，毕竟是求最大价值，所以递归公式为：

```jsx
dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
```

1. 一维dp数组如何初始化

dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]，那么dp[0]就应该是0，因为背包容量为0所背的物品的最大价值就是0。

那么dp数组除了下标0的位置，初始为0，其他下标应该初始化多少呢？

看一下递归公式：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

dp数组在推导的时候一定是取价值最大的数，如果题目给的价值都是正整数那么非0下标都初始化为0就可以了。

这样才能让dp数组在递归公式的过程中取的最大的价值，而不是被初始值覆盖了。

那么我假设物品价值都是大于0的，所以dp数组初始化的时候，都初始为0就可以了。

1. 一维dp数组遍历顺序

代码如下：

```jsx
for(let i = 0; i < weight.size(); i++) { // 遍历物品
    for(let j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

二维dp遍历的时候，背包容量是从小到大，而一维dp遍历的时候，背包是从大到小，因为倒序遍历是为了保证物品i只被放入一次！。但如果一旦正序遍历了，那么物品0就会被重复加入多次！

举一个例子：物品0的重量weight[0] = 1，价值value[0] = 15

如果正序遍历

dp[1] = dp[1 - weight[0]] + value[0] = 15

dp[2] = dp[2 - weight[0]] + value[0] = 30

此时dp[2]就已经是30了，意味着物品0，被放入了两次，所以不能正序遍历。

为什么倒序遍历，就可以保证物品只放入一次呢？

倒序就是先算dp[2]

dp[2] = dp[2 - weight[0]] + value[0] = 15 （dp数组已经都初始化为0）

dp[1] = dp[1 - weight[0]] + value[0] = 15

所以从后往前循环，每次取得状态不会和之前取得状态重合，这样每种物品就只取一次了。

为什么二维dp数组历的时候不用倒序呢？

因为对于二维dp，dp[i][j]都是通过上一层即dp[i - 1][j]计算而来，本层的dp[i][j]并不会被覆盖！

再来看看一维数组的两个嵌套for循环的顺序，代码中是先遍历物品嵌套遍历背包容量，那可不可以先遍历背包容量嵌套遍历物品呢？

不可以！因为一维dp的写法，背包容量一定是要倒序遍历（原因上面已经讲了），如果遍历背包容量放在上一层，那么每个dp[j]就只会放入一个物品，即：背包里只放入了一个物品。

倒序遍历的原因是，本质上还是一个对二维数组的遍历，并且右下角的值依赖上一层左上角的值，因此需要保证左边的值仍然是上一层的，从右向左覆盖。

1. 举例推导dp数组

一维dp，分别用物品0，物品1，物品2 来遍历背包，最终得到结果如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%209.png)

JavaScript完整代码：

```jsx
function testWeightBagProblem(wight, value, size) {
  const len = wight.length,
    dp = Array(size + 1).fill(0);
  for(let i = 1; i <= len; i++) {
    for(let j = size; j >= wight[i - 1]; j--) {
      dp[j] = Math.max(dp[j], value[i - 1] + dp[j - wight[i - 1]]);
    }
  }
  return dp[size];
}

function test () {
  console.log(testWeightBagProblem([1, 3, 4, 5], [15, 20, 30, 55], 6));
}

test();
```

## ****416. 分割等和子集****

[leetcode](https://leetcode.cn/problems/partition-equal-subset-sum/)

### 思路

首先确定能不能把01背包问题套到本题上来。

- 背包的体积为sum / 2
- 背包要放入的商品（集合里的元素）重量为元素的数值，价值也为元素的数值
- 背包如果正好装满，说明找到了总和为 sum / 2 的子集。
- 背包中每一个元素是不可重复放入

以上分析完，我们就可以套用01背包，来解决这个问题了。

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

本题中每一个元素的数值既是重量，也是价值。

套到本题，dp[j]表示背包总容量（所能装的总重量）是j，放进物品后，背的最大重量为dp[j]。

那么如果背包容量为target， dp[target]就是装满 背包之后的重量，所以 当 dp[target] == target 的时候，背包就装满了。

1. 确定递推公式

01背包的递推公式为：dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);

本题，相当于背包里放入数值，那么物品i的重量是nums[i]，其价值也是nums[i]。

所以递推公式：dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);

1. dp数组如何初始化

从dp[j]的定义来看，首先dp[0]一定是0。

如果题目给的价值都是正整数那么非0下标都初始化为0就可以了，如果题目给的价值有负数，那么非0下标就要初始化为负无穷。

这样才能让dp数组在递推的过程中取得最大的价值，而不是被初始值覆盖了。

本题题目中只包含正整数的非空数组，所以非0下标的元素初始化为0就可以了。

代码如下：

1. 确定遍历顺序

在01背包问题，一维dp数组（滚动数组）中就已经说明：如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历！

所以遍历顺序为：

```jsx
for (let i = 3; i <= n ; i++) {
    for (let j = 1; j <= i / 2; j++) {
        dp[i] = Math.max(dp[i], Math.max((i - j) * j, dp[i - j] * j));
    }
}
```

1. 举例推导dp数组

dp[j]的数值一定是小于等于j的。

如果dp[j] == j 说明，集合中的子集总和正好可以凑成总和j，理解这一点很重要。

用例1，输入[1,5,11,5] 为例，如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2042%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%20cb0272a9d75f44d893003a6b02ab8dab/Untitled%2010.png)

最后dp[11] == 11，说明可以将这个数组分割成两个子集，使得两个子集的元素和相等。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canPartition = function (nums) {
    let sum = 0;
    for (let i = 0; i < nums.length; i++) {
        sum += nums[i];
    }
    if (sum % 2 === 1) return false;

    let target = sum / 2;
    let dp = Array(target + 1).fill(0);

    // 开始 01背包
    for (let i = 0; i < nums.length; i++) {
        for (let j = target; j >= nums[i]; j--) { // 每一个元素一定是不可重复放入，所以从大到小遍历
            dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
        }
    }
    // 集合中的元素正好可以凑成总和target
    if (dp[target] == target) return true;
    return false;
};
```