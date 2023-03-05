# 代码随想录算法训练 Day 39 | 动态规划

Topic: Dynamic programming

Question: 62. 不同路径, 63. 不同路径 II

Difficulty: Medium

- [x] Done

Completed: January 19, 2023

- [ ] Redo


## ****62. 不同路径****

[leetcode](https://leetcode.cn/problems/unique-paths/)

### ****动态规划：****

动规五部曲：

1. 确定dp数组以及下标的含义

dp[i][j] ：表示从（0 ，0）出发，到(i, j) 有dp[i][j]条不同的路径。

1. 确定递推公式

dp[i - 1][j] 表示从(0, 0)的位置到(i - 1, j)有几条路径，dp[i][j - 1]同理。

那么很自然，dp[i][j] = dp[i - 1][j] + dp[i][j - 1]，因为dp[i][j]只有这两个方向过来。

1. dp数组如何初始化

首先dp[i][0]一定都是1，因为从(0, 0)的位置到(i, 0)的路径只有一条，那么dp[0][j]也同理。

所以初始化代码为：

```jsx
for (let i = 0; i < m; i++) dp[i][0] = 1;
for (let j = 0; j < n; j++) dp[0][j] = 1;
```

1. 确定遍历顺序

递推公式dp[i][j] = dp[i - 1][j] + dp[i][j - 1]，dp[i][j]都是从其上方和左方推导而来，那么从左到右一层一层遍历就可以了。

这样就可以保证推导dp[i][j]的时候，dp[i - 1][j] 和 dp[i][j - 1]一定是有数值的。

1. 举例推导dp数组

如图：

![Untitled](https://user-images.githubusercontent.com/101588752/213626723-54dce927-b43d-44d5-bec8-9be7482afd0d.png)

JavaScript完整代码：

```jsx
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function (m, n) {
    dp = Array(m).fill().map(item => Array(n).fill(0));
    for (let i = 0; i < m; i++) dp[i][0] = 1;
    for (let j = 0; j < n; j++) dp[0][j] = 1;
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    return dp[m - 1][n - 1];
};
```

## ****63. 不同路径 II****

[leetcode](https://leetcode.cn/problems/unique-paths-ii/)

### 思路

动规五部曲：

1. 确定dp数组以及下标的含义

dp[i][j] ：表示从（0 ，0）出发，到(i, j) 有dp[i][j]条不同的路径。

1. 确定递推公式

递推公式和62.不同路径一样，dp[i][j] = dp[i - 1][j] + dp[i][j - 1]。

但这里需要注意一点，因为有了障碍，(i, j)如果就是障碍的话应该就保持初始状态（初始状态为0）。

所以代码为：

```jsx
if (obstacleGrid[i][j] === 0) { // 当(i, j)没有障碍的时候，再推导dp[i][j]
    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
}
```

1. dp数组如何初始化

在62.不同路径 中我们给出如下的初始化：

```jsx
    dp = Array(m).fill().map(item => Array(n).fill(0));
    for (let i = 0; i < m; i++) dp[i][0] = 1;
    for (let j = 0; j < n; j++) dp[0][j] = 1;
```

因为从(0, 0)的位置到(i, 0)的路径只有一条，所以dp[i][0]一定为1，dp[0][j]也同理。

但如果(i, 0) 这条边有了障碍之后，障碍之后（包括障碍）都是走不到的位置了，所以障碍之后的dp[i][0]应该还是初始值0。

如图：

![Untitled 1](https://user-images.githubusercontent.com/101588752/213626758-2c124300-57d8-4aac-988f-233014153adc.png)

下标(0, j)的初始化情况同理。

所以本题初始化代码为：

```jsx
dp = Array(m).fill().map(item => Array(n).fill(0));
for (let i = 0; i < m && obstacleGrid[i][0] === 0; i++) dp[i][0] = 1;
for (let j = 0; j < n && obstacleGrid[0][j] === 0; j++) dp[0][j] = 1;
```

> 注意：
代码里for循环的终止条件，一旦遇到obstacleGrid[i][0] == 1的情况就停止dp[i][0]的赋值1的操作，dp[0][j]同理。
> 
1. 确定遍历顺序

从递归公式dp[i][j] = dp[i - 1][j] + dp[i][j - 1] 中可以看出，一定是从左到右一层一层遍历，这样保证推导dp[i][j]的时候，dp[i - 1][j] 和 dp[i][j - 1]一定是有数值

代码如下：

```jsx
for (let i = 1; i < m; i++) {
    for (let j = 1; j < n; j++) {
        if (obstacleGrid[i][j] === 1) continue;
        dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
    }
}
```

1. 举例推导dp数组

拿示例1来举例如题：

![Untitled 2](https://user-images.githubusercontent.com/101588752/213626787-20369765-6c9c-4d12-8eec-12452752569d.png)

对应的dp table 如图：

![Untitled 3](https://user-images.githubusercontent.com/101588752/213626804-10cdd9cb-b317-43b3-8409-e30358986772.png)

JavaScript完整代码：

```jsx
/**
 * @param {number[][]} obstacleGrid
 * @return {number}
 */
var uniquePathsWithObstacles = function (obstacleGrid) {
    let m = obstacleGrid.length;
    let n = obstacleGrid[0].length;
    if (obstacleGrid[m - 1][n - 1] === 1 || obstacleGrid[0][0] === 1) //如果在起点或终点出现了障碍，直接返回0
        return 0;
    dp = Array(m).fill().map(item => Array(n).fill(0))
    for (let i = 0; i < m && obstacleGrid[i][0] === 0; i++) dp[i][0] = 1;
    for (let j = 0; j < n && obstacleGrid[0][j] === 0; j++) dp[0][j] = 1;
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (obstacleGrid[i][j] === 1) continue;
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    return dp[m - 1][n - 1];
};
```
