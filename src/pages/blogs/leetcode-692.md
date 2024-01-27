---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 692. 前K个高频单词
date: 2021-05-20
tags:
  - 优先级队列
categories:
  - Leetcode

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
website: https://leetcode-cn.com/problems/top-k-frequent-words/submissions/
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/top-k-frequent-words/submissions/)

给一非空的单词列表，返回前 k 个出现次数最多的单词。

返回的答案应该按单词出现频率由高到低排序。如果不同的单词有相同出现频率，按字母顺序排序。

### **示例 1：**

```javascript
输入: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
输出: ["i", "love"]
解析: "i" 和 "love" 为出现次数最多的两个单词，均为2次。
    注意，按字母顺序 "i" 在 "love" 之前。
```

### **示例 2：**

```javascript
输入: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
输出: ["the", "is", "sunny", "day"]
解析: "the", "is", "sunny" 和 "day" 是出现次数最多的四个单词，
    出现次数依次为 4, 3, 2 和 1 次。
```

### **注意**

```javascript
1、假定 k 总为有效值， 1 ≤ k ≤ 集合元素数。
2、输入的单词均由小写字母组成。
```

### **扩展练习**

```javascript
尝试以 O(n log k) 时间复杂度和 O(n) 空间复杂度解决。
```

# **实现思路**

此类问题可归类为取 TopK 问题，思路是实现一个大小为 K 的小顶堆，通过 Map 统计单词频率，然后遍历单词数组，将单词及其频率存入小顶堆，当堆的 size>K 时，将堆顶元素弹出，这样就可以保证堆中元素是前 K 个频率最高的单词了，本题要求频率相同时按字典序排列，因此需要使用支持自定义 comparator 的 priorityqueue。

# **代码如下**

```javascript
/**
 * @param {string[]} words
 * @param {number} k
 * @return {string[]}
 */
class PriorityQueue {
  /**
   * 描述
   * @author Jarvey Linger
   * @date 2021-07-13
   * @constructor
   * @param (Function)comparator,决定队列元素优先级的函数，默认情况下元素值越小优先级越高。
   */
  constructor(comparator = (a, b) => a - b) {
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
}

var topKFrequent = function (words, k) {
  const pq = new PriorityQueue((a, b) => {
    if (a[1] === b[1]) {
      return b[0].localeCompare(a[0]);
    }
    return a[1] - b[1];
  });
  const map = new Map();
  for (let word of words) {
    map.set(word, (map.get(word) || 0) + 1);
  }
  for (let [word, count] of map.entries()) {
    pq.enqueue([word, count]);
    while (pq.size() > k) {
      pq.dequeue();
    }
  }
  const ans = [];
  while (pq.size() > 0) {
    ans.push(pq.dequeue()[0]);
  }
  return ans.reverse();
};
```

其中 PriorityQueue 的代码来自[支持自定义 comparator 的 priorityqueue](https://github.com/DoubleWoodLin/priorityQueue)，觉得不错的小伙伴们给个 star 吧^\_^。
