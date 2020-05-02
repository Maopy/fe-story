# JavaScript 词法

### Unicode

{% embed url="https://home.unicode.org/" caption="Unicode 官方网站" %}

{% embed url="https://www.fileformat.info/info/unicode/" %}

需要关注 Blocks 和 Categories；

| 码点起值 | 码点终值 | Byte1 | Byte2 | Byte3 | Byte4 | Byte5 | Byte6 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| U+0000 | U+007F | 0xxxxxxx |  |  |  |  |  |
| U+0080 | U+07FF | 110xxxxx | 10xxxxxx |  |  |  |  |
| U+0800 | U+FFFF | 1110xxxx | 10xxxxxx | 10xxxxxx |  |  |  |
| U+1 0000 | U+1 FFFFF | 11110xxx | 10xxxxxx | 10xxxxxx | 10xxxxxx |  |  |
| U+20 0000 | U+3FF FFFF | 111110xx | 10xxxxxx | 10xxxxxx | 10xxxxxx | 10xxxxxx |  |
| U+400 0000 | U+7FFF FFFF | 1111110x | 10xxxxxx | 10xxxxxx | 10xxxxxx | 10xxxxxx | 10xxxxxx |

跟据上表，解读 UTF-8 编码非常简单。如果一个字节的第一位是`0`，则这个字节单独就是一个字符；如果第一位是`1`，则连续有多少个`1`，就表示当前字符占用多少个字节。

Unicode 6.1 定义范围是：0 ~ 10 FFFF；大于这个范围的属于 `UCS-4` 编码，2003 年 11 月 `UTF-8` 被 `RFC 3629` 重新规范，只能使用原来 Unicode 定义的区域， U+0000 到 U+10FFFF。

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



