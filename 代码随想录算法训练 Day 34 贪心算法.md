# 代码随想录算法训练 Day 34 | 贪心算法

Completed: January 16, 2023
Difficulty: Medium
Done: Yes
Question: 1005. K次取反后最大化的数组和, 134. 加油站, 135. 分发糖果
Redo: No
Topic: Greedy Algorithm

## 1005. K次取反后最大化的数组和

[leetcode](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

### 思路：

两次贪心。

第一次：局部最优：让绝对值大的负数变为正数，当前数值达到最大，整体最优：整个数组和达到最大。

第二次：局部最优：只找数值最小的正整数进行反转，当前数值可以达到最大（例如正整数数组{5, 3, 1}，反转1 得到-1 比 反转5得到的-5 大多了）。全局最优：整个数组和达到最大。

解题步骤为：

- 第一步：将数组按照绝对值大小从大到小排序，注意要按照绝对值的大小
- 第二步：从前向后遍历，遇到负数将其变为正数，同时K--
- 第三步：如果K还大于0，那么反复转变数值最小的元素，将K用完
- 第四步：求和

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var largestSumAfterKNegations = function (nums, k) {
    nums.sort((a, b) => Math.abs(b) - Math.abs(a))   // 第一步
    for (let i = 0; i < nums.length; i++) {   // 第二步
        if (nums[i] < 0 && k > 0) {
            nums[i] = - nums[i];
            k--;
        }
    }
    while (k > 0) {    // 第三步
        nums[nums.length - 1] = - nums[nums.length - 1]
        k--;
    }
    return nums.reduce((a, b) => {    // 第四步
        a + b
    })
};
```

## ****134. 加油站****

[leetcode](https://leetcode.cn/problems/gas-station/)

### ****暴力方法****

遍历每一个加油站为起点的情况，模拟一圈。如果跑了一圈，中途没有断油，而且最后油量大于等于0，说明这个起点是ok的。

for循环适合模拟从头到尾的遍历，而while循环适合模拟环形遍历。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */
var canCompleteCircuit = function (gas, cost) {
    for (let i = 0; i < cost.length; i++) {
        let rest = gas[i] - cost[i]; // 记录剩余油量
        let index = (i + 1) % cost.length;
        while (rest > 0 && index != i) { // 模拟以i为起点行驶一圈
            rest += gas[index] - cost[index];
            index = (index + 1) % cost.length;
        }
        // 如果以i为起点跑一圈，剩余油量>=0，返回该起始位置
        if (rest >= 0 && index == i) return i;
    }
    return -1;
};
```

### ****贪心算法（方法一）****

直接从全局进行贪心选择，情况如下：

- 情况一：如果gas的总和小于cost总和，那么无论从哪里出发，一定是跑不了一圈的
- 情况二：rest[i] = gas[i]-cost[i]为一天剩下的油，i从0开始计算累加到最后一站，如果累加没有出现负数，说明从0出发，油就没有断过，那么0就是起点。
- 情况三：如果累加的最小值是负数，汽车就要从非0节点出发，从后向前，看哪个节点能这个负数填平，能把这个负数填平的节点就是出发节点。

> 情况三为什么从后向前遍历？
因为本题说了如果有解一定会有一个唯一答案对吧，说明总加油量一定是大于总消耗量的，只是不均匀，可能出现前面不足但是后面又比较多补上了情况，所以这里前面我们已经从前往后发现不能走得通，说明从前面的加油站，至少是从0号加油站开始，是行不通的，那么前面不足的后面必有补充（因为有唯一答案），所以是从后面开始遍历的，发现第一个可以填补汽油负数结余值的就是答案。
> 

JavaScript完整代码：

```jsx
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */
var canCompleteCircuit = function (gas, cost) {
    let curSum = 0;
    let min = Infinity; // 从起点出发，油箱里的油量最小值
    for (let i = 0; i < gas.length; i++) {
        let rest = gas[i] - cost[i];
        curSum += rest;
        if (curSum < min) {
            min = curSum;
        }
    }
    if (curSum < 0) return -1;  // 情况1
    if (min >= 0) return 0;     // 情况2
    // 情况3
    for (let i = gas.length - 1; i >= 0; i--) {
        let rest = gas[i] - cost[i];
        min += rest;
        if (min >= 0) {
            return i;
        }
    }
    return -1;
};
```

### ****贪心算法（方法二）****

如果总油量减去总消耗大于等于零那么一定可以跑完一圈，说明 各个站点的加油站剩油量rest[i]相加一定是大于等于零的。

i从0开始累加rest[i]，和记为curSum，一旦curSum小于零，说明[0, i]区间都不能作为起始位置，起始位置从i+1算起，再从0计算curSum。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2034%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%205e4f271c6b4345789c16d2d7e8291489/Untitled.png)

一旦[i，j] 区间和为负数，j+1后面出现更大的负数，就是更新j，那么起始位置又变成新的j+1了。

并且j之前出现了多少负数，j后面就会出现多少正数，因为耗油总和是大于零的（前提我们已经确定了一定可以跑完全程）。

局部最优：当前累加rest[j]的和curSum一旦小于0，起始位置至少要是j+1，因为从j开始一定不行。

全局最优：找到可以跑一圈的起始位置。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */
var canCompleteCircuit = function (gas, cost) {
    let curSum = 0;
    let totalSum = 0;
    let start = 0;
    for (let i = 0; i < gas.length; i++) {
        curSum += gas[i] - cost[i];
        totalSum += gas[i] - cost[i];
        if (curSum < 0) {   // 当前累加rest[i]和 curSum一旦小于0
            start = i + 1;  // 起始位置更新为i+1
            curSum = 0;     // curSum从0开始
        }
    }
    if (totalSum < 0) return -1; // 说明怎么走都不可能跑一圈了
    return start;
};
```

## ****135. 分发糖果****

[leetcode](https://leetcode.cn/problems/candy/)

### 思路

本题一定要比较每一个孩子的左边，然后再比较右边，两边一起考虑一定会顾此失彼。

先确定右边评分大于左边的情况（也就是从前向后遍历）：

局部最优：只要右边评分比左边大，右边的孩子就多一个糖果。

全局最优：相邻的孩子中，评分高的右孩子获得比左边孩子更多的糖果

如果ratings[i] > ratings[i - 1] 那么[i]的糖 一定要比[i - 1]的糖多一个，所以贪心：candyVec[i] = candyVec[i - 1] + 1

代码如下：

```jsx
// 从前向后
for (int i = 1; i < ratings.length; i++) {
    if (ratings[i] > ratings[i - 1]) candyVec[i] = candyVec[i - 1] + 1;
}
```

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2034%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%205e4f271c6b4345789c16d2d7e8291489/Untitled%201.png)

再确定左孩子大于右孩子的情况（从后向前遍历）：

> 为什么不能从前向后遍历呢？
因为如果从前向后遍历，根据 ratings[i + 1] 来确定 ratings[i] 对应的糖果，那么每次都不能利用上前一次的比较结果了。
> 

如果 ratings[i] > ratings[i + 1]，此时candyVec[i]（第i个小孩的糖果数量）就有两个选择了，一个是candyVec[i + 1] + 1（从右边这个加1得到的糖果数量），一个是candyVec[i]（之前比较右孩子大于左孩子得到的糖果数量）。

局部最优：取candyVec[i + 1] + 1 和 candyVec[i] 最大的糖果数量，保证第i个小孩的糖果数量既大于左边的也大于右边的。

全局最优：相邻的孩子中，评分高的孩子获得更多的糖果。

所以就取candyVec[i + 1] + 1 和 candyVec[i] 最大的糖果数量，candyVec[i]只有取最大的才能既保持对左边candyVec[i - 1]的糖果多，也比右边candyVec[i + 1]的糖果多。

代码如下：

```jsx
// 从后向前
for (int i = ratings.length - 2; i >= 0; i--) {
    if (ratings[i] > ratings[i + 1] ) {
        candyVec[i] = Math.max(candyVec[i], candyVec[i + 1] + 1);
    }
}
```

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2034%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%205e4f271c6b4345789c16d2d7e8291489/Untitled%202.png)

JavaScript完整代码：

```jsx
/**
 * @param {number[]} ratings
 * @return {number}
 */
var candy = function (ratings) {
    let candyVec = new Array(ratings.length).fill(1);
    // 从前向后
    for (let i = 1; i < ratings.length; i++) {
        if (ratings[i] > ratings[i - 1]) candyVec[i] = candyVec[i - 1] + 1;
    }
    // 从后向前
    for (let i = ratings.length - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candyVec[i] = Math.max(candyVec[i], candyVec[i + 1] + 1);
        }
    }
    // 统计结果
    let result = 0;
    for (let i = 0; i < candyVec.length; i++) result += candyVec[i];
    return result;
};
```