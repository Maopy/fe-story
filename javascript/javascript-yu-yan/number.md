# Number

Number Literal 定义：

```text
NumericLiteral ::
    DecimalLiteral
    BinaryIntegerLiteral
    OctalIntegerLiteral
    HexIntegerLiteral

DecimalLiteral ::
    DecimalIntegerLiteral "." [DecimalDigits] [ExponentPart]
    "." DecimalDigits [ExponentPart]
    DecimalIntegerLiteral [ExponentPart]

DecimalIntegerLiteral ::
    "0"
    NonZeroDigit [DecimalDigits]

DecimalDigits ::
    DecimalDigit
    DecimalDigits DecimalDigit

DecimalDigit :: one of
    "0" "1" "2" "3" "4" "5" "6" "7" "8" "9"

NonZeroDigit :: one of
    "1" "2" "3" "4" "5" "6" "7" "8" "9"

ExponentPart ::
    ExponentIndicator SignedInteger

ExponentIndicator :: one of
    "e" "E"

SignedInteger ::
    DecimalDigits
    "+" DecimalDigits
    "-" DecimalDigits

BinaryIntegerLiteral ::
    "0b" BinaryDigits
    "0B" BinaryDigits

BinaryDigits ::
    BinaryDigit
    BinaryDigits BinaryDigit

BinaryDigit :: one of
    "0" "1"

OctalIntegerLiteral ::
    "0o" OctalDigits
    "0O" OctalDigits

OctalDigits ::
    OctalDigit
    OctalDigits OctalDigit

OctalDigit :: one of
    "0" "1" "2" "3" "4" "5" "6" "7"

HexIntegerLiteral ::
    "0x" HexDigits
    "0X" HexDigits

HexDigits ::
    HexDigit
    HexDigits HexDigit

HexDigit :: one of
    "0" "1" "2" "3" "4" "5" "6" "7" "8" "9"
    "a" "b" "c" "d" "e" "f" "A" "B" "C" "D" "E" "F"
```

正则校验：

* 二进制数：`^(0b|0B)(0|1)+$` 
* 八进制数：`^(0o|0O)[0-7]+$` 
* 十六进制数：`^(0x|0X)[0-9a-fA-F]+$` 
* 十进制数：
  * &lt;十进制整数&gt; "." \[十进制数字\] \[指数部分\]：`^(0|([1-9]\d*)).(\d*)?((e|E){1}(\+|\-)?\d+)?$` 
  * "." &lt;十进制整数&gt; \[指数部分\]：`^.\d+((e|E){1}(\+|\-)?\d+)?$` 
  * &lt;十进制整数&gt; \[指数部分\]：`^(0|([1-9]\d*))((e|E){1}(\+|\-)?\d+)?$` 

