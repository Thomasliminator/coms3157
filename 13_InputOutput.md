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






*updated L10/22, L10/27, L11/5*
