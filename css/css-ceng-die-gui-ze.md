---
description: CSS 层叠规则主要由层叠上下文决定，本文将由浅入深剖析 CSS 层叠规则的整条脉络。
---

# CSS 层叠规则

{% hint style="info" %}
由于中文都翻译作「层叠」，概念容易跟 CSS 层叠样式表产生混淆，还原成英文：层叠上下文\(stacking context\)与层叠样式表\(cascading style sheets\)就能明显感受出这两者的区别。
{% endhint %}

## 回顾基础

古老的 CSS 2 时代，层叠规则比较简单：

![](../.gitbook/assets/image%20%2812%29.png)

这个基础的层叠规则体现了 CSS 2 时代对于页面排版的基本要求：内容&gt;布局&gt;装饰。

而当页面需要发生层叠的时候，遵循以下两条准则：

* 当层叠水平一致时，z-index 值大的会覆盖小的；
* 当层叠水平一致、层叠顺序相同时，DOM 中处在后面的会覆盖前面的；



## 当代 CSS 层叠规则



## CSS Next 新属性：isolate, blend-mode, 视角



## 旋转在不同浏览器的区别



## 自己实现一个

考虑浏览器为何这么设计？



