# 14. UNIX Processes, Shell

## Numeric IDs in UNIX

In UNIX, most of the IDs are numerical. 

You will see that files that have a user and group owner. If you do `ll -n` it will show you the underlying number for the User and Group ID.

`ps` command lists the processes that are running. 
- `ps au` will list for all users

In the second column, there is something called PID. Each process has a unique Process ID. 

## `fork` and `exec`

 Look at the source code `fork-example.c` and `shell.c`.

### `fork-example.c`

We have two for loops, one that prints "listen to me" every second and the other that prints "no way" every second. The first loop is if `p > 0` and the other is if `p == 0`.

`fork()` returns some number that is positive, zero. If it returns a negative number it is a fail. 

If we run the program, the two for loops run together. Two lines are getting printed every second.
The order is weird, sometimes it is flipped. 

When `fork()` is called, it goes deep into the OS and before it comes back, the OS pauses the process and duplicates the entire process into another process (clone).
Now you have two processes of a program running. 

`fork()` returns two different values depending on if it was the original process or the new process. 

The parent process will have the same PID, but the child process will have a new, unique PID. 

`fork()` returns 0 in the child process.
`fork()` returns the new PID of the child. 

If you do `ps auf` it will show the processes in a tree graphic. 

### `shell.c`

This program is a very small shell.

You can run programs such as:
- `/bin/ls`
- `/usr/bin/cal`
- `/bin/date`
- `/bin/bash`

Running bash inside this will run it inside of the AP shell.

There is a while loop with `fgets()` which allows you to type stuff into the buffer.

`waitpid(pid, &status, 0)` will pause the process until the process identified by the PID terminates. It will put the return value into the integer variable. `0` means normal behavior.

`execl(buf, buf, (char *)0)` you pass the program that you want to run. The second parameter (you can have multiple paramters). These parameters are the `argv` array.
The last parameter should be null pointer. 
This program, the execution function, runs a program.

`execl()` will clean the child process and will load a different program into the same process (replace the code and reset the heap, stack) and will start up a new program in place what you were. 
`execl()` is not supposed to come back: if it comes back that means it failed.

This is how the real bash program works as well. 

## Netcat program

Creating a chat session between CUNIX and CLAC servers.

`nc -l 10000`
- `-l` is on server or listening mode. This must be run first.
- 10000 is an arbitrary port number, we are listening at port 10000.
`nc clac.cs.columbia.edu 10000`
- this is running on client mode.

This is establishing a TCP (Transport Control Protocol) connection between two programs, a server and client program.

Reads `stdin` of one side and puts it in `stdout` of the other side.

### Remote mdb-lookup using netcat

Run the `mdb-lookup` program from CLAC but interact with it from CUNIX.

`./mdb-lookup-cs3157 < mypipe | nc -l 10000 > mypipe`
This does not work. The file `mypipe` is a regular file that doesn't work as the intermediary because we don't know which program runs first.

You can `mkfifo` to create a named pipe. It acts as a conduit that we can read and write to.

Now, the above command will be a circle, loop with our named pipe. It will allow us to `mdb-lookup` on CUNIX. 

A more intuitive way:
`cat mypipe | mdb-lookup | nc -l 10000 > mypipe`

## Internet Protocol

L5: Application Layer
L4: Transport Layer
L3: Network Layer
L2: Link Layer
L1: Physical Layer

There is a set of functions, Sockets API, that lets you send sequence of bytes from one computer to another across the internet. (In between L4 and L5)

L4 is a critical layer: below that you don't really worry about in application program.

HTTP is the protocol for the web. SMTP is the protocol for email. SSH, NTP are other protocols.

### Descriptions of the Layers

L1: Physical Layer
- cables, Ethernet jacks, voltage, frequency, amplitude
- electrical engineering stuff (who cares)

L2: Link Layer
- defines everything you need to send a bunch of bytes, called *frame*, from one device to another using a certain physical medium
- protocols: WiFi, Ethernet, LTE

L3: Network Layer
- the layer that makes sending bytes to another random computer on the internet possible. Called IP, internet protocol.
- a bunch of bytes in this layer is called *packet*
- IPv4 and IPv6, in transition to the latter.
- every IP address has four numbers, such as 128.59.0.27
- organizations may own some prefixes, for example Columbia owns 128.59.x.x

L4: Transport Layer
- gives you guaranteed delivery and of the exact stream without error
- deals with congestion
- gives the application layer a clean pipeline

## File Permissions in UNIX

If you look at the files, you can see file permissions on the first column.
There are 10 characters, for example, `-rw-r--r--`.

The first character tells us if it is a directory, denoted `d`, or not, denoted `-`.

The following nine characters are file permissions:
- The first three characters are the permissions for the owner of the file.
- The next three characters are the permissions for the group.
- The last three are the permissions for "other", everyone else.

We have read, write, and execute permissions: `rwx`.

For `mdb-add-cs3157`, we have `s` instead of `x`. 
The s permission has a `set-uid` permission that means if someone else runs the program it runs it as if the owner runs the program. 

The permission strings can be expressed in numbers.
Each one is a single integer in binary. 
Each set of 3 bits are converted into an integer which is the representation. 
These are called octal numbers. 

#### Example
`rw-r--r--` is 110 100 100 is 6 4 4

`chmod` is the change mode function; allows you to change modes.
- example usage: `chmod 700 ~`

The special permission is denoted by a 1, you add it to the beginning (left) of the number.

## Lab 5 Overview

Shell script is a text file with a bunch of commands.
If you have a file such as `myscript`, then you can run it by running the command `bash myscript`.

Give it execute permission with `chmod 755 myscript`. You can now run `myscript` as an executable. 

What you need to do is you need to have the first line that says `#!/bin/bash`. 
This is the name of the program you would like to invoke to run the commands. This first line acts as a directive to the command line.





*edited L11/5, NL10/17, NL10/22*
