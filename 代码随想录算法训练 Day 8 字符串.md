# 代码随想录算法训练 Day 8 | 字符串

Completed: December 19, 2022
Difficulty: Easy
Done: Yes
Redo: No
Topic: String

## ****344.反转字符串****

[leetcode link](https://leetcode.cn/problems/reverse-string/description/)

### 思路：

使用双指针法。

定义两个指针，i从头部开始向中间移动，j从尾部开始向中间移动，并交换元素。

以字符串`hello`为例，过程如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1gp0fvi91pfg30de0akwnq.gif](https://tva1.sinaimg.cn/large/008eGmZEly1gp0fvi91pfg30de0akwnq.gif)

循环里只要做交换s[i] 和s[j]操作就可以了，使用了swap 这个库函数交换元素。

JavaScript完整代码：

```jsx
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function(s) {
    const len = s.length;
    for (let i = 0, j = len - 1; i < len/2; i++, j--) {
        [s[i], s[j]] = [s[j], s[i]];
    }
};
```

## ****541. 反转字符串II****

[leetcode link](https://leetcode.cn/problems/reverse-string-ii/)

### 思路

本题的关键是在遍历字符串的时候，i+=2*k

JavaScript代码：

```
/**
 * @param {string} s
 * @param {number} k
 * @return {string}
 */
var reverseStr = function(s, k) {
    const len=s.length;
    let result=s.split("");
    for (i=0; i<len ;i+=2*k){
        let left =i;
        let right = i + k - 1 > len ? len -1 : i + k -1 ;
        while (left < right) {
        [result[left], result[right]]= [result[right], result[left]];
        left++;
        right--;
        }
    }
    return result.join('');  

};
```

## ****题目：剑指Offer 05.替换空格****

[leetcode link](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

### 思路

### 双指针法

1. 计算字符串里空格的数量；
2. 将字符串长度扩充到空格替换成"%20"之后的大小；
3. 利用双指针，替换空格，i指向新长度的末尾，j指向旧长度的末尾。从后往前一直遍历，直到i指到的下标0就结束。

```
/**
 * @param {string} s
 * @return {string}
 */
var replaceSpace = function(s) {
   // 字符串转为数组
  const strArr = Array.from(s);
  let count = 0;

  // 计算空格数量
  for(let k = 0; k < strArr.length; k++) {
    if (strArr[k] === ' ') {
      count++;
    }
  }

  let j = strArr.length - 1;
  let i = strArr.length + count * 2 - 1;  // 新数组的长度

  while(j >= 0) {
    if (strArr[j] === ' ') {
      strArr[i--] = '0';
      strArr[i--] = '2';
      strArr[i--] = '%';
      j--;
    } else {
      strArr[i--] = strArr[j--];
    }
  }

  // 数组转字符串
  return strArr.join('');
};
```

![https://tva1.sinaimg.cn/large/e6c9d24ely1go6qmevhgpg20du09m4qp.gif](https://tva1.sinaimg.cn/large/e6c9d24ely1go6qmevhgpg20du09m4qp.gif)

> 为什么要从后向前填充，从前向后填充不行么？从前向后填充就是O(n^2)的算法了，因为每次添加元素都要将添加元素之后的所有元素向后移动。
> 

这么做有两个好处：

1. 不用申请新数组。
2. 从后向前填充元素，避免了从前向后填充元素时，每次添加元素都要将添加元素之后的所有元素向后移动的问题。

JavaScript代码：

```jsx
/**
 * @param {string} s
 * @return {string}
 */
var replaceSpace = function(s) {
   // 字符串转为数组
  const strArr = Array.from(s);
  let count = 0;

  // 计算空格数量
  for(let k = 0; k < strArr.length; k++) {
    if (strArr[k] === ' ') {
      count++;
    }
  }

	let i = strArr.length + count * 2 - 1;  // i指向新数组的末尾
  let j = strArr.length - 1; // j指向旧数组的末尾
  

  while(j >= 0) {
    if (strArr[j] === ' ') {
      strArr[i--] = '0';
      strArr[i--] = '2';
      strArr[i--] = '%';
      j--;
    } else {
      strArr[i--] = strArr[j--];
    }
  }

  // 数组转字符串
  return strArr.join('');
};
```

## ****151. 翻转字符串里的单词****

[leetcode link](https://leetcode.cn/problems/4sum/)

### 思路

1. 移除多余空格

双指针法：这里的处理就跟数组题里移除元素的方法是类似的。

首先我们定义两个快慢指针，初始都指向数组的0下标，快指针获取字符串里的每个字母，每一次向前移动的时候判断当前元素是否是一个空格，如果不需要就把当前元素赋值给慢指针所指向的位置，随后快慢指针同时往前移动一步。

两个单词之间需要一个空格，所以我们必须需要判断slow指针是否指向字符串数组第一个下标，然后接下来每一次都让slow指向的位置都赋值为空格，这样两个不同的单词之间就会产生一个空格了。这里需要特别处理首字母，因为首字母前不需要空格

1. 将整个字符串反转

双指针法：只需要定义两个指针，分别指向数组的头部和尾部的位置，然后一起往中间靠拢，相互调换位置即可。

1. 将每个单词反转

双指针法：字符串中的空格跟最后一个单词的最后一个字符的位置就是每一次置换位置的判断条件；所以通过第二部的函数合理控制好边界就可以完成每一个单词的翻转。

JavaScript代码：

```jsx
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function(s) {
    // 1. 移除空格
    function removeExtraSpaces(s){
        let slow = 0;
        for(let fast = 0; fast<s.length; fast++){
            if(s[fast]!=' '){
                if(slow != 0){
                    s[slow++] = ' ';
                }
                while(fast < s.length && s[fast] != ' '){
                    s[slow] = s[fast];
                    fast++;
                    slow++;
                }
            }
        }
        s.length = slow;
    }

    // 2. 反转字符串
    function reverseStr(s, start, end){
        let slow = start;
        let fast = end;
        while(slow < fast){
            [s[slow], s[fast]] = [s[fast], s[slow]];
            slow++;
            fast--;
        }
    }

    let strArr = Array.from(s);
    removeExtraSpaces(strArr);
    reverseStr(strArr, 0 , strArr.length - 1);

    // 3. 反转字符串里的每个单词
    let begin = 0; 
    for(let i = 0; i <= strArr.length ; i++){
        if(strArr[i] === ' ' || i === strArr.length ){
            reverseStr(strArr, begin , i - 1);
            begin = i + 1;
        }
    }
    return strArr.join('')
};
```

## ****题目：剑指Offer58-II.左旋转字符串****

[leetcode link](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

### 思路

为了让本题更有意义，不申请额外空间，只能在本串上操作。

1. 反转区间为前n的子串
2. 反转区间为n到末尾的子串
3. 反转整个字符串

例如 ：示例1中 输入：字符串abcdefg，n=2

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%208%20%E5%AD%97%E7%AC%A6%E4%B8%B2%200aaad15eaa72462fb2264fba17de1499/Untitled.png)

> 跟翻转字符串里的单词的区别是，本题是先反转局部再整体，翻转字符串里的单词是先反转整体再局部。
> 

JavaScript代码：

```jsx
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function(s, n) {
    function reverseStr(s, start, end){
        let slow = start;
        let fast = end;
        while(slow < fast){
            [s[slow], s[fast]] = [s[fast], s[slow]];
            slow++;
            fast--;
        }
    }

    let strArr = Array.from(s);
    reverseStr(strArr, 0, strArr.length - 1);
    reverseStr(strArr, 0, strArr.length - n - 1);
    reverseStr(strArr, strArr.length - n, strArr.length - 1);
    return strArr.join('');
};
```