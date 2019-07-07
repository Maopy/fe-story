# Performance

## 首屏场景

### 首屏过程

![](../.gitbook/assets/image%20%282%29.png)

浏览器执行的阶段：

重定向 -&gt; 拉取缓存 -&gt; DNS 查询 -&gt; 建立 TCP 连接 -&gt; 发起请求 -&gt; 接收响应 -&gt; 处理 HTML 元素 -&gt; 元素加载完成

#### 性能指标

基于用户感受的分类：

是否正在打开页面？（跳转是否在进行、服务是否有了响应等）

* 首次绘制\(FP=First Paint\)
* **首次有内容绘制\(FCP=First Contentful Paint\)** 定义：以下任一个首次加载的时间戳
  * 文字
  * SVG
  * 图片
  * 背景图
  * canvas

加载的内容是有用的么？（是否渲染了关键内容）

* **首次有意义绘制\(FMP=First Meaningful Paint\)**  
  定义：主要内容绘制完成

  * 头部及其文字
  * 搜索引擎关键词
  * 电商网站的图片

  不作数的：

  * loading 态
  * FOUC\(A flash of unstyled content\)
  * 只渲染了导航或头部

是可用的么？（是否能够操作页面了）

* 视觉准备\(Visual Ready\)
* **首次可交互时间\(TTI=Time To Interactive\)** 定义：
  * FMP 已完成
  * 85%的视觉完备
  * DOMContentLoaded 被触发

是否足够轻量？



![](../.gitbook/assets/image%20%284%29.png)

### 优化分析

#### 拉取缓存

浏览器缓存策略：

![](../.gitbook/assets/image%20%283%29.png)



一些指标

![](../.gitbook/assets/image.png)



改进策略

![](../.gitbook/assets/image%20%281%29.png)



