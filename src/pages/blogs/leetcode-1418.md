---
layout: ../../layouts/MarkdownWorksLayout.astro
title: 1418. 点菜展示表
date: 2021-07-06
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

[原题地址](https://leetcode-cn.com/problems/display-table-of-food-orders-in-a-restaurant/)

给你一个数组 orders，表示客户在餐厅中完成的订单，确切地说， orders[i]=[customerNamei,tableNumberi,foodItemi] ，其中 customerNamei 是客户的姓名，tableNumberi 是客户所在餐桌的桌号，而 foodItemi 是客户点的餐品名称。

请你返回该餐厅的 点菜展示表 。在这张表中，表中第一行为标题，其第一列为餐桌桌号 “Table” ，后面每一列都是按字母顺序排列的餐品名称。接下来每一行中的项则表示每张餐桌订购的相应餐品数量，第一列应当填对应的桌号，后面依次填写下单的餐品数量。

注意：客户姓名不是点菜展示表的一部分。此外，表中的数据行应该按餐桌桌号升序排列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/display-table-of-food-orders-in-a-restaurant
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

### **示例 1：**

```javascript
输入：orders = [["David","3","Ceviche"],["Corina","10","Beef Burrito"],["David","3","Fried Chicken"],["Carla","5","Water"],["Carla","5","Ceviche"],["Rous","3","Ceviche"]]
输出：[["Table","Beef Burrito","Ceviche","Fried Chicken","Water"],["3","0","2","1","0"],["5","0","1","0","1"],["10","1","0","0","0"]]
解释：
点菜展示表如下所示：
Table,Beef Burrito,Ceviche,Fried Chicken,Water
3    ,0           ,2      ,1            ,0
5    ,0           ,1      ,0            ,1
10   ,1           ,0      ,0            ,0
对于餐桌 3：David 点了 "Ceviche" 和 "Fried Chicken"，而 Rous 点了 "Ceviche"
而餐桌 5：Carla 点了 "Water" 和 "Ceviche"
餐桌 10：Corina 点了 "Beef Burrito"
```

### **示例 2：**

```javascript
输入：orders = [["James","12","Fried Chicken"],["Ratesh","12","Fried Chicken"],["Amadeus","12","Fried Chicken"],["Adam","1","Canadian Waffles"],["Brianna","1","Canadian Waffles"]]
输出：[["Table","Canadian Waffles","Fried Chicken"],["1","2","0"],["12","0","3"]]
解释：
对于餐桌 1：Adam 和 Brianna 都点了 "Canadian Waffles"
而餐桌 12：James, Ratesh 和 Amadeus 都点了 "Fried Chicken"
```

### **示例 3：**

```javascript
输入：orders = [["Laura","2","Bean Burrito"],["Jhon","2","Beef Burrito"],["Melissa","2","Soda"]]
输出：[["Table","Bean Burrito","Beef Burrito","Soda"],["2","1","1","1"]]
```

### **提示：**

```
1 <= orders.length <= 5 * 10^4
orders[i].length == 3
1 <= customerNamei.length, foodItemi.length <= 20
customerNamei 和 foodItemi 由大小写英文字母及空格字符 ' ' 组成。
tableNumberi 是 1 到 500 范围内的整数。
```

# **实现思路**

业务性比较强的题目，用哈希表存好键值对就 OK。

# **代码如下**

```javascript
/**
 * @param {string[][]} orders
 * @return {string[][]}
 */
var displayTable = function (orders) {
  let names = new Set();
  const map = new Map();
  for (let item of orders) {
    names.add(item[2]);
    if (!map.has(item[1])) {
      map.set(item[1], new Map());
    }
    let foods = map.get(item[1]);
    foods.set(item[2], (foods.get(item[2]) || 0) + 1);
  }
  const titles = ['Table'].concat(Array.from(names.keys()).sort());
  const ans = [titles];
  const tables = Array.from(map.keys()).sort((a, b) => a - b);
  for (let id of tables) {
    const table = [id + ''];
    for (let food of titles) {
      if (food !== 'Table') {
        if (map.get(id).has(food)) {
          table.push(map.get(id).get(food) + '');
        } else {
          table.push('0');
        }
      }
    }
    ans.push(table);
  }
  return ans;
};
```
