# 次时代 web 前端性能监控

> The developer is encouraged to use ​[`PerformanceObserver`](https://www.w3.org/TR/performance-timeline-2/#dom-performanceobserver)​ where possible. Further, new performance API's and metrics may only be available through the ​[`PerformanceObserver`](https://www.w3.org/TR/performance-timeline-2/#dom-performanceobserver) interface.

## 背景

性能监控数据的准确性是指引我们选择性能优化方向的重中之重。长久以来，Web 开发者和浏览器厂商都对于性能监控的准确性做出了前赴后继的努力。

之前写过一篇性能监控历史的文章，​[前端性能统计方式概述——昨天、今天和明天](https://missfresh.feishu.cn/space/doc/doccnKKBqKM12FgVcPensQJr9Qh)​。里面提到了需要新的性能监控 API 的原因：

### 行业发展

2010 年 8 月，​`Performance Timeline API​` 正式被提出。

但随着时代的发展，前端页面的结构发生了翻天覆地的变化：前端页面从 HTML 为主展示型的简单页面结构，变成了动辄几 MB 甚至几十 MB JS bundle 的客户端渲染结构。

性能瓶颈也发生了变化：5G 到来了，​`HTTP/2`​ ​`HTTP/3`​ ​`TLS 1.3`​ 相继到来了。2019 年 6 月，Google 在 PerfMatters 性能大会上给出分析，当下前端性能的瓶颈，网络消耗的影响正逐渐变小，而本地执行 JavaScript 的消耗的影响逐渐变大。甚至在移动端，中档手机执行 JavaScript 的时长是高端手机的 3-4 倍，低端手机则要花超过 6 倍的时间。

### 问题

这带来了 2 个问题：

1. **过去定义的性能指标不再合理，**「首屏」「可交互时间」指标不再能很好的衡量页面性能**；**
2. **第一版 Performance API 的计算公式无法给出正确的性能数据：**例如之前通常以 DOM 加载完的时间近似为「首屏时间」，但目前前端页面的 SPA 结构，DOM 初始只有一个根节点，后面解析、执行 JS 代码的大量时间都被忽视了，导致无法发现真正的性能瓶颈**；**

### 进展

2017 年 5 月，Chrome Lighthouse 团队提出了以用户真实体验为中心的新性能指标，与下一代的 Performance API。

目前最新的进展，2019 年 11 月 11 日，Chrome Developer Summit 大会上，提出了 **LCP\(最大内容渲染\)**、**TBT\(页面阻塞总时长\)**、**CLS\(累积布局位移\)** 三个新指标与之前定义的 **FCP\(首次有内容绘制\)** 成为未来前端性能监控的主要指标与 Lighthouse 分数计算的新规则。

## Performance Observer API 介绍

### 问题

目前的前端性能监控面临 3 个问题：

1. 简单的使用过去的 performance API 测量的数据不准确；
2. 如果通过 performance API 进行复杂的模拟计算，测量性能的代码本身就消耗了性能；
3. 性能监控代码要在页面最前面引入，不然就错过了监控的时机，这依然会导致业务代码执行优先级被后置；

### 解决

然而正如前文提到的，浏览器最清楚页面的性能如何。浏览器提供 API 直接反馈页面性能，比开发者通过模拟手段计算要准确、便捷得多。

这个 API 就是本文的主角 ​`PerformanceObserver`​。它具有如下几个特点：

* 针对最有价值的性能指标直接给出数据，不需要开发者模拟计算；
* 在 CPU 空闲的时候才触发 observe 回调，不影响业务代码的执行优先级；
* Level 2 提供了 ​`buffered`​ 参数，可以后置 API 的引入时机，不影响业务代码的下载优先级；

目前浏览器支持度：

2016 年 7 月，Chrome 52 和 Andriod webview 开始支持，iOS 11 和 Firefox 57 也分别在 2017 年 9 月和 11 月提供了这个 API。

虽然这个 API 面世至今已 3 年有余，但仍随着性能指标的演进而迭代出新的特性。

### 用法

先来看下这个 API 的基本用法：

```javascript
const observer = new PerformanceObserver((list) => {})
observer.observe({entryTypes: ['paint', 'largest-contentful-paint']})
```

我们实例化了一个 ​`PerformanceObserver`​，并观察了 2 个类型的条目「paint」和「LCP」。​

`observe`​ 方法接收一个 ​`entryTypes`​ 的参数，据我目前能够搜集到的资料，目前被定义的 entryType 共有 12 种。

截止目前，Chrome 78 实现了 10 种：​`element(元素)`​、`​first-input(首次交互)`​、​`largest-contentful-paint(最大内容绘制)​`、​`layout-shift(布局位移)`​、`​longtask(长时任务)`​、​`mark(标记)`​、​`measure(测量)​`、​`navigation(导航)`​、`​paint(绘制)`​、`​resource(资源)​`。Firefox 实现了 4 种：元素、标记、测量、导航。

### 功能介绍

其中 ​`paint`​ 类型会返回 ​`PerformancePaintTiming`​ 类的值，值一共有 2 种： ​`first-paint`​ 和 ​`first-contentful-paint`​，分别表示浏览器绘制第一个元素的时间戳和浏览器绘制第一个有内容元素的时间戳。

而 LCP 这个类型是 Chrome 在 2019 年 9 月的 77 版本刚刚支持的：[https://developers.google.com/web/updates/2019/09/nic77\#lcp](https://developers.google.com/web/updates/2019/09/nic77#lcp)  

上面的性能指标的文章提到了，LCP 是作为首屏时间 FMP 的替代品被提出的，他的计算算法在 ​[LCP「标准化」算法](https://missfresh.feishu.cn/space/doc/doccnHBN876hP3IvQWu43mNkk1f)​ 中有详细介绍。大致概括如下：

1. 给每个新加入的节点添加标记，记录对应时间戳；
2. 当节点变化趋于稳定或达到阈值之后，结束监听；
3. 根据当前所有节点计算评分（元素在视口内的尺寸 \* 权重），得到主角元素；
4. 如果主角元素是普通元素则取该元素加入时的时间点为 LCP； 如果主角元素是媒体元素，则取媒体资源下载完的时间为 LCP。

## 未来已来 —— Polyfill 实现

正如 ES Module 时代不再需要 AMD & CMD，次时代的 web 前端性能监控 API 只需要 ​Performance Observer API​。如果浏览器不支持，那就写个 polyfill！  
[Chromium 实现](https://github.com/chromium/chromium/blob/master/third_party/blink/renderer/core/timing/performance_observer.cc)   
[node.js 实现](https://github.com/nodejs/node/blob/master/lib/perf_hooks.js)   
Maopy 的实现：[https://github.com/Maopy/split-time](https://github.com/Maopy/split-time)  
\* Split-time 项目命名取自赛跑运动中「分段计时」的概念。

### 设计

#### 原则

1. 如果浏览器本身支持 PerformanceObserver 则使用浏览器的；
2. 如果浏览器仅支持部分 entryType 则使用 polyfill 实现不支持的部分；

#### 妥协

* Observer 的优势是可以在合适的时候给我们返回值，但我们自己实现就只能通过轮询的手段实现；
* 或者实现另一个 API 的 polyfill；

#### 名词​

`PerformanceObserver`​ 不用说了；  
​`entry`​ ​`PerformanceEntry`​ 类型，性能条目；​  
`entryTypes`​ 性能条目类型，就是上面提到的 10 种；  
​`taskQueue`​ 观察者有一个全局的任务队列，负责收集性能条目，并控制合适的时机调用回调函数，返回这些值；​  
`buffer`​ 每个观察者即将返回的性能条目缓冲区

### 实现

首先定义类型。标准里面规定了，PerformanceObserver 需要有：

* 创建时设置 PerformanceObserverCallback；
* PerformanceEntryList 的 buffer，初始化为空；
* entryTypes 观察的类型，初始化为空；

```javascript
class SplitTime implements SplitTime {
  static readonly supportedEntryTypes: string[] = []
  public callback: PerformanceObserverCallback
  public buffer: Set<PerformanceEntry>
  public entryTypes: string[] = []
  private taskQueue: TaskQueue

  public constructor(callback: PerformanceObserverCallback) {
    this.callback = callback
    this.buffer = new Set()
    this.taskQueue = globalTaskQueue
    this.useNative = false
  }
}
```

Observe 方法：

```javascript
public observe(options?: PerformanceObserverInit): void {
  const { entryTypes } = options
  let supportedOptionsEntryTypes = []
  this.entryTypes = entryTypes
  this.unsupportedEntryTypes = entryTypes
  // 如果浏览器已经支持
  if (ifSupported) {
    const { supportedEntryTypes } = PerformanceObserver
    const supportedEntryTypesSet = new Set(supportedEntryTypes)
    supportedOptionsEntryTypes = entryTypes.filter((entryType) => supportedEntryTypesSet.has(entryType))
    this.unsupportedEntryTypes = entryTypes.filter((entryType) => !supportedEntryTypesSet.has(entryType))
    // 支持的部分用原生方法
    if (supportedOptionsEntryTypes.length) {
      this.useNative = true
      new PerformanceObserver((list, observer) => {
        this.processEntries(list)
      })
        .observe({ entryTypes: supportedOptionsEntryTypes })
    }
  }
  if (this.unsupportedEntryTypes.length) {
    // 不支持的部分用自己实现
    this.unsupportedEntryTypes.forEach((entryType) => {
      switch (entryType) {
        case 'paint':
          PaintObserve()
            .then((entries) => {
              // 合并原生条目与自己实现的条目
              this.taskQueue.performanceEntries = new Set([...this.taskQueue.performanceEntries, ...entries])
            })
          break
        case 'largest-contentful-paint':
          LCPObserve()
            .then((entries) => {
              this.taskQueue.performanceEntries = new Set([...this.taskQueue.performanceEntries, ...entries])
            })
      }
    })
    this.taskQueue.add(this)
  }
}
```

调用回调函数：

```javascript
private processEntries(list?: PerformanceObserverEntryList):void {
  this.useNative = true
  const entries = Array.from(this.buffer)
  const nativeEntries = (list && list.getEntries()) || []
  // Combine entries from native & SplitTime
  const entryList = new EntryList([...entries, ...nativeEntries])
  this.buffer.clear()
  this.callback(entryList, this)
  // if native callback isn't called for a long time & buffer is still not empty, call split-time process method
  self.setTimeout(() => {
    this.useNative = false
  }, 1000)
}
```

### 测试

下图为我们自己实现的 LCP 算法与浏览器原生 LCP 返回的对比。以「天天领钱」页面为例，可以看到选取的元素相同，计算的时间误差大概 30ms，基本可以认为准确。![](https://missfresh.feishu.cn/space/api/file/out/U8a5sGelXz1HP8MViZaU9yx5vF4HArOuU1svsvRIUwVaI2ktqG/)

![](../.gitbook/assets/image%20%286%29.png)

