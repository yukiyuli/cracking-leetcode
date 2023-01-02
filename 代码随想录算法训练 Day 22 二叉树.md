# 代码随想录算法训练 Day 22 | 二叉树

Completed: January 2, 2023
Difficulty: Medium
Done: Yes
Question: 235. 二叉搜索树的最近公共祖先, 450. 删除二叉搜索树中的节点, 701. 二叉搜索树中的插入操作
Redo: No
Topic: BFS, Binary Tree, DFS

## ****235. 二叉搜索树的最近公共祖先****

[leetcode](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

### 思路：

因为是有序树，所有如果中间节点是 q 和 p 的公共祖先，那么中节点的数组 一定是在 [p, q]区间的。即中节点 > p && 中节点 < q 或者 中节点 > q && 中节点 < p。

那么只要从上到下去遍历，遇到 cur节点是数值在[p, q]区间中则一定可以说明该节点cur就是q 和 p的公共祖先，并且一定是最近公共祖先。

原因，如图，根节点搜索，第一次遇到 cur节点是数值在[p, q]区间中，即节点5，此时可以说明 p 和 q 一定分别存在于节点5的左子树和右子树中。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2022%20%E4%BA%8C%E5%8F%89%E6%A0%91%209859a9751dfb4761b0f8cadbdd431bfa/Untitled.png)

此时节点5是不是最近公共祖先？ 如果从节点5继续向左遍历，那么将错过成为q的祖先， 如果从节点5继续向右遍历则错过成为p的祖先。

所以当我们从上向下去递归遍历，第一次遇到 cur节点是数值在[p, q]区间中，那么cur就是 p和q的最近公共祖先。

### ****递归法****

本题递归法不涉及到前中后序了（这里没有中节点的处理逻辑，遍历顺序无所谓了）。

如图所示：p为节点6，q为节点9

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2022%20%E4%BA%8C%E5%8F%89%E6%A0%91%209859a9751dfb4761b0f8cadbdd431bfa/Untitled%201.png)

可以看出直接按照指定的方向，就可以找到节点8，为最近公共祖先，而且不需要遍历整棵树，找到结果直接返回！

递归三步曲：

1. 确定递归函数返回值以及参数

参数就是当前节点，以及两个结点 p、q。

代码如下：

```jsx
var lowestCommonAncestor = function (root, p, q)
```

1. 确定终止条件

遇到空返回就可以了，代码如下：

```jsx
if (!root) return null; 
```

1. 确定单层递归的逻辑

在遍历二叉搜索树的时候就是寻找区间[p.val, q.val]（注意这里是左闭又闭）

本题是标准的搜索一条边的写法，遇到递归函数的返回值，如果不为空，立刻返回。

如果 cur.val > p.val，同时 cur.val >q.val，那么就应该向右遍历（说明目标区间在右子树上）。

剩下的情况，就是cur节点在区间（p.val <= cur.val && cur.val <= q.val）或者 （q.val <= cur.val && cur.val <= p.val）中，那么cur就是最近公共祖先了，直接返回cur。

代码如下：

```jsx
if (root.val > p.val && root.val > q.val) {. // 左
    return root.left = lowestCommonAncestor(root.left, p, q) 
}
if (root.val < p.val && root.val < q.val) {  // 右
    return root.right = lowestCommonAncestor(root.right, p, q)
}
return root;
```

JavaScript完整代码：

```jsx
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function (root, p, q) {
		// 1. 使用给定的递归函数lowestCommonAncestor
		// 2. 确定递归终止条件
    if (!root) return null;
		// 向左子树查询
    if (root.val > p.val && root.val > q.val) {
        return root.left = lowestCommonAncestor(root.left, p, q)
    }
		// 向右子树查询
    if (root.val < p.val && root.val < q.val) {
        return root.right = lowestCommonAncestor(root.right, p, q)
    }
    return root;
};
```

### 迭代法

利用其有序性，迭代的方式还是比较简单的，解题思路和递归法是一样的。

JavaScript完整代码：

```jsx
var lowestCommonAncestor = function(root, p, q) {
    // 使用迭代的方法
    while(root) {
        if(root.val > p.val && root.val > q.val) {
            root = root.left;
        }else if(root.val < p.val && root.val < q.val) {
            root = root.right;
        }else {
            return root;
        }

    }
    return null;
};
```

## ****701. 二叉搜索树中的插入操作****

[leetcode](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

### 思路：

可以不考虑题目中提示所说的改变树的结构的插入方式。只要按照二叉搜索树的规则去遍历，遇到空节点就插入节点就可以了。

### 递归法：

递归三步曲：

1. 确定递归函数参数以及返回值

参数就是根节点指针，以及要插入元素，可以利用返回值完成新加入的节点与其父节点的赋值操作。

代码如下：

```jsx
var insertIntoBST = function (root, val)
```

1. 确定终止条件

终止条件就是找到遍历的节点为null的时候，就是要插入节点的位置了，并把插入的节点返回。

代码如下：

```jsx
if (!root) {
   let node = new TreeNode(val);
   return node;
}
```

1. 确定单层递归的逻辑

搜索树是有方向的，可以根据插入元素的数值，决定递归方向。不需要遍历整棵树。

通过递归函数返回值完成了新加入节点的父子关系赋值操作了，下一层将加入节点返回，本层用root.left或者root.right将其接住，代码就简洁不少

代码如下：

```jsx
if (root.val > val)
    root.left = insertIntoBST(root.left, val);
else if (root.val < val)
    root.right = insertIntoBST(root.right, val);
return root;
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
 * @param {number} val
 * @return {TreeNode}
 */
var insertIntoBST = function (root, val) {
    if (!root) {
        let node = new TreeNode(val);
        return node;
    }
    if (root.val > val)
        root.left = insertIntoBST(root.left, val);
    else if (root.val < val)
        root.right = insertIntoBST(root.right, val);
    return root;
};
```

### 迭代法

[二刷的时候再学习](https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html#%E9%80%92%E5%BD%92)

## ****450. 删除二叉搜索树中的节点****

[leetcode](https://leetcode.cn/problems/delete-node-in-a-bst/)

### 思路：

递归三步曲：

1. 确定递归函数的参数和返回值

通过递归返回值删除节点。

代码如下：

```jsx
var deleteNode = function (root, key)
```

1. 确定终止条件

删除节点的操作在终止条件里。

五种情况：

- 第一种情况：

没找到删除的节点，遍历到空节点直接返回了.

- 第二种情况：

找到删除的节点，左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点

- 第三种情况：

找到删除的节点，删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点

- 第四种情况：

找到删除的节点，删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点

- 第五种情况：

找到删除的节点，左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点。

第五种情况动画如下：

[https://camo.githubusercontent.com/4a6ff55863bf30ec834ec9fb9d7bde209f4ecc33b71869ba7d55de4cd14ab970/68747470733a2f2f747661312e73696e61696d672e636e2f6c617267652f30303865476d5a456c7931676e626a336b3539366d673330647130616967797a2e676966](https://camo.githubusercontent.com/4a6ff55863bf30ec834ec9fb9d7bde209f4ecc33b71869ba7d55de4cd14ab970/68747470733a2f2f747661312e73696e61696d672e636e2f6c617267652f30303865476d5a456c7931676e626a336b3539366d673330647130616967797a2e676966)

动画中的二叉搜索树中，删除元素7，在右子数中找比7大一点的数，这个数一定在右子数的最左侧。 

那么就将删除节点（元素7）的左孩子放到删除节点（元素7）的右子树的最左面节点（元素8）的左孩子上，就是把5为根节点的子树移到了8的左孩子的位置。

再定义一个cur，让cur指向要删除节点的右子数。如果cur左子数不为空，就一直遍历，直到找到最左边的节点。再把要删除的节点（root）左子树放在cur的左孩子的位置。

要删除的节点（元素7）的右孩子（root.right元素9）为新的根节点。

代码如下：

```jsx
// 第一种情况：没找到删除的节点，遍历到空节点直接返回了
    if (!root) return root;
// 第二种情况：找到删除的节点，左右孩子都为空（叶子节点），直接删除节点，返回null为根节点
    if (root.val === key) {
    if (!root.left && !root.right) return null;
    // 第三种情况：找到删除的节点，其左孩子为空，右孩子不为空，删除节点，右孩子补位 ，返回右孩子为根节点
    else if (!root.left && root.right) return root.right;
    // 第四种情况：找到删除的节点，其右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
    else if (root.left && !root.right) return root.left;
    // 第五种情况：找到删除的节点，左右孩子节点都不为空，则将删除节点的左子树放到删除节点的右子树的最左面节点的左孩子的位置，并返回删除节点右孩子为新的根节点。
    else {
         let cur = root.right; // 找右子树最左面的节点
         while (cur.left) {
         cur = cur.left;
				}
    cur.left = root.left; // 把要删除的节点（root）左子树放在cur的左孩子的位置
	  root = root.right;
    return root;
		}
```

1. 确定单层递归的逻辑

这里相当于把新的节点返回给上一层，上一层就要用 root.left 或者 root.right接住，代码如下：

```jsx
if (root.val > key) root.left = deleteNode(root.left, key);
if (root.val < key) root.right = deleteNode(root.right, key);
return root;
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
 * @param {number} key
 * @return {TreeNode}
 */
var deleteNode = function (root, key) {
    // 第一种情况：没找到删除的节点，遍历到空节点直接返回了
    if (!root) return root;
    // 第二种情况：找到删除的节点，左右孩子都为空（叶子节点），直接删除节点，返回null为根节点
    if (root.val === key) {
        if (!root.left && !root.right) return null;
        // 第三种情况：找到删除的节点，其左孩子为空，右孩子不为空，删除节点，右孩子补位 ，返回右孩子为根节点
        else if (!root.left && root.right) return root.right;
        // 第四种情况：找到删除的节点，其右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
        else if (root.left && !root.right) return root.left;
        // 第五种情况：找到删除的节点，左右孩子节点都不为空，则将删除节点的左子树放到删除节点的右子树的最左面节点的左孩子的位置，并返回删除节点右孩子为新的根节点。
        else {
            let cur = root.right; // 找右子树最左面的节点
            while (cur.left) {
                cur = cur.left;
            }
            cur.left = root.left; // 把要删除的节点（root）左子树放在cur的左孩子的位置
            root = root.right;
            return root;
        }
    }
    if (root.val > key) root.left = deleteNode(root.left, key);
    if (root.val < key) root.right = deleteNode(root.right, key);
    return root;
};
```

### 迭代法

[二刷的时候再学习](https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html#%E8%BF%AD%E4%BB%A3%E6%B3%95)