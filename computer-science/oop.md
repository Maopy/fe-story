---
description: 本文是一篇用追溯法探寻面向对象概念的文章
---

# OOP

使用了“对象(objects)”和“面向(oriented)”术语的现代意义的面向对象编程语言，最早出现在 1950 年代末和 1960 年代初的 MIT。早在 1960年，在 MIT 计算中心人工智能组，“对象(objects)”就用来指代带有属性(properties)或特征(attributes)的特定的项\[LISP 语言的原子(atoms)]。Alan Kay 后来也证实了，他在 1966 年创立面向对象编程时，思想上受到了 LISP 语言实现细节很深影响。

![LISP I Programmers Manual (1960)](<../.gitbook/assets/image (15).png>)

![LISP 1.5 Programmer's Manual (1962)](<../.gitbook/assets/image (7).png>)

另一个早期 MIT 的案例是在 1960-61 年，由 Ivan Sutherland 创造的 [Sketchpad](https://en.wikipedia.org/wiki/Sketchpad)。他在 1963 年发表相关论文的技术报告词汇表中，定义了“对象(object)”和“实例(instance)”(相当于类的概念，当时被称为 "master" 或 "definition" )。Sketchpad 提出的 "master" 与 JavaScript 的原型继承有很多共同之处。相同的还有 MIT 的 ALGOL 语言，在数据结构(这种方言里称为 "plexes" )和处理过程\[是后来的 "消息(messages)",  "方法(methods)",  "成员函数(member functions)"这些术语的前身]间建立了一个连接。

1960 年代，Simula 语言实践了面向对象编程，并提出了当下我们提到面向对象编程的几个重要概念，如：类(class)、对象(object)、继承(inheritance)和动态绑定。在Simula语言中：当一个程序调用执行完成之后，程序会自动离开运行栈的活动记录，并向它返回一个指针。程序的这种更改形式在Simula中就叫做类，而栈中的活动记录就被叫做对象。Simula对象由垃圾处理器来进行销毁，当创建它的程序没有一个在使用它时才被销毁。这一思想如今在JavaScript中有所体现。

1970 年代，Alan Kay 和 Xerox PARC 的其他人开发了 [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk) 的第一个版本。Smalltalk 最初被设计成动态类型的解释型的语言，而不是编译型的语言。Smalltalk 中的所有东西都是一个对象，包括类、整数和块（闭包）。最初的 Smalltalk 72 并没有子类的特性。Dan Ingalls 在 Smalltalk 76 中才引入。虽然，Smalltalk 支持类并最终实现了子类，但 Smalltalk 并不是关于类和子类的东西。它是一个受 Lisp 和 Simula 启发的函数式语言。**Alan Kay 认为，业界对子类化的关注会混淆面向对象编程真正的优势。**

在 2003 邮件交流中，Alan Kay 澄清了他当时称 Smalltalk 是“面向对象的”的时候的想法：

> OOP 对我而言意味着仅有消息传递、本地保留和保护以及隐藏状态过程，还有所有事物的极致晚绑定
>
> \~ Alan Kay

JavaScript 是 Smalltalk 对全世界在 OOP 中误解的报复。Smalltalk 和 JavaScript 都支持：

* 对象
* 优等的函数及闭包
* 动态类型
* 晚绑定 (函数/方法在运行时可变)
* 无需类继承的OOP

按照 Alan Kay 的说法，OOP 的基本要素是：

* 消息传递
* 封装
* 动态绑定(程序可以在运行时进化或适应的能力)

哪些不是必须的?

* 类
* 类继承
* 对象/函数/数据的特殊对待
* `new` 关键字
* 多态
* 静态类型
* 将类 (class) 识别为“类型(type)”



