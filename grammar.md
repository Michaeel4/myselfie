Copyright (c) the Selfie Project authors. All rights reserved. Please see the AUTHORS file for details. Use of this source code is governed by a BSD license that can be found in the LICENSE file.

Selfie is a project of the Computational Systems Group at the Department of Computer Sciences of the University of Salzburg in Austria. For further information and code please refer to:

http://selfie.cs.uni-salzburg.at

This is the grammar of the C Star (C\*) programming language.

C\* is a tiny subset of the programming language C. C\* features global variable declarations with optional initialization as well as procedures with parameters and local variables. C\* has five statements (assignment, while loop, if-then-else, procedure call, and return) and standard arithmetic (`+`, `-`, `*`, `/`, `%`) and comparison (`==`, `!=`, `<`, `<=`, `>`, `>=`) operators. C\* includes the unary `*` operator for dereferencing pointers hence the name but excludes data types other than `uint64_t` and `uint64_t*`, bitwise and Boolean operators, and many other features. The C\* grammar is LL(1) with 6 keywords and 22 symbols. Whitespace as well as single-line (`//`) and multi-line (`/*` to `*/`) comments are ignored.

C\* Keywords: `uint64_t`, `void`, `if`, `else`, `while`, `return, structs, for`

C\* Symbols: `integer_literal`, `character_literal`, `string_literal`, `identifier`, `,`, `;`, `(`, `), [,],` `{`,`}`, `+`, `-`, `*`, `/`, `%`, `=`, `==`, `!=`, `<`, `<=`, `>`, `>=`, `..., <<, >>,  &, |, ~, structs, ->`

with:

```
integer_literal   = digit { digit } .

hex_literal = "0x" digit | hex_letter { digit | hex_letter } 

character_literal = "'" printable_character "'" .

string_literal    = """ { printable_character } """ .

identifier        = letter { letter | digit | "_" } .


```

and:

```
hex_letter = "A" | "B" | "C" | "D" | "E" | "F" | "a" | "b" | "c" | "e" | "f"

digit  = "0" | ... | "9" .

letter = "a" | ... | "z" | "A" | ... | "Z" .
```

C\* Grammar:

```


 cstar             = { type identifier [ "[" integer_literal "]" ";" ] | 
                      [ "=" [ cast ] [ "-" ] ( integer_literal | character_literal ) ] ";" |
                    ( "void" | type ) identifier procedure } .

type              = "uint64_t" [ "*" ], "structs" .

cast              = "(" type ")" .

procedure         = "(" [ variable { "," variable } [ "," "..." ] ] ")" ( ";" |
                    "{" { variable ";" } { statement } "}" ) .

variable          = type identifier .

statement         = ( [ "*" ] identifier | "*" "(" expression ")" ) "=" expression ";" |
                    call ";" | while | if | return ";" .

call              = identifier "(" [ expression { "," expression } ] ")" .

expression        = simple_expression

                    [ ( "==" | "!=" | "<" | "<<" | ">>" | ">" | "<=" | ">=" | "&" | "|") simple_expression ] .

bit_not_expression = expression [ "~" ] 


simple_expression = term { ( "+" | "-" ) term } .

term              = factor { ( "*" | "/" | "%" ) factor } .

factor            = [ cast ] [ "-" ] [ "*" ]
                    ( integer_literal | character_literal | string_literal |
                      identifier | call | "(" expression ")" ) .
for               = "for" "(" expression | statement ")"
                  "{" { statement } "}"
                  ) .
while             = "while" "(" expression ")"
                      ( statement | "{" { statement } "}" ) .

if                = "if" "(" expression ")"
                      ( statement | "{" { statement } "}" )
                    [ "else"
                      ( statement | "{" { statement } "}" ) ] .

return            = "return" [ expression ] .
```
