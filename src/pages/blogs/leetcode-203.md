---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 203. 移除链表元素
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
website: https://leetcode-cn.com/problems/remove-linked-list-elements
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/remove-linked-list-elements)

给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。

示例 1：

输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]

示例 2：

输入：head = [], val = 1
输出：[]

示例 3：

输入：head = [7,7,7,7], val = 7
输出：[]

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/remove-linked-list-elements
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

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
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function (head, val) {
  const dummy = new ListNode(0, head);
  let cur = dummy.next,
    prev = dummy;
  while (cur) {
    if (cur.val === val) {
      cur = cur.next;
      prev.next = cur;
    } else {
      prev = cur;
      cur = cur.next;
    }
  }
  return dummy.next;
};
```

非常基础的入门题，用虚拟头可以减少判断删除头节点的条件语句，顺带发一版递归解法，对递归还不是很熟悉的小伙伴可以拿这个练一下~

```javascript
var removeElements = function (head, val) {
  if (!head) {
    return null;
  }
  let node = removeElements(head.next, val);
  head.next = node;
  if (head.val === val) {
    return node;
  }
  return head;
};
```
