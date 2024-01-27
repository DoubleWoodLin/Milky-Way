---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 451. 根据字符出现频率排序
date: 2021-07-03
tags:
  - 每日一题
  - 哈希表
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
website: https://leetcode-cn.com/problems/sort-characters-by-frequency
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/sort-characters-by-frequency/)

给定一个字符串，请将字符串里的字符按照出现的频率降序排列。

### **示例 1：**

```javascript
输入:
"tree"

输出:
"eert"

解释:
'e'出现两次，'r'和't'都只出现一次。
因此'e'必须出现在'r'和't'之前。此外，"eetr"也是一个有效的答案。
```

### **示例 2：**

```javascript
输入:
"cccaaa"

输出:
"cccaaa"

解释:
'c'和'a'都出现三次。此外，"aaaccc"也是有效的答案。
注意"cacaca"是不正确的，因为相同的字母必须放在一起。
```

### **示例 3：**

```javascript
输入:
"Aabb"

输出:
"bbAa"

解释:
此外，"bbaA"也是一个有效的答案，但"Aabb"是不正确的。
注意'A'和'a'被认为是两种不同的字符。
```

# **实现思路**

用哈希表记录出现频率，然后排序即可。

# **代码如下**

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var frequencySort = function (s) {
  const map = new Map();
  for (let char of s) {
    map.set(char, (map.get(char) || 0) + 1);
  }
  let ans = '';
  for (let [char, count] of Array.from(map.entries()).sort(
    (a, b) => b[1] - a[1]
  )) {
    for (let i = 0; i < count; i++) {
      ans += char;
    }
  }
  return ans;
};
```
