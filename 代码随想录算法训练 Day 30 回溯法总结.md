# 代码随想录算法训练 Day 30 | 回溯法总结

Completed: January 12, 2023
Done: Yes
Redo: No
Topic: Backtrace

## ****回溯法理论基础****

回溯法就是暴力搜索，并不是什么高效的算法，最多再剪枝一下。

回溯算法能解决如下问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 棋盘问题：N皇后，解数独等等

回溯法不好理解，所以需要把回溯法抽象为一个图形来理解就容易多了，并利用回溯三部曲来分析回溯算法，并给出了回溯法的模板：

```jsx
const backtracking =(参数)=> {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

# **组合问题**

### ****组合问题****

1. [组合](https://www.notion.so/Day-24-09ef36cde8814608a33e3c8733d570d2) **用递归控制for循环嵌套的数量！**

本题把回溯问题抽象为树形结构，如图：for循环横向遍历，递归纵向遍历，回溯不断调整结果集。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled.png)

优化回溯算法只有剪枝一种方法，在[77. 组合](https://www.notion.so/Day-24-09ef36cde8814608a33e3c8733d570d2) ****中把回溯法代码做了剪枝优化，树形结构如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%201.png)

组合问题剪枝精髓是：for循环在寻找起点的时候要有一个范围，如果这个起点到集合终止之间的元素已经不够题目要求的k个元素了，就没有必要搜索了。

在for循环上做剪枝操作是回溯法剪枝的常见套路！

## **组合总和**

### ****组合总和（一）****

[216. 组合总和III](https://www.notion.so/Day-25-ef5b51363eec4550be1724d48c8ffc5b) 加了一个元素总和的限制。树形结构如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%202.png)

本题思路和之前一样，剪枝方法是将已选元素总和如果已经大于n（题中要求的和）了，那么往后遍历就没有意义了，直接剪掉，如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%203.png)

本题中，还有一个地方可以剪枝，就是对for循环选择的起始范围的剪枝。在for循环加上 `i <= 9 - (k - path.length) + 1` 的限制！

### ****组合总和（二）****

在[39. 组合总和](https://www.notion.so/Day-27-f261b08c1d95471b89ebffb10c86a316) 题目中没有数量要求，可以无限重复，但是有总和的限制，所以间接的也是有个数的限制。

本题还需要startIndex来控制for循环的起始位置，不能因为看到可以重复选择，就把startIndex去掉了。

所以剪枝的代码可以在for循环加上 `i <= 9 - (k - path.size()) + 1` 的限制！

> 对于组合问题，什么时候需要startIndex呢？
如果是一个集合来求组合的话，就需要startIndex，例如：[77. 组合](https://www.notion.so/Day-24-09ef36cde8814608a33e3c8733d570d2)，[216. 组合总和III](https://www.notion.so/Day-25-ef5b51363eec4550be1724d48c8ffc5b)
如果是多个集合取组合，各个集合之间相互不影响，那么就不用startIndex，例如：[17. 电话号码的字母组合](https://www.notion.so/Day-25-ef5b51363eec4550be1724d48c8ffc5b)
> 

树形结构如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%204.png)

最后还给出了本题的剪枝优化，如下：

```jsx
for (int i = startIndex; i < candidates.length && sum + candidates[i] <= target; i++)
```

优化后树形结构如下:

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%205.png)

### ****组合总和（三）****

在40.组合总和II 中集合元素会有重复，但要求解集不能包含重复的组合。所以难点在去重问题上了。

去重分两种：一种是“树枝去重”，另一种是“树层去重”。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%206.png)

图中将used的变化用橘黄色标注上，可以看出在candidates[i] === candidates[i - 1]相同的情况下：

- used[i - 1] === true，说明同一树枝candidates[i - 1]使用过
- used[i - 1] === false，说明同一树层candidates[i - 1]使用过

### ****多个集合求组合****

在[17.电话号码的字母组合](https://www.notion.so/Day-25-ef5b51363eec4550be1724d48c8ffc5b) 中，用多个集合来求组合。本题for循环不再是从startIndex开始遍历的。因为本题每一个数字代表的是不同集合，也就是求不同集合之间的组合。

树形结构如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%207.png)

## ****切割问题****

在 [131. 分割回文串](https://www.notion.so/Day-27-f261b08c1d95471b89ebffb10c86a316) 中，解决的是切割问题。

本题的难点：

- 切割问题其实类似组合问题
- 如何模拟那些切割线
- 切割问题中递归如何终止
- 在递归循环中如何截取子串
- 如何判断回文

树形结构如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%208.png)

## **子集问题**

### ****子集问题（一）****

在[78. 子集](https://www.notion.so/Day-28-30fb8e39229044079606cf955c220a8e) 题目里，在树形结构中子集问题是要收集所有节点的结果，而组合问题是收集叶子节点的结果。 ********

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%209.png)

本题其实可以不需要加终止条件，因为startIndex >= nums.length，本层for循环本来也结束了，本来我们就要遍历整棵树。

### ****子集问题（二）****

在[90. 子集II](https://www.notion.so/Day-28-30fb8e39229044079606cf955c220a8e) 中，开始针对子集问题进行去重。

树形结构如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%2010.png)

### ****递增子序列****

在 [491. 递增子序列](https://www.notion.so/Day-29-59e505b6176947a09253010a6511c2a5) 中，需要区分与题目 [90. 子集II](https://www.notion.so/Day-28-30fb8e39229044079606cf955c220a8e) 的区别。

题目 [90. 子集II](https://www.notion.so/Day-28-30fb8e39229044079606cf955c220a8e) 可以使用set针对同一父节点本层去重，但子集问题一定要排序。原因见下图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%2011.png)

## **排列问题**

### ****排列问题（一）****

[46.全排列](https://www.notion.so/Day-29-59e505b6176947a09253010a6511c2a5) 排列是有序的，也就是说 [1,2] 和 [2,1] 是两个集合，这就是和子集以及组合所不同的地方。

可以看出元素1在[1,2]中已经使用过了，但是在[2,1]中还要在使用一次1，所以处理排列问题就不用使用startIndex了。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%2012.png)

排列问题的不同之处：

- 每层都是从0开始搜索而不是startIndex
- 需要used数组记录path里都放了哪些元素了

### ****排列问题（二）****

[47.全排列 II](https://www.notion.so/Day-29-59e505b6176947a09253010a6511c2a5) ********本题重点在去重，强调了“树层去重”和“树枝去重”。

树形结构如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%2013.png)

本题特殊的地方在于used[i - 1] == false也可以，used[i - 1] == true也可以！

## ****去重问题****

统一使用used数组来去重的，但其实使用set也可以用来去重！

使用used数组去重 和 使用set去重 两种写法的性能差异：

使用set去重的版本相对于used数组的版本效率都要低很多。

而使用used数组在时间复杂度上几乎没有额外负担。

使用set去重，不仅时间复杂度高了，空间复杂度也高了。

## 总结

回溯专题汇聚为一张图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2030%20%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%80%BB%E7%BB%93%20f62a0674721a4b258235ac707fc236f4/Untitled%2014.png)