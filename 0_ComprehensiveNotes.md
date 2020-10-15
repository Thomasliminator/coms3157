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
# 4. Git Lesson

## Setting up Git

All the following is in the context of *lab submission*. 

### Setting up git directory in home directory

`git init` initializes a directory as a git repository.
Do not run this for the labs in the homework. You must use `git clone` instead.

### Staging

`git diff` compares the current version of the file with the version that's in the staging area.

Three versions of README
- A. README in git repo --> committed
- B. README in staging --> to be committed
- C. README in working directory

`git status` shows the status of the files being tracked/untracked by git.
`git log` shows commit log for git. 

`git diff` shows the difference between B and C. 
`git diff --cached` will show the difference between A and B.

`git commit` will pop up a Vim editor to put a commit message.
`git commit -m "[...]"` can be used to commit a single line message. 

### Git Workflow

You may be implementing one small feature but it requires you to modify many files. You want to work carefully but don't want to make 20 commits.
In this case, you will make a change to a file, `git diff` to show the change, `git add` to put it to staging. Collect all of the changes to staging and then you can commit after the feature is committed.

Type the following to submit a lab:
`/home/w3157/submit/submit-lab lab[N]`

### Other Commands in Git

If you want to remove something, you shouldn't do `rm` but you should do `git rm` which will remove it from the directory and the git repository.



*edited L9/22 L9/24*
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
# 7. Git Tutorial

Git is a source code version control system. It is useful to track changes in code.
Git is required for the labs in 3157.

## Configuring Git for COMS W3157

### Set `EDITOR` envionment variable

Type `echo $EDITOR` in the terminal. It should say `vim`. 

If it doesnt, add the following line to the end of the `.bashrc` file in the home directory:
`export EDITOR=[editor]`

### Configuring your Git Environment

We first need to tell git name and email. This information is stored in `~/.gitconfig`
    `git config --global user.name "your full name"`
    `git config --global user.email uni@columbia.edu`
    
### Creating a Project

Create a new directory ~/tmp/test1

Put the directory under git revision control:
    `git init`

`ll` will now show that there is a `.git` directory. The git repository for the current directory is stored here. 

Write and run a `hello.c` program.

`git status` will show that we have `a.out` and `hello.c` that are "Untracked". Let's move `hello.c` to the staging area using `git add hello.c`

Now, let's commit it: `git commit`. Include a one line commit message when prompted.

### Modifying Files

Modify `hello.c` to print "bye world" and run `git status`. It reports that the file is "changed but not updated." 

Before moving to staging, we can see what we changed in the file using `git diff` or `git diff --color`. 
This tells us that we took out the "hello world" line and added a "bye world" line. 

Move this to the staging area using `git add`. In git, "add" means this: move the change to the staging area. This could be a modification to a tracked file or the creation of a new file.

At this point, `git diff` will report no change. Our change has been moved to staging. `git diff` reports the difference between the staging area and the working copy of the file. To see the difference between the last commit and the staging area, use `git diff --cached`

Commit again with a one liner message: `git commit -m "changed hello to bye"`

To see commit history: `git log`

You can add a brief summary of what was done at each commit: `git log --stat --summary`

You can see the full diff at each commit: `git log -p` or `git log -p --color`

### The tracked, the modified, and the staged

A file in a directory under git revision control is either tracked or untracked. A tracked file can be unmodified, modified but unstaged, or modified and staged. 

There are four possibilities for a file: 

1. Untracked
    Object files and executable files that can be rebuilt are usually not tracked.
2. Tracked, unmodified
    The file is in the git repository, and it has not been modified since the last commit. "git status" says nothing about the file.
3. Tracked, modified, but unstaged
    You modified the file, but didn't `git add` the file. The change has not been staged, so it's not ready for commit yet.
4. Tracked, modified, and staged
    You modified the file, and did `git add` the file. The change has been moved to the staging area. It is ready for commit.
    
The staging area is also called the "index".

### Other Useful Git Commands

To rename a tracked file: `git mv old-filename new-filename`

To remove a tracked file from the repository: `git rm filename`

The mv or rm actions are automatically staged, but you still have to commit the actions.

If you make changes and want to go to the version last committed (the file must not be staged yet), then you can do: `git checkout -- filename`

If the file has been staged, you must unstage it first `git reset HEAD filename`

To pull up a manual page or a command, for example `git status`:
    `git help status` or
    `man git-status`

Search for specified patterns in all files in the repository: `git prep printf`

### Cloning a Project

Often times, a programmer starts with an existing code base. When the code base is under git version control, you can *clone* the repository.

Let's clone test 1 into test 2: `git clone test1 test2`

If we run `git log` you will see that the entire commit history is replicated here. The two repositories are indistinguishable after cloning.

You can run `git log origin` to see commit history after the clone.

### Generating a Patch Set

You can save the full details of everything done after cloning with: `git format-patch --stdout origin > mywork.mbox`

If you open `mywork.mbox` with editor, you will see that the file contains the full diffs of all commits. A diff is also called a patch. The file therefore contains a set of patches, one for each commit after cloning.

When you were looking at the mywork.mbox file in your editor, you might notice that each patch looks like an email message. That's because they are. `.mbox` is the UNIX mailbox format so it can be viewed using an email application. 

Use mutt, a terminal based email app, to view the file. Type `q` to exit out of mutt.
    `mutt -f mywork.mbox`
    
If you want to read the file with the diffs in color, run the following command. Use the arrow keys to scroll and `q` to quit. 
    `cat mywork.mbox | colordiff | less -R`
    
The patch file is what you submit when you complete a lab assignment. 

### Applying the Patch Set

The TAs will apply the patch to the original repository to grade the work. Let's go through that process.

Clone test1 into test 3. Then run `git log` to verify you have commits made in test1 but not the ones made in test2. 

Now, apply the patch using: git am ../test2/mywork.mbox

### Adding a Directory into your Repository

After the deadline, a solution will be added. Let's simulate that process. 

```
cd ../test1
mkdir solution
cd solution

cp ../hello.c .
echo 'hello:' > Makefile
```

Two files, `Makefile` and `hello.c` have been created in the solutions directory.
Now, commit the new directory.

### Pulling Changes from a Remote Repository

To view solutions, you have to pull the changes to your repository. Let's pull changes we just made in test1 into test2:

    cd ../test2/
    git pull

This looks at the original repository that you cloned from, fetches all changes made since clone, and merges the changes into the current repository.

## Learning More about Git

You can view the official git tutorial in `man gittutorial`

You can also go to the [Official Git Documentation](http://git-scm.com/documentation)

*edited T9/27*
# 8. Vim Tutor

## Lesson 1

### Lesson 1.1: Moving the Cursor

To move the cursor, press the h, j, k, l keys as follows:
- `j` moves down
- `k` moves up
- `h` moves left
- `l` moves right

You can hold down the keys until it repeats.
You can also use the normal arrow keys but "HJKL" is much faster.

If you ever type a command incorrectly, press `<ESC>` to go into Normal mode.

### Lesson 1.2: Exiting Vim

Press the `<esc>` key to make sure you are in Normal mode. 
Type `:q!` to exit and **discard** any changes that have been made. 

### Lesson 1.3: Deleting Text

Press `x` to delete the character under the cursor.

### Lesson 1.3: Inserting Text

Press `i` to insert text.

### Lesson 1.4: Appending Text (adding text to the end of the line)

Press `<shift> a` or `A` to append text.

### Lesson 1.5: Editing a File

Use `:wq` to save a file and exit.

## Lesson 2

### Lesson 2.1-2: Deletion Commands

Type `dw` to delete a word.

Type `d$` to delete to the end of the line. 

### Lesson 2.2: On Operators and Motions

Many commands that change text are made from an operator and a motion.

The `d` operator has this short list of (non comprehensive) motions:
- `w` until the start of the next word, EXCLUDING its first character.
- `e` to the end of the current word, INCLUDING the last character.
- `$` to the end of the line, INCLUDING the last character.

### Lesson 2.4: Using a Count for a Motion

Typing a number before a motion repeats it that many times.

So, `2e` goes to the end of two words forward. `2w` goes to the beginning of two words forward.
`0` goes to the start of the line.

### Lesson 2.5 Using a Count to Delete More

Typing a number with an operator repeats it that many times.

We can do something like `d2w` to delete two words.

### Lesson 2.6: Operating on Lines

Type `dd` to delete an entire line.
We can also do something like `2dd` to delete two lines.

### Lesson 2.7: The Undo Command

Type `u` to undo the last commands, `<shift>U` to fix a whole line.

Type `CTRL-R` to redo commands.

## Lesson 3

### Lesson 3.1: The Put Command

Type `p` to put previously deleted text after the cursor.

### Lesson 3.2: The Replace Command

Type `r[x`] to replace the character at the cursor with [`x]`.

### Lesson 3.3: The Change Command

To change until the end of a word, type `c[e]`

### Lesson 3.4: More Changes Using `c`

The change operator is used with the same motions as delete.
-`w` word
-`$` end of line

## Lesson 4

### Lesson 4.1: Cursor Location and File Status

Type `CTRL-G` to show location in the file and the file status. Type `G` to move to a line in the file.

`<shift> g` or `G` will go to the bottom of the file.
`gg` will go to the top of the file.
`[line number]<shift>g` will go to the specified line number.

### Lesson 4.2: Search Command

Type `/` followed by a phrase to search for the phrase. Use `?` to search in the backwards direction.

Type `n` to search in the forward direction.
Type `<shift>n` to search in the backwards direction.

To go back to where you came from, press `CTRL-O`. `CTRL-I` goes forward.

### Lesson 4.3: Matching Parenthesis Search

Place cursor on a (, [, { and type `%` to find the matching closing character.

### Lesson 4.4: The Substitute Command

Type `:s/old/new/g` to substitute "new" for "old". The `g` flag at the end means that it is global for that line. It may be omitted.

`:#,#s/old/new/g` where $,$ are the line numbers of the range of lines where the substitution is to be done.
`:%s/old/new/g` to change every occurence in the whole file.
`:%s/oild/new/gc` to find every occurence in the whole file with a prompt whether to substitute or not.

## Lesson 5

### Lesson 5.1: How to Execute an External Command

Type `:![command]` to execute that command.
The commands are finished by hitting `<ENTER>`.

### Lesson 5.2: More on Writing Files

Type `:w [FILENAME]` to save the changes made to the text.

### Lesson 5.3: Selecting Text to Write

To save part of the file type `v [motion]` `:w [FILENAME]`

For example, `:'<,'>w TEST` is what you might see.

### Lesson 5.4 Retrieving and Merging Files

To insert the contents of a file, type `:r [FILENAME]`

You can also read the output of an externam command. For example `:r !ls` reads the output of the `ls` command and puts it below the cursor.

## Lesson 6

### Lesson 6.1: The Open Command

Type `o` to open a new line below the cursor and be put in Insert mode. Do `<SHIFT>O` to open up a line above the cursor.

### Lesson 6.2: The Append Command

Type `a` to insert text after the cursor.

Remember, `<shift>A` is used to append at the end of a line.

### Lesson 6.3: Another Way to Replace

Type `<shift>R` to replace more than one character.

### Lesson 6.4: Copy and Paste Text

Use the `y` (yank) operator to copy text and use `p` to paste it. 

`y` can be used as an operator, for example, we can `yw` to yank one word.

### Lesson 6.5: Set Option

Set an option so a search or substitute ignores case.

`:set ic` will ignore case.
`:set hls is` sets the hlsearch and incsearch options.
`:set noic` will disable ignoring case

If you want to ignore case for just one search command, use `\c` in the phrase.
- `/ignore\c <ENTER>`

## Lesson 7

### Lesson 7.1: Getting Help

Vim has a comprehensive online hel system. Try one of these three:
- press `<HELP>` key
- press `<F1>` key
- type `:help <ENTER>`

Type `CTRL-W CTRL-W` to jump from one window to another.
Type `:q` to quit from the help window.

You can get help on a specific subject:
-`:help w`
-`:help c_CTRL-D`
-`:help insert-index`
-`:help user-manual`

### Lesson 7.2: Create a Startup Script

To use more Vim features, you have to create a "vimrc" file.

`:e ~/.vimrc` to edit the vimrc file.
Read example contents using `:r $VIMRUNTIME/vimrc_example.vim`
Write the file with `:w`

### Lesson 7.3: Completion

Command line completion with `CTRL-D` and `<TAB>`

Make sure Vim is not in compatible mode: `:set nocp`
Look what files exist in the directory `:!ls`
Type the start of a command `:e`
Press `CTRL-D` and Vim will show a list of commands that start with "e".
Type `d,TAB>` and Vim will complete the command name to `:edit`.
Now add a space and the start of an existing file name: `:edit FIL`
Press `<TAB>` Vim will complete the name if it is unique.

*edited T9/27*
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
In `struct` you cannot add a method in C. 

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

x1 = (*q1).x;
x1 = q1->x; //how to access through the pointer

printf("%lu\n", sizeof(p1)); //prints 16 
}
``` 

This represents a point in a plane. You can access individual items with `p1.x` just like in Java.

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

Create a node on the heap using `malloc()`.

`assert(argc == 1)` doesn't do anything if the condition is true. If it is not true, it stops the program. It is a debugging aid. Anything can be inside of the condition.
- You can do a short hand `assert(node)`.

When freeing memory for 2 nodes, free the second one first then free the first. Look at code for `struct Node *create2Nodes(int x, int y)` method.
