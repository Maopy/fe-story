# String

Syntax

```text
StringLiteral ::
    """ [DoubleStringCharacters] """
    "'" [SingleStringCharacters] "'"

DoubleStringCharacters ::
    DoubleStringCharacter [DoubleStringCharacters]

SingleStringCharacters ::
    SingleStringCharacter [SingleStringCharacters]

DoubleStringCharacter ::
    SourceCharacter but not one of """ or "\" or LineTerminator
    "<LS>"
    "<PS>"
    "\" EscapeSequence
    LineContinuation

SingleStringCharacter ::
    SourceCharacter but not one of "'" or "\" or LineTerminator
    "<LS>"
    "<PS>"
    "\" EscapeSequence
    LineContinuation

LineContinuation ::
    "\" LineTerminatorSequence

EscapeSequence ::
    CharacterEscapeSequence
    0 [lookahead ∉ DecimalDigit]
    HexEscapeSequence
    UnicodeEscapeSequence

CharacterEscapeSequence ::
    SingleEscapeCharacter
    NonEscapeCharacter

SingleEscapeCharacter :: one of
    "'" """ "\" "b" "f" "n" "r" "t" "v"

NonEscapeCharacter ::
    SourceCharacter but not one of EscapeCharacter or LineTerminator

EscapeCharacter ::
    SingleEscapeCharacter
    DecimalDigit
    "x"
    "u"

HexEscapeSequence ::
    "x" HexDigit HexDigit

UnicodeEscapeSequence ::
    "u" Hex4Digits
    "u{" CodePoint "}"

Hex4Digits ::
    HexDigit HexDigit HexDigit HexDigit
```



