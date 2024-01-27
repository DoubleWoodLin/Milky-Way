---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 冒泡排序
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

入门级排序算法，时间复杂度在 O(n<sup>2</sup>),正确性经过[912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)验证，当然，喜闻乐见的超时了。

```javascript
const bubbleSort = (array) => {
  for (let index = 0; index < array.length; index++) {
    for (let j = 0; j < array.length - index; j++) {
      if (array[j] > array[j + 1]) {
        let temp = array[j];
        array[j] = array[j + 1];
        array[j + 1] = temp;
      }
    }
  }
};
```
