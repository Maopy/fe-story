# 附录B 块作用域的替代方案

至少从 ES3 发布以来，JavaScript 中就有了块作用域，而 with 和 catch 分句就是块作用域的两个小例子。

## B.1 Traceur

Google 维护着一个名为 Traceur 的项目，该项目正是用来将 ES6 代码转换成兼容 ES6 之前的环境。

```javascript
{
  try {
    throw undefined
  } catch (a) {
    a = 2
    console.log(a)
  }
}

console.log(a)
```

## B.2 隐式和显式作用域

## B.3 性能

首先，try/catch 的性能的确很糟糕，但技术层面上没有合理的理由来说明 try/catch 必须这么慢，或者会一直慢下去。

其次，IIFE 和 try/catch 并不是完全等价的，因为如果将一段代码中的任意一部分拿出来用函数进行包裹，会改变这段代码的含义，其中的 this、return、break 和 contine 都会发生变化。IIFE 并不是一个普适的解决方案，它只适合在某些情况下进行手动操作。

