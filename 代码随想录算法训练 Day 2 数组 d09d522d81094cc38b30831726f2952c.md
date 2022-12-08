# 代码随想录算法训练 Day 2 | 数组

Completed: December 8, 2022
Difficulty: Medium
Done: Yes
Redo: No
Topic: Array

## ****977.有序数组的平方****

[leetcode link](https://leetcode.cn/problems/binary-search/)

### 方法1: 暴力排序

最直观的方法：每个数平方之后，排个序。

Javascript代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function(nums) {
  return nums.map(item => item * item).sort((a, b) => a - b)
};
```

### 方法2: ****双指针法****

一个指针i指向头部，另外一个指针j指向尾部，定义一个新数组result接收返回新数组，让k指向result数组终止位置。

如果`A[i] * A[i] < A[j] * A[j]` 那么`result[k--] = A[j] * A[j];` 

如果`A[i] * A[i] >= A[j] * A[j]` 那么`result[k--] = A[i] * A[i];` 

JavaScript代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function(nums) {
	// 定义n为新数组的长度与原数组长度一样
	let n=nums.length;
	// 定义一个新数组result接收返回新数组
	let result=new Array(n).fill(0);
	// i指向头部，j指向尾部，k指向result数组终止位置
	let i=0,j=n-1,k=n-1;
	while(i<=j){
		// 定义left, right分别接收nums[i]和nums[j]平方后的值
     let left=nums[i]*nums[i], right =nums[j]*nums[j];
			// 比较left,right值，大的那个存入新数组
     if(left<right){
         result[k--]=right;
         j--;
     } else{
         result[k--]=left;
         i++
     }
 }
 return result;
};
```

## ****209.长度最小的子数组****

[leetcode link](https://leetcode.cn/problems/remove-element/)

### 滑动窗口解法：

所谓滑动窗口，**就是不断的调节子序列的起始位置和终止位置，从而得出我们要想的结果。**

**思路：**

1. 定义两个指针i, j分别指向数组的头部，首先右移j指针。
2. 当 [i, j]子数组和 >= target，选择收缩窗口，i右移，直到子数和<target，并更新最小长度。
3. 当 [i, j]子数组和 < target，此时应该扩张窗口，j 右移，直到条件重新满足。

**动画：**

![https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif](https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif)

JavaScript代码：

```jsx
/**
 * @param {number} target
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(target, nums) {
    const len = nums.length;
    // 定义两个指针在开始位置
    let i = j = sum = 0, 
        res = len + 1; //最大的窗口不会超过自身长度
    while(j < len) {
        sum += nums[j++];//扩大窗口
        while(sum >= target) {
            res = res < j - i ? res : j - i;//更新最小长度
            sum-=nums[i++];//缩小窗口
        }
    }
    return res > len ? 0 : res; //未找到返回0
};
```

## ****59.螺旋矩阵II****

[leetcode link](https://leetcode.cn/problems/remove-element/)

**思路：**

1. 每一边选择一样的处理规则，比如左闭右开，或者左开右闭
2. 按 上 右 下 左，一层层向内，遍历矩阵填格子

**画图：**

左闭右开

![IMG_0662.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%202%20%E6%95%B0%E7%BB%84%20d09d522d81094cc38b30831726f2952c/IMG_0662.jpg)

JavaScript代码：

```jsx
/**
 * @param {number} n
 * @return {number[][]}
 */
var generateMatrix = function(n) {

let startX=startY=0; // 设置起始位置
let loop=Math.floor(n/2); // 每个圈循环几次
let offset=1; // 需要控制每一条边遍历的长度，左闭右开
let count=1; // 用来给矩阵中每一个空格赋值
let result=new Array(n).fill(0).map(() => new Array(n).fill(0));

while(loop--){
    let i=startX, j=startY;
    //模拟填充上行从左到右(左闭右开)
    for(i=startX;i<n-offset;i++){
        result[startY][i]=count++;
    }
    // 模拟填充右列从上到下(左闭右开)
    for(j=startY;j<n-offset;j++){
        result[j][i]=count++;
    }
    // 模拟填充下行从右到左(左闭右开)
    for(;i>startX;i--){
        result[j][i]=count++;
    }
    // 模拟填充左列从下到上(左闭右开)
    for(;j>startY;j--){
        result[j][i]=count++;
    }
    // 第二圈开始的时候，起始位置要各自加1
    startX++;
    startY++;
    // offset 控制每一圈里每一条边遍历的长度
    offset+=1;
}
// 如果n为奇数的话，需要单独给矩阵最中间的位置赋值
if(n%2===1){
    result[startX][startY]=count;
}
return result;
};
```

## 总结

### ****二分法****

1. 二分的使用前提：有序数组并且无重复元素。
2. 分清左闭右闭和左闭右开的不同情况。
- 左闭右闭

> right = nums.length - 1
while (left <= right) 
	    right = mid - 1; 
            left = mid + 1;
> 
- 左闭右开

> right = nums.length;
while (left < right) 
	    right = mid; 
      left = mid + 1;
> 

### ****双指针法****

通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。

### ****滑动窗口****

原理和双指针一样。两个指针都能固定住，并且一个指针固定，另一个指针不断移动，从而得出我们要想的结果。

### ****螺旋矩阵****

旋转时，保持每一边一样的处理规则，比如左闭右开，或者左开右闭。

对于正方形的二维教组只需要考虑转多少圈以及n为基数时中间剩一个值，如果剩一个一个值需要再转完圈后再单独加中间的值。