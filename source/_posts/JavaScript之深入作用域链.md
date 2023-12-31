---
title: JavaScript之深入作用域链
date: 2022-02-18 17:16:09
tags:
 - JavaScript
categories: web前端
---
### 1. 作用域链
在查找变量时，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

### 2. 函数创建
函数有一个内部属性[[scope]]，当函数创建时，会保存所有父变量对象到其中，你可以理解[[scope]]就是所有父变量对象的层级链，但它并不代表完整的作用域链！
```demo
function foo() {
    function bar() {
        ...
    }
}

foo.[[scope]] = [ 
  globalContext.VO //foo函数父层为window变量对象
];

bar.[[scope]] = [
    fooContext.AO,
    globalContext.VO // bar函数父层为foo函数上下文活动对象
];
```
### 3. 函数激活
当函数激活时，进入函数上下文，创建VO/AO后，就会将活动对象添加到作用域链的前端。
这时候执行上下午的作用域链(Scope)：
Scope = [AO].concat([[Scope]])

然后作用域链创建完毕。

### 4. 小结
函数执行过程如下：
1. 创建函数，将作用域链保存到函数内部属性[[scope]]
2. 执行函数，创建函数执行上下文，函数执行上下文被压入到执行上下文栈
3. 函数并不立刻执行，复制函数[[scope]]属性创建作用域链(Scope属性)
4. 用arguments创建活动对象，随后初始化活动对象，加入形参、函数声明、变量声明
5. 将活动对象压入函数作用域链顶端
6. 准备工作做完，开始执行函数，随着函数的执行，修改AO的属性值
7. 查找到函数执行上下文的值，返回后函数执行完毕，函数上下文从执行上下文栈中弹出
