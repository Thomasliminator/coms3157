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

### #include `<stdio.h>`

This is a directive that tells the compiler "please find the file `stdio.h` and put the content of the file in that line". This is textual replacement.

`printf()` is a function collected in `stdio` so it doesn't generate a warning when compiled.
- the prototype for `printf()` is included in `stdio.h`

Standard library functions are linked automatically. The `libc.a` is a standard C library file. `libm.a` is another standard C library file that stores math functions.  

## Makefiles and `make` tool

This does compilation, linking, and possibly other things, automatically. A programmable scriptor, automator. 

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

*edited L9/15*
