# Understanding Navigation Timings

* Navigation\
  `navigationStart` 这个属性用来标记上一个文档/页面被终止。如果没有上一个文档，这个值会等于 `performancetiming.fetchStart` ，这个值用来表示浏览器准备好发起一个请求文档的 HTTP 请求了。
* Unload Events\
  卸载事件在用户离开页面时抛出。利用 `unloadEventStart` 和 `unloadEventEnd` 事件追踪。
* Redirection\
  `redirectStart` 开始重定向，`redirectEnd` 重定向结束
* App Cache
  * 如果应用命中了请求的缓存，`fetchStart` 被触发
* DNS
  * `domainLookupStart` 在开始 DNS 请求时触发
  * `domainLookupEnd` 在结束 DNS 请求时触发
  * 如果发起请求了，`fetchStart` 被触发
* TCP
  * `connectStart` 在开始连接到服务器时被触发
  * 如果使用了 TLS ，`secureConnectionStart` 会在握手开始时被触发
  * `connectEnd` 在连接服务器完成时被触发
* Request
  * `requestStart` 在请求服务器资源时触发
* Response
  * `responseStart` 在服务器刚开始返回时触发
  * `responseEnd` 在请求结束并且数据完全返回时触发
* DOM 事件
  * `domLoading` 在开始解析 HTML 时触发
  * `domInteractive` 标记浏览器已经完整的解析完 HTML 并且构建好 DOM 树
  * 当 CSSOM 构建完并且没有阻塞或等待 JS 执行的样式表剩余，浏览器就开始创建渲染树。`domContentLoadedEventStart` 和`domContentLoadedEventEnd` 允许我们追踪这个过程执行了多久。
  * 当全部的过程都完成了并且所有页面资源都加载完了（浏览器的加载旋转停下了），被标记为`domComplete`&#x20;
* Load 事件\
  浏览器最后一步会触发 onload 事件，这会触发监听这个事件的函数或逻辑。这个时间开始和结束的标记为 `loadEventStart` 和 `loadEventEnd` 。
