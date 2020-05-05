# Object API/Grammer

* `{}` `.` `[]` `Object.defineProperty` 
* `Object.create` / `Object.setPrototypeOf` / `Object.getPrototypeOf` 
* `new` / `class` / `extends` 
* `new` /`function` / `prototype` 

## 函数对象

ECMAScript 函数对象内部插槽

| 内部插槽 | 类型 | 描述 |
| :--- | :--- | :--- |
| \[\[Environment\]\] | Lexical Environment |  |
| \[\[FormalParameters\]\] | Parse Node |  |
| \[\[FunctionKind\]\] | String |  |
| \[\[ECMAScriptCode\]\] | Parse Node |  |
| \[\[ConstructorKind\]\] | String |  |
| \[\[Realm\]\] | Realm Record |  |
| \[\[ScriptOrModule\]\] | Script Record 或 Module Record |  |
| \[\[ThisMode\]\] | \(lexical, strict, global\) |  |
| \[\[Strict\]\] | Boolean |  |
| \[\[HomeObject\]\] | Object |  |
| \[\[SourceText\]\] | String |  |

所有 ECMAScript 函数对象都有 `[[call]]` 内部方法，也都有 `[[Construct]]` 内部方法可以作为构造器使用。

## 内置舶来对象方法和插槽

### 被绑定函数的舶来对象

| 内部插槽 | 类型 | 描述 |
| :--- | :--- | :--- |
| \[\[BoundTargetFunction\]\] | Callable Object |  |
| \[\[BoundThis\]\] | Any |  |
| \[\[BoundArguments\]\] | List of Any |  |

* Array
* String
* Arguments
* Integer-Indexed
* Module Namespace
* Immutable Prototype
* Proxy Object





