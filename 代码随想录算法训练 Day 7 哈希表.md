# 代码随想录算法训练 Day 7 | 哈希表

Topic: HashMap

Difficulty: Medium

Completed: December 14, 2022

- [x] Done

- [ ] Redo


## ****454. 四数相加II****

[leetcode link](https://leetcode.cn/problems/valid-anagram/)

### 思路：

1. 这题使用map，因为要存放两个数的和以及出现的次数，key存nums1,nums2之和，value存nums1,nums2出现的次数。
2. 遍历数组nums1,nums2，统计两个数组元素之和以及出现的次数，放到map中。
3. 定义count，用来统计a+b+c+d=0出现的次数。
4. 再遍历数组nums3,nums4，找到如果0-(c+d) 在map中出现过的话，就用count把map中key对应的value也就是出现次数统计出来。
5. 最后返回统计值 count 就可以了。

> 为什么先遍历数组nums1,nums2，再遍历nums3,nums4，而不是先遍历数组num1，再遍历数组nums2,nums3,nums4？                   
> 先遍历数组nums1，时间复杂度是n的三次方，先遍历nums1,nums2再遍历nums3,nums4，时间复杂度是两个n的二次方。
> 

> 为什么count+=value?
> 因为value是统计a+b出现的次数，如果在0-(c+d) 在map出现过，那a+b原本在map中出现的次数是value。                                                              > 

JavaScript完整代码：

```jsx
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @param {number[]} nums3
 * @param {number[]} nums4
 * @return {number}
 */
var fourSumCount = function(nums1, nums2, nums3, nums4) {
    const tempMap = new Map();
    let count=0;
    for (const a of nums1) {
        for (const b of nums2) {
             // 统计a,b之和及对应的数量
            const sum=a+b;
            if (tempMap.has(sum)) {
                tempMap.set(sum, tempMap.get(sum) + 1);
            } else {
                tempMap.set(sum, 1);
            }
        }
    }
    for (const c of nums3) {
        for (const d of nums4) {
            // 若0-(c+d) 在map中出现过的话，就用count把map中key对应的value统计出来
            const sum=0-(c+d);
            if (tempMap.has(sum)) {
                count += tempMap.get(sum);
            }
        }
    }
    return count;
};
```

## ****383. 赎金信****

[leetcode link](https://leetcode.cn/problems/intersection-of-two-arrays/)

### 思路

这道题目和**242.有效的字母异位词**很像。

本题需要注意两点：

- “为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思”  这里*说明杂志里面的字母不可重复使用。*
- “你可以假设两个字符串均只含有小写字母。” *说明只有小写字母。*

### ****哈希解法****

1. 建一个长度为26的数组记录magazine里字母出现的次数。
2. 然后再遍历字符串ransomNote,每遍历一个字符就在数组对应位置将计数-1。
3. 判断当前遍历的字符在数组中对应元素的值是否小于0，若小于0则说明该字符数目不够用返回false。
4. 若未出现上述情况返回true。
5. 

JavaScript代码：

```
/**
 * @param {string} ransomNote
 * @param {string} magazine
 * @return {boolean}
 */
var canConstruct = function(ransomNote, magazine) {
    const strArr = new Array(26).fill(0);
    for(const c of magazine) {  // 记录 magazine里各个字符出现次数
        strArr[c.charCodeAt() - "a".charCodeAt()]++;
    }
    for(const c of ransomNote) { // 对应的字符个数做--操作
        strArr[c.charCodeAt() - 'a'.charCodeAt()]--;
        if(strArr[c.charCodeAt() - 'a'.charCodeAt()] < 0) {
            return false;
        }
    }
    return true;
};
```

## 15****. 三数之和****

[leetcode link](https://leetcode.cn/problems/3sum/)

### 思路

### 双指针法

> 此题不适合用哈希法，因为这道题需要去重，而哈希法的去重操作需要注意很多细节，并且很费时，很容易超时，所以不推荐使用哈希法。


1. 将数组排序，定义i从下标0开始，定义left=i+1，定义right=nums.size-1。
2. 用i遍历数组，使a = nums[i]，b = nums[left]，c = nums[right]。
3. 如果nums[i] + nums[left] + nums[right] > 0 就说明 此时三数之和大了，因为数组是排序后了，所以right下标就应该向左移动，这样才能让三数之和小一些。
4. 如果 nums[i] + nums[left] + nums[right] < 0 说明 此时 三数之和小了，left 就向右移动，才能让三数之和大一些，直到left与right相遇为止。

![https://code-thinking.cdn.bcebos.com/gifs/15.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.gif](https://code-thinking.cdn.bcebos.com/gifs/15.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.gif)

### 去重逻辑的思考

主要考虑三个数的去重。 a, b ,c, 对应的就是 nums[i]，nums[left]，nums[right]。

****a的去重****

<aside>
💡 判断nums[i] 与 nums[i + 1]是否相同，还是判断 nums[i] 与 nums[i-1] 是否相同？

</aside>


如果判断nums[i]  == nums[i + 1]，就把 三元组中出现重复元素的情况直接pass掉了。 

例如{-1, -1 ,2} 这组数据，当遍历到第一个-1 的时候，判断 下一个也是-1，那这组数据就pass了。我们要做的是不能有重复的三元组，但三元组内的元素是可以重复的！

如果判断nums[i] == nums[i-1]，判断的是前一位是不是一样的元素。

例如 {-1, -1 ,2} 这组数据，当遍历到第一个 -1 的时候，只要前一位没有-1，那么 {-1, -1 ,2} 这组数据一样可以收录到 结果集里。

****b与c的去重****

去重逻辑应该放在找到一个三元组之后，对b 和 c去重.

JavaScript代码：

```jsx
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
    let result = [];
    if(nums == null || nums.length < 3) return result;
    nums.sort((a, b) => a - b); // 排序
    for (let i = 0; i < nums.length - 2; i++) {
        if(nums[i] > 0) break; // 如果当前数字大于0，则三数之和一定大于0，所以结束循环
        if(i > 0 && nums[i] == nums[i-1]) continue; // 去重
        let left = i+1;
        let right = nums.length-1;
        while(left < right){
            const sum = nums[i] + nums[left] + nums[right];
            if(sum < 0) left++;
            else if (sum > 0) right--;
            else {
                result.push([nums[i],nums[left],nums[right]]);
                // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                while (left<right && nums[left] == nums[left+1]) left++; 
                while (left<right && nums[right] == nums[right-1]) right--; 
                // 找到答案时，双指针同时收缩
                left++;
                right--;
            }
        }       
            
    }        
    return result;

};
```

## ****18. 四数之和****

[leetcode link](https://leetcode.cn/problems/4sum/)

### 思路

与15. 三数之和一样，使用双指针法，但是在15. 三数之和的基础上再套了一层for循环。两层for循环nums[k] + nums[i]为确定值，依然是循环内有left和right下标作为双指针，找出nums[k] + nums[i] + nums[left] + nums[right] == target。

与15. 三数之和的区别是，不能单独判断`nums[k] > target`就返回了。三数之和=0是确定的，而四数之和target是不确定的，可以是负数，负数相加会更小。

例如：数组是`[-4, -3, -2, -1]`，`target`是`-10`，不能因为`-4 > -10`而跳过。所以，要改成`nums[i] > target && (nums[i] >=0 || target >= 0)` 。

JavaScript代码：

```jsx
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */
var fourSum = function(nums, target) {
    let result=[];
    if (nums.length < 4) return result;
    nums.sort((a, b) => a - b); // 排序

    for (let k = 0; k < nums.length - 3; k++) {
        if (k > 0 && nums[k] == nums[k - 1]) continue;  // 去重

        for (let i = k + 1; i < nums.length - 2; i++) {
            if (i > k + 1 && nums[i] == nums[i - 1]) continue;  // 去重

            let left = i + 1, right = nums.length - 1;
            while (left < right) {
                const sum = nums[k] + nums[i] + nums[left] + nums[right];
                if (sum < target)  left ++;
                else if (sum > target)  right--;
                else {
                    result.push([nums[k], nums[i], nums[left], nums[right]]);
                    while (left < right && nums[left] == nums[left+1]) left++;
                    while (left < right && nums[right] == nums[right-1]) right--;
                     left++;
                     right--;
                }    
               
            }
        }
    }
    return result;
};
```
