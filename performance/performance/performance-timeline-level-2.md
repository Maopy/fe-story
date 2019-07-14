# Performance Timeline Level 2

## 总览

Performance Timeline 规范定义了 web 开发者可以使用哪些性能指标来监测他们的 web 应用，使应用变得更快。它主要定义了两种获取这些指标的方法：通过 [Performance](https://w3c.github.io/hr-time/#sec-performance) 接口的 getter 方法、通过 PerformanceObserver 接口。更推荐使用后者，因为它能减少因查询这些指标而带来的性能影响。

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

## PerformanceObserver

PerformanceObserver 对象可以根据 `entryType` 值，来通知新的 PerformanceEntry 对象。它的构造函数必须接收一个回调函数，这个回调函数在浏览器分发了新的 `entryType` 值匹配了任一观察者观察中的条目时执行。这个回调函数并不每个 PerformanceEntry 运行一次，也不会在 PerformanceEntry 对象创建时立刻运行。取而代之的是，这些条目在 PerformanceObserver “排队”，而浏览器可以之后再执行这些函数。当回调函数被执行时，所有排队的条目都被传给函数，然后 PerformanceObserver 的队列会被重置。PerformanceObserver 初始状态不会观察任何东西：必须通过调用 `observe()` 函数来指定观察哪类 PerformanceEntry 对象。`observe()` 函数在调用时可以传条目类型的数组或单独的类型字符串，就像下面展示的那样。这些模式不能混用，否则会抛出一个异常。

**`supportedEntryTypes`**

静态属性 `PerformanceObserver.supportedEntryTypes` 返回浏览器支持的 `entryType` 数组，按照字母表顺序排序。可以用来检测支持的特性。

**`observe(entryTypes)`**

PerformanceObserver 可以在一次 `observe()` 的调用过程，指定多个 `entryTypes` 的值。然而，这种情况不允许传额外的参数。多次 `observe()` 调用会覆盖之前观察的类型。函数调用的示例：`observer.observe({entryTypes: ['resource', 'navigation']})`。

**`observe(type)`**

PerformanceObserver 在调用 `observe()` 方法时，可以只传一个单独的类型。这种情况可以接收额外的参数。多次 `observe()` 调用会堆叠，除非之前有相同 `type` 的观察被调用了，这时会被覆盖。函数调用的示例：`observer.observe({type: "mark"})`。

**`buffered` 标记**

这份标准定义了一个可以在 `observe(type)` 使用的参数：`buffered` 标记，默认是未设置的。当这个标记被设置时，浏览器会分发 PerformanceObserver 创建之前缓存的记录，它们会在`observe()` 调用发生之后的第一个回调中被接收。这可以让 web 开发者在方便的时候再注册 PerformanceObserver 而且不会因此错过页面加载早期分发的条目。使用这个标记的示例：`observer.observe({type: "measure", buffered: true})`。





