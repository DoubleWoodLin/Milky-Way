---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 1911. 最大子序列交替和
date: 2021-06-26
tags:
  - 贪心算法
  - 动态规划
  - 周赛
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
website: https://leetcode-cn.com
github: https://github.com/DoubleWoodLin
---

# **题目说明**

第 55 场双周赛 Shopee 第三题

[原题地址](https://leetcode-cn.com/problems/maximum-alternating-subsequence-sum)

一个下标从 0  开始的数组的 交替和   定义为 偶数   下标处元素之 和   减去 奇数   下标处元素之 和  。

比方说，数组  [4,2,5,3]  的交替和为  (4 + 5) - (2 + 3) = 4 。
给你一个数组  nums ，请你返回  nums  中任意子序列的   最大交替和  （子序列的下标 重新   从 0 开始编号）。

一个数组的 子序列   是从原数组中删除一些元素后（也可能一个也不删除）剩余元素不改变顺序组成的数组。比方说，[2,7,4]  是  [4,2,3,7,2,1,4]  的一个子序列（加粗元素），但是  [2,4,2] 不是。

示例 1：

输入：nums = [4,2,5,3]
输出：7
解释：最优子序列为 [4,2,5] ，交替和为 (4 + 5) - 2 = 7 。

示例 2：

输入：nums = [5,6,7,8]
输出：8
解释：最优子序列为 [8] ，交替和为 8 。

示例 3：

输入：nums = [6,2,1,2,4,5]
输出：10
解释：最优子序列为 [6,1,5] ，交替和为 (6 + 5) - 1 = 10 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-alternating-subsequence-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# **实现思路**

本题类似股票问题，可以用贪心算法或者动态规划的思路来解答，本篇题解以贪心算法为例，想象数组是一片山脉，本题要求的就是取这片山脉的每一个山峰（数学中的极大值）相加，减去每一个山谷（数学中的极小值）。当然，由于数组中没有负数，所以目标子序列最后一个下标一定是偶数。

代码如下：

```javascript
var maxAlternatingSum = function (nums) {
  // flag为true时需要取极大值，为false时需要取极小值。
  let flag = true;
  let res = 0;
  for (let i = 0; i < nums.length - 1; i++) {
    if (flag) {
      if (nums[i + 1] < nums[i]) {
        res += nums[i];
        flag = !flag;
      }
    } else {
      if (nums[i + 1] > nums[i]) {
        res -= nums[i];
        flag = !flag;
      }
    }
  }
  if (flag) {
    res += nums[nums.length - 1];
  }
  return res;
};
```
