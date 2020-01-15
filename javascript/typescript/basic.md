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
}
```



