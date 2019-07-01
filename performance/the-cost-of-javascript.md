---
description: '原文链接：https://v8.dev/blog/cost-of-javascript-2019'
---

# The cost of JavaScript

纵观过去几年 [JavaScript 的消耗](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4)，很大的变化在于浏览器解析和编译脚本性能的提升。**而 2019 年，首要的消耗是下载和 CPU 执行的耗时。**

如果浏览器的主线程处于繁忙执行 JavaScript 的状态，用户交互就会感受到延迟，所以优化脚本执行时间和网络耗时的瓶颈会是非常有效的。

### 非常可行的指导

这对于 web 开发者来说意味着什么？解析和编译的消耗不像我们一开始想象的那么慢了。JavaScript 包需要关注的三件事是：

* 优化下载时间
  * 将 JavaScript 包体积保持在足够小，特别是在移动平台上面。小的包会提升下载速度，减少内存使用并减少 CPU 的消耗。
  * 避免只打一个单独的大包；如果一个包超过大约 50 - 100 kB，就把它拆分成更小的包。
* 优化执行时间
* 避免大量内联脚本

