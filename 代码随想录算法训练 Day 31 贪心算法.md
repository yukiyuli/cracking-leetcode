# 代码随想录算法训练 Day 31 | 贪心算法

Completed: January 13, 2023
Difficulty: Medium
Done: Yes
Question: 376. 摆动序列, 455. 分发饼干, 53. 最大子序和
Redo: No
Topic: Greedy Algorithm

## **贪心算法理论基础**

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2031%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%205cc32c55f4e144de806bc1b59a6a7bce/Untitled.png)

### 什么是贪心

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。

举一个例子：

例如，有一堆钞票，你可以拿走十张，如果想达到最大的金额，你要怎么拿？

指定每次拿最大的，最终结果就是拿走最大数额的钱。

每次拿最大的就是局部最优，最后拿走最大数额的钱就是推出全局最优。

再举一个例子：

如果是 有一堆盒子，你有一个背包体积为n，如何把背包尽可能装满，如果还每次选最大的盒子，就不行了。这时候就需要动态规划。

### ****贪心的套路（什么时候用贪心）****

唯一的难点就是如何通过局部最优，推出整体最优。

如何验证可不可以用贪心算法呢？

手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心吧。

### ****贪心一般解题步骤****

贪心算法一般分为如下四步：

- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每一个子问题的最优解
- 将局部最优解堆叠成全局最优解

### ****总结****

贪心没有套路，说白了就是常识性推导加上举反例。

## ****455. 分发饼干****

[leetcode](https://leetcode.cn/problems/combinations/)

### 思路：

为了满足更多的小孩，就不要造成饼干尺寸的浪费。

大尺寸的饼干既可以满足胃口大的孩子也可以满足胃口小的孩子，那么就应该优先满足胃口大的。

这里的局部最优就是大饼干喂给胃口大的，充分利用饼干尺寸喂饱一个，全局最优就是喂饱尽可能多的小孩。

可以尝试使用贪心策略，先将饼干数组和小孩数组排序。然后从后向前遍历小孩数组，用大饼干优先满足胃口大的，并统计满足小孩数量。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2031%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%205cc32c55f4e144de806bc1b59a6a7bce/Untitled%201.png)

这个例子可以看出饼干9只有喂给胃口为7的小孩，这样才是整体最优解，并想不出反例，那么就可以撸代码了。

本题需要用一个index来控制饼干数组的遍历，遍历饼干并没有再起一个for循环，而是采用自减的方式，这也是常用的技巧。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} g
 * @param {number[]} s
 * @return {number}
 */
var findContentChildren = function (g, s) {
    g = g.sort((a, b) => a - b);
    s = s.sort((a, b) => a - b);
    let index = s.length - 1; // 饼干数组的下标
    let result = 0;
    for (let i = g.length - 1; i >= 0; i--) {
        if(index >= 0 && s[index] >= g[i]) {
            result++;
            index--;
        }
    }
    return result;
};
```

## ****376. 摆动序列****

[leetcode](https://leetcode.cn/problems/wiggle-subsequence/)

### 思路

本题要求删除元素使其达到最大摆动序列，应该删除什么元素呢？

以[1,17,5,10,13,15,10,5,16,8]为例，如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2031%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%205cc32c55f4e144de806bc1b59a6a7bce/Untitled%202.png)

局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值。

整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列。

例如序列[2,5]，它的峰值数量是2，如果靠统计差值来计算峰值个数就需要考虑数组最左面和最右面的特殊情况。

针对序列[2,5]，可以假设为[2,2,5]，这样它就有坡度了即preDiff = 0，如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2031%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%205cc32c55f4e144de806bc1b59a6a7bce/Untitled%203.png)

针对以上情形，result初始为1（默认最右面有一个峰值），此时curDiff > 0 && preDiff <= 0，那么result++（计算了左面的峰值），最后得到的result就是2（峰值个数为2即摆动序列长度为2）

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number}
 */
var wiggleMaxLength = function (nums) {
    if (nums.length <= 1) return nums.length;
    let curDiff = 0; // 当前一对差值
    let preDiff = 0; // 前一对差值
    let result = 1;  // 记录峰值个数，序列默认序列最右边有一个峰值
    for (let i = 0; i < nums.length - 1; i++) {
        curDiff = nums[i + 1] - nums[i];
        // 出现峰值
        if ((curDiff > 0 && preDiff <= 0) || (preDiff >= 0 && curDiff < 0)) {
            result++;
            preDiff = curDiff;
        }
    }
    return result;
};
```

## ****53. 最大子序和****

[leetcode](https://leetcode.cn/problems/maximum-subarray/)

### ****贪心解法****

### 思路

如果 -2 1 在一起，计算起点的时候，一定是从1开始计算，因为负数只会拉低总和，这就是贪心贪的地方！

局部最优：当前“连续和”为负数的时候立刻放弃，从下一个元素重新计算“连续和”，因为负数加上下一个元素 “连续和”只会越来越小。

全局最优：选取最大“连续和”。

局部最优的情况下，并记录最大的“连续和”，可以推出全局最优。

遍历nums，从头开始用count累积，如果count一旦加上nums[i]变为负数，那么就应该从nums[i+1]开始从0累积count了，因为已经变为负数的count，只会拖累总和。

区间的终止位置，就是如果count取到最大值了，及时记录下来了。

如动画所示：

![https://code-thinking.cdn.bcebos.com/gifs/53.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.gif](https://code-thinking.cdn.bcebos.com/gifs/53.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.gif)

红色的起始位置就是贪心每次取count为正数的时候，开始一个区间的统计。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
    let result = -Infinity;
    let count = 0;
    for (let i = 0; i < nums.length; i++) { // 设置起始位置
        count += nums[i];
        if (count > result) { // 取区间累计的最大值（相当于不断确定最大子序终止位置）
            result = count;
        }
        if (count <= 0) count = 0; // 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
    }
    return result;
};
```