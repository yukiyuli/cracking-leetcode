# 代码随想录算法训练 Day 50 | 背包问题总结

Topic: Dynamic programming

- [x] Done

Completed: January 29, 2023

- [ ] Redo


关于这几种常见的背包，其关系如下：

![Untitled](https://user-images.githubusercontent.com/101588752/215353075-e4c8689e-a266-4a28-ae2f-3c7a884ee316.png)

背包五部曲：

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

## ****背包递推公式****

问能否能装满背包（或者最多装多少）：dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]); ，对应题目如下：

- **[动态规划：416.分割等和子集](https://www.notion.so/Day-42-cb0272a9d75f44d893003a6b02ab8dab)**
- **[动态规划：1049.最后一块石头的重量 II](https://www.notion.so/Day-43-63866f92bccb4a588a467cd928ae5391)**

问装满背包有几种方法：dp[j] += dp[j - nums[i]] ，对应题目如下：

- **[动态规划：494.目标和](https://www.notion.so/Day-43-63866f92bccb4a588a467cd928ae5391)**
- [动态规划：518.零钱兑换II](https://www.notion.so/Day-44-c85f5f15441b4773b8f64a815c8eeaaa)
- **[动态规划：377.组合总和](https://www.notion.so/Day-44-c85f5f15441b4773b8f64a815c8eeaaa)**
- **[动态规划：70. 爬楼梯进阶版（完全背包）](https://www.notion.so/Day-45-8a38fb89400d424d80661c8cb8fbd74b)**

问背包装满最大价值：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]); ，对应题目如下：

- **[动态规划：](https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html)[474.一和零](https://www.notion.so/Day-43-63866f92bccb4a588a467cd928ae5391)**

问装满背包所有物品的最小个数：dp[j] = min(dp[j - coins[i]] + 1, dp[j]); ，对应题目如下：

- **[动态规划：322.零钱兑换](https://www.notion.so/Day-45-8a38fb89400d424d80661c8cb8fbd74b)**
- **[动态规划：279.完全平方数](https://www.notion.so/Day-45-8a38fb89400d424d80661c8cb8fbd74b)**

## **遍历顺序**

### **01背包**

在**[动态规划：关于01背包问题，你该了解这些！](https://www.notion.so/Day-42-cb0272a9d75f44d893003a6b02ab8dab)** 中我们讲解二维dp数组01背包先遍历物品还是先遍历背包都是可以的，且第二层for循环是从小到大遍历。

和**[动态规划：关于01背包问题，你该了解这些！（滚动数组）](https://www.notion.so/Day-42-cb0272a9d75f44d893003a6b02ab8dab)**中，我们讲解一维dp数组01背包只能先遍历物品再遍历背包容量，且第二层for循环是从大到小遍历。

**一维dp数组的背包在遍历顺序上和二维dp数组实现的01背包其实是有很大差异的，大家需要注意！**

### **完全背包**

在**[动态规划：关于完全背包，你该了解这些！](https://www.notion.so/Day-44-c85f5f15441b4773b8f64a815c8eeaaa)** 中，讲解了纯完全背包的一维dp数组实现，先遍历物品还是先遍历背包都是可以的，且第二层for循环是从小到大遍历。

但是仅仅是纯完全背包的遍历顺序是这样的，题目稍有变化，两个for循环的先后顺序就不一样了。

**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。

**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。

相关题目如下：

- 求组合数：**[动态规划：518.零钱兑换II](https://www.notion.so/Day-44-c85f5f15441b4773b8f64a815c8eeaaa)**
- 求排列数：**[动态规划：377. 组合总和 Ⅳ](https://www.notion.so/Day-44-c85f5f15441b4773b8f64a815c8eeaaa)** 、**[动态规划：70. 爬楼梯进阶版（完全背包）](https://www.notion.so/Day-45-8a38fb89400d424d80661c8cb8fbd74b)**

如果求最小数，那么两层for循环的先后顺序就无所谓了，相关题目如下：

- 求最小数：**[动态规划：322. 零钱兑换](https://www.notion.so/Day-45-8a38fb89400d424d80661c8cb8fbd74b)** 、[动态规划：279.完全平方数](https://www.notion.so/Day-45-8a38fb89400d424d80661c8cb8fbd74b)

**对于背包问题，其实递推公式算是容易的，难是难在遍历顺序上，如果把遍历顺序搞透，才算是真正理解了**。

## 总结

![Untitled 1](https://user-images.githubusercontent.com/101588752/215353083-ab5af152-eb04-4f1f-9df2-ee96c74b75f3.png)

这个图是 **[代码随想录知识星球](https://programmercarl.com/other/kstar.html)** 成员：**[海螺人](https://wx.zsxq.com/dweb2/index/footprint/844412858822412)** 所画。
