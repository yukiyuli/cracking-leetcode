# 代码随想录算法训练 Day 14 | 二叉树

Topic: BFS, Binary Tree, DFS

Question: 144. 二叉树的前序遍历, 145. 二叉树的后序遍历, 94. 二叉树的中序遍历

Difficulty: Easy

- [x] Done

Completed: December 26, 2022

- [ ] Redo


## ****二叉树理论基础****

### ****二叉树的种类****

### ****满二叉树****

> 如果一棵二叉树只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，则这棵二叉树为满二叉树。
> 

![Untitled](https://user-images.githubusercontent.com/101588752/209584914-cfe23210-cb83-47ca-ac63-603b22d0f9b3.png)

这棵二叉树为满二叉树，也可以说深度为k，有2^k-1个节点的二叉树。

### ****完全二叉树****

> 定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2^(h-1) 个节点。
> 

举例：

![Untitled 1](https://user-images.githubusercontent.com/101588752/209584937-f9172c06-63fc-4ca2-890b-265d04dbfa43.png)

之前我们讲过优先级队列其实是一个堆，堆就是一棵完全二叉树，同时保证父子节点的顺序关系。

### ****二叉搜索树****

> 二叉搜索树是有数值的了，二叉搜索树是一个有序树。
> 
- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉排序树

下面这两棵树都是搜索树

![Untitled 2](https://user-images.githubusercontent.com/101588752/209584942-e0deebee-6050-4349-87b0-f14fa0adb935.png)

### ****平衡二叉搜索树****

> 平衡二叉搜索树：又被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。
> 

![Untitled 3](https://user-images.githubusercontent.com/101588752/209584952-c7c35c83-ccd1-4cc2-a286-4fcf26e9222a.png)

最后一棵 不是平衡二叉树，因为它的左右两个子树的高度差的绝对值超过了1。

### ****二叉树的存储方式****

二叉树可以链式存储，也可以顺序存储。

顺序存储的方式就是用数组，存储的元素在内存是连续分布的。

顺序存储的方式如图：

![Untitled 4](https://user-images.githubusercontent.com/101588752/209584964-d4269403-50b4-494f-a10a-07bb2443efc2.png)

用数组来存储二叉树如何遍历的呢？

如果父节点的数组下标是 i，那么它的左孩子就是 i * 2 + 1，右孩子就是 i * 2 + 2。

链式存储方式就用指针，通过指针把分布在各个地址的节点串联一起。

链式存储如图：

![Untitled 5](https://user-images.githubusercontent.com/101588752/209584972-234b23d2-94df-4ffe-ace0-a99cbfab6253.png)

### ****二叉树的遍历方式****

二叉树主要有两种遍历方式：

1. 深度优先遍历DFS：先往深走，遇到叶子节点再往回走。
- 前序遍历（递归法，迭代法）：中左右
- 中序遍历（递归法，迭代法）：左中右
- 后序遍历（递归法，迭代法）：左右中
1. 广度优先遍历BFS：一层一层的去遍历。
- 层次遍历（迭代法）

深度优先遍历举例：

![Untitled 6](https://user-images.githubusercontent.com/101588752/209584981-074a24c5-9476-4e8f-8a5e-932bf4cf3625.png)

### ****二叉树的定义****

```jsx
function TreeNode(val, left, right) {
    this.val = (val===undefined ? 0 : val)
    this.left = (left===undefined ? null : left)
    this.right = (right===undefined ? null : right)
}
```

## ****二叉树的递归遍历****

### **144. 二叉树的前序遍历**

[leetcode](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

递归算法的三个要素：

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
2. **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
3. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

前序遍历步骤：

1. **确定递归函数的参数和返回值**：因为要打印出前序遍历节点的数值，所以参数里需要传入vector来放节点的数值，除了这一点就不需要再处理什么数据了也不需要有返回值，所以递归函数返回类型就是void，代码如下：

```jsx
const res=[];
const preorder=function(root)
```

1. **确定终止条件**：在递归的过程中，如何算是递归结束了呢，当然是当前遍历的节点是空了，那么本层递归就要结束了，所以如果当前遍历的这个节点是空，就直接return，代码如下：

```jsx
 if(root===null)return
```

1. **确定单层递归的逻辑**：前序遍历是中左右的循序，所以在单层递归的逻辑，是要先取中节点的数值，代码如下：

```jsx
res.push(root.val); // 中
preorder(root.left); // 左
preorder(root.right); // 右
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
 * @return {number[]}
 */
var preorderTraversal = function(root) {
    const res=[];
    const preorder=function(root) {
        if(root === null) return;
        res.push(root.val); // 中
        preorder(root.left); // 左
        preorder(root.right); // 右
    }
    preorder(root);
    return res;
};
```

### **94. 二叉树的中序遍历**

[leetcode](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

思路和前序遍历一样

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
 * @return {number[]}
 */
var preorderTraversal = function(root) {
    const res=[];
    const preorder=function(root) {
        if(root === null) return;
        preorder(root.left); // 左
        res.push(root.val); // 中
        preorder(root.right); // 右
    }
    preorder(root);
    return res;
};
```

### **145. 二叉树的后序遍历**

[leetcode](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

思路和前序遍历一样

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
 * @return {number[]}
 */
var preorderTraversal = function(root) {
    const res=[];
    const preorder=function(root) {
        if(root === null) return;
        preorder(root.left); // 左
			  preorder(root.right); // 右
        res.push(root.val); // 中
    }
    preorder(root);
    return res;
};
```

## ****二叉树的迭代遍历****

### ****前序遍历（迭代法）****

前序遍历是中左右，每次先处理的是中间节点，那么先将根节点放入栈中，然后将右孩子加入栈，再加入左孩子。

为什么要先加入 右孩子，再加入左孩子呢？ 因为这样出栈的时候才是中左右的顺序。

动画如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1gnbmss7603g30eq0d4b2a.gif](https://tva1.sinaimg.cn/large/008eGmZEly1gnbmss7603g30eq0d4b2a.gif)

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
 * @return {number[]}
 */
var preorderTraversal = function(root) {
		const res =[];
		const stack = [];
    if (root) stack.push(root);
    while(stack.length) {
        const node = stack.pop();
        if(!node) {
            res.push(stack.pop().val);
            continue;
        }
        if (node.right) stack.push(node.right); // 右
        if (node.left) stack.push(node.left); // 左
        stack.push(node); // 中
        stack.push(null);
    }; 
    return res;
};
```

### ****中序遍历（迭代法）****

中序遍历是左中右，先访问的是二叉树顶部的节点，然后一层一层向下访问，直到到达树左面的最底部，再开始处理节点（也就是在把节点的数值放进result数组中）。就造成了处理顺序和访问顺序是不一致的。就需要借用指针的遍历来帮助访问节点，栈则用来处理节点上的元素。

动画如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1gnbmuj244bg30eq0d4kjm.gif](https://tva1.sinaimg.cn/large/008eGmZEly1gnbmuj244bg30eq0d4kjm.gif)

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
 * @return {number[]}
 */
var inorderTraversal = function(root) {
    const res = [];
    const stack = [];
    if (root) stack.push(root);
    while(stack.length) {
        const node = stack.pop();
        if(!node) {
            res.push(stack.pop().val);
            continue;
        }
        if (node.right) stack.push(node.right); // 右
        stack.push(node); // 中
        stack.push(null);
        if (node.left) stack.push(node.left); // 左
    };
    return res;

};
```

### ****后序遍历（迭代法）****

先序遍历是中左右，后续遍历是左右中，只需要调整一下先序遍历的代码顺序，就变成中右左的遍历顺序，然后在反转result数组，输出的结果顺序就是左右中了，如下图：

![Untitled 7](https://user-images.githubusercontent.com/101588752/209585003-e08a2116-6fdb-4027-ac59-8028101f7b1d.png)

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
 * @return {number[]}
 */
var preorderTraversal = function(root) {
		const res =[];
		const stack = [];
    if (root) stack.push(root);
    while(stack.length) {
        const node = stack.pop();
        if(!node) {
            res.push(stack.pop().val);
            continue;
        }
				stack.push(node); // 中
        stack.push(null);
        if (node.right) stack.push(node.right); // 右
        if (node.left) stack.push(node.left); // 左
    }; 
    return res;
};
```

## ****二叉树的统一迭代法****

[二刷的时候再学习](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%BB%9F%E4%B8%80%E8%BF%AD%E4%BB%A3%E6%B3%95.html#%E6%80%BB%E7%BB%93)
