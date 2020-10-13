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
