---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 1711. 大餐计数
date: 2021-07-07
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
website: https://leetcode-cn.com
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/count-good-meals/)

大餐 是指 恰好包含两道不同餐品 的一餐，其美味程度之和等于 2 的幂。

你可以搭配 任意 两道餐品做一顿大餐。

给你一个整数数组 deliciousness ，其中 deliciousness[i] 是第 i​​​​​​​​​​​​​​ 道餐品的美味程度，返回你可以用数组中的餐品做出的不同 大餐 的数量。结果需要对 109 + 7 取余。

注意，只要餐品下标不同，就可以认为是不同的餐品，即便它们的美味程度相同。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/count-good-meals
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

### **示例 1：**

```javascript
输入：deliciousness = [1,3,5,7,9]
输出：4
解释：大餐的美味程度组合为 (1,3) 、(1,7) 、(3,5) 和 (7,9) 。
它们各自的美味程度之和分别为 4 、8 、8 和 16 ，都是 2 的幂。
```

### **示例 2：**

```javascript
输入：deliciousness = [1,1,1,3,3,3,7]
输出：15
解释：大餐的美味程度组合为 3 种 (1,1) ，9 种 (1,3) ，和 3 种 (1,7) 。
```

### **提示：**

```
1 <= deliciousness.length <= 1e5
0 <= deliciousness[i] <= 2**20
```

# **实现思路**

用哈希表记录每种美味出现的次数，再遍历 1 到可能出现的最大美味组合（使用左移）,即可统计答案，注意每次遍历要拷贝一遍哈希表。时间复杂度（nlogC)C 最大时是 2\*\*21。

# **代码如下**

```javascript
/**
 * @param {number[]} deliciousness
 * @return {number}
 */
var countPairs = function (deliciousness) {
  const MOD = 1e9 + 7;
  const counts = new Map();
  let max_VAl = 0;
  for (const food of deliciousness) {
    counts.set(food, (counts.get(food) || 0) + 1);
    max_VAl = Math.max(max_VAl, food);
  }
  max_VAl *= 2;
  let ans = 0;
  for (let i = 1; i <= max_VAl; i <<= 1) {
    let map = new Map(counts);
    for (let food of deliciousness) {
      let count = map.get(i - food) || 0;
      if (food === i - food) {
        count--;
      }
      ans += count;
      if (ans >= MOD) {
        ans %= MOD;
      }
      map.set(food, map.get(food) - 1);
    }
  }
  return ans;
};
```
