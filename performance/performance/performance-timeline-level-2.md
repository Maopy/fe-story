# Performance Timeline Level 2

## 总览

Performance Timeline 规范定义了 web 开发者可以使用哪些性能指标来监测他们的 web 应用，使他们变得更快。它主要定义了两种获取这些指标的方法：通过 [Performance](https://w3c.github.io/hr-time/#sec-performance) 接口的 getter 方法、通过 PerformanceObserver 接口。更推荐使用后者，因为它能减少因查询这些指标而带来的性能影响。

## PerformanceEntry

PerformanceEntry 对象包含一个确定指标的性能数据。PerformanceEntry 对象有 4 个属性：`name`, `entryType`, `startTime` 和 `duration` 。本规范并不定义具体的 PerformanceEntry 对象。定义具体 PerformanceEntry 对象的规范有：[Paint Timing](https://github.com/w3c/paint-timing), [User Timing](https://github.com/w3c/user-timing), [Resource Timing](https://github.com/w3c/resource-timing) 和 [Navigation Timing](https://github.com/w3c/navigation-timing/)。

  


