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
The CLAC account is configured to use Bash shell by default.

Type `echo $SHELL`. It should say `/bin/bash`.

There is something that needs to be added to the `.bashrc` file in the home directory to set up the shell environment. Add the following line to the end of the `.bashrc` file.

    export EDITOR=vim
    
Log out of CLAC by typing "exit" and then log in again. Type `echo $EDITOR` to see if the modification has taken effect. If it hasnt, then add the following lines to the `.bash_profile` file:

```
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi
```

*edited L9/10, 9/14*
