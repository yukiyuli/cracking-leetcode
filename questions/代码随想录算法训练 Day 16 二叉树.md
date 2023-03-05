# 代码随想录算法训练 Day 16 | 二叉树

Topic: BFS, Binary Tree, DFS

Question: 104. 二叉树的最大深度, 111. 二叉树的最小深度, 222.完全二叉树的节点个数, 559. n叉树的最大深度

Difficulty: Medium

- [x] Done

Completed: December 28, 2022

- [ ] Redo: No


## ****104. 二叉树的最大深度****

[leetcode](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

### 思路：

### ****递归法****

二叉树节点的深度：指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）。

二叉树节点的高度：指从该节点到叶子节点的最长简单路径边的条数后者节点数（取决于高度从0开始还是从1开始）。

本题可以使用前序（中左右），也可以使用后序遍历（左右中），使用前序求的就是深度，使用后序求的是高度。

根节点的高度就是二叉树的最大深度，所以本题中我们通过后序求的根节点高度来求的二叉树最大深度。

递归三步曲：

1. 确定递归函数的参数和返回值：参数就是传入树的根节点，返回就返回这棵树的深度，所以返回值为int类型。

```jsx
const getdepth = function(node)
```

1. 确定终止条件：如果为空节点的话，就返回0，表示高度为0。

```jsx
if(node === null) {
   return 0;
}
```

1. 确定单层递归的逻辑：先求它的左子树的深度，再求右子树的深度，最后取左右深度最大的数值 再+1 （加1是因为算上当前中间节点）就是目前节点为根节点的树的深度。

```jsx
let leftdepth = getdepth(node.left);
let rightdepth = getdepth(node.right);
let depth = 1 + Math.max(leftdepth, rightdepth);
return depth;
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
var maxDepth = function(root) {
    //使用递归的方法 递归三部曲
    //1. 确定递归函数的参数和返回值
    const getdepth = function(node) {
    //2. 确定终止条件
        if(node === null) {
            return 0;
        }
    //3. 确定单层逻辑
        let leftdepth = getdepth(node.left);
        let rightdepth = getdepth(node.right);
        let depth = 1 + Math.max(leftdepth, rightdepth);
        return depth;
    }
    return getdepth(root);
};
```

### ****迭代法****

二刷的时候再学习

## ****559. n叉树的最大深度****

[leetcode](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)

### 思路：

依然可以用递归法和迭代法，来解决这个问题，思路是和二叉树思路一样的。

### 递归法

JavaScript完整代码：

```
/**
 * // Definition for a Node.
 * function Node(val,children) {
 *    this.val = val;
 *    this.children = children;
 * };
 */

/**
 * @param {Node|null} root
 * @return {number}
 */
var maxDepth = function(root) {
// 递归出口
    if (!root) return 0;
    // 最大值
    let max = 0;
    // 遍历子树
    root.children &&
        root.children.forEach(item => {
            // 更新最大值
            max = Math.max(max, maxDepth(item));
        });
    return max + 1;

};
```

### 迭代法

二刷的时候再学习

## ****111. 二叉树的最小深度****

[leetcode](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

### 思路：

本题使用前序遍历和后序遍历都可以，前序求的是深度，后序求的是高度。

使用后序遍历，其实求的是根节点到叶子节点的最小距离，就是求高度的过程。

### ****递归法****

递归三部曲：

1. 确定递归函数的参数和返回值

参数为要传入的二叉树根节点，返回的是int类型的深度。

```jsx
var minDepth1 = function(root)
```

1. 确定终止条件

终止条件也是遇到空节点返回0，表示当前节点的高度为0。

```jsx
if(!root) return 0;
```

1.  确定单层递归的逻辑

这块和求最大深度可就不一样了。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。注意是叶子节点，左右孩子都为空的节点才是叶子节点！

![Untitled](https://user-images.githubusercontent.com/101588752/209870808-78ee6184-66ec-4959-bc52-a881a1c015cd.png)

如果这么求的话，没有左孩子的分支会算为最短深度。

所以，如果左子树为空，右子树不为空，说明最小深度是 1 + 右子树的深度。

反之，右子树为空，左子树不为空，最小深度是 1 + 左子树的深度。 最后如果左右子树都不为空，返回左右子树深度最小值 + 1 。

```jsx
if (root.left && root.right) { // 左右子树都存在，当前节点的高度1+左右子树递归结果的较小值
return 1 + Math.min(minDepth(root.left), minDepth(root.right));
} else if (root.left) {        // 左子树存在，右子树不存在
return 1 + minDepth(root.left);
} else if (root.right) {       // 右子树存在，左子树不存在
return 1 + minDepth(root.right);
} else {                       // 左右子树都不存在，光是当前节点的高度1
return 1;
}
```

> 求二叉树的最小深度和求二叉树的最大深度的差别主要在于处理左右孩子不为空的逻辑。
> 

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
var minDepth = function(root) {
    if (root == null) {            // 递归到null节点，返回高度0
        return 0;
    }
    if (root.left && root.right) { // 左右子树都存在，当前节点的高度1+左右子树递归结果的较小值
        return 1 + Math.min(minDepth(root.left), minDepth(root.right));
    } else if (root.left) {        // 左子树存在，右子树不存在
        return 1 + minDepth(root.left);
    } else if (root.right) {       // 右子树存在，左子树不存在
        return 1 + minDepth(root.right);
    } else {                       // 左右子树都不存在，光是当前节点的高度1
        return 1;
    }

};
```

### 迭代法

本题还可以使用层序遍历的方式来解决，思路是一样的。

> 需要注意的是，只有当左右孩子都为空的时候，才说明遍历到最低点了。如果其中一个孩子不为空则不是最低点
> 

二刷的时候再学习

## ****222. 完全二叉树的节点个数****

[leetcode](https://leetcode.cn/problems/count-complete-tree-nodes/)

### 思路：

### **普通二叉树：**：

递归法**：**

1. 确定递归函数的参数和返回值：参数就是传入树的根节点，返回就返回以该节点为根节点二叉树的节点数量。
2. 确定终止条件：如果为空节点的话，就返回0，表示节点数为0。
3. 确定单层递归的逻辑：先求它的左子树的节点数量，再求右子树的节点数量，最后取总和再加一 （加1是因为算上当前中间节点）就是目前节点为根节点的节点数量。

JavaScript完整代码：

```jsx
var countNodes = function(root) {
    //递归法计算二叉树节点数
    // 1. 确定递归函数参数
    const getNodeSum = function(node) {
    // 2. 确定终止条件
        if(node === null) {
            return 0;
        }
    // 3. 确定单层递归逻辑
        let leftNum = getNodeSum(node.left);
        let rightNum = getNodeSum(node.right);
        return leftNum + rightNum + 1;
    }
    return getNodeSum(root);
};
```

****迭代法****

二刷的时候再学习

### **完全二叉树：**

在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2^(h-1) 个节点。

完全二叉树只有两种情况：

情况一，就是满二叉树，可以直接用 2^树深度 - 1 来计算，注意这里根节点深度为1。

情况二，最后一层叶子节点没有满，分别递归左孩子，和右孩子，递归到某一深度一定会有左孩子或者右孩子为满二叉树，然后依然可以按照情况1来计算。

如图：

![Untitled 1](https://user-images.githubusercontent.com/101588752/209870836-7682969f-0110-45fe-9a35-f92b76d0d6b0.png)

![Untitled 2](https://user-images.githubusercontent.com/101588752/209870847-b8fa9061-a6d0-40d2-8cc5-c02f6d0aa608.png)

可以看出如果整个树不是满二叉树，就递归其左右孩子，直到遇到满二叉树为止，用公式计算这个子树（满二叉树）的节点数量。

在完全二叉树中，如果递归向左遍历的深度等于递归向右遍历的深度，那说明就是满二叉树。如图：

![Untitled 3](https://user-images.githubusercontent.com/101588752/209870862-d385da83-3868-41f7-b594-d54f30f69abb.png)

在完全二叉树中，如果递归向左遍历的深度不等于递归向右遍历的深度，则说明不是满二叉树，如图：

![Untitled 4](https://user-images.githubusercontent.com/101588752/209870872-04020d5c-127a-400a-a2c6-843d8e12ea1e.png)

递归向左遍历的深度等于递归向右遍历的深度，但也不是满二叉树，如图：

![Untitled 5](https://user-images.githubusercontent.com/101588752/209870885-549d1582-719c-47b4-ae8c-c5b20d8df3c4.png)

以上这棵二叉树，它根本就不是一个完全二叉树！

那么，如何判断一个完全二叉树是满二叉树呢？

对于一个根节点，不断访问其左子节点和右子节点，如果左右子节点的数量相等，就是一颗满二叉树。

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
var countNodes = function(root) {
    if(root === null) {
        return 0;
    }
    let left = root.left;
    let right = root.right;
    let leftDepth = 0, rightDepth = 0;
    // 求左节点的深度
    while(left) {
        left = left.left;
        leftDepth++;
    }
    // 统计右子节点的深度
    while(right) {
        right = right.right;
        rightDepth++;
    }
    // 如果是满二叉树，直接公式求节点数量
    if(leftDepth == rightDepth) {
        return Math.pow(2,leftDepth+1) - 1; // 满二叉树的结点数为：2^depth - 1
    }
    // 如果左右深度不相同，就按照后序遍历顺序（左右中），寻找子代中的满二叉树。
    let leftNum = countNodes(root.left);       // 左
    let rightNum = countNodes(root.right);     // 右
    let result = leftNum + rightNum + 1;    // 中
    return result;
};
```
