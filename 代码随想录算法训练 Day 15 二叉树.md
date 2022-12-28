# 代码随想录算法训练 Day 15 | 二叉树

Topic: Binary Tree, DFS, BFS

Questions: 102. 二叉树的层次遍历  226. 翻转二叉树  101. 对成二叉树

Difficulty: Easy

- [x] Done: Yes

Completed: December 27, 2022

- [ ] Redo


## ****102. 二叉树的层序遍历****

[leetcode](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

### 思路：

从左到右一层一层的去遍历二叉树。需要借用一个辅助数据结构即队列来实现，队列先进先出，符合一层一层遍历的逻辑，而用栈先进后出适合模拟深度优先遍历也就是递归的逻辑。

准备一个队列，将根节点加入队列，当队列不为空的时候循环队列，每次循环拿到当前队列的大小，在循环当前层的每个元素，然后加入输出数组res中，如果这个元素存在左右节点则将左右节点加入队列

动画如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1gnad5itmk8g30iw0cqe83.gif](https://tva1.sinaimg.cn/large/008eGmZEly1gnad5itmk8g30iw0cqe83.gif)

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
 * @return {number[][]}
 */
var levelOrder = function(root) {
    const res = [], queue = [];
    queue.push(root);//初始队列
    if (!root) {
        return res;
    }   
    while (queue.length !== 0) {
        // 当前层节点的数量
        let size = queue.length;
        // 存放每一层的节点 
        let curLevel = [];
        // 循环当前层的节点
        for (let i = 0; i < size; i++) {
            let node = queue.shift();
            curLevel.push(node.val);
            // 存放当前层下一层的节点
            // 检查左节点，存在左节点就继续加入队列
            if (node.left) queue.push(node.left);
            // 检查左右节点，存在右节点就继续加入队列
            if (node.right) queue.push(node.right);
        }
        // 把每一层的结果放到结果数组
        res.push(curLevel)
    }
       
    return res;
};
```

## ****107. 二叉树的层次遍历 II****

[leetcode](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

### 思路：

相对于102.二叉树的层序遍历，就是最后把result数组反转一下就可以了

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
 * @return {number[][]}
 */
var levelOrderBottom = function(root) {
    const res = [], queue = [];
    queue.push(root);//初始队列
    if (!root) {
        return res;
    }   
    while (queue.length !== 0) {
        // 当前层节点的数量
        let size = queue.length;
        // 存放每一层的节点 
        let curLevel = [];
        // 循环当前层的节点
        while (size--) {
            let node = queue.shift();
            curLevel.push(node.val);
            // 存放当前层下一层的节点
            // 检查左节点，存在左节点就继续加入队列
            if (node.left) queue.push(node.left);
            // 检查左右节点，存在右节点就继续加入队列
            if (node.right) queue.push(node.right);
        }
        // 把每一层的结果放到结果数组
        res.unshift(curLevel)
    }
       
    return res;
};
```

## ****199. 二叉树的右视图****

[leetcode](https://leetcode.cn/problems/binary-tree-right-side-view/)

### 思路：

层序遍历的时候，判断是否遍历到单层的最后面的元素，如果是，就放进result数组中，随后返回result就可以了。

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
var rightSideView = function(root) {
    //二叉树右视图 只需要把每一层最后一个节点存储到res数组
    const res = [], queue = [];
    queue.push(root);//初始队列
    if (!root) {
        return res;
    }   
    while (queue.length !== 0) {
        // 当前层节点的数量
        let size = queue.length;
         while (size--) {
            let node = queue.shift();
            // size为0的时候表明到了层级最后一个节点
            if(!size) {
                res.push(node.val);
            }
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
         }         
    }
    return res; 
};
```

## ****226. 翻转二叉树****

[leetcode](https://leetcode.cn/problems/invert-binary-tree/)

### 思路：

把每一个节点的左右孩子交换一下。

### ****递归法****

使用前序和后序遍历都可以。

> 中序遍历补方便，因为中序遍历会把某些节点的左右孩子翻转了两次！
> 

动图如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1gnakm26jtog30e409s4qp.gif](https://tva1.sinaimg.cn/large/008eGmZEly1gnakm26jtog30e409s4qp.gif)

递归三部曲：

1. 确定递归函数的参数和返回值

参数就是要传入节点的指针，不需要其他参数了，通常此时定下来主要参数，如果在写递归的逻辑中发现还需要其他参数的时候，随时补充。

返回值的话其实也不需要，但是题目中给出的要返回root节点的指针，可以直接使用题目定义好的函数。

1. 确定终止条件

当前节点为空的时候，就返回。

1. 确定单层递归的逻辑

因为是先前序遍历，所以先进行交换左右孩子节点，然后反转左子树，反转右子树。

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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    // 终止条件
    if (!root) {
        return null;
    }
    // 交换左右节点
    const rightNode = root.right;
    root.right = invertTree(root.left);
    root.left = invertTree(rightNode);
    return root;
};
```

### **迭代法**

### ****深度优先遍历****

使用迭代版本(统一模板))的前序遍历：

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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    //我们先定义节点交换函数
    const invertNode = function(root, left, right) {
        let temp = left;
        left = right;
        right = temp;
        root.left = left;
        root.right = right;
    }
    //使用迭代方法的前序遍历 
    let stack = [];
    if(root === null) {
        return root;
    }
    stack.push(root);
    while(stack.length) {
        let node = stack.pop();
        if(node !== null) {
            //前序遍历顺序中左右  入栈顺序是前序遍历的倒序右左中
            node.right && stack.push(node.right);
            node.left && stack.push(node.left);
            stack.push(node);
            stack.push(null);
        } else {
            node = stack.pop();
            //节点处理逻辑
            invertNode(node, node.left, node.right);
        }
    }
    return root;
};
```

### ****广度优先遍历****

也就是层序遍历，可以把每个节点的左右孩子都翻转一遍。

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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    //我们先定义节点交换函数
    const invertNode = function(root, left, right) {
        let temp = left;
        left = right;
        right = temp;
        root.left = left;
        root.right = right;
    }
    //使用层序遍历
    let queue = [];
    if(root === null) {
        return root;
    } 
    queue.push(root);
    while(queue.length) {
        let length = queue.length;
        while(length--) {
            let node = queue.shift();
            //节点处理逻辑
            invertNode(node, node.left, node.right);
            if (node.left) queue.push(node.left)
            if (node.right) queue.push(node.right);
        }
    }
    return root;
};
```

## ****101. 对称二叉树****

[leetcode](https://leetcode.cn/problems/symmetric-tree/description/?languageTags=javascript)

### 思路：

对于二叉树是否对称，要比较的是根节点的左子树与右子树是不是相互翻转的，所以在递归遍历的过程中，也是要同时遍历两棵树。如图所示：

![Untitled](https://user-images.githubusercontent.com/101588752/209859332-e3ffbe7c-136f-4f01-8235-9706d89988aa.png)

本题遍历只能是“后序遍历”，因为我们要通过递归函数的返回值来判断两个子树的内侧节点和外侧节点是否相等。准确的来说是一个树的遍历顺序是左右中，一个树的遍历顺序是右左中。

### ****递归法****

递归三部曲

1. 确定递归函数的参数和返回值

因为我们要比较的是根节点的两个子树是否是相互翻转的，进而判断这个树是不是对称树，所以要比较的是两个树，参数自然也是左子树节点和右子树节点。

```jsx
 const compareNode = function(left, right)
```

1. 确定终止条件
- 左节点为空，右节点不为空，不对称，return false
- 左不为空，右为空，不对称 return false
- 左右都为空，对称，返回true
- 左右都不为空，比较节点数值，不相同就return false

```jsx
if(left === null && right !== null || left !== null && right === null) {
   return false;
} else if(left === null && right === null) {
   return true;
} else if(left.val !== right.val) {
   return false;
}
```

1. 确定单层递归的逻辑

单层递归的逻辑就是处理 左右节点都不为空，且数值相同的情况。

- 比较二叉树外侧是否对称：传入的是左节点的左孩子，右节点的右孩子。
- 比较内测是否对称，传入左节点的右孩子，右节点的左孩子。
- 如果左右都对称就返回true ，有一侧不对称就返回false 。

```jsx
let outSide = compareNode(left.left, right.right);
let inSide = compareNode(left.right, right.left);
return outSide && inSide;
```

如上代码中，我们可以看出使用的遍历方式，左子树左右中，右子树右左中，所以我把这个遍历顺序也称之为“后序遍历”。

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
var isSymmetric = function(root) {
    // 1. 确定递归的参数 root.left root.right和返回值true false 
    const compareNode = function(left, right) {
        // 2. 确定终止条件 空的情况
        if(left === null && right !== null || left !== null && right === null) {
        return false;
        } else if(left === null && right === null) {
            return true;
        } else if(left.val !== right.val) {
            return false;
        }
        // 3. 确定单层递归逻辑
        let outSide = compareNode(left.left, right.right);
        let inSide = compareNode(left.right, right.left);
        return outSide && inSide;
    }
    if(root === null) {
        return true;
    }
    return compareNode(root.left, root.right);
};
```

### ****迭代法****

### ****使用队列****

### 思路：

通过队列来判断根节点的左子树和右子树的内侧和外侧是否相等，如动画所示：

![https://tva1.sinaimg.cn/large/008eGmZEly1gnwcimlj8lg30hm0bqnpd.gif](https://tva1.sinaimg.cn/large/008eGmZEly1gnwcimlj8lg30hm0bqnpd.gif)

JavaScript完整代码：

```jsx
var isSymmetric = function(root) {
  // 迭代方法判断是否是对称二叉树
  // 首先判断root是否为空
  if(root === null) {
      return true;
  }
  let queue = [];
  queue.push(root.left);
  queue.push(root.right);
  while(queue.length) {
      let leftNode = queue.shift();    //左节点
      let rightNode = queue.shift();   //右节点
      if(leftNode === null && rightNode === null) {
          continue;
      }
      if(leftNode === null || rightNode === null || leftNode.val !== rightNode.val) {
          return false;
      }
      queue.push(leftNode.left);     //左节点左孩子入队
      queue.push(rightNode.right);   //右节点右孩子入队
      queue.push(leftNode.right);    //左节点右孩子入队
      queue.push(rightNode.left);    //右节点左孩子入队
  }

  return true;
};
```

### ****使用栈****

JavaScript完整代码：

```jsx
var isSymmetric = function(root) {
  // 迭代方法判断是否是对称二叉树
  // 首先判断root是否为空
  if(root === null) {
      return true;
  }
  let stack = [];
  stack.push(root.left);
  stack.push(root.right);
  while(stack.length) {
      let rightNode = stack.pop();    //左节点
      let leftNode=stack.pop();       //右节点
      if(leftNode === null && rightNode === null) {
          continue;
      }
      if(leftNode === null || rightNode === null || leftNode.val !== rightNode.val) {
          return false;
      }
      stack.push(leftNode.left);      //左节点左孩子入队
      stack.push(rightNode.right);    //右节点右孩子入队
      stack.push(leftNode.right);     //左节点右孩子入队
      stack.push(rightNode.left);     //右节点左孩子入队
  }

  return true;
};
```
