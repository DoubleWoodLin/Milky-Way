---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 168. Excel表列名称
date: 2021-06-29
tags:
  - 每日一题
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
website: https://leetcode-cn.com/problems/excel-sheet-column-title/
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/excel-sheet-column-title/)

给你一个整数 columnNumber ，返回它在 Excel 表中相对应的列名称。

例如：

```javascript
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28
...
```

### **示例 1：**

```javascript
输入：columnNumber = 1
输出："A"
```

### **示例 2：**

```javascript
输入：columnNumber = 28
输出："AB"
```

### **示例 3：**

```javascript
输入：columnNumber = 701
输出："ZY"
```

### **示例 4：**

```javascript
输入：columnNumber = 2147483647
输出："FXSHRXW"
```

### **提示：**

```javascript
1 <= columnNumber <= 231 - 1;
```

# **实现思路**

进制转化类题目，注意偏移量的获取就可以。

# **代码如下**

```javascript
/**
 * @param {number} columnNumber
 * @return {string}
 */
var convertToTitle = function (columnNumber) {
  let ans = '';
  while (columnNumber !== 0) {
    let remain = (columnNumber - 1) % 26;
    ans = String.fromCharCode(remain + 65) + ans;
    columnNumber = Math.floor((columnNumber - 1) / 26);
  }
  return ans;
};
```
