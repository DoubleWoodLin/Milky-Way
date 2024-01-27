---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 5799. 最美子字符串的数目
date: 2021-06-27
tags:
  - 状态压缩
  - 前缀和
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

[原题地址](https://leetcode-cn.com/problems/number-of-wonderful-substrings/)

第 247 场恒生周赛第三题。

如果某个字符串中 至多一个 字母出现 奇数 次，则称其为 最美 字符串。

例如，"ccjjc" 和 "abab" 都是最美字符串，但 "ab" 不是。
给你一个字符串 word ，该字符串由前十个小写英文字母组成（'a' 到 'j'）。请你返回 word 中 最美非空子字符串 的数目。如果同样的子字符串在 word 中出现多次，那么应当对 每次出现 分别计数。

子字符串 是字符串中的一个连续字符序列。

示例 1：

输入：word = "aba"
输出：4
解释：4 个最美子字符串如下所示：

- "aba" -> "a"
- "aba" -> "b"
- "aba" -> "a"
- "aba" -> "aba"

来源：力扣（LeetCode）

[链接](https://leetcode-cn.com/problems/number-of-wonderful-substrings)

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# **实现思路**

根据题意,如果 word[i...j]是最美字符串,那么有 word[0...j]与 word[0...i-1]中包含的各种字母数量具有相同的奇偶性,或者最多有一种字母奇偶性不同。

比如有 word='aabb',’aabb'是最美字符串,count(a)=count(b)=2,其中'bb'也是最美字符串,那么'aa'与’aabb'应有一致的奇偶性,count(a)=2,count(b)=0,符合。这个即使就是前缀和的思路啦，我们可以用一个二进制变量 mask 来保存字符串的奇偶性状态，由于只有 10 种字母，mask 一共 10 位有效，0 表示该种字母的数量是偶数，1 表示是奇数。然后建一个哈希表把每种状态出现的次数映射进去，每遍历到一个状态，从哈希表中查询满足条件的状态出现了几次，符合条件的子串就加上几个。

代码如下:

```javascript
/**
 * @param {string} word
 * @return {number}
 */
var wonderfulSubstrings = function (word) {
  const n = word.length;
  const masks = new Array(n + 1);
  masks[0] = 0;
  const map = new Map(); // map用来映射每个字母相对的偏移量
  for (let i = 0; i < 10; i++) {
    map.set(String.fromCharCode(97 + i), i);
  }
  // 建立状态压缩的数组。
  for (let i = 1; i < n + 1; i++) {
    const char = word[i - 1];
    let xor = 1 << map.get(char);
    // 每一个新状态由前一个状态取反其对应新字母的奇偶性得来
    masks[i] = masks[i - 1] ^ xor;
  }
  const record = new Map();
  record.set(0, 1);
  let ans = 0;
  // 重新遍历状压数组，添加满足条件的子串数量
  for (let i = 1; i < n + 1; i++) {
    ans += record.get(masks[i]) || 0;
    let xor = 1;
    for (let j = 0; j < 10; j++) {
      let temp = xor << j;
      ans += record.get(masks[i] ^ temp) || 0;
    }
    record.set(masks[i], (record.get(masks[i]) || 0) + 1);
  }
  return ans;
};
```

时间复杂度是 O(n\*10)=O(n),n 是 word 的长度，10 是字母种类。空间复杂度在 O(n),建立了长度 n+1 的状压数组。
