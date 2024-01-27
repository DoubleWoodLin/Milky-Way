---
layout: ../../layouts/MarkdownWorksLayout.astro
title: Function = new Function()?聊聊原型链的起源所在
date: 2021-09-06
tags:
  - 原型链
categories:
  - Frontend
image:
  url: '/blog-post.webp'
  alt: 'blog placeholder'
worksImage1:
  url: '/blog-post.webp'
  alt: 'blog placeholder'
worksImage2:
  url: '/blog-post.webp'
  alt: 'blog placeholder.'
platform: Web
stack: JavaScript
website: https://astro-milky-way.netlify.app/
github: https://github.com/DoubleWoodLin
---

## **问题的提出**

众所周知，原型链是 JS 实现面向对象的一种方式。

```JavaScript
function Animal(name){
  this.name=name
}
Animal.prototype={
  sayHi(){
    console.log('Hi,I am '+this.name);
  }
}
let dog=new Animal('betty');
dog.sayHi();// Hi, I am betty
```

上例中，我们通过原型让实例 dog 拥有了 sayHi 的能力，而实例 dog 可以通过**proto**属性访问原型

```javascript
dog.__proto__ === Animal.prototype; // true
```

更多的例子：

```javascript
Number.__proto__ === Function.prototype;
String.__proto__ === Function.prototype;
```

可以认为 Number、String 都是 Function 的实例，重点来了：

```javascript
Function.__proto__ === Function.prototype; // true
```

是否可以认为 Function 是 Function 的实例呢？Function=new Function()成立？

## **解答**

答案显然是不能。如果能的话，不就变成先有鸡还是先有蛋的问题了吗？

Function.prototype 是浏览器内置的对象，而非任何一个构造函数生成的实例，同理 Object.prototype 也一样，显然 Object.prototype 是一个对象，但 Object.prototype 不是一个实例，而且 Object.prototype.**proto**===null,即实例皆对象，而对象并非都是实例。

关于这个问题的研究，可以看看[从探究 Function.**proto**===Function.prototype 过程中的一些收获](https://github.com/jawil/blog/issues/13)

## **总结**

通过对原型链的深入探讨，解决了一直以来的疑惑。
