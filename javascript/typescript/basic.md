# Basic

## 强类型与弱类型

在强类型语言中，当一个对象从调用函数传递到被调用函数时，其类型必须与被调用函数中声明的类型兼容。

强类型语言，不允许改变变量的数据类型，除非进行强制类型转换。

弱类型语言，变量可以被赋予不同的数据类型。

## 静态类型与动态类型

静态类型语言：在编译阶段确定所有变量的类型

动态类型语言：在执行阶段确定所有变量的类型

### JavaScript 与 C++ 内存对比

JavaScript：

* 在程序运行时，动态计算属性偏移量
* 需要额外的空间存储属性名
* 所有对象的偏移量信息各存一份

C++：

* 编译阶段确定属性偏移量
* 用偏移量访问代替属性名访问
* 偏移量信息共享

## 数据类型

* Boolean
* Number
* String
* Array
* Function
* Object
* Symbol
* undefined
* null
* **void**
* **any**
* **never**
* **元组**
* **枚举**
* **高级类型**

### 类型注解

```text
(变量/函数) : type
```

```typescript
let arr1 : number[] = [1, 2, 3]
let arr2 : Array<number> = [1, 2, 3]
let arr3 : Array<number | string> = [1, 2, '3']

let tuple : [number, string] = [0, '1']

let compute : (x : number, y : number) => number
compute = (a, b) => a + b

let obj : { x : number, y : number } = { x: 1, y: 2 }
obj.x = 3
```

### 枚举类型

```typescript
enum Role {
  Reporter = 1
}
```

## 接口

### 对象类型接口

```typescript
interface List {
  readonly id : number // 只读属性
  name : string
  age ?: number // 可选属性
  [x : string] : any // 字符串索引签名，用于支持多余属性
}
interface Result {
  data: List[]
}
function render (result : Result) {
  result.data.forEach((value) => {
    console.log(value.id, value.name)
  })
}
let result = {
  data: [
    {id: 1, name: 'a', sex: 'male'},
    {id: 2, name: 'b'}
  ]
}
render(result)
render({
  data: [
    {id: 1, name: 'a', sex: 'male'},
    {id: 2, name: 'b'}
  ]
} as Result) // 类型断言，用于绕过类型检查
```

### 函数接口类型

```typescript
let add1 : (x : number, y : number) => number

interface add2 {
  (x : number, y : number) : number
}

type add3 = (x : number, y : number) => number

let add : add1 = (a, b) => a + b

interface Lib {
  () : void
  version : string
  doSomething() : void
}

function getLib () {
  let lib : Lib = (() => {}) as Lib
  lib.version = '1.0'
  lib.doSomething = () => {}
  return lib
}
let lib1 = getLib()
lib1()
lib1.doSomething()
```

### 函数重载

```typescript
function add8(...rest : number[]) : number
function add8(...rest : string[]) : string
function add8(...rest : any[]) : any {
  let first = rest[0]
  if (typeof first === 'string') {
    return rest.join('')
  }
  if (typeof first === 'number') {
    return rest.reduce((pre, cur) => pre + cur)
  }
}
console.log(add8(1, 2, 3))
console.log(add8('a', 'b', 'c'))
```

## 类

```typescript
class Husky extends Dog {
  constructor (name : string, public color : string) {
    super(name)
    this.color = color
  }
  // color : string 构造函数参数中定义，节省代码
}
```

### 类实现接口

```typescript
interface Human {
  name : string
  eat() : void
}

class Asian implements Human {
  constructor (name : string) {
    this.name = name
  }
  name : string
  eat () {}
  sleep () {}
}

interface Man extends Human {
  run () : void
}

interface Child {
  cry () : void
}

// 接口继承接口
interface Boy extends Man, Child {}

// 需要实现全部继承的属性
let boy : Boy = {
  name: '',
  run () {},
  eat () {},
  cry () {}
}

class Auto {
  state = 1
  // private state2 = 0
}

interface AutoInterface extends Auto {
}
class C implements AutoInterface {
  state = 1
}
class Bus extends Auto implements AutoInterface {}
```

## 泛型

泛型：不预先确定的数据类型，具体的类型在使用的时候才能确定。

```typescript
function log<T> (value : T) : T {
  console.log(value)
  return value
}

log<string>[](['a', 'b'])
log(['a', 'b']) // 利用 TS 的类型推断

type Log = <T>(value : T) => T
let myLog : Log = log

interface Log {
  <T>(value : T) : T
}
interface Log<T = string> { // 约束所有接口变量，可以定义默认类型
  (value : T) : T
}
let myLog : Log<number> = log
myLog(1)
```

### 泛型类与泛型约束

```typescript
class Log<T> {
  run (value : T) {
    console.log(value)
    return value
  }
}
let log1 = new Log<number>()
log1.run(1)
let log2 = new Log()
log2.run({ a: 1 }) // 不指定类型时，可以传任意类型

interface Length {
  length : number
}
// 泛型约束
function log<T extends Length>(value : T) : T {
  console.log(value, value.length)
  return value
}
log([1])
log('123')
log({ length: 1 })
```

1. 函数和类可以轻松地支持多种类型，增强程序的扩展性；
2. 不必写多条函数重载，冗长的联合类型声明，增强代码可读性；
3. 灵活控制类型之间的约束；



