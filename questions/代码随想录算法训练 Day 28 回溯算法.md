# 代码随想录算法训练 Day 28 | 回溯算法

Topic: Backtrace

Question: 78. 子集, 90. 子集II, 93. 复原IP地址

Difficulty: Medium

- [x] Done

Completed: January 10, 2023

- [ ] Redo


## ****93. 复原IP地址****

[leetcode](https://leetcode.cn/problems/restore-ip-addresses/)

### 思路：

切割问题就可以使用回溯搜索法把所有可能性搜出来**。**

切割问题可以抽象为树型结构，如图：

![Untitled](https://user-images.githubusercontent.com/101588752/211605658-e0e107a5-c002-4806-873a-7bcc32a5acb0.png)

JavaScript完整代码：

```jsx
/**
 * @param {string} s
 * @return {string[]}
 */
var restoreIpAddresses = function(s) {
    const res = [], path = [];
    backtracking(0);
    return res;
    function backtracking(i) {
        const len = path.length;
        if(len > 4) return;
        if(len === 4 && i === s.length) { //限制长度 为4，且长度必须一样
            res.push(path.join('.'));
            return;
        }
        for(let j = i; j < s.length; j++) {
            const str = s.slice(i, j + 1); //切割字符放进 str
            if(str.length > 3 || str > 255) break; //str长度限制
            if(str.length > 1 && str[0] === '0') break;
            path.push(str);
            backtracking(j + 1);
            path.pop() 
        }
    }
};
```

## ****78. 子集****

[leetcode](https://leetcode.cn/problems/subsets/)

### 思路：

子集也是一种组合问题，因为它的集合是无序的，子集{1,2} 和 子集{2,1}是一样的。

既然是无序，取过的元素不会重复取，写回溯算法的时候，for就要从startIndex开始，而不是从0开始！

> 什么时候for可以从0开始呢？
求排列问题的时候，就要从0开始，因为集合是有序的，{1, 2} 和{2, 1}是两个集合。
> 

以示例中nums = [1,2,3]为例把求子集抽象为树型结构，如下：

![Untitled 1](https://user-images.githubusercontent.com/101588752/211605702-62665a8a-7707-4578-b189-14fda1b86395.png)

从图中红线部分，可以看出遍历这个树的时候，把所有节点都记录下来，就是要求的子集集合。

### 回溯三步曲

1. 确定递归函数参数以及返回值

全局变量数组path为子集收集元素，二维数组res存放子集组合。还需要一个需要startIndex。

代码如下：

```jsx
let res = [];
let path = [];
const backtracking = (nums, startIndex) =>
```

1. 确定终止条件

从上图中可以看初，剩余集合为空的时候，就是叶子节点。

那么什么时候剩余集合为空呢？

就是startIndex已经大于数组的长度了，就终止了，因为没有元素可取了。

代码如下：

```jsx
if (startIndex >= nums.length) { // 终止条件可以不加
   return;
}
```

> 其实可以不需要加终止条件，因为startIndex >= nums.length，本层for循环本来也结束了。
> 
1. 确定单层递归的逻辑

求取子集问题，不需要任何剪枝！因为子集就是要遍历整棵树。

代码如下：

```jsx
for (let i = startIndex; i < nums.length; i++) {
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
var subsets = function (nums) {
    let res = [];
    let path = [];
    const backtracking = (nums, startIndex) => {
        res.push([...path]); // 进入一层递归就要吧当前的path放到结果集里，并且要放在终止添加的上面，否则会漏掉自己
        if (startIndex >= nums.length) { // 终止条件可以不加
            return;
        }
        for (let i = startIndex; i < nums.length; i++) {
            path.push(nums[i]);
            backtracking(nums, i + 1);
            path.pop();
        }
    }
    backtracking(nums, 0);
    return res;
};
```

## ****90. 子集II****

[leetcode](https://leetcode.cn/problems/palindrome-partitioning/)

### ****思路****

用示例中的[1, 2, 2] 来举例，如图所示： （**注意去重需要先对集合排序**）

![Untitled 2](https://user-images.githubusercontent.com/101588752/211605756-c7b36ac9-1fe8-444c-a4d0-fd1e9636f02a.png)

从图中可以看出，同一树层上重复取2 就要过滤掉，同一树枝上就可以重复取2，因为同一树枝上元素的集合才是唯一子集！

本题其实是**78. 子集** 和 **[40.组合总和II](https://www.notion.so/Day-27-f261b08c1d95471b89ebffb10c86a316)** 的结合。

JavaScript完整代码：使用used数组

```jsx
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function (nums) {
    let res = [];
    let path = [];
    nums.sort((a, b) => a - b); // 去重需要排序
    let used = new Array(nums.length).fill(false);
    const backtracking = (nums, startIndex, used) => {
        res.push([...path]);
        for (let i = startIndex; i < nums.length; i++) {
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 而我们要对同一树层使用过的元素进行跳过
            if (i > 0 && nums[i] === nums[i - 1] && used[i - 1] === false) {
                continue;
            }
            path.push(nums[i]);
            used[i] = true;
            backtracking(nums, i + 1, used);
            used[i] = false;
            path.pop();
        }
    }
    backtracking(nums, 0, used);
    return res;
};
```

### **补充**

本题也可以不使用used数组来去重，因为递归的时候下一个startIndex是i+1而不是0。

如果要是全排列的话，每次要从0开始遍历，为了跳过已入栈的元素，需要使用used。

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
