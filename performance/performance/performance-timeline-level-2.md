# Performance Timeline Level 2

## 总览

Performance Timeline 规范定义了 web 开发者可以使用哪些性能指标来监测他们的 web 应用，使他们变得更快。它主要定义了两种获取这些指标的方法：通过 [Performance](https://w3c.github.io/hr-time/#sec-performance) 接口的 getter 方法、通过 PerformanceObserver 接口。更推荐使用后者，因为它能减少因查询这些指标而带来的性能影响。

## PerformanceEntry

PerformanceEntry 对象包含一个确定指标的性能数据。PerformanceEntry 对象有 4 个属性：`name`, `entryType`, `startTime` 和 `duration` 。本规范并不定义具体的 PerformanceEntry 对象。定义具体 PerformanceEntry 对象的规范有：[Paint Timing](https://github.com/w3c/paint-timing), [User Timing](https://github.com/w3c/user-timing), [Resource Timing](https://github.com/w3c/resource-timing) 和 [Navigation Timing](https://github.com/w3c/navigation-timing/)。

## Performance getter 方法

[Performance](https://w3c.github.io/hr-time/#sec-performance) 接口新增了三个返回 PerformanceEntry 对象列表的方法：

* `getEntries()`: 返回 [Performance](https://w3c.github.io/hr-time/#sec-performance) 对象支持的全部条目。
* `getEntriesByType(type)`: 返回 `entryType` 匹配 _type_ 的 [Performance](https://w3c.github.io/hr-time/#sec-performance) 对象支持的全部条目。
* `getEntriesByName(name, type)`: 返回 `name` 匹配 _name_ 的 [Performance](https://w3c.github.io/hr-time/#sec-performance) 对象支持的全部条目。如果指定了可选参数 _type_ , 则只返回 `entryType` 匹配 _type_ 的条目。

### 在 JavaScript 中使用 getter 方法

下面这个例子展示了如何使用 `getEntriesByName()` 获取首次绘制信息:

```javascript
// 返回 FirstContentfulPaint 条目, 如果不存在则返回 null.
function getFirstContentfulPaint() {
  // 我们想要名称是"first-contentful-paint"且 entryType 是"paint"的条目.
  // getter 方法均返回条目的数组.
  const list = performance.getEntriesByName("first-contentful-paint", "paint");
  // 如果我们找到了条目，则列表实际长度应该为 1，所以返回列表的第一个条目。
  if (list.length > 0)
    return list[0];
  // 否则，这个条目并不在那，所以返回 null.
  else
    return null;
}
```



  


