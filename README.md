# While language

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/b1705795c5f74b9289b6f4c942dd5911)](https://www.codacy.com/app/leonardo-lucena/whilelang?utm_source=github.com&utm_medium=referral&utm_content=lrlucena/whilelang&utm_campaign=badger)

> A small programming language created with ANTLR and Scala.

This is a programming language with only one loop instruction (while) and a single type (integer).
The goal is to show that with a few lines of code it is possible to implement a programming language.

The language is implemented in two ways:
 - as an [interpreter](interpreter.md)
 - as a [transpiler](transpiler.md) (compiler) for the Scala language.

<table>
  <thead>
    <tr>
      <td></td>
      <th>Interpreter</th>
      <th>Compiler (Transpiler)</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <th>Grammar</th>
      <td colspan="2" align="center"><a href="#grammar">Grammar</a> (36 lines)</td>
    </tr>
    <tr>
      <th>Parser Rules</th>
      <td><a href="interpreter.md#parser-rules">Listener</a> (74 lines)<br>
       <a href="interpreter.md#abstract-syntax">Abstract Syntax</a> (28 lines)<br>
          <a href="interpreter.md#semantics">Semantics</a> (36 lines)</td>
      <td><a href="transpiler.md#parser-rules">Compiler</a> (87 lines)</td>
    </tr>
    <tr>
      <th>Main</th>
      <td><a href="interpreter.md#main">Main</a> (14 lines)</td>
      <td><a href="transpiler.md#main">Main</a> (14 lines)</td>
    </tr>
    <tr>
      <th>Utility Classes</th>
      <td colspan="2" align="center"><a href="interpreter.md#antlr2scala">Antr2Scala</a> (13 lines)<br> <a href="interpreter.md#walker">Walker</a> (25 lines)</td>
    </tr>
    <tr>
      <th>Total</th>
      <td>226 lines</td>
      <td>175 lines</td>
    </tr>
  </tbody>
</table>


## Examples
Here are some code examples:

Hello World
````ruby
print "Hello World"
````

Sum of two numbers
````ruby
print "Enter the first number:";
a := read;
print "Enter the second number:";
b := read;
sum := a + b;
print "The sum is:";
write sum
````

Fibonacci Sequence
````ruby
print "Fibonacci Sequence";
a := 0;
b := 1;
while b <= 1000000 do {
  write b;
  b := a + b;
  a := b - a
}
````

## Grammar

The formal syntax is as follows (ANTLR syntax):

````antlr
grammar Whilelang;

program : seqStatement;

seqStatement: statement (';' statement)* ;

statement: ID ':=' expression                          # attrib
         | 'skip'                                      # skip
         | 'if' bool 'then' statement 'else' statement # if
         | 'while' bool 'do' statement                 # while
         | 'print' Text                                # print
         | 'write' expression                          # write
         | '{' seqStatement '}'                        # block
         ;

expression: INT                                        # int
          | 'read'                                     # read
          | ID                                         # id
          | expression '*' expression                  # binOp
          | expression ('+'|'-') expression            # binOp
          | '(' expression ')'                         # expParen
          ;

bool: ('true'|'false')                                 # boolean
    | expression '=' expression                        # relOp
    | expression '<=' expression                       # relOp
    | 'not' bool                                       # not
    | bool 'and' bool                                  # and
    | '(' bool ')'                                     # boolParen
    ;

INT: ('0'..'9')+ ;
ID: ('a'..'z')+;
Text: '"' .*? '"';

Space: [ \t\n\r] -> skip;
````
---

## Compiling & Running

To compile you need to install [sbt](https://www.scala-sbt.org/). The easiest way is to use [sdkman] (https://sdkman.io/install) (Linux) or [Scoop] (https://scoop.sh/) (Windows).

````shell
$ sbt
sbt> clean
sbt> compile

# To run the interpreter
sbt> runMain whilelang.interpreter.Main sum.while

# To run the transpiler
sbt> runMain whilelang.compiler.Main sum.while
````
