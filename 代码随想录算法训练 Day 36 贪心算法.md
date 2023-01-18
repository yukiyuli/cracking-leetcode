# 代码随想录算法训练 Day 36 | 贪心算法

Completed: January 16, 2023
Difficulty: Medium
Done: Yes
Question: 435. 无重叠区间, 56. 合并区间, 763. 划分字母区间
Redo: No
Topic: Greedy Algorithm

## ****435. 无重叠区间****

[leetcode](https://leetcode.cn/problems/non-overlapping-intervals/)

### 思路：

本题有四个难点：

- 难点一：一看题就有感觉需要排序，但究竟怎么排序，按左边界排还是右边界排。

按照右边界排序，就要从左向右遍历，因为右边界越小越好，只要右边界越小，留给下一个区间的空间就越大，所以从左向右遍历，优先选右边界小的。

按照左边界排序，就要从右向左遍历，因为左边界数值越大越好（越靠右），这样就给前一个区间的空间就越大，所以可以从右向左遍历。

- 难点二：排完序之后如何遍历，如果没有分析好遍历顺序，那么排序就没有意义了。

本题按照右边界排序，就要从左向右遍历，从左向右记录非交叉区间的个数。

- 难点三：直接求重复的区间是复杂的，转而求最大非重复区间个数。
- 难点四：求最大非重复区间个数时，需要一个分割点来做标记。

每次取非交叉区间的时候，都是从右边界最小的来做分割点（这样留给下一个区间的空间就越大）。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2036%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%20b4f0674b4d2340e3b8cc179721ef44d5/Untitled.png)

局部最优：优先选右边界小的区间，所以从左向右遍历，留给下一个区间的空间大一些，从而尽量避免交叉。

全局最优：选取最多的非交叉区间。

JavaScript完整代码：

```jsx
/**
 * @param {number[][]} intervals
 * @return {number}
 */
var eraseOverlapIntervals = function (intervals) {
    intervals.sort((a, b) => {
        return a[1] - b[1]
    })
    if (intervals.length === 0) return 0;
    let count = 1; // 记录非交叉区间的个数
    let end = intervals[0][1]; // 记录区间分割点
    for (let i = 1; i < intervals.length; i++) {
				//当区间的左边界大于上一个区间的右边界的时候 说明是一对不重合区间
        if (end <= intervals[i][0]) {
            end = intervals[i][1];     //更新right
            count++;
        }
    }
		//intervals的长度减去最多的不重复的区间 就是最少删除区间的个数
    return intervals.length - count;
};
```

## ****763. 划分字母区间****

[leetcode](https://leetcode.cn/problems/partition-labels/)

### 思路

在遍历的过程中相当于是要找每一个字母的边界，如果找到之前遍历过的所有字母的最远边界，说明这个边界就是分割点了
。此时前面出现过所有字母，最远也就到这个边界了。

可以分为如下两步：

- 统计每一个字符最后出现的位置
- 从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2036%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%20b4f0674b4d2340e3b8cc179721ef44d5/Untitled%201.png)

JavaScript完整代码：

```jsx
/**
 * @param {string} s
 * @return {number[]}
 */
var partitionLabels = function (s) {
    let hash = {}; // i为字符，hash[i]为字符出现的最后位置
    for (let i = 0; i < s.length; i++) { // 统计每一个字符最后出现的位置
        hash[s[i]] = i;
    }
    let result = [];
    let left = 0;
    let right = 0;
    for (let i = 0; i < s.length; i++) {
        right = Math.max(right, hash[s[i]]); // 找到字符出现的最远边界
        if (i === right) {
            result.push(right - left + 1);
            left = i + 1;
        }
    }
    return result;
};
```

## ****56. 合并区间****

[leetcode](https://leetcode.cn/problems/merge-intervals/)

### 思路

本题左边界排序或右边界排序都可以。

排序之后局部最优：每次合并都取最大的右边界，这样就可以合并更多的区间了。

整体最优：合并所有重叠的区间。

定义prev 初始为第一个区间，cur 表示当前的区间，res 表示结果数组。

开始遍历，尝试合并 prev 和 cur，合并后更新到 prev。

合并后的新区间还可能和后面的区间重合，继续尝试合并新的 cur，更新给 prev，• 直到不能合并即 `prev[1] < cur[0]`，此时将 prev 区间推入 res 数组。

合并的方法：因为先按区间的左端排升序，就能保证 `prev[0] < cur[0]` ，所以合并只需这条：`prev[1] = max(prev[1], cur[1])`。

我们是先合并，遇到不重合再推入 prev。 当考察完最后一个区间，后面没区间了，遇不到不重合区间，最后的 prev 没推入 res。 要单独补上。

下图解释了intervals[i]的左边界在intervals[i - 1]左边界和右边界的范围内，那么一定有重复！：（注意图中区间都是按照左边界排序之后了）

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2036%20%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%20b4f0674b4d2340e3b8cc179721ef44d5/Untitled%202.png)

JavaScript完整代码：

```jsx
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function (intervals) {
    intervals.sort((a, b) => a[0] - b[0]);    // 按照起始位置排序
    let prev = intervals[0];    //p rev 初始为第一个区间
    let result = [];

    for (let i = 1; i < intervals.length; i++) {
        let cur = intervals[i];     // cur 表示当前的区间
        if (prev[1] >= cur[0]) {   // 当prev[1] >= cur[0]说明重叠
            prev[1] = Math.max(cur[1], prev[1]);//更新prev的右边界
        } else {
            result.push(prev);    //不重叠 加入result
            prev = cur;    // 更新区间
        }
    }
    result.push(prev);
    return result;
};
```