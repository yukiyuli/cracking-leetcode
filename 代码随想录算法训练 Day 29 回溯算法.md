# 代码随想录算法训练 Day 29 | 回溯算法

Topic: Backtrace

Question: 46. 全排列, 47. 全排列 II, 491. 递增子序列

Difficulty: Medium

- [x] Done

Completed: January 12, 2023

- [ ] Redo


## ****491. 递增子序列****

[leetcode](https://leetcode.cn/problems/non-decreasing-subsequences/)

### 思路：

本题和[90. 子集II](https://www.notion.so/Day-28-30fb8e39229044079606cf955c220a8e) 很像，[90. 子集II](https://www.notion.so/Day-28-30fb8e39229044079606cf955c220a8e) 中我们是通过排序，再加一个标记数组来达到去重的目的。

本题求自增子序列，是不能对原数组进行排序的，排完序的数组都是自增子序列了。所以不能使用之前的去重逻辑！

举例[4, 7, 6, 7]，抽象为树形结构如图：

![Untitled](https://user-images.githubusercontent.com/101588752/212138410-01b52bbf-4f68-4d8d-8d00-a7535edb2f29.png)

### ****回溯三步曲****

1. 确定递归函数返回值以及参数

全局变量数组path为子集收集元素，二维数组res存放子集组合。还需要一个需要startIndex。

代码如下：

```jsx
let res = [];
let path = [];
const backtracking = (nums, startIndex) =>
```

1. 确定终止条件

本题类似求子集问题，也是要遍历树形结构找每一个节点，所以和[78. 子集](https://www.notion.so/Day-28-30fb8e39229044079606cf955c220a8e) 一样，可以不加终止条件，startIndex每次都会加1，并不会无限递归。

但本题收集结果有所不同，题目要求递增子序列大小至少为2，所以代码如下：

```jsx
if (path.length > 1) {
    res.push([...path]);
}
```

1. 确定单层递归的逻辑

同一父节点下的同层上使用过的元素就不能再使用了，用哈希记录本层元素是否重复使用，题目中说了，数值范围[-100,100]，所以完全可以用数组来做哈希。

代码如下：

```jsx
let used = []; // 这里使用数组来进行去重操作，题目说数值范围[-100, 100]
    for (let i = startIndex; i < nums.length; i++) {
       if ((path.length > 0 && nums[i] < path[path.length - 1])
            || used[nums[i] + 100] == 1) {
            continue;
       }
       used[nums[i] + 100] = 1; // 记录这个元素在本层用过了，本层后面不能再用了
       path.push(nums[i]);
       backtracking(nums, i + 1);
       path.pop();
}
```

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var findSubsequences = function (nums) {
    let res = [];
    let path = [];
    const backtracking = (nums, startIndex) => {
        if (path.length > 1) {
            res.push([...path]);
        }
        let used = []; // 这里使用数组来进行去重操作，题目说数值范围[-100, 100]
        for (let i = startIndex; i < nums.length; i++) {
            if ((path.length > 0 && nums[i] < path[path.length - 1])
                || used[nums[i] + 100] == 1) {
                continue;
            }
            used[nums[i] + 100] = 1; // 记录这个元素在本层用过了，本层后面不能再用了
            path.push(nums[i]);
            backtracking(nums, i + 1);
            path.pop();
        }
    }
    backtracking(nums, 0);
    return res;

};
```

## ****46. 全排列****

[leetcode](https://leetcode.cn/problems/permutations/)

### 思路：

以[1,2,3]为例，抽象成树形结构如下：

从图中红线部分，可以看出遍历这个树的时候，把所有节点都记录下来，就是要求的子集集合。

![Untitled 1](https://user-images.githubusercontent.com/101588752/212138435-6db138ac-bc9c-4c59-b345-cd70194ce4e8.png)

### 回溯三步曲

1. 确定递归函数参数以及返回值

排列是有序的，也就是说 [1,2] 和 [2,1] 是两个集合。可以看出元素1在[1,2]中已经使用过了，但是在[2,1]中还要在使用一次1，所以处理排列问题就不用使用startIndex了。

但排列问题需要一个used数组，标记已经选择的元素，如图橘黄色部分所示:

![Untitled 2](https://user-images.githubusercontent.com/101588752/212138455-43cf1c66-0f77-4864-b205-76c50fc39d06.png)

代码如下：

```jsx
let res = [];
let path = [];
const backtracking = (nums, used) =>
```

1. 确定终止条件

从上图中可以看出，叶子节点，就是收割结果的地方。

当收集元素的数组path的大小达到和nums数组一样大的时候，说明找到了一个全排列，也表示到达了叶子节点。

代码如下：

```jsx
// 此时说明找到了一组
if (path.length === nums.length) {
    return res.push([...path]);
}
```

1. 确定单层递归的逻辑

因为排列问题，每次都要从头开始搜索，例如元素1在[1,2]中已经使用过了，但是在[2,1]中还要再使用一次1。

而used数组，其实就是记录此时path里都有哪些元素使用了，一个排列里一个元素只能使用一次。

代码如下：

```jsx
for (let i = 0; i < nums.length; i++) {
     if (used[i] === true) continue; // path里已经收录的元素，直接跳过
     used[i] = true;
     path.push(nums[i]);
     backtracking(nums, used);
     path.pop();
     used[i] = false;
}
```

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function (nums) {
    let res = [];
    let path = [];
    const backtracking = (nums, used) => {
        // 此时说明找到了一组
        if (path.length === nums.length) {
            return res.push([...path]);
        }
        for (let i = 0; i < nums.length; i++) {
            if (used[i] === true) continue; // path里已经收录的元素，直接跳过
            used[i] = true;
            path.push(nums[i]);
            backtracking(nums, used);
            path.pop();
            used[i] = false;
        }
    }

    backtracking(nums, []);
    return res;

};
```

## ****47. 全排列 II****

[leetcode](https://leetcode.cn/problems/permutations-ii/)

### ****思路****

去重一定要对元素进行排序，这样我们才方便通过相邻的节点来判断是否重复使用了。

以示例中的 [1,1,2]为例 （已经排序）抽象为一棵树，去重过程如图：

![Untitled 3](https://user-images.githubusercontent.com/101588752/212138489-46205044-8b77-43e5-ac58-0c1c8226b069.png)

图中对同一树层，前一位（也就是nums[i-1]）如果使用过，要进行去重。

> 一般来说：组合问题和排列问题是在树形结构的叶子节点上收集结果，而子集问题就是取树上所有节点的结果。
> 

JavaScript完整代码：使用used数组

```jsx
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permuteUnique = function (nums) {
    let res = [];
    let path = [];
    nums.sort((a, b) => a - b); // 排序
    let used = new Array(nums.length).fill(false);
    const backtracking = (nums, used) => {
        // 此时说明找到了一组
        if (path.length === nums.length) {
            return res.push([...path]);
        }
        for (let i = 0; i < nums.length; i++) {
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过 
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 && nums[i] === nums[i - 1] && used[i - 1] === false) {
                continue;
            }
            if (used[i] === false) {
                used[i] = true;
                path.push(nums[i]);
                backtracking(nums, used);
                path.pop();
                used[i] = false;
            }
        }
    }

    backtracking(nums, used);
    return res;
};
```

### ****拓展****

去重关键：

```jsx
if (i > 0 && nums[i] === nums[i - 1] && used[i - 1] === false)
```

如果改成 `used[i - 1] == true`， 也是正确的!，去重代码如下：

```jsx
if (i > 0 && nums[i] === nums[i - 1] && used[i - 1] === true)
```

因为如果要对树层中前一位去重，就用`used[i - 1] === false`，如果要对树枝前一位去重用`used[i - 1] === true`。

对于排列问题，树层上去重和树枝上去重，都是可以的，但是树层上去重效率更高！

用[1,1,1] 来举一个例子。树层上去重(used[i - 1] == false)，的树形结构如下：

![Untitled 4](https://user-images.githubusercontent.com/101588752/212138512-955fca65-0590-4b2b-b724-a72ffcd15c3a.png)

树枝上去重（used[i - 1] == true）的树型结构如下：

![Untitled 5](https://user-images.githubusercontent.com/101588752/212138528-b77d1f99-be99-473e-b824-fdf380e730fd.png)

代码如下：

```jsx
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function (nums) {
    let res = [];
    let path = [];
    nums.sort((a, b) => a - b); // 去重需要排序
    const backtracking = (nums, startIndex) => {
        res.push([...path]);
        for (let i = startIndex; i < nums.length; i++) {
            // 我们要对同一树层使用过的元素进行跳过
            if (i > startIndex && nums[i] === nums[i - 1]) {
                continue;
            }
            path.push(nums[i]);
            backtracking(nums, i + 1);
            path.pop();
        }
    }
    backtracking(nums, 0);
    return res;
};
```
