---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 726. 原子的数量
date: 2021-07-05
tags:
  - 每日一题
  - 哈希表
  - 栈
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
website: https://leetcode-cn.com/problems/number-of-atoms/
github: https://github.com/DoubleWoodLin
---

# **题目说明**

[原题地址](https://leetcode-cn.com/problems/number-of-atoms/)

给定一个化学式 formula（作为字符串），返回每种原子的数量。

原子总是以一个大写字母开始，接着跟随 0 个或任意个小写字母，表示原子的名字。

如果数量大于 1，原子后会跟着数字表示原子的数量。如果数量等于 1 则不会跟数字。例如，H2O 和 H2O2 是可行的，但 H1O2 这个表达是不可行的。

两个化学式连在一起是新的化学式。例如  H2O2He3Mg4 也是化学式。

一个括号中的化学式和数字（可选择性添加）也是化学式。例如 (H2O2) 和 (H2O2)3 是化学式。

给定一个化学式  formula ，返回所有原子的数量。格式为：第一个（按字典序）原子的名字，跟着它的数量（如果数量大于 1），然后是第二个原子的名字（按字典序），跟着它的数量（如果数量大于 1），以此类推。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/number-of-atoms
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

### **示例 1：**

```javascript
输入：formula = "H2O"
输出："H2O"
解释：
原子的数量是 {'H': 2, 'O': 1}。
```

### **示例 2：**

```javascript
输入：formula = "Mg(OH)2"
输出："H2MgO2"
解释：
原子的数量是 {'H': 2, 'Mg': 1, 'O': 2}。
```

### **示例 3：**

```javascript
输入：formula = "K4(ON(SO3)2)2"
输出："K4N2O14S4"
解释：
原子的数量是 {'K': 4, 'N': 2, 'O': 14, 'S': 4}。
```

### **示例 4：**

```javascript
输入：formula = "Be32"
输出："Be32"
```

### **提示：**

```
1 <= formula.length <= 1000
formula 由小写英文字母、数字 '(' 和 ')' 组成。
formula 是有效的化学式。
```

# **实现思路**

模拟到吐 🤮，简单说下思路吧，我们用一个栈来对这个字符串”序列化“，确保序列化之后栈中每一个原子和它的数目成对出现，比如：

```
Mg(OH)2=>[Mg,1,O,2,H,2]
K4(ON(SO3)2)2=>[K,4,O,2,N,2,S,4,O,12]
```

序列化完之后，我们就可以用哈希表进行统计了。至于序列化的过程嘛，要考虑大写字母、小写字母、左右括号、原子后有无数字（无数字要补 1），比较考验基本功了，基本功不好就会跟我一样写得又臭又长了 😁，纯体力活，代码大家看看就好。写了有一个小时吧，一遍过还是很开心的，拒绝当 cv 工程师，从对困难题重拳出击开始 QAQ

# **代码如下**

```javascript
/**
 * @param {string} formula
 * @return {string}
 */
var countOfAtoms = function (formula) {
  const n = formula.length;
  const stack = [];
  const stacktemp = [];
  const map = new Map();
  const shouldAdd = (index) => {
    if (index === n) {
      return true;
    }
    const char = formula[index];
    if (char === '(' || char === ')') {
      return true;
    }
    const num = char.charCodeAt();
    if (num >= 65 && num <= 90) {
      return true;
    }
    return false;
  };
  for (let i = 0; i < n; i++) {
    if (formula[i] === '(') {
      stack.push(formula[i]);
      continue;
    }
    if (formula[i] === ')') {
      let tag = 1;
      if (
        i < n - 1 &&
        Number(formula[i + 1]) >= 0 &&
        Number(formula[i + 1] <= 9)
      ) {
        tag = Number(formula[i + 1]);
        i++;
        for (let j = i + 1; j < n; j++) {
          const temp = formula[j].charCodeAt();
          if (temp < 48 || temp > 57) {
            break;
          }
          tag *= 10;
          tag += Number(formula[j]);
          i = j;
        }
      }
      while (stack[stack.length - 1] !== '(') {
        let cur = stack.pop();
        if (typeof cur === 'number') {
          cur *= tag;
        }
        stacktemp.push(cur);
      }
      stack.pop();
      while (stacktemp.length > 0) {
        stack.push(stacktemp.pop());
      }
      continue;
    }
    let code = formula[i].charCodeAt();
    if (code >= 65 && code <= 90) {
      stack.push(formula[i]);
      if (shouldAdd(i + 1)) {
        stack.push(1);
      }
    }
    if (code >= 97 && code <= 122) {
      let str = stack.pop();
      str += formula[i];
      stack.push(str);
      if (shouldAdd(i + 1)) {
        stack.push(1);
      }
    }
    if (code >= 48 && code <= 57) {
      let num = Number(formula[i]);
      for (let j = i + 1; j < n; j++) {
        const temp = formula[j].charCodeAt();
        if (temp < 48 || temp > 57) {
          break;
        }
        num *= 10;
        num += Number(formula[j]);
        i = j;
      }
      stack.push(num);
    }
  }
  // console.log(stack,formula);
  while (stack.length > 0) {
    let value = stack.pop();
    let key = stack.pop();
    map.set(key, (map.get(key) || 0) + value);
  }
  let ans = '';
  const arr = Array.from(map.entries()).sort((a, b) =>
    a[0].localeCompare(b[0])
  );
  for (let [key, value] of arr) {
    if (value === 1) {
      ans += key;
    } else {
      ans += key + value;
    }
  }
  return ans;
};
```
