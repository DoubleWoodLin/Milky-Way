---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 希尔排序
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

进阶级排序算法，非稳定排序，时间复杂度最差 O(n<sup>2</sup>),正确性经过[912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)验证。

```javascript
const shellSort = function (nums) {
  // 希尔排序
  let len = nums.length;
  for (let gap = Math.floot(len / 2); gap > 0; gap = Math.floor(gap / 2)) {
    for (let i = gap; i < len; i++) {
      let tem = nums[i];
      let j = i - gap;
      for (j; j >= 0 && nums[j] > tem; j -= gap) {
        nums[j + gap] = nums[j];
      }
      nums[j + gap] = tem;
    }
  }
  return nums;
};
```
