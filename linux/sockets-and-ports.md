# Sockets and ports

## Sockets

A socket is an endpoint for communication between two machines. It is a combination of an IP address and a port number. Sockets are used to establish a connection between a client and a server.

Socket is actually a data structure on RAM that contains all the necessary information about a connection such as the local and remote IP addresses, port numbers, protocol being used, state of the connection, data buffers, sequence numbers, and more. When a socket is created, it is assigned a file descriptor which works as a mapping number that the operating system uses to keep track of the socket in the RAM. The file descriptor is used by applications to refer to the socket when performing operations such as sending or receiving data (e.g., `write/send` and `read/recv` system calls).

## ss

Use `ss` to list all the sockets on a machine. The output will show the local address (IP address and port number) and the remote address (IP address and port number) for each socket.

## Ports

A port is like an address for a specific service on a machine. It is a 16-bit number that ranges from 0 to 65535. Ports are used to identify specific services running on a machine. For example, port 80 is commonly used for HTTP traffic, while port 443 is used for HTTPS traffic.

When a client wants to connect to a server, it specifies the IP address and port number of the server's socket. The server listens for incoming connections on that port and establishes a connection with the client when it receives a request.

### portmon

`portmon` is a TUI application that allows you to monitor and manage network ports on your machine.
