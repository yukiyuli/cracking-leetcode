# 代码随想录算法训练 Day 17 | 二叉树

Topic: BFS, Binary Tree, DFS

Questions: 110. 平衡二叉树, 257.  二叉树的所有路径, 404. 左叶子之和

Difficulty: Easy

- [x] Done

Completed: December 28, 2022

- [ ] Redo


## ****110. 平衡二叉树****

[leetcode](https://leetcode.cn/problems/balanced-binary-tree/)

### 思路：

### ****递归法****

如何判断二叉树是平衡二叉树？

节点的左右子数差高度≤1

递归三步曲分析：

1. 明确递归函数的参数和返回值

参数：当前传入节点。返回值：以当前传入节点为根节点的树的高度。

那么如何标记左右子树是否差值大于1呢？

如果当前传入节点为根节点的二叉树已经不是二叉平衡树了，还返回高度的话就没有意义了。

所以如果已经不是二叉平衡树了，可以返回-1 来标记已经不符合平衡树的规则了。

```jsx
const getDepth = function(node)
```

1. 明确终止条件

递归的过程中依然是遇到空节点了为终止，返回0，表示当前节点为根节点的树高度为0。

```jsx
if(node === null) return 0;
```

1. 确定单层递归的逻辑

后序遍历，分别求出左右子树的高度，然后如果差值小于等于1，则返回当前二叉树的高度，否则返回-1，表示已经不是二叉平衡树了。

```jsx
// 3. 确定单层递归逻辑
let leftDepth = getDepth(node.left); //左子树高度
// 当判定左子树不为平衡二叉树时,即可直接返回-1
if(leftDepth === -1) return -1;
let rightDepth = getDepth(node.right); //右子树高度
// 当判定右子树不为平衡二叉树时,即可直接返回-1
if(rightDepth === -1) return -1;
if(Math.abs(leftDepth - rightDepth) > 1) {
     return -1;
} else {
     return 1 + Math.max(leftDepth, rightDepth);
     }
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
 * @param {TreeNode} root
 * @return {boolean}
 */
var isBalanced = function(root) {
    // 1. 确定递归函数参数以及返回值
    const getDepth = function(node) {
        // 2. 确定递归函数终止条件
        if(node === null) return 0;
        // 3. 确定单层递归逻辑
        let leftDepth = getDepth(node.left);
        // 当判定左子树不为平衡二叉树时,即可直接返回-1
        if(leftDepth === -1) return -1;
        let rightDepth= getDepth(node.right);
        // 当判定右子树不为平衡二叉树时,即可直接返回-1
        if(rightDepth === -1) return -1;
        // 左右子数绝对值差>1,即可直接返回-1
        if (Math.abs(leftDepth - rightDepth) > 1) {
            return -1;
        } else {
            // 取左右子数深度大的
            return 1 + Math.max(leftDepth, rightDepth);
        }
    }
    return !(getDepth(root) === -1);
};
```

## ****257.  二叉树的所有路径****

[leetcode](https://leetcode.cn/problems/binary-tree-paths/)

### 思路：

这道题目涉及到前序遍历和回溯，过程如图：

![Untitled](https://user-images.githubusercontent.com/101588752/209912304-aea0365f-7aae-493d-847a-aebf98b7fc91.png)

### 递归法

递归散步曲：

1. 递归函数函数参数以及返回值

要传入根节点，记录每一条路径的path，和存放结果集的res，这里递归不需要返回值，代码如下：

```jsx
let res = [];
const getPath = function(node,path)
```

1. 确定递归终止条件

当是叶子节点的时候就终止，path来记录路径

```jsx
//2. 确定终止条件，到叶子节点就终止
if(node.left === null && node.right === null) {
   res.push(path);
    return;      
}
```

1. 确定单层递归逻辑

因为是前序遍历，需要先处理中间节点，中间节点就是我们要记录路径上的节点，先放进path中。

然后是递归和回溯的过程，这里递归的时候，如果为空就不进行下一层递归了。所以递归前要加上判断语句，下面要递归的节点是否为空。

此时还没完，递归完，要做回溯啊，因为path 不能一直加入节点，它还要删节点，然后才能加入新的节点。

回溯和递归是一一对应的，有一个递归，就要有一个回溯。

```jsx
path += "->";
// 递归左子树
if(node.left){
   getPath(node.left, path);
}
// 递归右子树
if(node.right){
   getPath(node.right, path);
}
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
 * @param {TreeNode} root
 * @return {string[]}
 */
var binaryTreePaths = function(root) {
    let res= [];
    // 1. 确定递归函数 函数参数
    const getPath = function(node,path) {
        path += node.val; // 中，中为什么写在这里，因为最后一个节点也要加入到path中 
        //2. 确定终止条件，到叶子节点就终止
        if(node.left === null && node.right === null) {
            res.push(path);
            return;
        }
        path += "->";
        // 递归左子树
        if(node.left){
            getPath(node.left, path);
        }
        // 递归右子树
        if(node.right){
            getPath(node.right, path);
        }
    }
    getPath(root, '');
    return res;
};
```

### 迭代法

二刷的时候再学习

## ****404. 左叶子之和****

[leetcode](https://leetcode.cn/problems/sum-of-left-leaves/)

### 思路：

> 首先要注意是判断左叶子，不是二叉树左侧节点。
> 

左叶子的明确定义：节点A的左孩子不为空，且左孩子的左右孩子都为空（说明是叶子节点），那么A节点的左孩子为左叶子节点。

如下图中二叉树，左叶子之和=0，因为这棵树根本没有左叶子！

![Untitled 1](https://user-images.githubusercontent.com/101588752/209912328-b29ca0cb-f72a-4d79-a8d7-a7dcdc57b235.png)

如下图中二叉树，左叶子之和=21

![Untitled 2](https://user-images.githubusercontent.com/101588752/209912338-18d73443-e2d2-449f-8448-2633e15a3798.png)

判断当前节点是不是左叶子是无法判断的，必须要通过节点的父节点来判断其左孩子是不是左叶子。

如果该节点的左节点不为空，该节点的左节点的左节点为空，该节点的左节点的右节点为空，则找到了一个左叶子。

### ****递归法****

递归三部曲：

1. 确定递归函数的参数和返回值

判断一个树的左叶子节点之和，那么一定要传入树的根节点。

```jsx
var sumOfLeftLeaves = function(root)
```

1. 确定终止条件

如果遍历到空节点，那么左叶子值一定是0

```jsx
if(root === null) return 0;
```

注意，只有当前遍历的节点是父节点，才能判断其子节点是不是左叶子。 所以如果当前遍历的节点是叶子节点，那其左叶子也必定是0，那么终止条件为：

```jsx
if(root === null) return 0;
if(root.left === null && root.right === null) return 0;
```

1.  确定单层递归的逻辑

使用后序遍历。

当遇到左叶子节点的时候，记录数值，然后通过递归求取左子树左叶子之和，和 右子树左叶子之和，相加便是整个树的左叶子之和。

```jsx
// 3. 单层递归
        let leftValue = sumOfLeftLeaves(root. left);    // 左
        if (root.left && !root.left.left && !root.left.right) { // 左子树就是一个左叶子的情况
            leftValue = root.left.val;
        }
        let rightValue = sumOfLeftLeaves(root.right);  // 右
        let sum = leftValue + rightValue;               // 中
        return sum;
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
 * @param {TreeNode} root
 * @return {number}
 */
var sumOfLeftLeaves = function(root) {
        // 2. 终止条件
        if (root === null) return 0;
        if (root.left === null && root.right === null) return 0;
        // 3. 单层递归
        let leftValue = sumOfLeftLeaves(root. left);    // 左
        if (root.left && !root.left.left && !root.left.right) { // 左子树就是一个左叶子的情况
            leftValue = root.left.val;
        }
        let rightValue = sumOfLeftLeaves(root.right);  // 右
        let sum = leftValue + rightValue;               // 中
        return sum;
};
```

### 迭代法

本题迭代法使用前中后序都是可以的，只要把左叶子节点统计出来，就可以了

二刷的时候再学习
