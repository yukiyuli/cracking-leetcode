# 代码随想录算法训练 Day 18 | 二叉树

Topic: BFS, Binary Tree, DFS

Question: 105. 从前序与中序遍历序列构造二叉树, 106. 从中序与后序遍历序列构造二叉树, 112. 路径总和, 113. 路径总和ii, 513. 找树左下角的值

Difficulty: Medium

- [x] Done

Completed: December 30, 2022

- [ ] Redo


## ****513. 找树左下角的值****

[leetcode](https://leetcode.cn/problems/find-bottom-left-tree-value/solutions/)

### 思路：

### ****递归法****

关键是找到深度最大的叶子结点。

如何找最左边的呢？本题前中后序遍历都可以，保证优先左边搜索，然后记录深度最大的叶子节点，此时就是树的最后一行最左边的值。

递归三步曲分析：

1. 明确递归函数的参数和返回值

参数必须有要遍历的树的根节点，还有就是一个int型的变量用来记录最长深度。 这里就不需要返回值了，所以递归函数的返回类型为void。

本题还需要类里的两个全局变量，maxLen用来记录最大深度，result记录最大深度最左节点的数值。

代码如下：

```jsx
let maxDepth = 0, res = null;
    // 1. 确定递归函数的函数参数
    const dfsTree = function(node, depth)
```

1. 明确终止条件

当遇到叶子节点的时候，就需要统计一下最大的深度了，所以需要遇到叶子节点来更新最大深度。

代码如下：

```jsx
if(node.left === null && node.right === null) {
    if (depth > maxDepth) {
         maxDepth = depth;
         res = node.val;
    }
}
```

1. 确定单层递归的逻辑

```
// 3. 单层遍历
        if (node.left) {    // 左
            depth ++;
            dfsTree(node.left, depth);
            depth --;  // 回溯
        }
        if (node.right) {    // 右
            depth ++;
            dfsTree(node.right, depth);
            depth --;  // 回溯
        }
```

在找最大深度的时候，递归的过程中依然要使用回溯。

代码如下

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
 * @return {number}
 */
var findBottomLeftValue = function(root) {
    let maxDepth = 0, res = null;
    // 1. 确定递归函数的函数参数
    const dfsTree = function(node, depth) {
        // 2. 确定递归函数终止条件
        if(node.left === null && node.right === null) {
            if (depth > maxDepth) {
                maxDepth = depth;
                res = node.val;
            }
        }
        // 3. 单层遍历
        if (node.left) {    // 左
            depth ++;
            dfsTree(node.left, depth);
            depth --;  // 回溯
        }
        if (node.right) {    // 右
            depth ++;
            dfsTree(node.right, depth);
            depth --;  // 回溯
        }
        
    }
    dfsTree(root, 1);
    return res;
};
```

### 迭代法

本题迭代法比递归简单很多，也好理解很多。只需要记录最后一行第一个节点的数值就可以了。

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
var findBottomLeftValue = function(root) {
    let queue = [];
    if(root === null) { 
        return null;
    }
    queue.push(root);
    let res;
    while(queue.length) {
        let length = queue.length;
        for(let i = 0; i < length; i++) {
            let node = queue.shift();
            if(i === 0) {
                res = node.val;
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    return res;
};
```

## ****112. 路径总和****

[leetcode](https://leetcode.cn/problems/path-sum/)

### 思路：

### 递归法：

本题前中后序遍历都可以。

递归三步曲：

1. 递归函数函数参数以及返回值

参数：需要二叉树的根节点，还需要一个计数器，这个计数器用来计算二叉树的一条边之和是否正好是目标和，计数器为int型。

返回值：

递归函数什么时候需要返回值，什么时候不需要返回值？这结如下三点：

- 如果需要搜索整棵二叉树且不用处理递归返回值，递归函数就不要返回值。（这种情况如113. 路径总和ii）
- 如果需要搜索整棵二叉树且需要处理递归返回值，递归函数就需要返回值。 （这种情况我们在236. 二叉树的最近公共祖先中介绍）
- 如果要搜索其中一条符合条件的路径，那么递归一定需要返回值，因为遇到符合条件的路径了就要及时返回。（本题的情况）

而本题要找一条符合条件的路径，所以递归函数需要返回值，及时返回，那么返回类型是什么呢？

如图所示：

![Untitled](https://user-images.githubusercontent.com/101588752/210129720-f86d97e0-a9f9-4425-b3e5-979f990d0f6c.png)

图中可以看出，遍历的路线，并不要遍历整棵树，所以递归函数需要返回值，用箭头函数。

代码如下：

```jsx
const traversal = (node, count) => {
}
```

1. 确定递归终止条件

可以用递减，让计数器count初始为目标和，然后每次减去遍历路径节点上的数值。

> 如果用累加然后判断是否等于目标和，代码比较麻烦
> 

如果最后count == 0，同时到了叶子节点的话，说明找到了目标和。

如果遍历到了叶子节点，count不为0，就是没找到。

代码如下：

```jsx
// 遇到叶子节点，并且计数为0
if (!node.left && !node.right && count ===0 ) return true;
// 遇到叶子节点直接返回
if (!node.left && !node.right) return false;
```

1. 确定单层递归逻辑

递归函数是有返回值的，如果递归函数返回true，说明找到了合适的路径，应该立刻返回。

有递归就有回溯，代码里把回溯体现出来。

代码如下：

```jsx
// 左，遇到叶子节点返回true，则直接返回true
if (node.left) {
   count -= node.left.val;   // 递归，处理节点;
   if (traversal(node.left, count)) return true;
   count += node.left.val;   // 回溯，撤销处理结果
}
// 右，遇到叶子节点返回true，则直接返回true
if (node.right) {
   count -= node.right.val;   // 递归，处理节点;
   if (traversal(node.right, count)) return true;
   count += node.right.val;   // 回溯，撤销处理结果
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
 * @param {number} targetSum
 * @return {boolean}
 */
var hasPathSum = function(root, targetSum) {
    const traversal = (node, count) => {
        // 2. 递归终止条件
        // 遇到叶子节点，并且计数为0
        if (!node.left && !node.right && count ===0 ) return true;
        // 遇到叶子节点直接返回
        if (!node.left && !node.right) return false;

        // 3. 单层遍历
        // 左，遇到叶子节点返回true，则直接返回true
        if (node.left) {
            count -= node.left.val;   // 递归，处理节点;
            if (traversal(node.left, count)) return true;
            count += node.left.val;   // 回溯，撤销处理结果
        }
        // 右，遇到叶子节点返回true，则直接返回true
        if (node.right) {
            count -= node.right.val;   // 递归，处理节点;
            if (traversal(node.right, count)) return true;
            count += node.right.val;   // 回溯，撤销处理结果
        }
        return false;
    }
    if (!root) return false;
    return traversal(root, targetSum - root.val);
};
```

### 迭代法

[二刷的时候再学习](https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html#%E6%80%9D%E8%B7%AF-2)

## ****113. 路径总和ii****

[leetcode](https://leetcode.cn/problems/path-sum-ii/)

### 思路：

要遍历整个树，找到所有路径，所以递归函数不要返回值！

如图：

![Untitled 1](https://user-images.githubusercontent.com/101588752/210129726-f741c175-5aba-4790-823d-2d92d96bd1cb.png)

### ****递归法****

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
 * @param {number} targetSum
 * @return {number[][]}
 */
var pathSum = function(root, targetSum) {
    // 要遍历整个树找到所有路径，所以递归函数不需要返回值, 与112不同
    const res=[];
    const travelsal = (node, count, path) => {
        // 2. 终止条件
        // 遇到了叶子节点且找到了和为sum的路径
        if (!node.left && !node.right && count===0) {
            res.push([...path]); // 不能写res.push(path), 要深拷贝
            return;
        }
        // 遇到叶子节点而没有找到合适的边，直接返回
        if (!node.left && !node.right) return;

        // 3. 单层遍历
        // 左 （空节点不遍历）
        if (node.left) {
            path.push(node.left.val);
            count -= node.left.val;
            travelsal (node.left, count, path);   // 递归
            count += node.left.val;   // 回溯
            path.pop();    // 回溯
        }
        // 右 （空节点不遍历）
        if (node.right) {
            path.push(node.right.val);
            count -= node.right.val;
            travelsal (node.right, count, path);   // 递归
            count += node.right.val;   // 回溯
            path.pop();    // 回溯
        }
        return;
    }
    if (!root) return res;
    travelsal(root, targetSum - root.val, [root.val]); // 把根节点放进路径
    return res;
};
```

## ****106. 从中序与后序遍历序列构造二叉树****

[leetcode](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### 思路：

画一颗二叉树，流程如图：

![Untitled 2](https://user-images.githubusercontent.com/101588752/210129731-4986842e-4c8f-4897-96ca-67044c1b9b80.png)

一层一层切割，步骤如下：

- 第一步：如果数组大小为零的话，说明是空节点了。
- 第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。
- 第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点
- 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）
- 第五步：切割后序数组，切成后序左数组和后序右数组
- 第六步：递归处理左区间和右区间

代码如下：

```jsx
const travelsal = (inBegin, inEnd, postBegin, postEnd) => {
		// 第一步
    if( postBegin === postEnd ) return null;

    // 第二步：后序遍历数组最后一个元素，就是当前的中间节点
     let rootVal = postorder[postEnd -1];

    // 创建中间节点
    const root = new TreeNode(rootVal);
    
    // 第三步：找切割点
    const index = inorder.indexOf(rootVal);

    // 第四步：切割中序数组，得到中序左数组和中序右数组
    // 第五步：切割后序数组，得到后序左数组和后序右数组

    // 第六步
    root.left = travelsal (中序左数组, 后序左数组);
    root.right = travelsal (中序右数组, 后序右数组);

    return root;
};
```

本题的难点在于如何切割，以及边界值。

首先要切割中序数组，因为切割点在后序数组的最后一个元素，就是用这个元素来切割中序数组的，所以必要先切割中序数组。

用rootIndex来保存切终须数组之前在后序里边中节点的位置。

在切割的过程中会产生四个区间，始终保持用一种不变量。

代码如下：

```jsx
// 左中序区间，左闭右开[leftInBegin, leftInEnd)
    let leftInBegin = inBegin;
    let leftInEnd = index;
    // 右中序区间，左闭右开[rightInBegin, rightInEnd)
    let rightInBegin = index + 1 ;
    let rightInEnd = inEnd;
```

然后切割后序数组，用中序数组做区间大小切后序数组左区间，得到左后序和右后序。

代码如下：

```jsx
// 左后序区间，左闭右开[leftPostorderBegin, leftPostorderEnd)
    let leftPostBegin = postBegin; 
    let leftPostEnd = postBegin + index - inBegin ;  // 终止位置是 需要加上 中序区间的大小size
    // 右后序区间，左闭右开[rightPostorderBegin, rightPostorderEnd)
    let rightPostBegin = postBegin + (index - inBegin);
    let rightPostEnd = postEnd -1 ;  // 排除最后一个元素，已经作为节点了
```

中序数组切成了左中序数组和右中序数组，后序数组切割成左后序数组和右后序数组。

接下来可以递归了，代码如下：

```jsx
root.left = travelsal(leftInBegin, leftInEnd, leftPostBegin, leftPostEnd);
root.right = travelsal(rightInBegin, rightInEnd, rightPostBegin, rightPostEnd);
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
 * @param {number[]} inorder
 * @param {number[]} postorder
 * @return {TreeNode}
 */
var buildTree = function(inorder, postorder) {
    const travelsal = (inBegin, inEnd, postBegin, postEnd) => {
		    // 第一步    
        if( postBegin === postEnd ) return null;

        // 第二步：后序遍历数组最后一个元素，就是当前的中间节点
        let rootVal = postorder[postEnd -1];

        // rootVal 在中序遍历数组中的索引
        let index = inorder.indexOf(rootVal);

        // 先构造出当前根节点
        let root = new TreeNode(rootVal);

        // 第四步：切割中序数组，得到中序左数组和中序右数组
        // 左中序区间，左闭右开[leftInBegin, leftInEnd)
        let leftInBegin = inBegin;
        let leftInEnd = index;
        // 右中序区间，左闭右开[rightInBegin, rightInEnd)
        let rightInBegin = index + 1 ;
        let rightInEnd = inEnd; 

        // 第五步：切割后序数组，得到后序左数组和后序右数组
        // 左后序区间，左闭右开[leftPostorderBegin, leftPostorderEnd)
        let leftPostBegin = postBegin; 
        let leftPostEnd = postBegin + index - inBegin ;  // 终止位置是 需要加上 中序区间的大小size
        // 右后序区间，左闭右开[rightPostorderBegin, rightPostorderEnd)
        let rightPostBegin = postBegin + (index - inBegin);
        let rightPostEnd = postEnd -1 ;  // 排除最后一个元素，已经作为节点了

        // 递归构造左右子树
        root.left = travelsal(leftInBegin, leftInEnd, leftPostBegin, leftPostEnd);
        root.right = travelsal(rightInBegin, rightInEnd, rightPostBegin, rightPostEnd);
        return root;
    };

    // 左闭右开的原则
    return travelsal(0, inorder.length, 0, postorder.length );

};
```

## 105. ****从前序与中序遍历序列构造二叉树****

[leetcode](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### ****思路:****

本题和106是一样的道理。

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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    const traversal = (preBegin, preEnd, inBegin, inEnd) => {
        if (preBegin === preEnd) return null;

        // root 节点对应的值就是前序遍历数组的第一个元素
        let rootVal = preorder[preBegin];  // 注意用preorderBegin 不要用0

        // rootVal 在中序遍历数组中的索引
        let index = inorder.indexOf(rootVal);
        
        // 先构造出当前根节点
        let root = new TreeNode(rootVal);

        // 切割中序数组
        // 中序左区间，左闭右开[leftInBegin, leftInEnd)
        let leftInBegin = inBegin;
        let leftInEnd = index;
        // 中序右区间，左闭右开[rightInBegin, rightInEnd)
        let rightInBegin = index + 1;
        let rightInEnd = inEnd;

        // 切割前序数组
        // 前序左区间，左闭右开[leftPreBegin, leftPreEnd)
        let leftPreBegin = preBegin + 1; 
        // 终止位置是起始位置加上中序左区间的大小size
        let leftPreEnd = preBegin + 1 + index - inBegin;
        // 前序右区间, 左闭右开[rightPreorderBegin, rightPreorderEnd)
        let rightPreBegin = preBegin + 1 + (index - inBegin);
        let rightPreEnd = preEnd;

        // 递归构造左右子树
        root.left = traversal (leftPreBegin, leftPreEnd, leftInBegin, leftInEnd);
        root.right = traversal (rightPreBegin, rightPreEnd, rightInBegin, rightInEnd);

        return root;
    };
  return traversal(0, preorder.length, 0, inorder.length);

};
```

# **思考题**

前序和中序可以唯一确定一棵二叉树。

后序和中序可以唯一确定一棵二叉树。

那么前序和后序可不可以唯一确定一棵二叉树呢？

**前序和后序不能唯一确定一棵二叉树！**，因为没有中序遍历无法确定左右部分，也就是无法分割。

举一个例子：

![https://img-blog.csdnimg.cn/20210203154720326.png](https://img-blog.csdnimg.cn/20210203154720326.png)

tree1 的前序遍历是[1 2 3]， 后序遍历是[3 2 1]。

tree2 的前序遍历是[1 2 3]， 后序遍历是[3 2 1]。

那么tree1 和 tree2 的前序和后序完全相同，这是一棵树么，很明显是两棵树！

所以前序和后序不能唯一确定一棵二叉树！
