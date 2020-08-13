---
description: 本文将由浅入深剖析 CSS 层叠规则的整条脉络。
---

# CSS 层叠规则

## 缘起

![](../.gitbook/assets/lark20200809-144601.jpeg)

在开发 CSS 动画时，遇到了 Safari 和 Chrome 表现不一致的情况，Chrome 表现正常，Safari 却出现了「穿牌」的现象。明明写了 z-index，为什么还会出现这种情况呢？

于是，我决定系统地梳理一下 CSS 的层叠规则。

## 基本规则

![](../.gitbook/assets/image%20%2812%29.png)

这个图体现了 CSS 页面排版的基本准则：内容\(inline\)&gt;布局\(float\)&gt;装饰\(background, border...\)。

而当页面需要发生层叠的时候，遵循以下两条基本准则：

* 当层叠水平一致时，z-index 值大的会覆盖小的；
* 当层叠水平一致、层叠顺序相同时，DOM 中处在后面的会覆盖前面的；

## 深入 CSS 层叠

### 层叠上下文

你能说清楚为什么下图的元素会是这个层叠顺序么？

![](../.gitbook/assets/image%20%2813%29.png)

能够产生层叠上下文的场景：

* 文档的根元素
* `position` 值为 `absolute` 或 `relative`，且 `z-index` 值不为 `auto` 
* `position` 值为 `fixed` 或 `sticky` （sticky 仅在全部移动端浏览器和现代桌面浏览器产生）
* 元素为 flex 布局元素（父元素为 `flexbox`）且 `z-index` 值不为 `auto` 
* 元素为 grid 布局元素，且 `z-index` 值不为 `auto` 
* 元素的 `opacity` 值小于 `1` 
* 元素 `mix-blend-mode` 值不为 `normal` 
* 以下属性的值不为 `none` ：
  * `transform`
  * `filter`
  * `perspective` 
  * `clip-path` 
  * `mask` /`mask-image` /`mask-border` 
* 元素的 `isolation` 值为 `isolate` 
* 元素 `-webkit-overflow-scrolling` 值为 `touch` 
* 元素的 `will-change` 元素的值含有以上任意一条的属性，且值不为初始值
* 元素 `contain` 值为 `layout` 或`paint` 或者包含这两者的复合值（例如：`contain: strict` , `contain: content`）

一旦元素有了层叠上下文，其层叠顺序就会变高。在一个层叠上下文内，子元素的`z-index` 仅在该上下文内有效。

发现了一堆没见过的属性，继续挖吧。

#### perspective

观察者与 z 平面的距离。z&gt;0 的三维元素比正常大，而 z&lt;0 时则比正常小，大小程度由该属性的值决定。

#### mix-blend-mode

PS、SVG、Canvas 中都有类似的混合模式。CSS 的 mix-blend-mode 有 19 个值，分别对应不同的混色算法。`mix-blend-mode`默认情况下是会混合所有比起层叠顺序低的元素。如果我们希望值混合某一两个元素，而不是全部，该怎么办呢？

> 值得注意的是，`mix-blend-mode` 规范是由 CSSWG 和 SVGWG 下属的 Effects task force \(FX TF\) 公开小组制定的，可以同时在 CSS 和 SVG 中使用。

#### isolation

当 `isolation` 的值设为 `isolate` 时，可以隔离`mix-blend-mode` 元素的混合。其原理也是因为创建了一个新的层叠上下文，其他的事情什么也没有做！

## 层叠在 Safari 和 Chrome 的区别

![http://jsfiddle.net/eJsZx/4/](../.gitbook/assets/image%20%2816%29.png)

![http://jsfiddle.net/Lwfrp9q0/](../.gitbook/assets/image%20%2815%29.png)

![](../.gitbook/assets/image%20%2814%29.png)

![](../.gitbook/assets/image%20%2817%29.png)

### 考虑浏览器为何这么设计？

Safari 中当发生 3D 效果时，会打破原有层叠的顺序，而 Chrome 则不会。

## 参考文献

* [https://drafts.csswg.org/css2/](https://drafts.csswg.org/css2/)
* [https://drafts.csswg.org/css-transforms-2/](https://drafts.csswg.org/css-transforms-2/)
* [https://en.wikipedia.org/wiki/Blend\_modes](https://en.wikipedia.org/wiki/Blend_modes)
* [https://drafts.fxtf.org/compositing-1/\#mix-blend-mode](https://drafts.fxtf.org/compositing-1/#mix-blend-mode)

