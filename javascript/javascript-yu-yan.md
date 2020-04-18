# JavaScript 语言

### Unicode

{% embed url="https://home.unicode.org/" caption="Unicode 官方网站" %}

{% embed url="https://www.fileformat.info/info/unicode/" %}

需要关注 Blocks 和 Categories；

### 词法分析 Lexical Grammer

```text
InputElement
    WhiteSpace
    LineTerminator
    Comment
    Token
        Punctuator
        IdentifierName
            Keywords
            Identifier
            Futrue reserved Keywords: enum
        Literal
```

```text
WhiteSpace ::
    <TAB>
    <VT>
    <FF>
    <SP>
    <NBSP>
    <ZWNBSP>
    <USP>
```

零宽空格 不在基础字符



