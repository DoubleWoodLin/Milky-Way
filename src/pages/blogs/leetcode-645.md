---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 645. 错误的集合
date: 2021-07-04
tags:
  - 每日一题
  - 位运算
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
website: https://leetcode-cn.com/problems/set-mismatch
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/set-mismatch/)

集合 s 包含从 1 到  n  的整数。不幸的是，因为数据错误，导致集合里面某一个数字复制了成了集合里面的另外一个数字的值，导致集合 丢失了一个数字 并且 有一个数字重复 。

给定一个数组 nums 代表了集合 S 发生错误后的结果。

请你找出重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

### **示例 1：**

```javascript
输入：nums = [1,2,2,4]
输出：[2,3]
```

### **示例 2：**

```javascript
输入：nums = [1,1]
输出：[1,2]
```

### **提示：**

```
2 <= nums.length <= 104
1 <= nums[i] <= 104
```

# **实现思路**

这道题用哈希表可以轻松找出重复的数，计算两个数组的和再结合重复的数，可以得出缺少的数，这里给出一种空间复杂度 O(1)的解法：利用位运算异或的特性，对两个数组异或和可以得到缺失数字和重复数字的异或和，再利用

```
mark=xor&(-xor)
```

得到异或和最右边 1 的位置，利用这个位置可以把 1-n 分为两组，一组是该位置为 1 的数字，另一组是该位置为 0 的数字。遍历两个数组，将 mark&item===1 的分为一组，mark&item===0 分在另一组，在两个数组中，缺失的数字总共出现 1 次，而重复的数字总共出现了 3 次，其它数字总共出现了 2 次，因此其它数字会被抵消掉，而凭借 mark 分出来的两组异或和即缺失的数字和重复的数字，最后再借助前面遍历数组时顺带计算出来的 sum1,sum2 验证缺失数字和重复数字即可。

# **代码如下**

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var findErrorNums = function (nums) {
  let xor = 0,
    sum1 = 0,
    sum2 = 0,
    n = nums.length;
  for (let i = 1; i <= n; i++) {
    xor ^= nums[i - 1] ^ i; //缺失的数和重复的数异或结果
    sum1 += i;
    sum2 += nums[i - 1];
  }
  let mark = xor & -xor,
    x1 = 0,
    x2 = 0;
  for (let i = 1; i <= n; i++) {
    if (i & mark) {
      x1 ^= i;
    }
    if (nums[i - 1] & mark) {
      x1 ^= nums[i - 1];
    }
    if (!(i & mark)) {
      x2 ^= i;
    }
    if (!(nums[i - 1] & mark)) {
      x2 ^= nums[i - 1];
    }
  }
  if (sum2 - x1 + x2 === sum1) {
    return [x1, x2];
  } else {
    return [x2, x1];
  }
};
```
