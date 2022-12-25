# 代码随想录算法训练 Day 11 | 栈与队列

Completed: December 25, 2022
Difficulty: Medium
Done: Yes
Question: 1047. 删除字符串中的所有相邻重复项, 150. 逆波兰表达式求值, 20. 有效的括号
Redo: No
Topic: Queue, Stack

## ****20. 有效的括号****

[leetcode link](https://leetcode.cn/problems/valid-parentheses/)

### 思路

一共有三种不匹配的情况：

1. 字符串里左方向的括号多余了 ，已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，不匹配。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2011%20%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%20f9fb703b23e942b9a751a8daa5677240/Untitled.png)

1. 遍历字符串匹配的过程中，发现栈里没有要匹配的字符。所以return false。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2011%20%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%20f9fb703b23e942b9a751a8daa5677240/Untitled%201.png)

1. 历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号return false。

![Untitled](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83%20Day%2011%20%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%20f9fb703b23e942b9a751a8daa5677240/Untitled%202.png)

字符串遍历完之后，栈是空的，就说明全都匹配了。

> 字符串长度是奇数的话，一定不匹配。
> 

> 在匹配左括号的时候，右括号先入栈，就只需要比较当前元素和栈顶相不相等就可以了，比左括号先入栈代码实现要简单的多了！
> 

动画效果如下：

![https://code-thinking.cdn.bcebos.com/gifs/20.%E6%9C%89%E6%95%88%E6%8B%AC%E5%8F%B7.gif](https://code-thinking.cdn.bcebos.com/gifs/20.%E6%9C%89%E6%95%88%E6%8B%AC%E5%8F%B7.gif)

JavaScript完整代码：

```jsx
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    const stack = []
    if (s.length % 2 !== 0) return false // 长度为奇数肯定错误
    for (let i = 0; i < s.length; i++) {
        let ch = s.charAt(i);
        //碰到左括号，就把相应的右括号入栈
        if (ch === '(') {
            stack.push(')');
        } else if (ch === '[') {
            stack.push(']');
        } else if (ch === '{') {
            stack.push('}');
        } else if (ch !== stack[stack.length - 1] || !stack.length) {
            return false
        } else {//如果是右括号判断是否和栈顶元素匹配
             stack.pop();
        }
    }
    //最后判断栈中元素是否匹配
    return !stack.length;
};
```

## ****1047. 删除字符串中的所有相邻重复项****

[leetcode link](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

### 思路

用栈存放便利过的元素，当遍历当前的这个元素的时候，去栈里看一下我们是不是遍历过相同数值的相邻元素。然后再去做对应的消除操作。

如动画所示：

![https://code-thinking.cdn.bcebos.com/gifs/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.gif](https://code-thinking.cdn.bcebos.com/gifs/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.gif)

从栈中弹出剩余元素，此时是字符串ac，因为从栈里弹出的元素是倒序的，所以再对字符串进行反转一下，就得到了最终的结果。

> 更简洁的操作：拿字符串直接作为栈，省去了栈还要转为字符串的操作。
> 

JavaScript完整代码：

```
/**
 * @param {string} s
 * @return {string}
 */
var removeDuplicates = function(s) {
    let stack = [];
    for (let i = 0; i < s.length; i++) {
        let ch = s.charAt(i);
        if (ch === stack[stack.length - 1]) {
            stack.pop();
        } else {
            stack.push(ch);
        }
    }

     return stack.join('');

};
```

## ****150. 逆波兰表达式求值****

[leetcode](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

### 思路：

遇到数字加入到栈，遇到符号就把数字从栈中取出做运算，然后再加回栈中。

如动图所示：

![https://code-thinking.cdn.bcebos.com/gifs/150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.gif](https://code-thinking.cdn.bcebos.com/gifs/150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.gif)

> 需要注意的是，如果是除法操作，先出栈的为除数，后出栈的为被除数，其它操作都满足交换律，所以不需要在意出栈的数字的顺序。
> 

JavaScript完整代码：

```jsx
/**
 * @param {string[]} tokens
 * @return {number}
 */
var evalRPN = function(tokens) {
    const stack=[]
    for (const i of tokens) {
        // 运算符
        if (isNaN(Number(i))) {
            // 弹出两个数字
            const n1 = stack.pop();
            const n2 = stack.pop();
            // 判断运算符类型，算出新数入栈
            switch (i) {
                case "+":
                    stack.push(n2 + n1);
                    break;
                case "-":
                    stack.push(n2 - n1);
                    break;
                case "*":
                    stack.push(n2 * n1);
                    break;
                case "/":
                    stack.push(n2 / n1 | 0);
                    break;
            }
        }
        else {
            stack.push(Number(i)); // 数字入栈
        }
    }
    return stack.pop()
};
```