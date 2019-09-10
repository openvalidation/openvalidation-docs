# Keywords

## Rules

A rule is the most important construct in the grammar of openVALIDATION. It consists of a condition and an action. With a validation rule, the action is always an error message. The easiest way to express such a rule is a **IF / THEN** construct.

| Keyword | Description |
| :--- | :--- |
| `IF` | Selects the beginning of a rule and the next condition |
| `THEN` | Marks the beginning of an error message |
| `AND` | Selects the start of a new linked AND condition |
| `OR` | Selects the start of a new linked OR condition |

## Implicit Conditions / Alternative Rule Expression <a id="implizite-bedingungen-alternativer-regelausdruck"></a>

| Keyword | Description |
| :--- | :--- |
| `MUST, SHOULD, HAVE, HAS` | An indicator identifies an expression as a rule. _The condition in such a MUST expression contains an Implicit Negation!_ |
| `MUST NOT, MUSTN'T, SHOULD NOT, SHOULDN'T, HAS NOT, HASN'T, HAVE NOT, HAVEN'T` | An indicator identifies an expression, as a rule |

## Relational operators

Relational operations always has a left and a right operand and the corresponding comparison operator.

| Keyword | Description |
| :--- | :--- |
| `IS, EQUAL, EQUALS` | A relational operator '=' for numeric and string operands |
| `ISN'T, IS NOT, NOT EQUAL, NOT EQUALS, NOT` | A relational operator '!=' for numeric and string operands |
| `LESS, SMALLER, LOWER, FEWER, SHORTER` | A relational operator '&lt;' for numeric operands |
| `GREATER, BIGGER, LARGER, MORE, EXCEED, EXCEEDS, HIGHER` | A relational operator '&gt;' for numeric operands |
| `GREATER OR EQUAL, GREATER OR EQUALS, LEAST, AT LEAST` | A relational operator '&gt;=' for numeric operands |
| `LESS OR EQUAL, LESS OR EQUALS, MOST` | A relational operator '&lt;=' for numeric operands |
| `EXIST, EXISTS, GIVEN` | A relational operator for non-"null" |
| `DOESN'T EXIST, DON'T EXIST, NOT EXIST` | A relational operator for null |

## Arithmetic operations <a id="arithmetik"></a>

| Keyword | Description |
| :--- | :--- |
| `+, PLUS` | A mathematical operation for a simple addition |
| `-, MINUS` | A mathematical operation for a simple subtraction |
| `*, TIMES` | A mathematical operation for a simple multiplication |
| `/, DIVIDED BY` | A mathematical operation for a simple division |
| `MOD, MODULO` | A mathematical operation for a simple modulo calculation |

## Comment <a id="kommentar"></a>

You can write comments in the rulebook. These do not contain any logic.

| Keyword | Description |
| :--- | :--- |
| `COMMENT` | A comment |

## Error message <a id="fehlermeldung"></a>

| Keyword | Description |
| :--- | :--- |
| `ERRORCODE, WITH ERRORCODE, WITH CODE, WITH ERROR` | Error messages often contain their own error codes for unique identification. |

