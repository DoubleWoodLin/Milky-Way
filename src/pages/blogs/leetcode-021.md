---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 21. 合并两个有序的链表
date: 2021-04-15
tags:
  - 链表
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
website: https://leetcode-cn.com/problems/merge-two-sorted-lists/
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

题目大意是给出两个有序链表的头节点，合成一个有序链表并返回其头节点。

思路是创建一个哨兵节点作为虚拟头，然后双指针遍历两个链表，将较小的元素接在虚拟头后方，直到其中一条链表遍历结束，当前节点的 next 指向另一条链表的剩余部分，即可得到完整的升序链表。最后返回虚拟头的 next 指针，即答案链表的头节点。

代码如下：

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function (l1, l2) {
  let head = new ListNode();
  let temp = head;
  while (l1 !== null && l2 !== null) {
    if (l1.val >= l2.val) {
      temp.next = l2;
      l2 = l2.next;
    } else {
      temp.next = l1;
      l1 = l1.next;
    }
    temp = temp.next;
  }
  temp.next = l1 === null ? l2 : l1;
  return head.next;
};
```

虚拟头不参与直接遍历，它的作用就是方便我们返回合成链表的头部，减少一些逻辑判断，非常方便。
