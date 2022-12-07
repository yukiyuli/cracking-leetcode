# 代码随想录算法训练 Day 1 | 数组

Completed: December 7, 2022

Topic: Array, Binary Search

Difficulty: Easy

- [ ] Done

- [ ] Redo



## ****数组理论基础****

首先要知道数组在内存中的存储方式：

> **数组是存放在连续内存空间上的相同类型数据的集合。**
> 

数组可以方便的通过下标索引的方式获取到下标下对应的数据。

举一个字符数组的例子，如图所示：

![Untitled](https://user-images.githubusercontent.com/101588752/206104315-6c03d91b-7ea0-4fc6-9d53-7faa6fffad4b.png)

需要两点注意的是：

- **数组下标都是从0开始的。**
- **数组内存空间的地址是连续的。**

正是**因为数组的在内存空间的地址是连续的，所以我们在删除或者增添元素的时候，就难免要移动其他元素的地址。**

例如删除下标为3的元素，需要对下标为3的元素后面的所有元素都要做移动操作，如图所示：

![Untitled 1](https://user-images.githubusercontent.com/101588752/206104333-54233032-3002-4d7b-b552-f16715139bdb.png)

**数组的元素是不能删的，只能覆盖。**

二维数组：

![Untitled 2](https://user-images.githubusercontent.com/101588752/206104368-bee1d954-c134-4652-a078-cb9008754c50.png)

**那么二维数组在内存的空间地址是连续的么？**

不同编程语言的内存管理是不一样的，**C++中二维数组在地址空间上是连续的**。**Java的二维数组是不绚丽的。**



## ****704. 二分查找****

[leetcode link](https://leetcode.cn/problems/binary-search/)

### 重点：

使用二分法的前提：

1. 有序数组
2. 数组中没有重复元素

### 二分法的两种写法：

第一种：**[left, right]**， target 是在一个在左闭右闭的区间里。

- while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
- if (nums[middle] > target) right 要赋值为 middle - 1，因为当前这个nums[middle]一定不是target，那么接下来要查找的左区间结束下标位置就是 middle - 1

![Untitled 3](https://user-images.githubusercontent.com/101588752/206104409-8a47d37c-b24b-42bb-a54a-c01125b16fc4.png)

Javascript代码：

```jsx
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    // target 是在一个在左闭右闭的区间里
    let mid, left = 0, right = nums.length - 1;
    // 当left=right时，由于nums[right]在查找范围内，所以要包括此情况
    while (left <= right) {
        // 防止溢出
        mid = left + ((right - left) >> 1);
        // 如果中间数大于目标值，中间数一定不是目标值，排除查找范围，所以右边界更新为mid-1
        if (nums[mid] > target) {
	            right = mid - 1;  // 去左面闭区间寻找
        } else if (nums[mid] < target) {
            left = mid + 1;   // 去右面闭区间寻找
        } else {
            return mid;
        }
    }
    return -1;
};
```

第二种：**[left, right]**， target 是在一个在左闭右开的区间里。

- while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
- if (nums[middle] > target) right 更新为 middle，因为当前nums[middle]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为middle，即：下一个查询区间不会去比较nums[middle]

![Untitled 4](https://user-images.githubusercontent.com/101588752/206104469-a850a9c5-7a03-4d49-b028-3fb5151660ae.png)

Javascript代码：
```jsx
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    // target是在一个在左闭右闭的区间里, right是数组最后一个数的下标+1，nums[right]不在查找范围内
    let mid, left = 0, right = nums.length;
    // 当left=right时，由于nums[right]不在查找范围，所以不必包括此情况
    while (left < right) {
        // 位运算 + 防止大数溢出
        mid = left + ((right - left) >> 1);
        // 如果中间值大于目标值，中间数一定不是目标值，去左区间继续寻找，而寻找区间是左闭右开区间，所以将右边界更新为中间值
        if (nums[mid] > target) {
            right = mid;  // 去左区间寻找
        } else if (nums[mid] < target) {
            left = mid + 1;   // 去右区间寻找
        } else {
            return mid;
        }
    }
    return -1;
};
```



## 27. 移除元素

[leetcode link](https://leetcode.cn/problems/remove-element/)

### 重点：

> 数组的元素在内存地址中是连续的，不能单独删除数组中的某个元素，只能覆盖。
> 

### 双指针解法：

双指针法（快慢指针法）： **通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。**

定义快慢指针

- 快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组
- 慢指针：指向更新 新数组下标的位置

Javascript代码：
```jsx
var removeElement = function(nums, val) {
  // 初始慢指针在0位置
	let slow = 0;
	// 初始快指针在0位置并循环数组
	for (let fast = 0; fast < nums.length; fast++) { 
		// 当快指针指向的nums不为val，则快慢指针同时向右移动，若快指针指向的val=nums, 快指针向右移动，慢指针不动
		if (nums[fast] !== val) {
		nums[slow++] = nums[fast];
		}
	}
	return slow;
};
```
