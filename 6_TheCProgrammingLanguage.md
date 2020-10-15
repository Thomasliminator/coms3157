# 6. The C Programming Language
*Brian W. Kernighan & Dennis M. Ritchie*

## Chapter 1: A Tutorial Introduction

### 1.1: Getting Started (Hello World)

`hello.c`
```C
#include <stdio.h>

main()
{
    printf("hello, world\n");
}
```

We can compile this with the command `gcc hello.c` and can run by typing `a.out`
"`main`" is where the program begins executing. Therefore, every program must have `main` somewhere.
`#include <stdio.h>` tells the compiler about the standard input/output library.

### 1.2: Variables and Arithmetic Expressions

We can declare variables such as:
`int fahr, celsius;`

Data types in C:
`int`: 16 bits
`float` 32 bits
`char` single byte
`short` short integer
`long` long ingteger
`double` double precision floating point

Assignment in C works as the following:
`lower = 0;`
`fahr = lower;`

The format for a while loop in C:
```C
while (condition){
    ...
}
```
### 1.3: For Loops 
The format for a for loop in C:
```C
int i;
for (i = 0; i < 100; i++){
    ...
}
```

In C, it is convention to declare the iterator variable outside of the loop.

### 1.4: Symbolic Constants

A `#define` line defines a symbolic name or symbolic constant to be a particular string of characters:
`#define name replacement text`
Any occurence of name will be replaced by the corresponding replacement text. The replacement text can be any sequence of characters and is not limited to numbers.

### 1.5: Character Input and Output

`c = getchar()` will contain the next character of input.
`putchar(c)` prints a character each time it is called.

### 1.6: Arrays

We can declare an array similarly to in Java:
```C
int ndigit[10];
for(int i = 0; i < 10; i++){
    ndigit[i] = i*i;
}
```

General form for an if-else statement:
```C
if(condition1)
    statement1
else if(condition2)
    statement2
...
    ...
else
    statementN
```

### 1.7: Functions

A function definition has the form:
```C
return-type function-name( parameter declarations, if any )
{
    declarations
    statements
}
```

### 1.8: Arguments - Call by Value

In C, the called function cannot directly alter a variable in the calling function; it can only alter its private, temporary copy. It is only possible to alter it using the address, via pointer, of the variable.

*No notes for 1.9: Character Arrays*

### 1.10: External Variables and Scope

Automatic variables are local variables that only exist when the function is called. They disappear when the function terminates.

As an alternative, we can declare external variables which can be accessed by any function. They must be `defined` and must also be declared in each function that wants to access it. The declaration may be an explicit `extern` statement or it may be implicit. 
