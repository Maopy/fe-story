# User-centric Performance Metrics

## 以用户为中心的性能指标

### 是否发生？(Visual metrics)

* 首次绘制(FP=First Paint)\
  标记浏览器渲染_任何_在视觉上不同于导航前屏幕内容之内容的时间点。
* **首次有内容绘制(FCP=First Contentful Paint)**\
  浏览器渲染来自 DOM 第一位内容的时间点，该内容可能是文本、图像、SVG 甚至 `<canvas>` 元素。

### 是否有用？(Interactive)

*   **首次有效绘制(FMP=First Meaningful Paint)**\
    “有效”这一概念很难通用于所有网页的方式规范化（因此尚不存在任何规范），但是网页开发者自己很清楚其页面的哪些部分对用户最为有用。\
    定义：主要内容绘制完成

    不作数的：

    * loading 态
    * FOUC(A flash of unstyled content)
    * 只渲染了导航或头部
* **主角元素耗时(Hero Element Timing)**\
  ****定义：网页“最重要的部分”绘制完成。

### 是否可用？(是否能够操作页面了)

* 视觉准备(Visual Ready)
* **首次可交互时间(TTI=Time To Interactive)**\
  应用已进行视觉渲染并能可靠响应用户输入的时间点。TTI 指标可识别页面初始 JavaScript 已加载且主线程处于空闲状态（没有耗时较长的任务）的时间点。
  * FMP 已完成
  * 85%的视觉完备
  * DOMContentLoaded 被触发
* **首次输入延迟(FID=First Input Delay)**\
  ****定义：从用户首次产生交互到浏览器实际给出反馈的延迟时间

### 是否令人愉快？(是否没有滞后和卡顿)

* **耗时较长的任务(Long Tasks)**\
  ****任何耗时超过 50 毫秒的任务。

### 指标示意图

![](../../.gitbook/assets/image.png)

![](<../../.gitbook/assets/image (8).png>)

![](<../../.gitbook/assets/image (3).png>)

## 衡量指标的方法

得益于新增的几个浏览器 API，我们终于可以在实际设备上衡量这些指标，而无需使用大量可能降低性能的黑客手段或变通方法。

这些新增的 API 是 [`PerformanceObserver`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserver)、[`PerformanceEntry`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEntry) 和 [`DOMHighResTimeStamp`](https://developer.mozilla.org/en-US/docs/Web/API/DOMHighResTimeStamp)。

### 追踪 FP/FCP

```javascript
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    // `name` will be either 'first-paint' or 'first-contentful-paint'.
    const metricName = entry.name
    const time = Math.round(entry.startTime + entry.duration)
  }
})
observer.observe({entryTypes: ['paint']})
```

{% hint style="info" %}
必须确保 PerformanceObserver 注册在 \<head> 元素且位于任何样式表的前面，这样才会在 FP/FCP 时触发。

实现  [Performance Observer 规范](https://w3c.github.io/performance-timeline/) Level 2 之后就不必如此了，因为它引入了 [`buffered`](https://w3c.github.io/performance-timeline/#dom-performanceobserverinit-%20%20buffered) 标记，可用于访问 `PerformanceObserver` 实例创建之前排队的性能条目。
{% endhint %}

### 利用主角元素追踪 FMP

目前尚无标准化的 FMP 定义，因此也没有性能条目类型。 部分原因在于很难以通用的方式确定“有效”对于所有页面意味着什么。

可以利用 mark 和 measure 标记，来追踪 FMP。[https://speedcurve.com/blog/user-timing-and-custom-metrics/](https://speedcurve.com/blog/user-timing-and-custom-metrics/)

```javascript
function fetchData() {
  performance.clearMarks("start update")
  performance.mark("start update")
  // Do an XHR or JSON request that calls updateData() with the new data.
}
function updateData(data) {
  // Update the DOM with the new data.
  performance.clearMarks("finish update")
  performance.mark("finish update")
  perforance.measure("update data", "start update", "finish update")
}
```

### 追踪 TTI

Chrome 提供的 [https://github.com/GoogleChromeLabs/tti-polyfill](https://github.com/GoogleChromeLabs/tti-polyfill) 是基于浏览器提供的 Long Task API 的。该 polyfill 公开 `getFirstConsistentlyInteractive()` 方法，后者返回使用 TTI 值进行解析的 promise。

```javascript
import ttiPolyfill from './path/to/tti-polyfill.js'
ttiPolyfill.getFirstConsistentlyInteractive().then((tti) => {
})
```

`getFirstConsistentlyInteractive()` 方法接受可选的 `startTime` 配置选项，让您可以指定下限值（您知道您的应用在此之前无法进行交互）。 默认情况下，该 polyfill 使用 DOMContentLoaded 作为开始时间，但通常情况下，使用主角元素呈现的时刻或您知道所有事件侦听器都已添加的时间点这类时间会更准确。

{% hint style="info" %}
与 FMP 相同，很难规范化适用于所有网页的 TTI 指标定义。我们在 polyfill 中实施的版本适用于大部分应用，但可能不适用于您的特定应用。 因此，您最好在使用前，先进行测试。如需有关 TTI 定义及实施细节的详情，请阅读 [TTI 指标定义文档](https://goo.gl/OSmrPk)。
{% endhint %}

### 追踪耗时较长的任务

要在 JavaScript 中检测耗时较长的任务，请创建新的 `PerformanceObserver`，并观察类型为 `longtask` 的条目。 耗时较长的任务条目的一个有点是包含[提供方属性](https://w3c.github.io/longtasks/#sec-TaskAttributionTiming)，有助于您更轻松地追查导致出现耗时较长任务的代码：

```javascript
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
  }
})
observer.observe({entryTypes: ['longtask']})
```

提供方属性会指出导致耗时较长任务的帧上下文，这有助于您确定问题是否由第三方 iframe 脚本所致。 未来版本的规范计划添加更多详细信息并公开脚本网址、行号和列号，这有助于确定速度缓慢问题是否由您自己的脚本所致。

### 追踪交互延迟

阻塞主线程的耗时较长任务可能会导致事件侦听器无法及时执行。若要在代码中检测输入延迟，您可将事件时间戳与当前时间作比较，如果两者相差超过 100 毫秒，您可以并应该进行报告。

```javascript
const subscribeBtn = document.querySelector('#subscribe')
subscribeBtn.addEventListener('click', (event) => {
  // Event listener logic goes here...
  const lag = performance.now() - event.timeStamp
  if (lag > 100) {
    // REPORT
  }
})
```

由于事件延迟通常是由耗时较长的任务所致，因此您可将事件延迟检测逻辑与耗时较长任务检测逻辑相结合：如果某个耗时较长的任务在 `event.timeStamp` 所示的时间阻塞主线程，您也可以报告该耗时较长任务的提供方值。 如此，您即可在负面性能体验与导致该体验的代码之间建立明确的联系。

虽然这种方法并不完美（不会处理之后在传播阶段耗时较长的事件侦听器，也不适用于不在主线程中运行的滚动或合成动画），但确实是良好的开端，让您能更好地了解运行时间较长的 JavaScript 代码对用户体验产生影响的频率。

### 追踪 FID

Chrome 提供的 [https://github.com/GoogleChromeLabs/first-input-delay](https://github.com/GoogleChromeLabs/first-input-delay)&#x20;

```javascript
// The perfMetrics object is created by the code that goes in <head>.
perfMetrics.onFirstInputDelay(function(delay, evt) {
})
```

### 加载放弃

可以直接通过 [Measurement Protocol](https://developers.google.com/analytics/devguides/collection/protocol/v1/) 记录。以下代码为 [`visibilitychange`](https://developer.mozilla.org/en-US/docs/Web/Events/visibilitychange) 事件添加侦听器（该事件在页面卸载或进入后台时触发），并在该事件触发时发送 `performance.now()` 值。

```javascript
window.__trackAbandons = () => {
  // Remove the listener so it only runs once.
  document.removeEventListener('visibilitychange', window.__trackAbandons)
  // Collect the data via the Measurement Protocol.
  Math.round(performance.now())
}
document.addEventListener('visibilitychange', window.__trackAbandons)
```

此外，您还应确保在页面可进行交互时移除此侦听器，否则您在报告 TTI 时还需报告放弃加载。

```javascript
document.removeEventListener('visibilitychange', window.__trackAbandons)
```

### 新的性能 API 提案

* [https://w3c.github.io/hr-time/](https://w3c.github.io/hr-time/)
* [https://w3c.github.io/performance-timeline/](https://w3c.github.io/performance-timeline/)
* [https://w3c.github.io/resource-timing/](https://w3c.github.io/resource-timing/) 收集文档依赖资源的得性能指标
* [https://w3c.github.io/navigation-timing/](https://w3c.github.io/navigation-timing/) 收集 HTML 文档的性能指标
* [https://w3c.github.io/user-timing/](https://w3c.github.io/user-timing/)
* [https://w3c.github.io/page-visibility/](https://w3c.github.io/page-visibility/)
* [https://w3c.github.io/requestidlecallback/](https://w3c.github.io/requestidlecallback/)
* [https://w3c.github.io/beacon/](https://w3c.github.io/beacon/)
* [https://w3c.github.io/resource-hints/](https://w3c.github.io/resource-hints/)
* [https://w3c.github.io/preload/](https://w3c.github.io/preload/)
* [https://w3c.github.io/server-timing/](https://w3c.github.io/server-timing/)
* [https://w3c.github.io/longtasks/](https://w3c.github.io/longtasks/)

## 收集方法

### 实际用户数据(RUM=Real User Measurements)

局限：

* performance API 聚焦在技术指标，但我们更关心用户实际体验
* 用户实际体验因素过于复杂，很难区分具体受哪一因素影响
* 噪声数据较多，需要统计技巧

### 实验环境数据(synthetic testing)

* 更可控的测试环境(网络、操作系统、浏览器版本、CPU 等等)，提供更精准的数据
* 与 RUM 相辅相成

![](<../../.gitbook/assets/image (7).png>)

## 参考文献

* &#x20;[https://phabricator.wikimedia.org/phame/live/7/post/117/performance\_testing\_in\_a\_controlled\_lab\_environment\_-\_the\_metrics/](https://phabricator.wikimedia.org/phame/live/7/post/117/performance\_testing\_in\_a\_controlled\_lab\_environment\_-\_the\_metrics/) 指标定义相关
* ​
