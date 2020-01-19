# Types

## 类型检查机制

TypeScript 编译器在做类型检查时，所秉承的一些原则，以及表现出的一些行为。

作用：辅助开发，提高开发效率。

### 类型推断

不需要指定变量的类型（函数的返回值类型），TypeScript 可以根据某些规则自动地为其推断出一个类型。

```typescript
let a = 1 // a 推断为 number
let b = [1] // b 推断为 number[]
let c = (x = 1) => x + 1 // x 推断为 number，c 推断为 number
let d = [1, null] // d 推断为 (number | null)[]

window.onKeydown = (event) => {
  // event 推断为 KeyboardEvent
}

interface Foo {
  bar : number
}
let foo = {} as Foo
foo.bar = 1
```

### 类型兼容性

当一个类型 Y 可以被赋值给另一个类型 X 时，我们就可以说类型 X 兼容类型 Y

X 兼容 Y : X（目标类型）= Y（源类型）

```typescript
interface X {
  a : any
  b : any
}
interface Y {
  a : any
  b : any
  c : any
}
let x : X = { a: 1, b: 2 }
let y : Y = { a: 1, b: 2, c: 3 }
x = y
// y = x // x 不能赋值给 y

type Handler = (a : number, b : number) => void
function hof (handler : Handler) {
  return handler
}

// 1.参数个数
let handler1 = (a : number) => {}
hof(handler1)
let handler2 = (a : number, b : number, c : number) => {}
// hof(handler2) // 不能赋值

// 可选参数和剩余参数
let a = (p1 : number, p2 : number) => {}
let b = (p1 ?: number, p2 ?: number) => {}
let c = (...args : number[]) => {}
a = b
a = c
b = c
b = a // 关闭 strictFunctionTypes 可实现兼容
c = a
b = b

// 2.参数类型
let handler3 = (a : string) => {}
// hof(hadnler3) // 不兼容

interface Point3D {
  x : number
  y : number
  z : number
}
interface Point2D {
  x : number
  y : number
}
let p3d = (point : Point3D) => {}
let p2d = (point : Point2D) => {}
p3d = p2d
// p2d = p3d // 不兼容，关闭 strictFunctionTypes 可实现兼容

// 3.返回值类型
let f = () => ({ name: 'Alice' })
let g = () => ({ name: 'Alice', location: 'Beijing' })
f = g
// g = f // 不兼容

function overload(a : number, b : number) : number
function overload(a : string, b : string) : string
// function overload(a : any, b : any, c : any) : any {} // 不兼容，参数个数多于目标函数

```





