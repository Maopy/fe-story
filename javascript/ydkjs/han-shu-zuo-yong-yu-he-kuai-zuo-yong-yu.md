# 3. 函数作用域和块作用域

## 3.1 函数中的作用域

函数作用域的含义是指，属于这个函数的全部变量都可以在整个函数的范围内使用及复用\(事实上在嵌套的作用域中也可以使用\)。

## 3.2 隐藏内部实现

最小特权原则，也叫最小授权或最小暴露原则。这个原则是指在软件设计中，应该最小限度地暴露必要内容，而将其他内容都“隐藏”起来，比如某个模块或对象的 API 设计。

### 规避冲突

“隐藏”作用域中的变量和函数所带来的另一个好处，是可以避免同名标识符之间的冲突。

#### 1. 全局命名空间

当程序中加载了多个第三方库时，通常会在全局作用域中声明一个名字足够独特的变量，通常是一个对象。这个对象 被用作库的命名空间，所有需要暴露给外界的功能都会成为这个对象\(命名空间\)的属 性，而不是将自己的标识符暴漏在顶级的词法作用域中。

#### 2. 模块管理

无需将标识符加入到全局作用域中，而是通过依赖管理器的机制将库的标识符显式地导入到另外一个特定的作用域中。

## 3.3 函数作用域

在任意代码片段外部添加包装函数，可以将内部的变量和函数定义“隐藏”起来，但是它并不理想，因为会导致一些额外的问题。首先，必须声明一个具名函数 foo\(\)，意味着 foo 这个名称本身“污染”了所在作用域。其次，必须显式地通过函数名\(foo\(\)\)调用这个函数才能运行其中的代码。

解决方案：包装函数。包装函数的声明以 \(function... 而不仅是以 function... 开始。函数会被当作函数表达式而不是一 个标准的函数声明来处理。

函数声明和函数表达式之间最重要的区别是它们的名称标识符将会绑定在何处。

* 函数声明中 foo 被绑定在所在作用域中，可以直接通过 foo\(\) 来调用它。
* 函数表达式中 foo 被绑定在函数表达式自身的函数中而不是所在作用域中。

### 3.3.1 匿名和具名

匿名函数表达式书写起来简单快捷，但是它也有几个缺点需要考虑。

1. 匿名函数在栈追踪中不会显示出有意义的函数名，使得调试很困难。
2. 当函数需要引用自身时只能使用已经过期的arguments.callee引用，比如在递归中。
3. 匿名函数省略了对于代码可读性/可理解性很重要的函数名。

解决方案：**行内函数表达式**非常强大且有用——匿名和具名之间的区别并不会对这点有任何影响。始终给函数表达式命名是一个最佳实践。

### 3.3.2 立即执行函数表达式

IIFE，代表立即执行函数表达式 \(Immediately Invoked Function Expression\);

IIFE 还有一种变化的用途是倒置代码的运行顺序，将需要运行的函数放在第二位，在 IIFE 执行之后当作参数传递进去。这种模式在 UMD\(Universal Module Definition\)项目中被广 泛使用。

## 3.4 块作用域

变量的声明应该距离使用的地方越近越好，并最大限度地本地化。

块作用域是一个用来对之前的最小授权原则进行扩展的工具，将代码从在函数中隐藏信息扩展为在块中隐藏信息。但可惜，表面上看 JavaScript 并没有块作用域的相关功能。除非你更加深入地研究。

有些人认为块作用域不应该完全作为函数作用域的替代方案。两种功能应该同时存在，开发者可以并且也应该根据需要选择使用何种作用域，创造可读、可维护的优良代码。

### 3.4.1 with

用 with 从对象中创建出的作用域仅在 with 声明中而非外 部作用域中有效。

### 3.4.2 try/catch

非常少有人会注意到 JavaScript 的 ES3 规范中规定 try/catch 的 catch 分句会创建一个块作用域，其中声明的变量仅在 catch 内部有效。

### 3.4.3 let

ES6 引入了新的 let 关键字，可以将变量绑定到所在的任意作用域中\(通常是 { .. } 内部\)。换句话说，let 为其声明的变量隐式地了所在的块作用域。

注意：用 let 将变量附加在一个已经存在的块作用域上的行为是隐式的。在开发和修改代码的过程中，如果没有密切关注哪些块作用域中有绑定的变量，并且习惯性地移动这些块或者将其包含在其他的块中，就会导致代码变得混乱。

为块作用域**显式地**创建块可以部分解决这个问题，使变量的附属关系变得更加清晰。

#### 1. 垃圾收集

另一个块作用域非常有用的原因和闭包及回收内存垃圾的回收机制相关。

#### 2. let 循环

for 循环头部的 let 不仅将 i 绑定到了 for 循环的块中，事实上它将其重新绑定到了循环的每一个迭代中，确保使用上一个循环迭代结束时的值重新进行赋值。

### 3.4.4 const

ES6 引入了 const，同样可以用来创建块作用域变量，但其值是固定的 \(常量\)。


