# 代码随想录算法训练 Day 27 | 回溯算法

Completed: January 8, 2023
Difficulty: Medium
Done: Yes
Redo: No
Topic: Backtrace

## ****39. 组合总和****

[leetcode](https://leetcode.cn/problems/combination-sum/)

### 思路：

本题和**77.组合** ，**216.组合总和III**的区别是：本题没有数量要求，可以无限重复，但是有总和的限制，所以间接的也是有个数的限制。

本题搜索的过程抽象成树形结构如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled.png)

注意图中叶子节点的返回条件，因为本题没有组合数量要求，仅仅是总和的限制，所以递归没有层数的限制，只要选取的元素总和超过target，就返回！

### ****回溯三步曲****

1. 确定递归函数返回值以及参数

定义两个全局变量，二维数组result存放结果集，数组path存放符合条件的结果。

此外还需要定义int型的sum变量来统计单一结果path里的总和。

本题还需要startIndex来控制for循环的起始位置。

> 对于组合问题，什么时候需要startIndex呢？
如果是一个集合来求组合的话，就需要startIndex，例如：**7**7.组合 ，216.组合总和III 。
如果是多个集合取组合，各个集合之间相互不影响，那么就不用startIndex，例如：17.电话号码的字母组合。
> 

代码如下：

```jsx
let result = []; // 存放结果集
let path = []; // 符合条件的结果
const backtracking = (candiates, target, sum, startIndex) =>
```

1. 确定终止条件

在如下树形结构中：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled%201.png)

从叶子节点可以清晰看到，终止只有两种情况，sum大于target和sum等于target。

sum等于target的时候，需要收集结果。

代码如下：

```jsx
if (sum > target) return;
if (sum === target) return res.push([...path]);  // 深copy
```

1. 确定单层递归的逻辑

单层for循环依然是从startIndex开始，搜索candidates集合。

> 注意：本题和77.组合 、216.组合总和III 的一个区别是：本题元素为可重复选取的。
> 

代码如下：

```jsx
for (let i = startIndex; i < candiates.length; i++) {
     sum += candiates[i];
     path.push(candiates[i]);
     backtracking(candiates, target, sum, i);  // 不用i+1了，表示可以重复读取当前的数
     sum -= candiates[i];  // 回溯
     path.pop();   // 回溯
}
```

> 别忘了处理过程和回溯过程是一一对应的，处理有加，回溯就要有减！
> 

JavaScript完整代码：

```jsx
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function (candidates, target) {
    let res = [];
    let path = [];
    const backtracking = (candiates, target, sum, startIndex) => {
        if (sum > target) return;

        if (sum === target) return res.push([...path]);  // 深copy

        for (let i = startIndex; i < candiates.length; i++) {
            sum += candiates[i];
            path.push(candiates[i]);
            backtracking(candiates, target, sum, i);  // 不用i+1了，表示可以重复读取当前的数
            sum -= candiates[i];  // 回溯
            path.pop();   // 回溯
        }
    }
    candidates.sort((a, b) => a - b);  // 排序
    backtracking(candidates, target, 0, 0);
    return res;
};
```

### 剪枝

对于sum已经大于target的情况，其实是依然进入了下一层递归，只是下一层递归结束判断的时候，会判断sum > target的话就返回。其实如果已经知道下一层的sum会大于target，就没有必要进入下一层递归了。那么可以在for循环的搜索范围上做做文章了。

对总集合排序之后，如果下一层的sum（就是本层的 sum + candidates[i]）已经大于target，就可以结束本轮for循环的遍历。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled%202.png)

for循环剪枝代码如下：

```jsx
for (int i = startIndex; i < candidates.length && sum + candidates[i] <= target; i++)
```

优化后JavaScript代码：

```jsx
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function (candidates, target) {
    let res = [];
    let path = [];
    const backtracking = (candiates, target, sum, startIndex) => {
        if (sum === target) return res.push([...path]);  // 深copy

        // 如果 sum + candidates[i] > target 就终止遍历
        for (let i = startIndex; i < candiates.length && sum + candidates[i] <= target; i++) {
            sum += candiates[i];
            path.push(candiates[i]);
            backtracking(candiates, target, sum, i);  // 不用i+1了，表示可以重复读取当前的数
            sum -= candiates[i];  // 回溯
            path.pop();   // 回溯
        }
    }
    candidates.sort((a, b) => a - b);  // 排序
    backtracking(candidates, target, 0, 0);
    return res;
};
```

## ****40. 组合总和II****

[leetcode](https://leetcode.cn/problems/combination-sum-ii/)

### 思路：

本题的难点：集合（数组candidates）有重复元素，但还不能有重复的组合。

根据题目要求，元素在同一个组合内是可以重复的，怎么重复都没事，但两个组合不能相同。

组合问题都可以抽象为树形结构，树形结构又分树层和树枝，本题去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重。

举一个例子，candidates = [1, 1, 2], target = 3，（方便起见candidates已经排序了）

> 强调：树层去重的话，需要对数组排序！
> 

选择过程树形结构如图所示：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled%203.png)

> 这里的used记录树层上的数是否重复
> 

### 回溯三步曲

1. 确定递归函数参数以及返回值

与**39.组合总和** 套路相同，此题还需要加一个bool型数组used，用来记录同一树枝上的元素是否使用过。集合去重的重任就是used来完成的。

代码如下：

```jsx
let res = [];
let path = [];
let used = new Array(candidates.length).fill(false);
const backtracking = (candidates, target, sum, startIndex, used) =>
```

1. 确定终止条件

与**39.组合总和**相同，终止条件为 `sum > target`和 `sum == target`。

代码如下：

```jsx
if (sum > target) { // 这个条件其实可以省略
    return;
}
if (sum === target) {
    retrun res.push([...path]);
}
```

1. 确定单层递归的逻辑

最大的不同就是去重。

如果`candidates[i] == candidates[i - 1]` 并且 `used[i - 1] == false`，就说明：前一个树枝，使用了candidates[i - 1]，也就是说同一树层使用过candidates[i - 1]。此时for循环里就应该做continue的操作。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled%204.png)

图中将used的变化用橘黄色标注上，可以看出在candidates[i] == candidates[i - 1]相同的情况下：

- used[i - 1] == true，说明是进入下一层递归，去下一个数，所以说明同一树枝candidates[i - 1]使用过
- used[i - 1] == false，表示当前取的 candidates[i] 是从 candidates[i - 1] 回溯而来的，所以说明同一树层candidates[i - 1]使用过

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled%205.png)

代码如下：

```jsx
for (let i = startIndex; i < candidates.length && sum + candidates[i] <= target; i++) {  // sum + candidates[i] <= target为剪枝操作
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if (i > 0 && candidates[i] === candidates[i - 1] && used[i - 1] === false) continue;
            sum += candidates[i];
            path.push(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, sum, i + 1, used);  // 和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次
            used[i] = false;
            used[i] = false;
            sum -= candidates[i];
            path.pop()

        }
```

JavaScript完整代码：使用used数组

```jsx
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum2 = function (candidates, target) {
    let res = [];
    let path = [];
    candidates.sort((a, b) => a - b);  // 要先排序
    let used = new Array(candidates.length).fill(false);
    const backtracking = (candidates, target, sum, startIndex, used) => {
        if (sum === target) return res.push([...path]);

        for (let i = startIndex; i < candidates.length && sum + candidates[i] <= target; i++) {  // sum + candidates[i] <= target为剪枝操作
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if (i > 0 && candidates[i] === candidates[i - 1] && used[i - 1] === false) continue;
            sum += candidates[i];
            path.push(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, sum, i + 1, used);  // 和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次
            used[i] = false;
            used[i] = false;
            sum -= candidates[i];
            path.pop()

        }
    }
    backtracking(candidates, target, 0, 0, used);
    return res;
};
```

### 补充：

这里直接用startIndex来去重也是可以的， 就不用used数组了。

JavaScript完整代码：不使用used数组

```jsx
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum2 = function (candidates, target) {
    let res = [];
    let path = [];
    candidates.sort((a, b) => a - b);  // 首先把给candidates排序，让其相同的元素都挨在一起。
    const backtracking = (candidates, target, sum, startIndex) => {
        if (sum === target) return res.push([...path]);

        for (let i = startIndex; i < candidates.length && sum + candidates[i] <= target; i++) {  // sum + candidates[i] <= target为剪枝操作
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if (i > startIndex && candidates[i] === candidates[i - 1]) continue;
            sum += candidates[i];
            path.push(candidates[i]);
            backtracking(candidates, target, sum, i + 1);  // 和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次
            sum -= candidates[i];
            path.pop()

        }
    }
    backtracking(candidates, target, 0, 0);
    return res;
};
```

## ****131. 分割回文串****

[leetcode](https://leetcode.cn/problems/palindrome-partitioning/)

### ****思路****

本题这涉及到两个关键问题：

1. 切割问题，有不同的切割方式
2. 判断回文

> “回文”(顺读和倒读都一样的字符串称“回文”,如level)。
> 

**其实切割问题类似组合问题**。

例如对于字符串abcdef：

- 组合问题：选取一个a之后，在bcdef中再去选取第二个，选取b之后在cdef中再选取第三个.....。
- 切割问题：切割一个a之后，在bcdef中再去切割第二段，切割b之后在cdef中再切割第三段.....。

所以切割问题，也可以抽象为一棵树形结构，如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled%206.png)

递归用来纵向遍历，for循环用来横向遍历，切割线（就是图中的红线）切割到字符串的结尾位置，说明找到了一个切割方法。

### 回溯三步曲

1. 确定递归函数参数以及返回值

全局变量数组path存放切割后回文的子串，二维数组res存放结果集，还需要startIndex，记录切割过的地方，不能重复切割。

代码如下：

```jsx
let res = [];
let path = []; // 放已经回文的子串
const backtracking = (s, startIndex) =>
```

1. 确定终止条件

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2027%20%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%20f261b08c1d95471b89ebffb10c86a316/Untitled%207.png)

从树形结构的图中可以看出：切割线切到了字符串最后面，说明找到了一种切割方法，此时就是本层递归的终止条件。

在代码里，递归参数需要传入startIndex，表示下一轮递归遍历的起始位置，这个startIndex就是切割线。

代码如下：

```jsx
if (startIndex >= s.length) {
    return res.push([...path]);
}
```

1. 确定单层递归的逻辑

在`for (int i = startIndex; i < s.length; i++)`循环中，我们 定义了起始位置startIndex，那么 [startIndex, i] 就是要截取的子串。

首先判断这个子串是不是回文，如果是回文，就加入在`path`中，path用来记录切割过的回文子串。

代码如下：

```jsx
for (let i = startIndex; i < s.length; i++) {
    if (isPalindrome(s, startIndex, i)) {   // 是回文子串
       // 获取[startIndex,i]在s中的子串
       let str = s.substring(startIndex, i + 1);
       path.push(str);
     } else {                                // 不是回文，跳过
       continue;
     }
   backtracking(s, i + 1); // 寻找i+1为起始位置的子串
   path.pop(); // 回溯过程，弹出本次已经填在的子串
}
```

> 注意：切割过的位置，不能重复切割，所以，backtracking(s, i + 1); 传入下一层的起始位置为i + 1。
> 

### ****判断回文子串****

可以使用双指针法，一个指针从前向后，一个指针从后向前，如果前后指针所指向的元素是相等的，就是回文字符串了。

代码如下：

```jsx
const isPalindrome = (s, start, end) => {
        for (let i = start, j = end; i < j; i++, j--) {
            if (s[i] != s[j]) return false;
        }
        return true;
}
```

JavaScript完整代码：

```jsx
/**
 * @param {string} s
 * @return {string[][]}
 */
var partition = function (s) {
    // 判断回文子串
    const isPalindrome = (s, start, end) => {
        for (let i = start, j = end; i < j; i++, j--) {
            if (s[i] != s[j]) return false;
        }
        return true;
    }
    let res = [];
    let path = []; // 放已经回文的子串
    const backtracking = (s, startIndex) => {
        // 如果起始位置已经大于s的大小，说明已经找到了一组分割方案了
        if (startIndex >= s.length) {
            return res.push([...path]);
        }
        for (let i = startIndex; i < s.length; i++) {
            if (isPalindrome(s, startIndex, i)) {   // 是回文子串
                // 获取[startIndex,i]在s中的子串
                let str = s.substring(startIndex, i + 1);
                path.push(str);
            } else {                                // 不是回文，跳过
                continue;
            }
            backtracking(s, i + 1); // 寻找i+1为起始位置的子串
            path.pop(); // 回溯过程，弹出本次已经填在的子串
        }
    }
    backtracking(s, 0);
    return res;
};
```

### 总结

本题的难点：

- 切割问题可以抽象为组合问题
- 如何模拟那些切割线
- 切割问题中递归如何终止
- 在递归循环中如何截取子串
- 如何判断回文