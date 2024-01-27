---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 快速排序
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

进阶级排序算法，非稳定排序，时间复杂度 O(nlogn),最差 O(n<sup>2</sup>),正确性经过[912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)验证，该版本为原地排序（无重新开辟数组空间，但有递归调用的栈空间消耗）。

```javascript
const partion = (arr, left, right) => {
  let pivot = arr[right];
  let p = left;
  for (let i = left; i < right; i++) {
    if (arr[i] < pivot) {
      let temp = arr[i];
      arr[i] = arr[p];
      arr[p++] = temp;
    }
  }
  arr[right] = arr[p];
  arr[p] = pivot;
  return p;
};

const quickSort = (arr, left, right) => {
  if (left >= right) {
    return;
  }
  let q = partion(arr, left, right);
  quickSort(arr, left, q - 1);
  quickSort(arr, q + 1, right);
};
```
