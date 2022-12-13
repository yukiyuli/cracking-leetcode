# 代码随想录算法训练 Day 6 | 哈希表

Completed: December 13, 2022
Difficulty: Easy
Done: Yes
Redo: No
Topic: HashMap

## ****哈希表基本理论****

### 什么是哈希表

> 哈希表（英文名字为Hash table，国内也有一些算法书籍翻译为散列表）。哈希表是根据关键码的值而直接进行访问的数据结构。
> 

直白来讲其实数组就是一张哈希表。

哈希表中关键码就是数组的索引下标，然后通过下标直接访问数组中的元素，如下图所示：

![https://img-blog.csdnimg.cn/20210104234805168.png](https://img-blog.csdnimg.cn/20210104234805168.png)

**一般哈希表都是用来快速判断一个元素是否出现集合里。**

例如要查询一个名字是否在这所学校里。

要枚举的话时间复杂度是O(n)，但如果使用哈希表的话， 只需要O(1)就可以做到。

## ****哈希函数****

哈希函数，把学生的姓名直接映射为哈希表上的索引，然后就可以通过查询索引下标快速知道这位同学是否在这所学校里了。

将学生姓名映射到哈希表上就涉及到了**hash function ，也就是哈希函数。**

哈希函数如下图所示，通过hashCode把名字转化为数值，一般hashcode是通过特定编码方式，可以将其他数据格式转化为不同的数值，这样就把学生名字映射为哈希表上的索引数字了。

![https://img-blog.csdnimg.cn/2021010423484818.png](https://img-blog.csdnimg.cn/2021010423484818.png)

如果hashCode得到的数值大于 哈希表的大小了，也就是大于tableSize了，为了保证映射出来的索引数值都落在哈希表上，我们会在再次对数值做一个取模的操作，就要我们就保证了学生姓名一定可以映射到哈希表上了。

此时问题又来了，哈希表我们刚刚说过，就是一个数组。

如果学生的数量大于哈希表的大小怎么办，此时就算哈希函数计算的再均匀，也避免不了会有几位学生的名字同时映射到哈希表 同一个索引下标的位置。

接下来**哈希碰撞**登场

## ****哈希碰撞****

如图所示，小李和小王都映射到了索引下标 1 的位置，**这一现象叫做哈希碰撞。**

![https://img-blog.csdnimg.cn/2021010423494884.png](https://img-blog.csdnimg.cn/2021010423494884.png)

一般哈希碰撞有两种解决方法， 拉链法和线性探测法。

### ****拉链法****

刚刚小李和小王在索引1的位置发生了冲突，发生冲突的元素都被存储在链表中。 这样我们就可以通过索引找到小李和小王了。

![https://img-blog.csdnimg.cn/20210104235015226.png](https://img-blog.csdnimg.cn/20210104235015226.png)

（数据规模是dataSize， 哈希表的大小为tableSize）

其实拉链法就是要选择适当的哈希表的大小，这样既不会因为数组空值而浪费大量内存，也不会因为链表太长而在查找上浪费太多时间。

### ****线性探测法****

使用线性探测法，一定要保证tableSize大于dataSize。 我们需要依靠哈希表中的空位来解决碰撞问题。

例如冲突的位置，放了小李，那么就向下找一个空位放置小王的信息。所以要求tableSize一定要大于dataSize ，要不然哈希表上就没有空置的位置来存放 冲突的数据了。如图所示：

![https://img-blog.csdnimg.cn/20210104235109950.png](https://img-blog.csdnimg.cn/20210104235109950.png)

## ****常见的三种哈希结构****

当我们想使用哈希法来解决问题的时候，我们一般会选择如下三种数据结构。

- 数组
- set （集合）
- map(映射)

### set

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
Set本身是一个构造函数，用来生成 Set 数据结构。
Set是一组key的集合，不存储value。由于key不能重复，所以，在Set中，没有重复的key。
Set是多用来操作数组的(比如数组去重，查找数组中是否存在某个元素，等等)。

### map

JS中只有Map，没有HashMap。

在引入Map之前，js中保存键值对是通过对象的形式，而对象中，键的类型只能是字符串类型。而引入Map后，用Map来存储键值对，键的类型可以是数字类型，可以是字符串类型，可以是对象类型，函数类型等等。

## ****总结****

**当遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**。

但是哈希法也是**牺牲了空间换取了时间**，因为我们要使用额外的数组，set或者是map来存放数据，才能实现快速的查找。

如果在做面试题目的时候遇到需要判断一个元素是否出现过的场景也应该第一时间想到哈希法！

## ****242.有效的字母异位词****

[leetcode link](https://leetcode.cn/problems/valid-anagram/)

### 思路：

定义一个数组record，大小为26，能放下26个字母就可以了，初始化为0，因为字符a到字符z的ASCII也是26个连续的数值。

为了方便举例，判断一下字符串s= "aee", t = "eae"。

操作动画如下：

![https://tva1.sinaimg.cn/large/008eGmZEly1govxyg83bng30ds09ob29.gif](https://tva1.sinaimg.cn/large/008eGmZEly1govxyg83bng30ds09ob29.gif)

遍历字符串s，将 s[i] - ‘a’ 所在的元素+1 ，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了。 这样就将字符串s中字符出现的次数，统计出来了。

遍历字符串t，将 s[i] - ‘a’ 所在的元素-1，检查是否出现了这些字符，如果t中出现一些不在s中的字符，则返回false。

最后检查一下，record数组是不是为0。如果不为零0，说明字符串s和t一定是谁多了字符或者谁少了字符，return false。如果record数组所有元素都为零0，说明字符串s和t是字母异位词，return true。

时间复杂度为O(n)，空间上因为定义是的一个常量大小的辅助数组，所以空间复杂度为O(1)。

> 为什么这里用数组？因为反胃小且可控。如果数值很大，用set。key对应有value，用map。
> 

JavaScript完整代码：

```jsx
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    if (s.length !== t.length) {//长度不想等 直接返回false
        return false;
    }
    const record = new Array(26).fill(0);//大小为26的数组
    for (let i = 0; i < s.length; i++) {//循环字符串s，每个元素出现一次加1
        record[s.codePointAt(i) - 'a'.codePointAt(0)]++;
    }
    for (let i = 0; i < t.length; i++) {//循环t元素
        record[t.codePointAt(i) - 'a'.codePointAt(0)]--;//每次出现的字符减1
        }
    for (let i = 0; i < 26; i++) { //检查record是否为0
        if(record[i] !=0){ // record不为0，说明字符串s和t一定是谁多了字符或者谁少了字符
            return false;
        }
    }  
    return true;//recrod=0，说明字符串s和t是字母异位词
};
```

## ****349. 两个数组的交集****

[leetcode link](https://leetcode.cn/problems/intersection-of-two-arrays/)

### 思路

如果题目规定了数值范围，用数组。

set适合数值很大，并且分布很分散，跨度非常大，如果用数组就会造成空间的极大浪费。

先将num1转化成set，定义一个新数组result。

遍历num2，检查num2的元素是否存在num1，如果存在就加入到result，最后返回result。

JavaScript代码：

```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    let num1Set = new Set(nums1); // num1转成set
    let result= new Set();//定义一个新数组返回结果

    for (let i of nums2) {// 遍历num2
        if (num1Set.has(i)) {//元素如果存在num1Set中就加入reslut
            result.add(i);
        }
    }
    return Array.from(result);
};
```

## ****202. 快乐数****

[leetcode link](https://leetcode.cn/problems/happy-number/)

- 定义一个函数，循环计算每个位置上的数字的平方和。
- 定义一个新Set，Set里的数是唯一的。
- 如果在循环中某个值重复出现，说明此时陷入死循环，永远不可能到1，说明这个值不是快乐数。

JavaScript代码：

```jsx
/**
 * @param {number} n
 * @return {boolean}
 */
var isHappy = function(n) {
    let set = new Set();   // 定义一个新Set,Set() 里的数是唯一的
    // 如果在循环中某个值重复出现，说明此时陷入死循环，也就说明这个值不是快乐数
    while (n !== 1 && !set.has(n)) {
        set.add(n);
        n = getSum(n);
    }
    return n === 1;
};

var getSum = function (n) {
    let sum = 0;
    while (n) {
        sum += (n % 10) ** 2;
        n =  Math.floor(n/10);
    }
    return sum;
};
```

## ****1. 两数之和****

[leetcode link](https://leetcode.cn/problems/two-sum/)

### 思路

- 为什么会想到用哈希表？

需要一个集合来存放遍历过的元素，然后在遍历数组的时候去询问这个集合，某元素是否遍历过，也就是是否出现在这个集合。

- 哈希表为什么用map

本题不仅要知道元素有没有遍历过，还有知道这个元素对应的下标，需要使用 key value结构来存放，key来存元素，value来存下标，那么使用map正合适。

- 本题map是用来存什么的？

map目的用来存放访问过的元素，因为遍历数组的时候，需要记录之前遍历过哪些元素和对应的下标，这样才能找到与当前元素相匹配的（也就是相加等于target）。

- map中的key和value用来存什么的？

判断元素是否出现，这个元素就要作为key，有key对应的就是value，value用来存下标。

所以map中的存储结构为 {key：数据元素，value：数组元素对应的下表}。

本题在遍历数组的时候，只需要向map去查询是否有和目前遍历元素比配的数值，如果有，就找到的匹配对，如果没有，就把目前遍历的元素放进map中，因为map存放的就是我们访问过的元素。

过程图如下：

![https://code-thinking-1253855093.file.myqcloud.com/pics/20220711202638.png](https://code-thinking-1253855093.file.myqcloud.com/pics/20220711202638.png)

![https://code-thinking-1253855093.file.myqcloud.com/pics/20220711202708.png](https://code-thinking-1253855093.file.myqcloud.com/pics/20220711202708.png)

JavaScript代码：

```jsx
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    map = new Map()
    // 遍历当前元素，并在map中寻找是否有匹配的key
    for(let i = 0; i < nums.length; i++) {
        x = target - nums[i]
        if(map.has(x)) {
            return [map.get(x),i]
        }
        map.set(nums[i],i) // 如果没找到匹配对，就把访问过的元素和下标加入到map中
    }
};
```