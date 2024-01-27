---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 栈
date: 2021-07-08
tags:
  - 栈
categories:
  - datastructure
image:
  url: '/blog-post.webp'
  alt: 'blog placeholder'
worksImage1:
  url: '/blog-post.webp'
  alt: 'blog placeholder'
worksImage2:
  url: '/blog-post.webp'
  alt: 'blog placeholder.'
platform: Leetcode
stack: JavaScript
website: https://leetcode-cn.com
github: https://github.com/DoubleWoodLin
---

## **栈的定义**

栈是一种满足“先进后出”（First In Last Out)的线性结构。在 JavaScript 中，数组拥有 shift(),unshift(),push(),pop()方法，如果我们将数组中的 shift、unshift 屏蔽掉，那么数组就满足栈的定义了。

下面用代码实现用数组模拟栈：

```javascript
class Stack {
  constructor() {
    this._stack = [];
  }
  // 返回栈顶元素
  peek() {
    return this._stack[-1];
  }
  // 删除栈顶元素并返回
  pop() {
    return this._stack.pop();
  }
  // 入栈
  push(item) {
    this._stack.push(item);
  }
  // 判断栈是否为空
  isEmpty() {
    return this._stack.length === 0;
  }
}
```

## **栈的应用**

浏览器的前进后退、内存模型中的函数调用栈、括号匹配等都属于栈的应用。

## **进阶： 单调栈**

单调栈是指栈中数据保持一定程度的单调性，比如：[5,4,3,2,1]这样的栈就保持栈底到栈顶从大到小，假设我们要根据[2,7,6,9,3,1,8]这样一组数据构建单调栈：

```
[2,7,6,9,3,1,8]
第一步，栈为空，直接进栈：[2],
第二步，7>2,不满足单调性，应该将2弹出，此时栈为空，直接进栈:[7]
第三步，6<7,[7,6]
第四部，9>6弹出6，9>7,弹出7，进栈[9]
...
相信你已经掌握其规律了，最后构建的单调栈：[9,8]
```

## **实战**

### [LeetCode 155.最小栈](https://leetcode-cn.com/problems/min-stack/)

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。

pop() —— 删除栈顶的元素。

top() —— 获取栈顶元素。

getMin() —— 检索栈中的最小元素。

```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function () {
  this.stack = [];
  this.min_stack = [];
};

/**
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function (val) {
  this.stack.push(val);
  if (
    this.min_stack.length === 0 ||
    val <= this.min_stack[this.min_stack.length - 1]
  ) {
    this.min_stack.push(val);
  }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
  if (this.stack.pop() === this.min_stack[this.min_stack.length - 1]) {
    this.min_stack.pop();
  }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function () {
  return this.stack[this.stack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function () {
  return this.min_stack[this.min_stack.length - 1];
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(val)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```

## **总结**

栈是一种基础的数据结构，它具有先进后出的特点，其中单调栈是栈的一种应用，在许多场景可以用到，我们应该掌握它。
