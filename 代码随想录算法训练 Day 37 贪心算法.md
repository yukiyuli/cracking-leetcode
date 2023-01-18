# 代码随想录算法训练 Day 37 | 贪心算法

Topic: Greedy Algorithm

Question: 738. 单调递增的数字

Difficulty: Medium

- [x] Done

Completed: January 18, 2023

- [ ] Redo


## ****738. 单调递增的数字****

[leetcode](https://leetcode.cn/problems/monotone-increasing-digits/)

### 思路：

要找到单调递增并且最大的数，对于其中的两位来说，例如：98，前一位大于后一位，就要将前一位减1，然后将后一位置为9，这样这个整数就是89，即小于98的最大的单调递增整数。

为了让这个数最大，就需要找到一个位置，从这里往后就都要置为9。

其次，遍历方法是从前向后还是从后向前？

如果从前向后遍历，遇到strNum[i - 1] > strNum[i]的情况，让strNum[i - 1]减一，但此时如果strNum[i - 1]减一了，可能又小于strNum[i - 2]。举个例子，数字：332，从前向后遍历的话，那么就把变成了329，此时2又小于了第一位的3了，真正的结果应该是299。

如果从后向前遍历，就可以重复利用上次比较得出的结果了，从后向前遍历332的数值变化为：332 -> 329 -> 299。

局部最优：遇到strNum[i - 1] > strNum[i]的情况，让strNum[i - 1]--，然后strNum[i]给为9，可以保证这两位变成最大单调递增整数。

全局最优：得到小于等于N的最大单调递增的整数。

JavaScript完整代码：

```jsx
/**
 * @param {number} n
 * @return {number}
 */
var monotoneIncreasingDigits = function (n) {
    //将n转化为数组
    n = n.toString();
    let strNum = n = n.split('').map(item => +item);
    // flag用来标记赋值9从哪里开始
    // 设置为这个默认值，为了防止第二个for循环在flag没有被赋值的情况下执行
    let flag = strNum.length;
    for (let i = strNum.length - 1; i > 0; i--) {
        if (strNum[i - 1] > strNum[i]) {
            flag = i;
            strNum[i - 1]--;
        }
    }
    for (let i = flag; i < strNum.length; i++) {
        strNum[i] = '9';
    }
    return parseInt(strNum.join(''));
};
```

## ****714. 买卖股票的最佳时机含手续费****

[leetcode](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

[二刷的时候再学习](https://programmercarl.com/0714.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA%E5%90%AB%E6%89%8B%E7%BB%AD%E8%B4%B9.html)。

## ****968. 监控二叉树****

[leetcode](https://leetcode.cn/problems/binary-tree-cameras/)

[二刷的时候再学习](https://programmercarl.com/0968.%E7%9B%91%E6%8E%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html)。
