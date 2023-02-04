---
description: >-
  原文链接：https://docs.google.com/document/d/1GGiI9-7KeY3TPqS3YT271upUVimo-XiL5mwWorDUD4c/preview#
---

# First CPU Idle & Time to Interactive(TTI)

## 拆分首次可交互指标

关于首次可交互指标应该如何被定义，有两种思想流派：

1.  首次可交互是网站首个最低程度可交互的时间点：足够多（但可能不是全部）在屏幕上展示

    的 UI 组件是可交互的，并且页面响应用户输入的平均时间是合理的，做不到每次都立即响应也是可接受的。
2. 首次可交互是网站首个完全加载且交互流畅的时间点 —— 不仅页面上的每个元素都是可交互的，而且页面严格地满足 RAIL 准则：页面在每 50 毫秒内就将控制返回给主线程，给浏览器足够的空间保证交互过程流畅。

用相同的定义去满足两个阵营是不可行的：倾向于后者的人认为前者的定义过于学术，没有优化的价值；倾向于前者的人认为第二个定义过于宽松没有优化的意义。

所以我们将首次可交互拆分成两个指标 —— 首次 CPU 空闲和首次可交互时间。

在我们深入探讨之前要说明的：

* **这些定义是模糊的**：我们对于首次可交互应该表现成的样子有一个模糊的直觉，但通常不可能精确定位一个页面在加载过程中“确实”首次可交互的时间戳，特别是对于首次的定义。
* **这些定义必然是启发式驱动的**：总会有阻挠我们启发的网站出现。我们理想的是一个这样的指标：
  * 它指向一个对于足够多网站样本的可交互性跃进的时间戳。
  * 让这个数字更小的与更好的用户体验正相关。

## **我们如何评估首次可交互的定义？**

有两个准则：

**⇒ 正确性**：我们可能不知道什么时候是真正首次可交互的，但是这个时间戳是_有道理的_么？

**⇒ 稳定性**：这个指标在相同网站稳定地返回相同的值，还是表现的多模态？

## 可交互时间 :: 正确性

[我们做了一个研究](https://docs.google.com/document/d/1pZsTKqcBUb1pc49J89QbZDisCmHLpMyUqElOwYqTpSI/edit?usp=sharing)，我们得到了三个定义。我们发现：

* 轨迹反向搜索和网络+主线程反向搜索表现是完全一样的。
* 二者都没有过早触发，并且都在一些测试中过晚触发了。相反的是，正向搜索定义在一些测试中触发得过早。
* 因为我们把指标拆分成两个了，那些“可能过晚”的情况变得非常可接受了。

**反向搜索网络+主线程静默的定义并不依赖我们停止记录轨迹的时机，绝对反向搜索则不是这样。**绝对反向搜索中，在时间 A 和时间 B 停止记录会出现两个不同的首次可交互时间的值。单纯基于网络的反向搜索，可能发现不了 A 或 B 中的一个或两个的首次可交互的时间，我们不会上报两个不同的非空首次可交互时间的值。

这是对于稳定性的重要属性，这就是为什么**反向搜索网络+主线程静默**是更推荐的定义。

![](https://docs.google.com/drawings/d/sBx\_jhCVHUGaUaOGgkTReXg/image?w=926\&h=518\&rev=500\&ac=1\&parent=1GGiI9-7KeY3TPqS3YT271upUVimo-XiL5mwWorDUD4c)

首次可交互间定义的基本概念是，我们查找在 5 秒时间内窗口 W 的网络几乎是静默的（给定时间内不超过 2 个网络请求）并且没有超过 50 毫秒的任务。然后我们找到这个窗口最后一个长时任务，并称这个任务结束的时间为可交互时间。

准确定义详见[附录 B](https://docs.google.com/document/d/1GGiI9-7KeY3TPqS3YT271upUVimo-XiL5mwWorDUD4c/preview#heading=h.wa7ef059vxyu)。

## 可交互时间 :: 稳定性

_基于 100 个流行站点的可变性研究，推荐的定义在 **82%** 的情况是稳定地。_

我们之前对于在线的网站做了一个[可变性研究](https://docs.google.com/document/d/1sHy6R58olikMTwk5hkJL4jd9S1jbksdMY5ve3Shdg-g/edit?usp=sharing)，发现 65% 的情况，指标有可接受的可变性。回顾这个评估过程：我们挑选了 100 个网站然后每个加载 25 次，把每次加载生成一张图，观察每张图然后做了一个主观判断，指标是否足够稳定（粗略来说，我们认为 25 次中如果不超过 3 次的数据是离谱的，就认为这个网站的指标是稳定的），然后数出稳定地网站的数量。

研究中大量的噪声数据是网络造成的 —— 我们看到很多页面在不同的加载中，加载了不同的内容（例如：不同的广告），所以期待首次可交互时间始终在同一时间触发是不公平的。

我们重新跑了一次可变性测试，这次了利用 WPR 记录来去除噪声数据。我们的可交互时间现在稳定在 **82%** 了。

这些研究的数据：

* [**All graphs**](http://deepanjan.me/tti-variability-v2/pessimistic/generated\_graphs/allgraphs.html)
* 全部判断的 [**Spreadsheet**](https://docs.google.com/a/chromium.org/spreadsheets/d/13z4ILDw\_BLU59yhUmYfi-es-pJIcZ1uKtD2Vy13wO1o/edit?usp=sharing#gid=1544954469)**.**

## 首次 CPU 空闲 :: 正确性

为了评估不同的首次 CPU 空闲定义，我们重用了之前决定可交互时间正确性的 25 个轨迹标注，然后识别时间轴中的一个区域，作为合理区域（看[**column I-J on this spreadsheet**](https://docs.google.com/spreadsheets/d/14xVEkk0yUV9kCaPERLzUpB057hjdV66KP24AyExayh0/edit?usp=sharing)）。

* 合理区域的末端总是由可交互时间决定 —— 首次 CPU 空闲时间比首次可交互时间晚触发是非常不合理的。
* 合理区域的开始是手动标注的，我们以一个非常慷慨的角度考虑可交互 —— 通常在时间 t，如果用户可以轻按/点击页面的大多数部分，页面有了反馈，甚至表现得不是完全加载之后的反馈的样子，我们也视为是合理的首次可交互的值 t。提一句，因为我们更包容这个时间，大多数传统的正向搜索可交互时间的值，曾经被我们贴上“过早”标签的值，现在在可接受范围了。

对于任何我们提出的首次 CPU 空闲的定义，我们希望：

1. 它产生的值在我们全部 25 个轨迹标注中都在合理区域。
2. 首次 CPU 空闲时间和可交互时间的差值是尽可能大的。\
   任何对于这个差值的测量都是有效的 —— 我们会使用 25 个网站的差值的和，后文称总差值。

现在介绍几个首次 CPU 空闲时间的候选定义，并通过我们的测试轨迹检验它们。

**备注：首次 CPU 空闲时间的下边界在 DOMContentLoadedEnd**&#x20;

DOMContentLoadedEnd 是所有 DOMContentLoaded 事件监听函数执行完成的时间点。在这个时间点之前在网页里面注册事件监听器是非常罕见的。一小部分网站在一些我们实验的首次 CPU 空闲的定义过早的触发了，因为那些定义只观察了长时任务和网络活动（而不是说有多少事件监听器被注册了），而有时在加载过程中的头 5 到 10 秒都没有长时任务，我们在 FMP 时触发首次 CPU 空闲时间，此时网站通常还没有准备好处理用户输入。我们发现如果使用 `max(DOMContentLoadedEnd, 首次 CPU 空闲)` 作为最终的首次 CPU 空闲时间值，这个值就回到了合理区域。等待 DOMContentLoadedEnd 再声称首次 CPU 空闲是明智的，所以后文介绍的全部定义的下边界都在 DOMContentLoadedEnd。

定义 1：正向搜索 5 秒没有长时任务

![](https://docs.google.com/drawings/d/sbT0FfF-HKzP4iOxM3K2q5g/image?w=926\&h=329\&rev=119\&ac=1\&parent=1GGiI9-7KeY3TPqS3YT271upUVimo-XiL5mwWorDUD4c)

定义 2：宽松的比例

![](https://docs.google.com/drawings/d/svaQ1D6\_JQtLI6huqXovTpQ/image?w=926\&h=379\&rev=346\&ac=1\&parent=1GGiI9-7KeY3TPqS3YT271upUVimo-XiL5mwWorDUD4c)

定义 3：宽恕独立的任务

![](https://docs.google.com/drawings/d/sEU5q1BBd4CP8H-z-QESA6w/image?w=926\&h=384\&rev=532\&ac=1\&parent=1GGiI9-7KeY3TPqS3YT271upUVimo-XiL5mwWorDUD4c)

定义 4：合并宽松比例和独立任务

![](https://docs.google.com/drawings/d/smbX1M0RFgLnaTbLCEB6ldQ/image?w=926\&h=452\&rev=35\&ac=1\&parent=1GGiI9-7KeY3TPqS3YT271upUVimo-XiL5mwWorDUD4c)

**定义 4 是我们目前最推荐的。**

\


****

\
\
