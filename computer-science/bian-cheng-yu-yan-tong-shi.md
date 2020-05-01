# 编程语言通识

## 语言按语法分类

* 非形式语言
  * 中文、英文
* 形式语言（乔姆斯基谱系）
  * 0型 无限制文法或短语结构文法
  * 1型 上下文相关文法
    * 如：`a.this` 和 `this.a` 中 `this` 就是上下文相关；
  * 2型 上下文无关文法
  * 3型 正则文法

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x6587;&#x6CD5;</th>
      <th style="text-align:left">&#x8BED;&#x8A00;</th>
      <th style="text-align:left">&#x81EA;&#x52A8;&#x673A;</th>
      <th style="text-align:left">&#x4EA7;&#x751F;&#x5F0F;&#x89C4;&#x5219;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">0-&#x578B;</td>
      <td style="text-align:left">&#x9012;&#x5F52;&#x53EF;&#x679A;&#x4E3E;&#x8BED;&#x8A00;</td>
      <td style="text-align:left">&#x56FE;&#x7075;&#x673A;</td>
      <td style="text-align:left">&#x3B1; -&gt; &#x3B2;&#xFF08;&#x65E0;&#x9650;&#x5236;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">1-&#x578B;</td>
      <td style="text-align:left">&#x4E0A;&#x4E0B;&#x6587;&#x76F8;&#x5173;&#x8BED;&#x8A00;</td>
      <td style="text-align:left">&#x7EBF;&#x6027;&#x6709;&#x754C;&#x975E;&#x786E;&#x5B9A;&#x56FE;&#x7075;&#x673A;</td>
      <td
      style="text-align:left">&#x3B1;A&#x3B2; -&gt; &#x3B1;&#x3B3;&#x3B2;</td>
    </tr>
    <tr>
      <td style="text-align:left">2-&#x578B;</td>
      <td style="text-align:left">&#x4E0A;&#x4E0B;&#x6587;&#x65E0;&#x5173;&#x8BED;&#x8A00;</td>
      <td style="text-align:left">&#x975E;&#x786E;&#x5B9A;&#x4E0B;&#x63A8;&#x81EA;&#x52A8;&#x673A;</td>
      <td
      style="text-align:left">A -&gt; &#x3B3;</td>
    </tr>
    <tr>
      <td style="text-align:left">3-&#x578B;</td>
      <td style="text-align:left">&#x6B63;&#x5219;&#x8BED;&#x8A00;</td>
      <td style="text-align:left">&#x6709;&#x9650;&#x72B6;&#x6001;&#x81EA;&#x52A8;&#x673A;</td>
      <td style="text-align:left">
        <p>A -&gt; aB</p>
        <p>A -&gt; a</p>
      </td>
    </tr>
  </tbody>
</table>## 产生式\(BNF\)

BNF 可以表示 2、3型语法。

```text
# 例子："a(bb)*c"
<v0> ::= a <w>
<w> ::= bb <w> | c
```

递归出现一次，并在最右，称为**正规**的语法，正则文法。  
3型语法中的递归产生式都是正规的。

```text
# 例子：十进制数
<十进制数> ::= <无符号整数> | <小数> | <无符号整数> <小数>
<无符号整数> ::= <数字> | <数字> <无符号整数>
<小数> ::= . <无符号整数>
<数字> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

```text
# 可选项：用 "[]" 表示
<if 语句> ::= if <逻辑表达式> then <语句> [ else <语句> ] end if;

# 重复项：用 "{}" 表示重复 0 次或者多次
<标识符> ::= <字母> { <字母> | <数字> }
```

用 BNF 定义 BNF：

```text
<BNF> ::= <规则> | <规则> <BNF>
<规则> ::= <非终结符> "::=" <表达式>
<非终结符> ::= "<" <单词> ">"
<单词> ::= <字母> | <字母> <单词>
<表达式> ::= <项> | <项> "|" <表达式>
<项> ::= <因子> | <因子> <项>
<因子> ::= <单词> | <非终结符>
```

定义四则运算：

```text
<digit> ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
<decimal> ::= "0" | (("1" | "2" | ... | "9") <digit>*)

S -> AddExp
AddExp -> AddExp opt1 MulExp | MulExp
opt1 -> + | -
MulExp -> MulExp opt2 AtomicExp | AtomicExp
opt2 -> * | /
AtomicExp -> (AddExp) | alphabet

<expr> ::= <term> <expr_tail>
<expr_tail> ::= + <term> <expr_tail> |
  - <term> <expr_tail> |
  <empty>
<term> ::= <factor> <term_tail>
<term_tail> ::= * <factor> <term_tail> |
  / <factor> <term_tail> |
  <empty>
<factor> ::= ( <expr> ) | Num
```



