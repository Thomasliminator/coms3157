# 15. Sockets API and HTTP

## UNIX I/O Using File Descriptors

There are `read()` `write()` and `open()` functions that are unique to UNIX. 

`int open()`: returns an `int` type rather than a `file *` type.
These numbers are called file descriptors: uses this instead of a file pointer. They are small integers representing open files.

When a program starts, kernel opens 3 files without being asked, `stdin`, `stdout`, `stderr` which are on the descriptors 0, 1, 2.
Subsequent open files are given 3, 4, 5, ...

Everything is unbuffered.

File descriptors are used for sockets.

## Endianness

See `endian-demo.c` for sample code.

Intel CPUs store numbers backwards in memory. The LSByte of the number is in the first byte.
This is called little-endian.

Little-endian: stored backwards (host byte order)
Big-endian: stored naturally (network byte order)

`htonl()` will convert a number to big-endian number.

Always convert into network-byte order (big-endian) then write into file so that another computer can read it. 

`ntohl()` is the reverse function that will convert to little-endian number. 

## Sockets API Basics

We can use these functions for networking.

### Client Server Model

In any communication, one computer is the client the other is the server.
Server waits for incoming requests over the network from clients. 

`dig` is a command where you can find the IP address of a machine.

### What is a Socket?

Socket is the end point of a TCP connection.
Socket has two things, the IP address and the port number.

### API Summary

Server calls `socket()` which returns an int, the file descriptor.
Server then calls `bind()` which passes a port number to bind the socket with the chosen port number.
Server then calls `listen()` to start listening.
At this point, the server can start accepting connection request from the client.

On the client side, it calls `socket()` and `connect()`.
You must pass the IP address and port number of the server to `connect()` so that the client knows where to go.

When the connection request comes in, it starts to queue up on the server side. When the server is ready to start communication it calls `accept()`. 

If there is a client, then `accept()` will return with the full connection.

Then, we can do `send()` and `recv()` to have two way communication.

### Listening vs. Connected Socket

When we fully connect, `accept()` returns a new socket that is fully connected to the client.

## IPv4 Address Strcutures

```C
struct sockaddr {
    sa_family_t sa_family;
    char sa_data[14];
};

struct sockaddr_in {
    sa_family_t sin_family; //address family: AF_INET
    in_port_t sin_port; //port in network byte order
    struct in_addr sin_addr; //internet address
};

struct in_addr {
    uint32_t s_addr; //address in network byte order
};
```

## tcp-echo-client.c

### `socket()` to create a socket for TCP connection

`sock = socket(AF_INET, SOCK_STREAM, 0)`

### `connect()` establishes TCP connection to the server

`connect(sock, (struct sockaddr *) &servaddr, sizeof(servaddr))`

### `send()` is used to send to the socket

`send(int socket, const void *buffer, size_t length, int flags)`
`send(sock, buf, len, 0)`

Normally, `send()` blocks until it sends all bytes requested.
Returns num bytes sent or -1 for error.
This is equivalent to `write(sock, buf, len)`.

### `recv()` to recieve from the server and print

`recv(int socket, void *buffer, size_t length, int flags)`
`recv(sock, buf, sizeof(buf), 0)`

Normally, `recv()` blocks until it has received at least 1 byte
Returns num bytes recieved, 0 if connection closed, -1 if error
This is equivalent to `read(sock, buf, len)`.

## tcp-echo-server.c

`socket()` call is exactly the same as in the client. (server socket/listening socket)

### `bind()` to the local address

`bind(servsock, (struct sockaddr *) &servaddr, sizeof(servaddr))`

### `listen()` for incoming connections

`listen(servsock, 5 /* queue size for connection requests */ )`

### `accept()` to accept an incoming connection

`accept(servsock, (struct sockaddr *) &clntaddr, &clntlen)`

Takes the original servsock and will return a brand new socket.
The new one is the one we use for communication.

## HTTP 1.0

The protocol for web communication. 

Client sends an HTTP request for a resource on the server, server sends a HTTP response.

*See HTTP request/response example on the sockets-http slides.*

By default, port number 80 is used by default for the web server.
- we can use netcat and connect to port 80 to act like a web browser
- the browser will send `GET / HTTP/1.1`

## tcp-sender.c

The sender first sends file size as a 4-byte unsigned int in network byte order.
- `stat` is a command that will return file information into a `struct stat`.

Secondly, send the file content.
- For `fread` we want to read 1 byte at a time `sizeof(buf)` times, not the opposite. 

Thirdly, receive fie size back from the server as acknowledgement.

## tcp-recver.c

Firstly, receive file size.

Secondly, receive the file content.
- `limit = remaining > sizeof(buf) ? sizeof(buf) : remaining;` is called a tertiary expression.
- means that if `remaining > sizeof(buf)` then `limit = sizeof(buf)`; else, `limit = remaining`. 

Thirdly, send the file size back as acknowledgement.
- again, use `stat` command to send the file size back.
- server will close connection here; then, will go back to the top of the infinite loop.




*edited L11/12, L11/17, L11/24*
