# 代码随想录算法训练 Day 20 | 二叉树

Completed: December 31, 2022
Difficulty: Easy
Done: Yes
Question: 617. 合并二叉树, 654. 最大二叉树, 700. 二叉搜索树中的搜索, 98. 验证二叉搜索树
Redo: No
Topic: BFS, Binary Tree, DFS

## ****654. 最大二叉树****

[leetcode](https://leetcode.cn/problems/maximum-binary-tree/)

### 思路：

### ****递归法****

构造树一般采用的是前序遍历，因为先构造中间节点，然后递归构造左子树和右子树。

递归三步曲：

1. 明确递归函数的参数和返回值

参数传入的是存放元素的数组，返回该数组构造的二叉树的头结点，返回类型是指向节点的指针

代码如下：

```
const build = (arr, left, right) => {
}
```

1. 明确终止条件

定义一个新的节点，并把这个数组的数值赋给新的节点，然后返回这个节点。 这表示一个数组大小是1的时候，构造了一个新的节点，并返回。

题目中说了输入的数组大小一定是大于等于1的，所以我们不用考虑小于1的情况。

那么当递归遍历的时候，如果传入的数组大小为1，说明遍历到了叶子节点了。

那么应该定义一个新的节点，并把这个数组的数值赋给新的节点，然后返回这个节点。 这表示一个数组大小是1的时候，构造了一个新的节点，并返回。

代码如下：

```
if (left >= right) return null
```

1. 确定单层递归的逻辑

这里有三步工作

(1) 先要找到数组中最大的值和对应的下标， 最大的值构造根节点，下标用来下一步分割数组。

这里有三步工作

(1) 先要找到数组中最大的值和对应的下标， 最大的值构造根节点，下标用来下一步分割数组。

代码如下：

```
// 最大值所在位置
let maxIndex = left;
// 最大值
let maxValue = arr[maxIndex];
// 找到数组中的最大值及对应的索引
for (let i = left +1; i < right; i++) {
    if (arr[i] > maxValue) {
        maxValue = arr[i];
        maxIndex = i;
     }
}
let root = new TreeNode(maxValue);
```

(2) 最大值所在的下标左区间 构造左子树

(2) 最大值所在的下标左区间 构造左子树

这里要判断maxValueIndex > 0，因为要保证左区间至少有一个数值。

代码如下：

```jsx
// 左闭右开：[left, maxValueIndex)
root.left = traversal(arr, left, maxIndex);
```

(3) 最大值所在的下标右区间 构造右子树

代码如下：

```jsx
// 左闭右开：[maxValueIndex + 1, right)
root.right = traversal(arr, maxIndex + 1, right);
```

JavaScript完整代码：

```jsx
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {number[]} nums
 * @return {TreeNode}
 */
var constructMaximumBinaryTree = function(nums) {
    const traversal = (arr, left, right) => {
        if (left >= right) return null;

        // 最大值所在位置
        let maxIndex = left;
        // 最大值
        let maxValue = arr[maxIndex];
        // 找到数组中的最大值及对应的索引
        for (let i = left +1; i < right; i++) {
            if (arr[i] > maxValue) {
                maxValue = arr[i];
                maxIndex = i;
            }
        }
        let root = new TreeNode(maxValue);

        // 递归调用构造左右子树
        // 左闭右开：[left, maxValueIndex)
        root.left = traversal(arr, left, maxIndex);
        // 左闭右开：[maxValueIndex + 1, right)
        root.right = traversal(arr, maxIndex + 1, right);
        return root;
    };
    let root = traversal(nums, 0, nums.length);
    return root;

};
```

## ****617. 合并二叉树****

[leetcode](https://leetcode.cn/problems/merge-two-binary-trees/)

### 思路：

### 递归法：

本题三种遍历都可以。

递归三步曲：

1. 递归函数函数参数以及返回值

首先要合入两个二叉树，那么参数至少是要传入两个二叉树的根节点，返回值就是合并之后二叉树的根节点。

代码如下：

```jsx
var mergeTrees = function(root1, root2)
```

1. 确定递归终止条件

因为是传入了两个树，那么就有两个树遍历的节点root1 和 root2，如果root1 === null 了，两个树合并就应该是 root2 了（如果root2也为null也无所谓，合并之后就是null）。

反过来如果root2 === null，那么两个数合并就是root1（如果roott1也为null也无所谓，合并之后就是nulll）。

代码如下：

```jsx
if (root1 === null) return root2; // 如果root1为空，合并之后就应该是root2
if (root2 === null) return root1; // 如果root2为空，合并之后就应该是root1
```

1. 确定单层递归逻辑

不创建新的树，在root1上做修改。单层递归中，就要把两棵树的元素加到一起

代码如下：

```jsx
root1.val += root2.val;                             // 中
root1.left = mergeTrees(root1.left, root2.left);      // 左
root1.right = mergeTrees(root1.right, root2.right);   // 右
return root1;
```

JavaScript完整代码：

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root1
 * @param {TreeNode} root2
 * @return {TreeNode}
 */
var mergeTrees = function(root1, root2) {
        if (root1 === null) return root2; // 如果root1为空，合并之后就应该是root2
        if (root2 === null) return root1; // 如果root2为空，合并之后就应该是root1
        // 修改了root1的数值和结构
        root1.val += root2.val;                             // 中
        root1.left = mergeTrees(root1.left, root2.left);      // 左
        root1.right = mergeTrees(root1.right, root2.right);   // 右
        return root1;
};
```

### 迭代法

[二刷的时候再学习](https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF)

## ****700. 二叉搜索树中的搜索****

[leetcode](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

### 思路：

二叉搜索树定义：

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉搜索树

### ****递归法****

递归三步曲：

1. 确定递归函数的参数和返回值

递归函数的参数传入的就是根节点和要搜索的数值，返回的就是以这个搜索数值所在的节点。

代码如下：

```jsx
var searchBST = function(root, val
```

1. 确定终止条件

如果root为空，或者找到这个数值了，就返回root节点。

```jsx
 if (!root || root.val === val) return root;
```

代码如下：

1. 确定单层递归的逻辑

因为二叉搜索树的节点是有序的，所以可以有方向的去搜索。

如果root. val > val，搜索左子树，如果root. val < val，就搜索右子树，最后如果都没有搜索到，就返回null。

代码如下：

```jsx
if (val < root.val) return searchBST(root.left, val)
if (val > root.val) return searchBST(root.right, val)
```

不要忘了递归函数还有返回值，返回值是左子树如果搜索到了val，要将该节点返回。

JavaScript完整代码：

```jsx
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} val
 * @return {TreeNode}
 */
var searchBST = function(root, val) {
    if (!root || root.val === val) return root;
    if (val < root.val) return searchBST(root.left, val)
    if (val > root.val) return searchBST(root.right, val)
};
```

### 迭代法

因为二叉搜索树的特殊性，也就是节点的有序性，不使用辅助栈或者队列就可以写出迭代法。

对于二叉搜索树，不需要回溯的过程，因为节点的有序性就帮我们确定了搜索的方向。

例如要搜索元素为3的节点，我们不需要搜索其他节点，也不需要做回溯，查找的路径已经规划好了。

中间节点如果大于3就向左走，如果小于3就向右走，如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2020%20%E4%BA%8C%E5%8F%89%E6%A0%91%205b99d1914817463e81781f3607d93435/Untitled.png)

JavaScript完整代码：

```jsx
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} val
 * @return {TreeNode}
 */
var searchBST = function(root, val) {
    while (root != null) {
        if (root.val > val) root = root.left;
        else if (root.val < val) root = root.right;
        else return root;
    }   
    return null;
};
```

## ****98. 验证二叉搜索树****

[leetcode](https://leetcode.cn/problems/validate-binary-search-tree/)

### 思路：

中序遍历下，输出的二叉搜索树节点的数值是有序序列。

有了这个特性，**验证二叉搜索树，就相当于变成了判断一个序列是不是递增的了。**

### 递归法

这道题目比较容易陷入两个陷阱：

- 陷阱1

不能单纯的比较左节点小于中间节点，右节点大于中间节点就完事了。

写出了类似这样的代码：

```jsx
if (root->val > root->left->val && root->val < root->right->val) {
    return true;
} else {
    return false;
}
```

要比较的是 左子树所有节点小于中间节点，右子树所有节点大于中间节点。所以以上代码是错误的。

例如： [10,5,15,null,null,6,20] 这个case：

![https://img-blog.csdnimg.cn/20200812191501419.png](https://img-blog.csdnimg.cn/20200812191501419.png)

节点10大于左节点5，小于右节点15，但右子树里出现了一个6 这就不符合了！

- 陷阱2

样例中最小节点可能是int的最小值，如果这样使用最小的int来比较也是不行的。

此时可以初始化比较元素为longlong的最小值。

问题可以进一步演进：如果样例中根节点的val 可能是longlong的最小值 又要怎么办呢？文中会解答。

递归三步曲：

1. 确定递归函数，返回值以及参数

代码如下：

```jsx
const inOrder = (root) =>
```

1. 确定终止条件

如果是空节点也是二叉搜索树。

代码如下：

```jsx
if (root === null) return true;
```

1. 确定单层递归的逻辑

中序遍历，一直更新maxVal，一旦发现maxVal >= root.val，就返回false，注意元素相同时候也要返回false。

代码如下：

```jsx
let left = inOrder(root.left);
if (pre !== null && pre.val >= root.val) return false;
pre = root;

let right = inOrder(root.right);
return left && right;
```

如果测试数据中有 longlong的最小值，怎么办？

不可能在初始化一个更小的值了吧。 建议避免 初始化最小值，如下方法取到最左面节点的数值来比较。

JavaScript完整代码：

```jsx
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */

var isValidBST = function(root) {
    let pre = null; // 用来记录前一个节点
    const inOrder = (root) => {
        if (root === null) return true;

        let left = inOrder(root.left);
        if (pre !== null && pre.val >= root.val) return false;
        pre = root;

        let right = inOrder(root.right);
        return left && right;
    }
    return inOrder(root);
};
```

### 迭代法

[二刷的时候再看](https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E9%80%92%E5%BD%92%E6%B3%95)