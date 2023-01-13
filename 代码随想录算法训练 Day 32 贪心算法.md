# 代码随想录算法训练 Day 32 | 贪心算法

Completed: January 13, 2023
Difficulty: Medium
Done: Yes
Redo: No
Topic: Greedy Algorithm

## ****122. 买卖股票的最佳时机II****

[leetcode](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

### 思路：

想获得利润至少要两天为一个交易单元。

### ****贪心算法****

分解最终利润。

假如第0天买入，第3天卖出，那么利润为：prices[3] - prices[0]。

相当于(prices[3] - prices[2]) + (prices[2] - prices[1]) + (prices[1] - prices[0])。

此时就是把利润分解为每天为单位的维度，而不是从0天到第3天整体去考虑！

那么根据prices可以得到每天的利润序列：(prices[i] - prices[i - 1]).....(prices[1] - prices[0])。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2032%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%2003124238966546afae27d0e0879574c3/Untitled.png)

第一天没有利润，至少要第二天才会有利润，所以利润的序列比股票序列少一天！

从图中可以发现，其实需要收集每天的正利润就可以，收集正利润的区间，就是股票买卖的区间，而我们只需要关注最终利润，不需要记录区间。

那么只收集正利润就是贪心所贪的地方！

局部最优：收集每天的正利润，全局最优：求得最大利润。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
    let result = 0;
    for (let i = 1; i < prices.length; i++) {
        result += Math.max(prices[i] - prices[i - 1], 0);
    }
    return result;
};
```

## ****55. 跳跃游戏****

[leetcode](https://leetcode.cn/problems/jump-game/)

### 思路

这道题目关键点在于：不用拘泥于每次究竟跳几步，而是看覆盖范围，覆盖范围内一定是可以跳过来的，不用管是怎么跳的。

每次移动取最大跳跃步数（得到最大的覆盖范围），每移动一个单位，就更新最大覆盖范围。

贪心算法局部最优解：每次取最大跳跃步数（取最大覆盖范围），整体最优解：最后得到整体最大覆盖范围，看是否能到终点。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2032%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%2003124238966546afae27d0e0879574c3/Untitled%201.png)

i每次移动只能在cover的范围内移动，每移动一个元素，cover得到该元素数值（新的覆盖范围）的补充，让i继续移动下去。

而cover每次只取 max(该元素数值补充后的范围, cover本身范围)。

如果cover大于等于了终点下标，直接return true就可以了。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function (nums) {
    let cover = 0;
    if (nums.length === 1) return true; // 只有一个元素，就是能达到
    for (let i = 0; i <= cover; i++) { // 注意这里是小于等于cover
        cover = Math.max(i + nums[i], cover);
        if (cover >= nums.length - 1) return true; // 说明可以覆盖到终点了
    }
    return false;
};
```

## ****45. 跳跃游戏II****

[leetcode](https://leetcode.cn/problems/jump-game-ii/)

### ****贪心解法****

### 思路

本题要计算最小步数，那么就要想清楚什么时候步数才一定要加一呢？

贪心的思路，局部最优：当前可移动距离尽可能多走，如果还没到终点，步数再加一。整体最优：一步尽可能多走，从而达到最小步数。

解题的时候，要从覆盖范围出发，不管怎么跳，覆盖范围内一定是可以跳到的，以最小的步数增加覆盖范围，覆盖范围一旦覆盖了终点，得到的就是最小步数！

这里需要统计两个覆盖范围，当前这一步的最大覆盖和下一步最大覆盖。

如果移动下标达到了当前这一步的最大覆盖最远距离了，还没有到终点的话，那么就必须再走一步来增加覆盖范围，直到覆盖范围覆盖了终点。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2032%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%2003124238966546afae27d0e0879574c3/Untitled%202.png)

图中覆盖范围的意义在于，只要红色的区域，最多两步一定可以到！（不用管具体怎么跳，反正一定可以跳到）

**方法一：**

从图中可以看出来，就是移动下标达到了当前覆盖的最远距离下标时，步数就要加一，来增加覆盖距离。最后的步数就是最少步数。

这里还有个特殊情况需要考虑，当移动下标达到了当前覆盖的最远距离下标时

- 如果当前覆盖最远距离下标不是是集合终点，步数就加一，还需要继续走。
- 如果当前覆盖最远距离下标就是是集合终点，步数不用加一，因为不能再往后走了。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function (nums) {
    if (nums.length == 1) return 0;
    let curDistance = 0;    // 当前覆盖最远距离下标
    let steps = 0;            // 记录走的最大步数
    let nextDistance = 0;   // 下一步覆盖最远距离下标
    for (let i = 0; i < nums.length; i++) {
        nextDistance = Math.max(nums[i] + i, nextDistance);  // 更新下一步覆盖最远距离下标
        if (i === curDistance) {                         // 遇到当前覆盖最远距离下标
            if (curDistance != nums.length - 1) {       // 如果当前覆盖最远距离下标不是终点
                steps++;                                  // 需要走下一步
                curDistance = nextDistance;             // 更新当前覆盖最远距离下标（相当于加油了）
                if (nextDistance >= nums.length - 1) break; // 下一步的覆盖范围已经可以达到终点，结束循环
            } else break;                               // 当前覆盖最远距离下标是集合终点，不用做steps++操作了，直接结束
        }
    }
    return steps;
};
```

****方法二：****

依然是贪心，思路和方法一差不多，代码可以简洁一些。

针对于方法一的特殊情况，可以统一处理，即：移动下标只要遇到当前覆盖最远距离的下标，直接步数加一，不考虑是不是终点的情况。

想要达到这样的效果，只要让移动下标，最大只能移动到nums.length - 2的地方就可以了。

因为当移动下标指向nums.length - 2时：

- 如果移动下标等于当前覆盖最大距离下标， 需要再走一步（即steps++），因为最后一步一定是可以到的终点。（题目假设总是可以到达数组的最后一个位置），如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2032%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%2003124238966546afae27d0e0879574c3/Untitled%203.png)

- 如果移动下标不等于当前覆盖最大距离下标，说明当前覆盖最远距离就可以直接达到终点了，不需要再走一步。如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2032%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%2003124238966546afae27d0e0879574c3/Untitled%204.png)

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function (nums) {
    let curDistance = 0;    // 当前覆盖的最远距离下标
    let steps = 0;            // 记录走的最大步数
    let nextDistance = 0;   // 下一步覆盖的最远距离下标
    for (let i = 0; i < nums.length - 1; i++) { // 注意这里是小于nums.size() - 1，这是关键所在
        nextDistance = Math.max(nums[i] + i, nextDistance); // 更新下一步覆盖的最远距离下标
        if (i === curDistance) {                 // 遇到当前覆盖的最远距离下标
            curDistance = nextDistance;         // 更新当前覆盖的最远距离下标
            steps++;
        }
    }
    return steps;
};
```