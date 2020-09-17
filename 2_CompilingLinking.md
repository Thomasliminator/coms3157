# 2. Vim Tutorial and C Basics

## Vim Tutorial

### Controls in Vim

`:set number` will display line numbers.
`shift g` will go to the end of the file.
`gg` goes to the beginning of the file.
`:wq` to quit out of Vim. 
`/` will allow you to search for something
- `n` lets you go to the next term

`cp file1 file2` will copy file1 onto file2 and you can use this to restart to correct an error.

`o` will allow you to add a line right after the current line. 

## C Basics, Compiling and Linking

### See `hello.c` in CLAC.

Every program in C has a method called `main()`.

`return 0` from main means that "everything went well". It is a C convention. 

`printf` is formatted printing: `%d` will look at the next value and subsitute, `\n` means new line.

`gcc` is the compilier. In the textbook, the compiler is `cc`. 
- swap all occurences of `cc` with `gcc` in the textbook.
- this is not how you usually compile unless you are doing something really simple. 

After compiling the program, you need to open the `a.out` file that will show the output.

Two things must be done now: compiling and **linking**. 
- `gcc` does both compiling and linking at the same time.

### Compiling and Linking, Step by Step

#### Step 1: Compiling

`gcc - c hello.c` does only the compilation
- Now we have a `hello.o` file which is called an "object file".
- this is not the program itself, it is a part of a program: it cannot be run

#### Step 2: Linking
`gcc hello.o -o hello`
- give it the object file that will produce the executable file
- `-o` will name the output file

Run `hello`

### `myadd()` function added to `hello.c`



### Compiling Multiple Buffers



### #include `<stdio.h>`

This is a directive that tells the compiler "please find the file `stdio.h` and put the content of the file in that line". This is textual replacement.

`printf()` is a function collected in `stdio` so it doesn't generate a warning when compiled.
- the prototype for `printf()` is included in `stdio.h`

Standard library functions are linked automatically. The `libc.a` is a standard C library file. `libm.a` is another standard C library file that stores math functions.  

### `make` tool

This does compilation, linking, and possibly other things, automatically. A programmable scriptor, automator. 

A   `Makefile` has the format:
- what you want to produce `main.o` (target)
- what you need to produce that file `main.c` (prerequisite)
- how to produce `gcc -c -Wall -g main.c` (recipe)

```
main.o: main.c
    gcc -c -Wall -g main.c
```

