# 2. Vim Tutorial and C Basics

## Vim Tutorial

### Controls in Vim

`:set number` will display line numbers.
`shift g` will go to the end of the file.
`:wq` to quit out of Vim. 

`cp file1 file2` will copy file1 onto file2 and you can use this to restart to correct an error.

`o` will allow you to add a line right after the current line. 

## C Basics

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

