---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 815. 公交路线
date: 2021-06-28
tags:
  - BFS
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
website: https://leetcode-cn.com
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/bus-routes)
每日一题

给你一个数组 routes ，表示一系列公交线路，其中每个 routes[i] 表示一条公交线路，第 i 辆公交车将会在上面循环行驶。

例如，路线 routes[0] = [1, 5, 7] 表示第 0 辆公交车会一直按序列 1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ... 这样的车站路线行驶。
现在从 source 车站出发（初始时不在公交车上），要前往 target 车站。 期间仅可乘坐公交车。

求出 最少乘坐的公交车数量 。如果不可能到达终点车站，返回 -1 。

示例 1：

输入：routes = [[1,2,7],[3,6,7]], source = 1, target = 6
输出：2
解释：最优策略是先乘坐第一辆公交车到达车站 7 , 然后换乘第二辆公交车到车站 6 。

示例 2：

输入：routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12
输出：-1

提示：

1 <= routes.length <= 500.
1 <= routes[i].length <= 105
routes[i] 中的所有值 互不相同
sum(routes[i].length) <= 105
0 <= routes[i][j] < 106
0 <= source, target < 106

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bus-routes
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# **实现思路**

同 909，考虑用 BFS 实现，当然，本题难度为 Hard,在传统 BFS 的基础上，需要自己动手建立邻接矩阵，如何合理建立车站和路线的映射，也是本题的难点之一。

代码如下：

```javascript
/**
 * @param {number[][]} routes
 * @param {number} source
 * @param {number} target
 * @return {number}
 */
var numBusesToDestination = function (routes, source, target) {
  if (source === target) {
    return 0;
  }
  const map = new Map();
  // 建立起公交站到线路的映射。
  for (let i = 0; i < routes.length; i++) {
    for (let j = 0; j < routes[i].length; j++) {
      if (!map.has(routes[i][j])) {
        map.set(routes[i][j], []);
      }
      map.get(routes[i][j]).push(i);
    }
  }
  let ans = 1;
  // 队列保存的是线路
  const q = [];
  if (!map.has(source)) {
    return -1;
  }
  // 避免重复访问
  const visted = new Set();
  for (let item of map.get(source)) {
    q.push(item);
    visted.add(item);
  }
  while (q.length) {
    let len = q.length;
    for (let cnt = 0; cnt < len; cnt++) {
      let cur = q.shift(); // (1)
      for (let item of routes[cur]) {
        if (item === target) {
          return ans;
        }
        for (let route of map.get(item)) {
          // 去重逻辑，为什么不是在（1）那里做？
          if (!visted.has(route)) {
            q.push(route);
            visted.add(route);
          }
        }
      }
    }
    ans++;
  }
  return -1;
};
```

# **关于去重逻辑**

为什么去重逻辑是在队列加入新元素那里做而不是队列弹出元素那里做呢？

原因在于如果入队那里不做去重，那么`cur=q.shift()`要多执行很多次，而这个操作在数组中并不能达到 O(1),所以性能开销会很大。（所以 JS 基础库没有 queue 真的大丈夫吗 QAQ)。
