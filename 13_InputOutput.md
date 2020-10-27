# 13. Input/Output

## Standard I/O

C libarary provides 3 I/O channels:

`stdin` (standard input)

`stdout` (standard output)

`stderr` (standard error)

`<stdio.h>` contains prototypes for standard I/O functions such as `printf()` and `scanf()`.

### Redirection

We can make a `myinput` file and then run the command `./a.out < myinput` to redirect the standard input. 
This changes the standard input of `./a.out` so that it comes from the file rather than reading from the keyboard.

Can also do `./a.out < myinput > myoutout` to 

`>` will override the file (deletes everything). Use `>>` to append instead. 

Use `2>` to redirect `stderr`. `2>&1` means redirect `stderr` to the same place `stdout` is going to. 

### Pipeline

`cat myinput | ./a.out` the `stdout` of the first goes directly to the `stdin` of the next. The effect is the same as the command  `./a.out < myinput`. 

### Formatted I/O

Things such as `scanf()` `printf()`.
- there are a ton of options: 

## File I/O

### Buffering






*updated L10/22, L10/27*
