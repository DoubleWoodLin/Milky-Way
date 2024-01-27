---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 堆排序
date: 2021-07-15
tags:
  - 排序算法
categories:
  - Algorithm
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

进阶级排序算法，时间复杂度 O(nlogn),正确性经过[912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)验证。

### 堆排序代码

```javascript
// 归并排序
/**
 * 堆排序
 * @author Jarvey Linger
 * @date 2021-07-15
 * @returns {any}
 */
sort = () => {
  const queue = this._queue;
  for (let i = queue.length - 1; i >= 1; i--) {
    let temp = queue[0];
    queue[0] = queue[i];
    queue[i] = temp;
    this._downAdjust(0, i);
  }
};
```

### 完整版

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
class PriorityQueue {
  /**
   * 描述
   * @author Jarvey Linger
   * @date 2021-07-13
   * @constructor
   * @param (Function)comparator,决定队列元素优先级的函数，默认情况下元素值越小优先级越高。
   */
  constructor(comparator = (a, b) => b - a) {
    if (comparator !== void 0 && typeof comparator !== 'function') {
      throw new Error('.constructor expects a valid comparator function');
    }
    this._queue = [];
    this._comparator = comparator;
  }

  /**
   * 返回队列长度
   * @author Jarvey Linger
   * @date 2021-07-13
   * @returns {number}
   */
  size = () => {
    return this._queue.length;
  };

  /**
   * 描述
   * @author Jarvey Linger
   * @date 2021-07-13
   * @returns {Array}
   */
  toArray = () => {
    return Array.from(this._queue);
  };

  /**
   * 下沉堆化
   * @author Jarvey Linger
   * @date 2021-07-13
   * @param {number} parentIndex
   * @param {number} n
   */
  _downAdjust = (parentIndex, n) => {
    let childIndex = parentIndex * 2 + 1;
    let temp = this._queue[parentIndex];
    while (childIndex < n) {
      if (
        childIndex + 1 < n &&
        this._comparator(this._queue[childIndex], this._queue[childIndex + 1]) >
          0
      ) {
        childIndex++;
      }
      if (this._comparator(this._queue[childIndex], temp) > 0) {
        break;
      }
      this._queue[parentIndex] = this._queue[childIndex];
      parentIndex = childIndex;
      childIndex = childIndex * 2 + 1;
    }
    this._queue[parentIndex] = temp;
  };

  /**
   * 入队
   * @author Jarvey Linger
   * @date 2021-07-13
   * @param {any} data
   */
  enqueue = (data) => {
    if (data == null) {
      return;
    }
    const queue = this._queue;
    queue.push(data);
    let childIndex = queue.length - 1;
    let parentIndex = (childIndex - 1) >> 1;
    while (parentIndex >= 0 && this._comparator(queue[parentIndex], data) > 0) {
      queue[childIndex] = queue[parentIndex];
      childIndex = parentIndex;
      parentIndex = (childIndex - 1) >> 1;
    }
    queue[childIndex] = data;
  };

  /**
   * 出队
   * @author Jarvey Linger
   * @date 2021-07-13
   * @returns {any}
   */
  dequeue = () => {
    const queue = this._queue;
    if (queue.length === 0) {
      return;
    }
    const data = queue.shift();
    if (queue.length === 0) {
      return data;
    }
    let top = queue.pop();
    queue.unshift(top);
    this._downAdjust(0, queue.length);
    // this.buildHeap();
    return data;
  };

  /**
   * 判断队列是否为空
   * @author Jarvey Linger
   * @date 2021-07-13
   * @returns {boolean}
   */
  isEmpty = () => {
    return this._queue.length === 0;
  };

  /**
   * 返回队首元素
   * @author Jarvey Linger
   * @date 2021-07-13
   * @returns {any}
   */
  peek = () => {
    if (this.isEmpty()) {
      return null;
    }
    return this._queue[0];
  };

  /**
   * 清空队列
   * @author Jarvey Linger
   * @date 2021-07-13
   */
  clear = () => {
    this._queue.length = 0;
  };

  /**
   * 堆排序
   * @author Jarvey Linger
   * @date 2021-07-15
   * @returns {any}
   */
  sort = () => {
    const queue = this._queue;
    for (let i = queue.length - 1; i >= 1; i--) {
      let temp = queue[0];
      queue[0] = queue[i];
      queue[i] = temp;
      this._downAdjust(0, i);
    }
  };
}

var sortArray = function (nums) {
  const pq = new PriorityQueue();
  for (let i = 0; i < nums.length; i++) {
    pq.enqueue(nums[i]);
  }
  pq.sort();
  return pq.toArray();
};
```
