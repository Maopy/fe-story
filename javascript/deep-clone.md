# deep clone

## 业界实现

#### zepto 实现：

```javascript
function extend(target, source, deep) {
  for (key in source)
    if (deep && (isPlainObject(source[key]) || isArray(source[key]))) {
      if (isPlainObject(source[key]) && !isPlainObject(target[key]))
        target[key] = {}
      if (isArray(source[key]) && !isArray(target[key]))
        target[key] = []
      extend(target[key], source[key], deep)
    }
    else if (source[key] !== undefined) target[key] = source[key]
}

// Copy all but undefined properties from one or more
// objects to the `target` object.
$.extend = function(target){
  var deep, args = slice.call(arguments, 1)
  if (typeof target == 'boolean') {
    deep = target
    target = args.shift()
  }
  args.forEach(function(arg){ extend(target, arg, deep) })
  return target
}
```



