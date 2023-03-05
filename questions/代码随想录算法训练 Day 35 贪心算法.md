# 代码随想录算法训练 Day 35 | 贪心算法

Topic: Greedy Algorithm

Difficulty: Medium

Question: 406. 根据身高重建队列, 452. 用最少数量的箭引爆气球, 860. 柠檬水找零

- [x] Done

Completed: January 16, 2023

- [ ] Redo


## ****860. 柠檬水找零****

[leetcode](https://leetcode.cn/problems/lemonade-change/)

### 思路：

只需要维护三种金额的数量，5，10和20。分三种情况：

- 情况一：账单是5，直接收下。
- 情况二：账单是10，消耗一个5，增加一个10
- 情况三：账单是20，优先消耗一个10和一个5，如果不够，再消耗三个5

账单是20的情况，为什么要优先消耗一个10和一个5呢？

因为美元10只能给账单20找零，而美元5可以给账单10和账单20找零，美元5更万能！

所以局部最优：遇到账单20，优先消耗美元10，完成本次找零。全局最优：完成全部账单的找零。

JavaScript完整代码：

```jsx
/**
 * @param {number[]} bills
 * @return {boolean}
 */
var lemonadeChange = function (bills) {
    let five = 0, ten = 0, twenty = 0;
    for (let bill of bills) {
        // 情况一
        if (bill == 5) five++;
        // 情况二
        if (bill == 10) {
            if (five <= 0) return false;
            ten++;
            five--;
        }
        // 情况三
        if (bill == 20) {
            // 优先消耗10美元，因为5美元的找零用处更大，能多留着就多留着
            if (five > 0 && ten > 0) {
                five--;
                ten--;
                twenty++; // 其实这行代码可以删了，因为记录20已经没有意义了，不会用20来找零
            } else if (five >= 3) {
                five -= 3;
                twenty++; // 同理，这行代码也可以删了
            } else return false;
        }
    }
    return true;
};
```

## ****406. 根据身高重建队列****

[leetcode](https://leetcode.cn/problems/queue-reconstruction-by-height/)

### 思路

本题要思考是先按hi排序还是先按ki排序？

如果按照ki来从小到大排序，排完之后，会发现ki的排列并不符合条件，身高也不符合条件，两个维度哪一个都没确定下来。

那么按照身高hi来排序呢，身高一定是从大到小排（身高相同的话则k小的站前面），让高个子在前面。 

按照身高排序之后，优先按身高高的people的ki来插入，后序插入节点也不会影响前面已经插入的节点，最终按照k的规则完成了队列。

以图中{5,2} 为例：

![Untitled](https://user-images.githubusercontent.com/101588752/212737531-f6aff368-e395-4dc8-a53e-cc558017eeb9.png)

所以在按照身高从大到小排序后：

局部最优：优先按身高高的people的ki来插入。插入操作过后的people满足队列属性

全局最优：最后都做完插入操作，整个队列满足题目队列属性

整个插入过程如下：

排序完的people： [[7,0], [7,1], [6,1], [5,0], [5,2]，[4,4]]

插入的过程：

- 插入[7,0]：[[7,0]]
- 插入[7,1]：[[7,0],[7,1]]
- 插入[6,1]：[[7,0],[6,1],[7,1]]
- 插入[5,0]：[[5,0],[7,0],[6,1],[7,1]]
- 插入[5,2]：[[5,0],[7,0],[5,2],[6,1],[7,1]]
- 插入[4,4]：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]

JavaScript完整代码：

```jsx
/**
 * @param {number[][]} people
 * @return {number[][]}
 */
var reconstructQueue = function (people) {
    let queue = []
    people.sort((a, b) => {
				//如果身高不相等，按照身高从大到小排列
        if (b[0] !== a[0]) {
            return b[0] - a[0]
        } else {
						//如果身高相等，按照k从小到大排列
            return a[1] - b[1]
        }
    })
    for (let i = 0; i < people.length; i++) {
				//people[i][1] = 当前人的k，下面这句意思为在queue队列中k位置添加people[i]
        queue.splice(people[i][1], 0, people[i])
    }
    return queue
};
```

## ****452. 用最少数量的箭引爆气球****

[leetcode](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

### 思路

局部最优：当气球出现重叠，一起射，所用弓箭最少。全局最优：把所有气球射爆所用弓箭最少。

模拟气球射爆的过程就是被射过的气球仅仅跳过就行了，没有必要让气球数组remove气球，只要记录一下箭的数量就可以了。

为了让气球尽可能的重叠，需要对数组进行排序。

按照气球的起始进行排序。从前向后遍历气球数组，靠左尽可能让气球重复。如果气球重叠了，重叠气球中右边边界的最小值 之前的区间一定需要一个弓箭。

以题目示例： [[10,16],[2,8],[1,6],[7,12]]为例，如图：（已经排序）

![Untitled 1](https://user-images.githubusercontent.com/101588752/212737562-ecd3aaaa-4157-493f-8272-4f61bb0ceeeb.png)

可以看出首先第一组重叠气球，一定是需要一个箭，气球3，的左边界大于了 第一组重叠气球的最小右边界，所以再需要一支箭来射气球3了。

```jsx
/**
 * @param {number[][]} points
 * @return {number}
 */
var findMinArrowShots = function (points) {
    if (points.length == 0) return 0;
    points.sort((a, b) => {
        return a[0] - b[0]
    })
    let result = 1; // points 不为空至少需要一支箭
    for (let i = 1; i < points.length; i++) {
        if (points[i][0] > points[i - 1][1]) {  // 气球i和气球i-1不挨着，注意这里不是>=
            result++; // 需要一支箭
        }
        else {  // 气球i和气球i-1挨着
            points[i][1] = Math.min(points[i - 1][1], points[i][1]); // 更新重叠气球最小右边界
        }
    }
    return result;
};
```
