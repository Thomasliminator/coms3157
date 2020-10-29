# 1. Linux Programming Environment for AP

## CLAC Server

Students will recieve an account on CLAC, a Linux server where we will be doing our work. All HW will be submitted on CLAC. 

## Logging into CLAC

On Mac OS X, launch Terminal app and type the following:

    ssh -X YOUR_UNI@clac.cs.columbia.edu
    // -X sets up X forwarding 


## Using the Terminal

### Terminal Commands

`ls` will list all the files in the folder/directory.
- `ls -p` displays directories with `/`
- `ls -F` flags filenames (* / = > @)
- `ls -F -l` lists in long format (provides more information)
- `ls -a` lists "all" which gives the entire contents of the directory

`pwd` will tell me the current directory that I am in. 
- the root directory is just denoted by `/`

`man` gives us the manual for a command.
- for example `man ls` gives us instructions on the `ls` command
- to leave the page, just type q

Linux convention is to put a `.` in front of file name to hide it.
- for example .config files don't need to be seen (`ls` command will not show)

Some basic UNIX commands:
`man, cat, less, rm, cp, ls, ll (an alias for ls -alF), pwd, cd, mkdir, alias, locate, gcc, make, touch, clear, history, date, mv, grep, diff, find, tar`

### `cmp` and `diff`

The `cmp` and `diff` functions tell compare files. The first tells us if they are different and the second tells us where they are different. `cmp` compares it byte by byte.

### Aliases and Configuration

We can use aliases to make shortcuts for commands.

`alias` will show all aliases.

### Creating a New Directory

`mkdir ap` will create a new directory called "ap".
`cd ap ` will move into the "ap" directory. 
- this is a relative path, not a full path.
`cd ~` will go back to home directory
- `cd FULL_PATH` is also valid to go back to home directory
- `cd` by itself will also go back to home directory

In every new directory you will see two files `./` and `../`
- these can be shown by `ls -alF`
- `./` is an entry that refers to this directory itself (the current directory)
- `../` is the directory that is one level up from the current directory
- you can `cd ..` to move up directories

### Writing a File

`nano hello.c` will create a file called `hello.c`
- uses the nano editor to edit the file `hello.c`

`cat hello.c` will dump the contents of the file in the terminal.

`cp hello.c hello2.c` will make a copy of `hello.c `

### Moving a File

`mv hello2.c main.c` will rename our file (move).

`mv hello.c ../` will move the file up one directory.

## Picking your Editor 

There are two popular text editors in UNIX: Emacs and Vim. Vim is recommended in this class. 

### Vim

Characters are all commands. Vim starts in a command mode. It has two modes, command and insert mode.
- `x` deletes a character
- `dd` deletes a whole line
- `p` will paste
- `w` will save the file
- `esc : q` will exit

Type `i` to initiate "insert" and now you can type.
- `esc` to leave insert mode

Run `vimtutor` in the command line to get a built-in tutorial for Vim. 

## Setting up your Shell Environment

The application with which you interactin a termianl window is called a "shell".
The CLAC account is configured to use the Bash shell by default.

Type `echo $SHELL`. It should say `/bin/bash`.

There is something that needs to be added to the `.bashrc` file in the home directory to set up the shell environment. Add the following line to the end of the `.bashrc` file.

    export EDITOR=vim
    
Log out of CLAC by typing "exit" and then log in again. Type `echo $EDITOR` to see if the modification has taken effect. If it hasnt, then add the following lines to the `.bash_profile` file:

```
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi
```

*edited L9/10, T9/14*
# 2. Vim Tutorial and Compiling

## Vim Tutorial

### Controls in Vim

`:set number` will display line numbers.

`shift g` will go to the end of the file.
`gg` goes to the beginning of the file.
`zz` moves the current line to the middle of the page.
`z 'enter'` moves the current line to the top of the page.

`:wq` to quit out of Vim. 
`/` will allow you to search for something
- `n` lets you go to the next term

`cp file1 file2` will copy file1 onto file2 and you can use this to restart to correct an error.

`o` will allow you to add a line right after the current line. Changes automatically to insert mode.

## C Basics, Compiling and Linking

### See `hello.c` in CLAC.

Every program in C has a method called `main()`.

`return 0` from main means that "everything went well". It is a C convention. 

`printf` is formatted printing: `%d` will look at the next value (specifically looks for a decimal) and subsitute, `\n` means new line.

`gcc` is the compilier. In the textbook, the compiler is `cc`. 
- swap all occurences of `cc` with `gcc` in the textbook.
- this is not how you usually compile unless you are doing something really simple. 

After compiling the program, you need to open the `a.out` file that will show the output.
Using the -F option will show a `*` next to the `a.out` file which indicates that it is runnable.

We can run the `a.out` file by either putting the entire file path or `./a.out`

Two things must be done now: compiling and **linking**. 
- `gcc` does both compiling and linking at the same time.

### Compiling and Linking, Step by Step

#### Step 1: Compiling

`gcc -c hello.c` does only the compilation
- Now we have a `hello.o` file which is called an "object file".
- this is not the program itself, it is a part of a program: it cannot be run

#### Step 2: Linking
`gcc hello.o -o hello`
- give it the object file that will produce the executable file
- `-o` will name the output file

Run `./hello` to run the program.

### `myadd()` function added to `hello.c`

The following are all commands in Vim:

`:ls` in Vim will list the files that are open in Vim.
Can switch between buffers by using `:b1` or `:b2`
`:bn` goes to the next buffer.

`'shift' v` will go to visual mode where we can go up and down to select.
Type `d` to delete from the current file. (cut)
Type `p` to paste. 

### Compiling and Linking Multiple Buffers

`gcc hello.o myadd.o -o hello` is proper linking.

Add `-Wall` and `-g` options when compiling:
- `-g` will be useful for debugging
- `-Wall` will enable all warnings; we can also enable specific warnings

We have a warning when we compile `hello.c` because of "implicit declaration of function `myadd`".

We must declare the function by writing the first line of the function and terminating with a semicolon. This is also known as the prototype of the function, also a signature of a function. 

**You must have the prototype, at a minimum, of the function**

Why don't we have to list `printf()` prototype?

### #include `<stdio.h>`

This is not the same as "importing" in Java. This is an inclusion.

This is a directive that tells the compiler "please find the file `stdio.h` and put the content of the file in that line". This is textual replacement.

`printf()` is a function collected in `stdio` so it doesn't generate a warning when compiled.
- the prototype for `printf()` is included in `stdio.h`

If you do `"myadd.h"` this tells to look in the current directory first in comparison to `<stdio.h>`.

Standard library functions are linked automatically by the gcc. The `libc.a` is the core standard C library file. `libm.a` is another standard C library file that stores math functions. `.a` files are like zip files, "a for archive". 

Use `-lc` to link with `libc.a` and `-lm` to link with `libm.a`. However, `libc.a` is automatically linked.

#### `myadd.c` has been changed to take 3 parameters

Compilation is simply taking a `.c` file and making a `.o` file. It will check that when you call `myadd(x, y)` that it matches the prototype given. There is no error when compiling. No error with linking either. 

However, if we run it, we don't get the expected value. Linking only looks and matches names of functions. It doesn't check if the parameters are correct. It pulls in garbage from the next memory location to fill in `salt`.  

To fix this, we declare it in the `.c` file by including the header file. Now, if we try to compile `myadd.c` it will give us an error of "conflicting types". 

## Makefiles and `make` tool

We don't want to type `gcc ...` every single time to compile and link all the time. We want to automate.

`make` does compilation, linking, and possibly other things, automatically. A programmable scriptor, automator. It is a program that we run. It reads a `Makefile` and follows the directions. 

Do `cat -t Makefile` which will turn all tabs into `^I`. `-n` is the line number option. `-e` gives up the end of the line option. `-v` will show any control characters embedded in the file. 

Run the Makefile using `make`. It will only make the first target and then quit. You have to explicitly say if you want to make a second target. The mission is to make the first target.

Use `touch [fileName]` to update a time stamp. This will simulate a change. 

A   `Makefile` has the format:
- what you want to produce `main.o` (target)
- what you need to produce that file `main.c` (prerequisite)
- how to produce `gcc -c -Wall -g main.c` (recipe)

```
main: main.o myadd.o
    gcc main.o myadd.o -o main

main.o: main.c
    gcc -c -Wall -g main.c
    
myadd.o myadd.c myadd.h
    gcc -c -Wall -g myadd.c
```

We can make variables in `Makefile`. Use the format `$([variableName])` to use them.

`make` assumes that if we are building something `.o` out of something `.c` using `CC` and `CFLAGS` variables, it can deduce the commands.

We can ultimately simplify the Makefile as follows:

```
CC = gcc
CFLAGS = -Wall -g

main: main.o myadd.o

main.o: main.c myadd.h

myadd.o: myadd.c myadd.h

.PHONY: clean
clean:
    rm -f *.0 main core
    
.PHONY: all
all: clean main
```

### `clean`

We add `clean` that will remove `.o` files and executable files.
The `-f` flag forces the remove even if the file is not seen.
This is a fake "`phony`" target because we only want the command to be run, we don't need `clean` to be produced. 

### `all`

`all: clean main`
It first does the clean and then does the full make. Tries to produce clean first which will run the rm command. Then it will produce the target main: since all the files are gone it will rebuild everything. 
`make all` will rebuild everything. 

## Macros

The `#include <stdio.h>` was a macro.

### #define

`#define PI 3.14` will replace all instances of PI as 3.14
`define SQR(x) x*x` can be used as a function.
- for example, `SQR(3)` will calculate `3*3`

Macros are not used very much anymore. Macros are highly discoraged and if they must be used, then use parenthesis everywhere. Should just use a proper function.

#### possible error example
`SQR(3+4)` will give `3 + 4 * 3 + 4`

###  #ifdef (conditional compilation)

```C
#ifdef __unix__
void f() {
    ---
}

#else
void f() {
    ---
}
#endif
```

This can be used if we are using different OS or something. Then, we can bring in a different version of the function. 

From now on, any time you write a header file you have a file of this convention:

```
#ifndef __MYADD_H__
#define __MYADD_H__
int myadd(int, int, int);

#endif
```

*edited L9/15 L9/17 NL9/10 L9/22*
# 3. Integer Data Types and Binary Numbers

## Integer Data Types in C

`char` is an integer (smallest data type) that takes up one byte (8 bits) of memory.
- can represent 256 distinct values (-128 ~ 127)

`short` is a type that is two bytes.
- represents values from -2<sup>15</sup> ~ 2<sup>15</sup> - 1

`int` is four bytes. Most common integer data type that is used
- from -2<sup>31</sup> ~ 2<sup>31</sup> - 1

`long` is typically 8 bytes. In older computers it was sometimes 4 bytes.

`long long` is always 8 bytes

2<sup>10</sup> = 1024 (1K)
2<sup>20</sup> = 1024 * 1024 (1M)
2<sup>30</sup> = 1G

### `unsigned`

if you declare something as `unsigned` it only uses positive numbers but has twice the range. Can also be expressed as `uint`.

## Binary Numbers

### Translating from bits to numbers

The standard is two's complement notation. 

Assume we have a 4 bit integer type `int4_t`.

The rule for negative numbers are is that:
- the MSB is given a negative weight

#### ex. 1101

-1 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 1 * 2<sup>0</sup>

### Consequences of Two's Complement

If the MSB is 1, then can you can always assume that it is a negative number.

If all the bits are 1, then it will be equal to -1. This is the case for any number of bits.

The number 100...0 is the largest (magnitude) negative number.

### Hexadecimal Numbering System

The hexadecimal numbering system goes from 0-9-A-F.

If you mean hex number, you have to write preface it with `0x`
- for example `0x12` is a hexadecimal number. 

#### Examples

`int x = 0xffffffff;` is the same as `int x = -1`

`0x7fffffff` this is the maximum positive number.

`0x80000000` is the greatest magnitude negative number.

### Differences between `0` `\0` and `'0'`

`0` is the number 0.
`\0` is also the number 0, it is identical to above.
-written like this to indicate "characterness" of the number: it is more instructive

`'0'` is different --> this is number 48. This is a character that is found in the ASCII table. 


### ASCII

```C
{
    int x = 'a';
    
    //is the same as
    int x = 97
    
    int y = 'a' + 1; //is a valid expression
}
``` 

*edited NL9/12*
# 5. Expressions and Statements in C

## Expressions

Expressions are a piece of C program that has a *value*. Every value in C has a *type*.

#### Examples of expressions:

`5` (int)
`x+1` (depends on x)
`f(100)` some function that has a return value
`prime(5)` a function that was written

Things might be less straightforward with an expression such as `x = 1`, where x was declared to be an integer.
- the value of an assignment expression is the value of the variable on the left side (L-value) after the assignment. (This is what should be expected)
- that means that we can do stuff like `(x = 1) = 2` (although unconventional)
- x is still 1 after the above expression although the expression evaluates to 3.

`int x = 1;` this is not an expression, this is declaring and initializing. All expressions are statements but not all statements are expressions.

#### The following are all equivalent:

`x = x + 1;`
`x += 1;`
`++x` (prefix)

### Prefix vs. Postfix

```C
int x = 1;
int y = ++x;
printf("%d", x); //will print 2.
printf("%d", y); //will print 2.
```

```C
int x = 1
int y = x++;
printf("%d", x); //will print 2;
printf("%d", y); //will print 1;
```

Prefix increment, the value of the expression `++x` is the value of the variable after the increment.
The value of the expression `x++` is the value of x before the increment.

What if we do `int y = (x++);`? We still get 1!

### Comparisons (Logical Expressions)

`x == 5` is a comparison. C uses integers as Boolean. Boolean does not exist in C. The type of this expression is int.
- 0 is false; 1 is true; anything that is nonzero is true. 

`if(x < 100 && f(x)) { } ` is a typical complex Boolean expression.

With a logical and (&&) it will short circuit the logical expression if the first half of the expression is false. The same thing works with logical or (||). 

### Bitwise Operator

```C
int x = 1;
int y = 2;
int z;

z = x | y; //3; this is bitwise or.
z = x & y; //0; this is bitwise and.

z = x << 2; //4
```

For bitwise or, you line up the binary representation and do "or" on each digit. If you have at least one "1" you return 1. Same with "and"

`<<` is a left shift. You shift the digits to the left. The ones that fall off are discarded. The remaining on the right side are filled in with zeros. 
This is the same thing as multiplying by 2<sup>n</sup>. It doesn't hold if bits are falling off the left side. 

Right shift `>>` works the same way except, you fill in what was the left most bit to begin with. You do this because it makes sense to preserve the sign. 

`int z = ~0;` this will flip all of the bits. The previous expression evaluates to -1. 

`int z = 19 & (1<<4);` evaluates to 16. It just picked out a particular bit `000...010000`.
- picks out the 5th bit from the right

#### Practice Problems

```C
int f(int x){
    return x |= (1<<4);
}
```

The above will turn on the 5th bit from the right, whether it was on or not. 

```C
int g(int x){
    return x &= ~(1<<4);
}
```

The above function turns off the 5th bit from the right. 

## Statements and Variables

### Loops

```C
//the following are all ways to loop 10 times.

int i;
for(int=0; i<10; i++){
    
}

while(i<10){
    
    i++;
}
```

### Scopes

A scope in C is a section/region of code, enclosed within {}. Variables declared inside only exist inside the scope. 

```C
foo(){
    int x; //automatic/local/stack variable
    x = 0;
    {
        int x;
        x = 1;
        printf("%d", x);
    }
    printf("%d", x); //this x is from the outside, will print 0.
}
```
Local/**automatic**/stack variables only exist inside the scope they are declared, or any inner scopes. Function parameters are also automatic variables.

### Static Variables

There is a second type of variable called **static** variable. (Has nothing to do with Java "static") There are three different kinds of static variables.
- 1. Global static: these are global variables, declared outside of any function. Discouraged, passing parameters through functions are much better.
- 2. File static
- 3. Function static

bar.c
```C
int x = 5; //this is a global static variable

int f1()
{
    int z; //local variable
    x = x + 1;
    printf("%d", x);
}

int f2() {
```

foo.c
```C
extern int x; //declare int x is external to this file, will be found in link time.
int f3()
{
    x  = x+2; //you can access x declared in main.c
}
```

The difference between the three static variables types is who can access it. In file static, we cannot access outside of the file that it is declared in:

bar.c
```C
static int x = 5; //the static keyword prevents access from any other file.

int f1(){
   
}

int f2() {
```

Function static variables don't behave like they look like they should. 

```C
int f()
{
    static int count = 0; //this does not get reset, the gets set to 0 when it first starts the program.
    count ++;
    return count;
}

int main()
{
    int s = 0;
    for(int i = 0; i < 10; i++) //will count 10 times.
        s += count();
    printf("%d", s);
}
```

`static int count = 0` will get skipped every time the function is called. However, it makes it so that no one else can modify count. This is what you do if you want a global life time but only want to be able to modify it inside of the function.

## Process Address Space

The memory diagram is: 
1. operating system code & data
2. stack
3. heap
4. static variables
5. program code

RAM memory has addresses from 0 to (5   12) (bytes).

Every program/running process thinks that it has 0 to 512G access to RAM. This is fake, **virtual** memory. For example, every single tab in Chrome thinks it has access to this. 

A lot of underlying mapping that goes from the virtual memory to the RAM. Assume you have access to the entire address space. 

64-bit computer means you can fetch/store 8 bytes at a time. Memory address 0 is not used because it is reserved for something special.

In some low address, the program code is stored. Before it calls the main function, it takes up some more on top to put the static variables. 

The automatic variables grow and shrink in size, they are stored at the top region called "stack". 

There is another place called "heap". `malloc` works with the heap region. 


*edited NL9/17, NL9/19, L9/24*
# 9. Pointers

## Pointer Types

```C
foo()
{
    int x = 1, y = 2;
    int *p;
    
    p = &x;
    y = *p; //basically same thing as y = x;
    
    *p = 0; 
    ++*p; 
    (*p)++; //if you omit the parenthesis, this is *(p++)
}
```

`int *` is a type. You can also write it as `int* = p` although the former is convention. This is a variable that contains the memory address to another (int) variable. They are always 8 bytes long.

`p = &x;` will give you the address. 

`*` is an operator that operates on a pointer variable and it goes into what it's pointing to. It "dereferences" a pointer.

```
int i = 3;
double d = 3.14;
int *p = &i;
double *q = &d;

//p = &d cannot be done because they are incompatbile

p = (int*) &d //you can do this to force it, but this is more complicated
```

`void *r` is a generic pointer. Then you can do something like `r = p;` then `q = r`.

### The Null Pointer

`char *q = 0;` is the same as `char *q = NULL;`

Normally, you cannot add values to a pointer variables. i.e. 1000, 23 ...
However, `0` is an exception. It can be assigned to a pointer variable, there is an automatic conversion to a pointer value.

`#define NULL((void*)0)` is how the NULL pointer is defined. This is done (casted) so that expressions such as `NULL + 1` would be illegal.

```
char c = 0;
char *p = &c;
```

Is `p` a null pointer? No! This has an actual memory address.

You can also do something like `if(p) {` which will test if `p` is a null pointer. In the above case, it is true. Every pointer is true except for the NULL pointer. 

`if(*p){` is false because it is "what it is pointing to". 

If you try to access a region that hasn't yet been mapped, segmentation error.
If you try to write to the code region, segmentation error. This is because it is "read only" at runtime. 

## Arrays

```C
int foo()
{
    int a[10] = {100, 101, 102, 103, 104, 105, 106, 107, 108, 109};
    int *p = &a[0]; //address of (a[0])
    printf("%d", sizeof(a));
    
}
```

Same way as declaring a variable. The format is (type) (name)(type). Accessing the array is typical.

`sizeof()` will evaluate to the byte size. `a` is an array of 10 integers. 
Therefore, `sizeof(a)` is 40. `sizeof(a[0])` = 4. `sizeof(p)` = 8. `sizeof(*p)` = 4.

With an array, C will make sure that the elements of the array are put in memory contiguously. 

Consider the expression `p+1`. What does it mean to add 1 to a pointer? It is the address of the next element after the current variable. 
`p+1` would add the value of `sizeof(*p)`. 

`*p` is the same as `a[0]`
`*(p+1)` is the same as `a[1]`
`p+1` is the same as `&a[1]`

Given a pointer, `*` means you are going to what it points to, what it has the address of. If you take the address of that, `&`, you get the pointer of that. Therefore, the `*` and `&` operators cancel out. 

**Theorem No. 1**
In general, `*(p+1) = a[i]`

### Theorem No. 2

```C
for(int i=0; i<sizeof(a)/sizeof(a[0]); i++){ //goes 0-9
    //method 1
    printf("%d", a[i]);
    
    //method 2
    printf("%d", *(p+i));
    
    //method 3
    printf("%d", *p);
    p++;
    
    //method 4
    printf("%d", *p++);
}
```

In method 3, we do `p++` which will move the pointer to the next element. 

`*p++` is a super idiomatic expression in C. The expression is completely equivalent to `*(p++)`. The incrementation is "delayed". An easier way to think of it is method 3, but you have to recognize that it is NOT equal to `(*p)++`.

`a` is an array name, but sometimes (most of the times), it immediately becomes `&a[0]`. 
- for example, you can do something like `a + 1` which becomes `&a[0] + 1` which is `p + 1`. 
- `*a`: dereferencing an array makes no sense but it becomes the expression `*(&a[0])` which is the same as `a[0]` which is `*p`.
- in most cases, the array name is interchangeable with the address of the first element. 

**Theorem No. 2**
`a` --> `&a[0]` "array name is the address of the first element"

### Grand Unified Theory (GUT) of Arrays and Pointers

From Theorem 1 and Theorem 2, we get:

**Theorem No. 3**
`*(a+i)` is the same as `a[i]`.
`*(p+i)` is the same as `p[i]`
**`*(x+y)` <==> `x[y]`**(This is the GUT).

*Dereferencing and the bracket are basically the same thing.*

If you have `*p` is the same as `p[0]`.
If you have `a[0]` it is `*a`. 

Let's say, `0[a]` was written instead of `a[0]`. This will be immediately transformed into `*(0+a)`. This has to be equivalent to `*(a+0)` due to commutative properties. This is therefore the same as `a[0]`. 
This is legal in C!

## Heaps

When you return from a function, local variables and arrays will cease to exist.
How can we declare an array that multiple functions can use? 
- that is what the heap is for.

```C
int *foo()
{
    int *p;
    p = malloc(10 * sizeof(int)); //value is 40
    
    ...
    
    return p;
}

int main()
{
    int *a = foo();
    bar(a); 
    
    ...
    
    free(a);
}
```

`malloc()` is a library function that stands for "memory allocation". It will create some (40) byte region in the heap area and will return a pointer to the first byte of the memory.  
Even though it was allocated inside of `foo()` it will continue to live on as long as we keep the pointer. 

`a` will get the pointer that points to the piece of memory in the heap. Then, you can pass that to another function. 

When we do `free(a)` it will mark the (40) bytes as not allocated so we can use that memory for another call of `malloc()`. 

You can keep `malloc()`ing and the heap will continue to grow. It will also consist of a lot of holes as the program keeps executing. 

*edited NL9/19, NL9/24, L9/29*
# 10. Arrays and Strings

## char [ ]

It works exactly the same way as an `int` array. The only difference is the size of each element. 

```C
{ 
    int a[10];
    char b[5] = {100, -2, 37, -5, 42};
    
    //char c[4] = {97, 98, 99, 0};
    char c[4] {'a', 'b', 'c', '\0'}; //this is equivalent to the line above
    char c[4] = "abc"; //also equivalent
    
    char *p = "abc";
}
```

The array `b` takes up the space of 5 bytes that are initialized with the five numbers listed.
If we have `char` arrays that end with 0, then we treat those as a special case. They are treated as strings.

Note that `'0'` <==> 48. `\0` <==> 0.

String literal expression such as `"abc"` is an anonymous array consisting of ASCII characters and one more character, which is `0`. The type of the expression is `char[4]`.

It becomes a little bit more confusing if we have `char *p = "abc";`. There is no array being declared here. We are only declaring a pointer `p`. We are initializing it with the anonymous array `"abc"` with type `char[4]`. 
Immediately turns itself into `char *` type which will point to the first element. Then where is this `"abc"` stored?
- it is just part of the code, it is a string literal that is not able to be changed or modified. Immutable!

`printf("%s", "hello" + 1)` is a valid line of code. The string is of type `char *` so this is perfectly valid. This statement will print `"ello"`.

## The `strcpy()` function

```C
    void strcpy(char *t, char *s)
    {
        //method 1
        while((*t = *s) != 0)
        {
            t++;
            s++;
        }
        
        //method 2 (canonical way to write strcpy function)
        while((*t++ = *s++) != 0)
            ;
    }
    
    void strcat(char *t, char *s)
    {
        while(*t) {t++;}
        strcpy(t, s);
    }
    
    int main()
    {
        /*
        char t[4];
        char *s = "abc";
        strcpy(t, s);
        */
        
        char t[6];
        strcpy(t, "abc");
        strcat(t, "xy");
    }
```

`t` and `s` are different variables in `strcpy()` and `main()`. In the "canonical" version, at the end of the loop we will be pointing one past the last element, which is okay in C.

`strcat()` is concatenation, appending to the end of the string. We move the pointer of `t` to `0` and then call `strcpy()` to copy `s` at the end of the string.

Buffer overflow: because the string that you are copying is the target allocated array, you will keep writing past the array. That behavior can be exploited by malicious attackers.

`strcpy()` and `strcat()` will blindly do the while loop until it sees a 0, which is kind of dangerous.

### `strncpy()`

Better safer versions of copy and concatenation:

`strncpy(char *t, char *s, size_t n)`
- `size_t` is basically an unsigned int. It is a limit for the copy (number of characters)

`strncat(char *t, char *s, size_t n)`

## args

Can you write a program that takes command line arguments?

```C
int main(int argc, char **argv) //in the textbook written as char *argv[]
{
```

If we do `$ ./a.out hello world` it will 

`argc` is 3. It will prepare an array of length four where each element is a pointer to an array. It points to the arguments in the command line. The final element in the array is a null pointer.

`argv` points to the array of pointers. 

```C
{
int a[10]
int *p = &a[0];
}
```

Writing a piece of code that prints out the arguments in the command line. Also skip the executable file name. 

```C
int main(int argc, char **argv)
{
    argv++;
    while(*argv)
    {
        printf("%s", *argv++);
    }
}
```

## Lab 2, Part II

In lab 2, make sure that everything is of the right type. Type analysis is very important in the lab.

There will be a main function that can't be changed. 

Recommend doing it in stages.
1. Make sure that you successfully copied and make sure they point to the same strings.
2. Then duplicate the strings and change to capital letters. 

When freeing, you have to free the strings first before you free the middle. 

*updated NL9/26*
# 11. Function Pointers

## The `const` keyword

This is a useful function to set a variable to something constant. Makes it a read only variable.

You can say things like:
`const int BUF_SIZE = 1024;` which is a good alternative to #define.

`const int *p = &x;`
- a `const` pointer only reads to what it is pointing to but cannot write to it. 
- in the `mystrcpy` we can make `s` a `const char` pointer because we are only reading it. We will not be changing it.
- this is how `strcpy` is implemented in C. Run the command `man strcpy` to see the documentation. 

`int *const p = &x;`
`const int *const p = &x`

You can cast away "const-ness". Let's say that `t` is a `const char` pointer, then you can do the following:
`char *t2 = (char *)t;` which will make it not `const` anymore.

## Function Pointers

`qsort()` is a sorting function (quicksort) that is provided by the C library. 
You must pass in the following parameters:
1. pointer to the first element (base)
2. how many members/elements (nmemb)
3. size of the individual element (size)
4. pointer to a function (*compar)

Reference the file `lectnote07.c` in `tmp` directory to see code.

How is the `qsort()` going to compare the elements to sort? It has no way to compare two elements.
The strategy that the function takes is that it asks the caller of the function to provide another function that can compare the elements for the sort.

## Complex Declaration

Reference the file `lectnote07.c` to see the code.

`int (*f1)(const void *v1, const void *v2);` is the declaration of a variable (pointer to a function)
- you dereference the pointer `f1` and then call it using the parenthesis after. After you call it you get an integer.
- `char *a[3]` a is an array of 3 pointers to char.
- you can call the function either by dereferencing it or not (the compiler will allow you to not include the *)


`int *f2 (const void *v1, const void *v2);` is a function prototype

`int (*f3[5])(const void *v1, const void *v2);` 
- `f3` is an array of five pointers that calls a function that returns integer. The only difference between f1 and f3 is an array of 5 `f1`s. 


*updated L10/8*
# 11. Structures and Linked Lists

## `struct`

Similar to class in Java. Collects data fields into one object and then you can add methods to it.
In `struct` you cannot add a method in C. It defines a type. 

```C
struct Pt {
    double x;
    double y;
};

int main()
{
struct Pt p1 = { 1.1, 2.2 };

double x1 = p1.x;

struct Pt *q1;
q1 = &p1;

x1 = (*q1).x; //. binds more tightly than *
x1 = q1->x; //how to access through the pointer

printf("%lu\n", sizeof(p1)); //prints 16 
}
``` 

This represents a point in a plane. You can access individual items with `p1.x` just like in Java.

We use `.x` to access from a structure. We use `->` to access from a pointer.

`printf("%p\n", q1);` vs. `printf("%p\n", &q1->x);` print the same pointer value. The first is of type `struct Pt *` and the latter is of `double *` type. 

`printf("%p\n", q1 + 1);` vs. `printf("%p\n", &q1->x + 1);` no longer print the same value. They differ by 8. 

## Linked List

Use `struct` to create a Node.

```C
struct Node{
    struct Node *next;
    int val;
};
```

See src: `list1.c` `list2.c`.

Create a node on the heap using `malloc(sizeof(struct Node))`.

`assert(argc == 1)` doesn't do anything if the condition is true. If it is not true, it stops the program. It is a debugging aid. Anything can be inside of the condition.
- You can do a short hand `assert(node)`.

When freeing memory for 2 nodes, free the second one first then free the first. Look at code for `struct Node *create2Nodes(int x, int y)` method. 
If you free the head then you lose the access to the next nodes and you cannot free. 





*updated L10/13, L10/20*
# 13. Input/Output

## Standard I/O

C libarary provides 3 I/O channels:

`stdin` (standard input)
- incoming character stream, normally from keyboard

`stdout` (standard output)
- outgoing character stream, normally to terminal screen
- buffered until newline comes or buffer is filled 

`stderr` (standard error)
- outgoing character stream, normally to terminal screen
- unbuffered

`<stdio.h>` contains prototypes for standard I/O functions such as `printf()` and `scanf()`.

### Redirection

We can make a `myinput` file and then run the command `./a.out < myinput` to redirect the standard input. 
This changes the standard input of `./a.out` so that it comes from the file rather than reading from the keyboard.

Can also do `./a.out < myinput > myoutout` to redirect standard ouuput.

`>` will override the file (deletes everything). Use `>>` to append instead. 

Valgrind outputs to `stderr`.

Use `2>` to redirect `stderr`. 
`valgrind --leak-check=yes ./a.out > myout 2> myerr`

`2>&1` means redirect `stderr` to the same place `stdout` is going to. 
** this does not work `./myprogram 2>&1 my_output_and_errors`

### Pipeline

`cat` puts the contents of the file into the `stdout`. You can chain two commands using a pipe.

`cat myinput | ./a.out` the `stdout` of the first goes directly to the `stdin` of the next. 
The effect is the same as the command  `./a.out < myinput`. 

`grep` is a program that reads line by line from whatever comes through the program and then will only print the lines that contain the following argument.
`cat myinput | ./a.out | grep array`

`w` program tells you who logs into the machine right now. 

`tail` is a program that will print the last 10 lines. There is a corresponding `head` program.
- the `-n` option will allow us to print the last n lines.
- use `-n +NUM` to output starting with line NUM (line number starts from 1)

`cut` is a program that can cut the columns. 
- use `-f` to choose a field (column)
- `-d` option allows you to choose a delimiter

`sed 's/one/two'` changes all occurences of one to two.
`sed 's/$/@columbia.edu` will add @columbia.edu to the end of the line ($)

`w | tail -n +3 | grep vim | cut -f 1 -d " " | sed 's/$/@columbia.edu/ | sort | uniq > vimusers.txt`

In shell you can do a for loop:
- `for i in $(cat vimusers.txt); do echo $i; done`

To email everyone:
`for i in $(cat vimusers.txt); do mutt -s "spam from ap" $i < myinput ; done` 

### To summarize pipelines:

A pipe `|` connects `stdout` to one program to `stdin` of another.
You can throw redirections in there.
You can include `sterr` in the flow as well using `2>&1`.

### Formatted I/O

Things such as `scanf()` `printf()`.
- there are a ton of options: 

## File I/O

`ncat.c` dumps the output but it numbers the lines.

### `EOF`

A special value indicating end-of-file, usually `#defined` to be -1.

### `fprintf(FILE *file, const char *format, ...)`
More general version of printf. 

For example, 
`fprintf(stderr, "%s\n", "usage: ncat <file_name>");`

`fprintf(stdout, "%s\n", "usage: ncat <file_name>");` 
*is equivalent to*
`printf(stderr, "%s\n", "usage: ncat <file_name>");`

### `fopen(const char *filename, char char *mode)`
Opens a file. Tell the OS or the stdlib that you would like to work with this file. 
It prepares the file for subsequent reading and writing.

It will return a pointer to a file `FILE *`.

Take two parameters, filename and one of the following modes:
- "r" open for reading (file must exist)
- "w" open for writing (will trash existing file)
- "a" open for appending (writes will go to end of the file)
- "r+" for reading and writing (file must already exist)
- "w+" for reading and writing (will trash existing file)
- "a+" open for reading adn appending (writes will go to end of the file)

For example,
`fopen(filename, "r");` 

### `fgets(char *buffer, int size, FILE *file)`

Reads at most size-1 characters into buffer, stopping if newline (included) is read and terminating the buffer with `\0`. 
Returns NULL on EOF or error.
Never use `gets()` it is unsafe!

The parameters: pointer to buffer, int size, FILE *file

In `ncat.c` it would read up to (99) characters, including the newline.

#### Example: reading the line `/*`

Let's say that `fgets()` is reading the above line.
This line has three characters `/` `*` `new line`.
It stops after reading 3 bytes and then terminates the buffer with NULL.

By the time that `fgets()` returns, you actually put 4 bytes into the buffer.
This is why `fgets()` only reads size-1 characters.

### `int fputs(const char *str, FILE *file)`

Simpler version of `printf()`, basially.
Takes a raw string and sends it to the file.

**UNIX operating systems will treat `stdin` `stdout` `stderr` the same way as files**
It thinks that screen, keyboard are files. Internally, writing to a file or writing to screen is the same thing, it's just that there is a special file for the terminal screen and if you write into it then it will show up on the screen. 

### `fclose(FILE *file)`

Closes the file. You must do this to prevent leaks. 

## Buffering






*updated L10/22, L10/27*
