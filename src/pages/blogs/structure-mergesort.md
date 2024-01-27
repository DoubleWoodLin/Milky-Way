---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 归并排序
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

```javascript
// 归并排序
const merge = (arr, left, right, low, high) => {
  let ans = [];
  let i = left,
    j = low;
  while (i <= right && j <= high) {
    ans.push(arr[i] < arr[j] ? arr[i++] : arr[j++]);
  }
  while (i <= right) {
    ans.push(arr[i++]);
  }
  while (j <= high) {
    ans.push(arr[j++]);
  }
  for (let index = left, k = 0; index <= high; index++) {
    arr[index] = ans[k++];
  }
};
const mergesort = (arr, left, right) => {
  if (left >= right) {
    return;
  }
  let mid = left + ((right - left) >> 1);
  mergesort(arr, left, mid);
  mergesort(arr, mid + 1, right);
  merge(arr, left, mid, mid + 1, right);
};
```
