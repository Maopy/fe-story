# 附录A 动态作用域

动态作用域是 JavaScript 另一个重要机制 this 的表亲。

动态作用域似乎暗示有很好的理由让作用域作为一个在运行时就被动态确定的形式，而不是在写代码时进行静态确定的形式。动态作用域并不关心函数和作用域是如何声明以及在何处声明的，只关心它们从何处调用。换句话说，作用域链是基于调用栈的，而不是代码中的作用域嵌套。

事实上 JavaScript 并不具有动态作用域。它只有词法作用域，简单明了。但是 this 机制某种程度上很像动态作用域。

