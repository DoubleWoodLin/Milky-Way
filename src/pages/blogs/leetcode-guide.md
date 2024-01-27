---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 指南
date: 2020-11-07
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

# **JavaScript 题解**

LeetCode 题解的主流是 Java，Python 和 C++,有时一些比较冷门的题目没有 JS 题解（比如周赛官方题解一般是 C++和 Python），为了锻炼 coding 能力，本系列题解基本都是使用 JS 写的（以后看情况加入其他语言），适合有刷题需求的前端 er，由于笔者水平有限，有些代码实现可能不够优雅，或者思路描述不够清晰，如果你有疑问或者想看的题解，欢迎留言交流。

# **实用技巧**

下面介绍几个编程经常用到的技巧和笔者踩过的坑。

## 1、哨兵编程

哨兵编程通常是用来简化程序逻辑，比如在需要操作链表的题目中，我们可以创建一个虚拟头来帮助我们获取真实的链表头部，这样就算链表头部被删除，我们也不需要增加额外的判断逻辑，详情参考[203. 移除链表元素](./203.md)。要注意的是，哨兵不会参与到实际的业务里，它存在的意义就是简化逻辑。

## 2、位运算技巧

位运算算是出现频率比较高的了，熟练掌握位运算技巧可以帮助我们更高效刷题。

#### **使用 x>>1：**

x>>1 即将 x 二进制位向右移动一位，相当于 Math.floor(x/2),有时我们可以利用这一点来代替地板除。要注意的是，尽管 JS 中最大安全整数是

```javascript
2 ** 53 - 1;
```

但是支持移位操作的数字不得大于 2\*\*31-1。

#### **x&1：**

x&1===0 可以用来判断 x 的奇偶性，相当于 x%2===0。

#### **x&(-x)：**

保留二进制下最右边出现的 1 的位置，其余位置置 0。

#### **x&(x-1):**

消除二进制下最右边出现 1 的位置，其余保持不变。

#### **高低位交换**

```javascript
x = (x >> 16) | (x << 16);
```

将 x 的前 16 位与后 16 位交换，当然，x 必须是无符号整数。

## 3、堆（优先级队列）

众所周知，JS 标准库是没有实现优先级队列的，好在 Node 生态蓬勃，npm 上 priorityqueue 相关的包还是很多，平时开发没影响。但我们刷题时该怎么办呢？LeetCode 内置了[datastructures-js/priority-queue](https://github.com/datastructures-js/priority-queue),这个库基本可以满足构造大/小顶堆的需求，但美中不足的是，这个库不支持自定义的 comparator，构造队列时传入的函数只能指明哪一项属性是 priority，而不是像 sort 方法传入一个类似

```javascript
（a,b)=>a-b
```

这样的 comparator。而在很多场景下，单纯的大顶堆、小顶堆是满足不了业务需求的，比如[692. 前 K 个高频单词](./leetcode-692.md),这道题要求当单词频率相同时，按字典序排列，如果我们用优先级队列来做，就需要自定义的 comparator：

```javascript
//a:[string,number],b:[string,number]
const pq = new PriorityQueue((a, b) => {
  if (a[1] === b[1]) {
    return b[0].localeCompare(a[0]);
  }
  return a[1] - b[1];
});
```

单纯的小顶堆无法实现字典序的排列的需求。目前已经有人提出 issue，相信该库不久后会支持自定义 comparator。<br>
我实现了一版[支持自定义 comparator 的 priorityqueue](https://github.com/DoubleWoodLin/priorityQueue),遇到优先级队列相关的题目直接 CV,觉得好用的童鞋给个 star 吧^\_^。
