# 代码随想录算法训练 Day 21 | 二叉树

Completed: January 1, 2023
Difficulty: Easy
Done: Yes
Question: 236.  二叉树的最近公共祖先, 501. 二叉搜索树中的众数, 530. 二叉搜索树的最小绝对差
Redo: No
Topic: BFS, Binary Tree, DFS

## ****530. 二叉搜索树的最小绝对差****

[leetcode](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

### 思路：

### ****递归法****

二叉搜素树中序遍历的过程中直接算出最小差值。需要用一个pre节点记录一下cur节点的前一个节点。

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2021%20%E4%BA%8C%E5%8F%89%E6%A0%91%20918c397aecf0457783915cd5624701b3/Untitled.png)

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
var getMinimumDifference = function(root) {
    let res = Infinity;
    let pre = null; 
    const traversal = (cur) => {
        if(!cur) return;
        
        traversal (cur.left); // 左
        if (pre) res= Math.min(res, cur.val - pre.val) // 中
        pre = cur; // 记录前一个
        traversal (cur.right);
    }
traversal(root);
return res;

};
```

### 迭代法

[二刷的时候再学习](https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html#go)

## ****501. 二叉搜索树中的众数****

[leetcode](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

### 思路：

### 递归法：

如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2021%20%E4%BA%8C%E5%8F%89%E6%A0%91%20918c397aecf0457783915cd5624701b3/Untitled%201.png)

中序遍历代码如下：

```jsx
    const traversal = (cur) => {
        if (!cur) return;
        traversal (cur.left); // 左
        // 中
        traversal (cur.right); // 右
        return;
    }
```

遍历有序数组的元素出现频率，从头遍历，那么一定是相邻两个元素作比较，然后就把出现频率最高的元素输出就可以了。

双指针，一个指针指向前一个节点，这样每次cur（当前节点）才能和pre（前一个节点）作比较。

而且初始化的时候pre = null，这样当pre为null时候，我们就知道这是比较的第一个元素。

代码如下：

```jsx
if (!pre) { // 第一个节点
     count = 1;
} else if (pre.val ===cur.val) { // 与前一个节点数值相同
     count++;
} else { // 与前一个节点数值不同
     count = 1;
}
pre = cur; // 更新上一个节点
```

因为要求最大频率的元素集合（注意是集合，不是一个元素，可以有多个众数），应该是先遍历一遍数组，找出最大频率（maxCount），然后再重新遍历一遍数组把出现频率为maxCount的元素放进集合。

这种方式遍历了两遍数组，其实只需要遍历一次就可以找到所有的众数。

如果单个元素频率count 等于 maxCount（最大频率），当然要把这个元素加入到结果集中（以下代码为result数组），代码如下：

```jsx
// 如果和最大值相同，放进result中
if (count === maxCount) res.push(cur.val);
```

这里又有问题，如果这个maxCount此时还不是真正最大频率呢，所以又要对maxCount操作。

频率count 大于 maxCount的时候，不仅要更新maxCount，而且要清空结果集（以下代码为result数组），因为结果集之前的元素都失效了。代码如下：

```jsx
// 如果计数大于最大值频率
if (count > maxCount) { 
     maxCount = count;   // 更新最大频率
     res = [];     // 很关键的一步，不要忘记清空result，之前result里的元素都失效了
     res.push(cur.val);
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
 * @return {number[]}
 */
var findMode = function(root) {
    // 不使用额外空间，使用中序遍历
    let pre =null, count=0, maxCount =0, res = []; // count统计单个元素频率, maxCount统计最大频率

    // 1.确定递归函数及函数参数
    const traversal = (cur) => {
        // 2. 确定递归终止条件
        if (!cur) return;

        // 3. 单层递归
        traversal (cur.left); // 左

        // 中
				if (!pre) { // 第一个节点
				     count = 1;
				} else if (pre.val ===cur.val) { // 与前一个节点数值相同
				     count++;
				} else { // 与前一个节点数值不同
				     count = 1;
				}
				pre = cur; // 更新上一个节点

        // 如果和最大值相同，放进result中
        if (count === maxCount) res.push(cur.val);
        
        // 如果计数大于最大值频率
        if (count > maxCount) { 
            maxCount = count;   // 更新最大频率
            res = [];     // 很关键的一步，不要忘记清空result，之前result里的元素都失效了
            res.push(cur.val);
        }

        traversal (cur.right);  // 右
        return;

    }
    traversal(root);
    return res;
};
```

### 迭代法

[二刷的时候再学习](https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html#%E9%80%92%E5%BD%92%E6%B3%95)

## ****236.  二叉树的最近公共祖先****

[leetcode](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

### 思路：

利用二叉树回溯的过程，让二叉树自底向上查找。

后序遍历（左右中）就是天然的回溯过程，可以根据左右子树的返回值，来处理中节点的逻辑。

如何判断一个节点是节点q和节点p的公共祖先呢？

情况一：如果找到一个节点，发现左子树出现结点p，右子树出现节点q，或者左子树出现结点q，右子树出现节点p，那么该节点就是节点p和q的最近公共祖先。如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2021%20%E4%BA%8C%E5%8F%89%E6%A0%91%20918c397aecf0457783915cd5624701b3/Untitled%202.png)

判断逻辑是如果递归遍历遇到q，就将q返回，遇到p 就将p返回，那么如果左右子树的返回值都不为空，说明此时的中节点，一定是q 和p 的最近祖先。

> 不可能出现左子树遇到q 返回，右子树也遇到q返回，这样并没有找到 q 和p的最近祖先。因为题目强调：二叉树节点数值是不重复的，而且一定存在 q 和 p。
> 

情况二：节点本身p(q)，它拥有一个子孙节点q(p)。如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2021%20%E4%BA%8C%E5%8F%89%E6%A0%91%20918c397aecf0457783915cd5624701b3/Untitled%203.png)

其实情况一 和情况二 代码实现过程都是一样的，实现情况一的逻辑，顺便包含了情况二。

因为遇到 q 或者 p 就返回，这样也包含了 q 或者 p 本身就是 公共祖先的情况。

### ****递归法****

递归三步曲：

1. 确定递归函数的参数和返回值

需要递归函数返回值，来告诉我们是否找到节点q或者p。还要返回最近公共节点，可以利用上题目中返回值，那么如果遇到p或者q，就把q或者p返回，返回值不为空，就说明找到了q或者p。

代码如下：

```jsx
var lowestCommonAncestor = function(root, p, q)
```

1. 确定终止条件

遇到空的话，因为树都是空了，所以返回空。

如果 root === q，或者 root == =p，说明找到 q p ，则将其返回。

```jsx
 if(root === null || root === p || root === q) return root;
```

代码如下：

1. 确定单层递归的逻辑

值得注意的是本题函数有返回值，是因为回溯的过程需要递归函数的返回值做判断，但本题我们依然要遍历树的所有节点。

如果递归函数有返回值，如何区分要搜索一条边，还是搜索整个树呢？

搜索一条边的写法：

```jsx
if (递归函数(root.left)) return ;

if (递归函数(root.right)) return ;
```

搜索整个树写法：

```jsx
left = 递归函数(root.left);  // 左
right = 递归函数(root.right); // 右
left与right的逻辑处理;         // 中
```

在递归函数有返回值的情况下区别：

搜索一条边，递归函数返回值不为空的时候，立刻返回。

搜索整个树，直接用一个变量left、right接住返回值，这个left、right后序还有逻辑处理的需要，也就是后序遍历中处理中间节点的逻辑（也是回溯）。

本题为什么要遍历整棵树呢？如图：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2021%20%E4%BA%8C%E5%8F%89%E6%A0%91%20918c397aecf0457783915cd5624701b3/Untitled%204.png)

就像图中一样直接返回7，多美滋滋。

但事实上还要遍历根节点右子树（即使此时已经找到了目标节点了），也就是图中的节点4、15、20。

因为在如下代码的后序遍历中，如果想利用left和right做逻辑处理， 不能立刻返回，而是要等left与right逻辑处理完之后才能返回。

```jsx
left = 递归函数(root.left);  // 左
right = 递归函数(root.right); // 右
left与right的逻辑处理;         // 中
```

那么先用left和right接住左子树和右子树的返回值，代码如下：

```jsx
let left = lowestCommonAncestor(root.left,p,q);
let right = lowestCommonAncestor(root.right,p,q);
```

如果left 和 right都不为空，说明此时root就是最近公共节点。这个比较好理解。

如果left为空，right不为空，就返回right，说明目标节点是通过right返回的，反之依然。

为什么left为空，right不为空，目标节点通过right返回呢？

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2021%20%E4%BA%8C%E5%8F%89%E6%A0%91%20918c397aecf0457783915cd5624701b3/Untitled%205.png)

图中节点10的左子树返回null，右子树返回目标值7，那么此时节点10的处理逻辑就是把右子树的返回值（最近公共祖先7）返回上去！

代码如下：

```jsx
if(left === null && right === null) { // 若未找到节点 p 或 q
   return null;
}else if(left === null && right !== null) { // 若找到一个节点
   return right;
}else if(left !== null && right === null) { // 若找到一个节点
   return left;
}else { // 若找到两个节点
   return root;
}
```

完整流程图如下：

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2021%20%E4%BA%8C%E5%8F%89%E6%A0%91%20918c397aecf0457783915cd5624701b3/Untitled%206.png)

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
var lowestCommonAncestor = function(root, p, q) {
    // 2. 确定递归终止条件
    if(root === null || root === p || root === q) return root;
        
    // 3. 确定递归单层逻辑
    let left = lowestCommonAncestor(root.left,p,q);
    let right = lowestCommonAncestor(root.right,p,q);
    if(left === null && right === null) { // 若未找到节点 p 或 q
        return null;
    }else if(left === null && right !== null) { // 若找到一个节点
        return right;
    }else if(left !== null && right === null) { // 若找到一个节点
        return left;
    }else { // 若找到两个节点
        return root;
    }
};
```

## 回溯过程**总结**

**归纳如下三点**：

1. 求最小公共祖先，需要从底向上遍历，那么二叉树，只能通过后序遍历（即：回溯）实现从底向上的遍历方式。
2. 在回溯的过程中，必然要遍历整棵二叉树，即使已经找到结果了，依然要把其他节点遍历完，因为要使用递归函数的返回值（也就是代码中的left和right）做逻辑判断。
3. 要理解如果返回值left为空，right不为空为什么要返回right，为什么可以用返回right传给上一层结果。