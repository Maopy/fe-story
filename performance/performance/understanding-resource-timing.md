# Understanding Resource Timing

* Redirect
  * 立刻开始 `startTime` 
  * 如果开始重定向，`redirectStart` 同时开始
  * 重定向结束时，`redirectEnd` 被触发
* App Cache
  * 如果应用命中了请求的缓存，`fetchStart` 被触发
* DNS
  * `domainLookupStart` 在开始 DNS 请求时触发
  * `domainLookupEnd` 在结束 DNS 请求时触发
* TCP
  * `connectStart` 在开始连接到服务器时被触发
  * 如果使用了 TLS ，`secureConnectionStart` 会在握手开始时被触发
  * `connectEnd` 在连接服务器完成时被触发
* Request
  * `requestStart` 在请求服务器资源时触发
* Response
  * `responseStart` 在服务器刚开始返回时触发
  * `responseEnd` 在请求结束并且数据完全返回时触发

