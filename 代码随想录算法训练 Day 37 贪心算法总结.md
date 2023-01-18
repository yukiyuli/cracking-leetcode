# 代码随想录算法训练 Day 37 | 贪心算法总结

Topic: Greedy Algorithm

- [x] Done

Completed: January 18, 2023

- [ ] Redo


## ****贪心理论基础****

1. 贪心很简单，就是常识？

贪心思路往往很巧妙，并不简单。

1. 贪心有没有固定的套路？

贪心无套路，也没有框架之类的，需要多看多练培养感觉才能想到贪心的思路。

1. 究竟什么题目是贪心呢？

手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心吧。

## ****贪心简单题****

以下三题贪心就是常识。

- **[贪心算法：分发饼干](https://www.notion.so/Day-31-5cc32c55f4e144de806bc1b59a6a7bce)**
- **[贪心算法：K次取反后最大化的数组和](https://www.notion.so/Day-34-5e4f271c6b4345789c16d2d7e8291489)**
- **[贪心算法：柠檬水找零](https://www.notion.so/Day-35-42299fd2aedc4f9ba007ab89439ed4ff)**

## ****贪心中等题****

贪心中等题，靠常识可能就有点想不出来了。

- **[贪心算法：摆动序列](https://www.notion.so/Day-31-5cc32c55f4e144de806bc1b59a6a7bce)**
- **[贪心算法：单调递增的数字](https://www.notion.so/Day-37-e6a2016b84dd42e39eb7917ae90e4229)**

## ****贪心解决股票问题****

贪心也可以解决股票系列问题。

- **[贪心算法：买卖股票的最佳时机II](https://www.notion.so/Day-32-03124238966546afae27d0e0879574c3)**
- **[贪心算法：买卖股票的最佳时机含手续费](https://programmercarl.com/0714.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA%E5%90%AB%E6%89%8B%E7%BB%AD%E8%B4%B9.html)**

## ****两个维度权衡问题****

在出现两个维度相互影响的情况时，两边一起考虑一定会顾此失彼，要先确定一个维度，再确定另一个一个维度。

- **[贪心算法：分发糖果](https://www.notion.so/Day-34-5e4f271c6b4345789c16d2d7e8291489)**
- **[贪心算法：根据身高重建队列](https://www.notion.so/Day-35-42299fd2aedc4f9ba007ab89439ed4ff)**

## ****贪心难题****

这里的题目不要做一遍，要多练！

### **贪心解决区间问题**

以下都是关于区间问题，各种覆盖各种去重。

- **[贪心算法：跳跃游戏](https://www.notion.so/Day-32-03124238966546afae27d0e0879574c3)**
- **[贪心算法：跳跃游戏II](https://www.notion.so/Day-32-03124238966546afae27d0e0879574c3)**
- **[贪心算法：用最少数量的箭引爆气球](https://www.notion.so/Day-35-42299fd2aedc4f9ba007ab89439ed4ff)**
- **[贪心算法：无重叠区间](https://www.notion.so/Day-36-b4f0674b4d2340e3b8cc179721ef44d5)**
- **[贪心算法：划分字母区间](https://www.notion.so/Day-36-b4f0674b4d2340e3b8cc179721ef44d5)**
- **[贪心算法：合并区间](https://www.notion.so/Day-36-b4f0674b4d2340e3b8cc179721ef44d5)**

### ****其他难题****

**[贪心算法：最大子序和](https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html)** 其实是动态规划的题目，但贪心性能更优。

**[贪心算法：加油站](https://programmercarl.com/0134.%E5%8A%A0%E6%B2%B9%E7%AB%99.html)** 可能以为是一道模拟题，但就算模拟其实也不简单，需要把while用的很娴熟。但其实是可以使用贪心给时间复杂度降低一个数量级。

## ****最后总结****

贪心专题汇聚为一张图：

![Untitled](https://user-images.githubusercontent.com/101588752/213280954-57f83760-5863-41b9-b3fd-3c4ae4055177.png)

感谢代码随想录知识星成员：**[海螺人](https://wx.zsxq.com/dweb2/index/footprint/844412858822412)** 所画。
