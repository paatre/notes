# Networking basics

Networking is a field of a wide variety of topics. Within this note we focus on the basics of networking and its fundamental issues that we can identify. We go through the topics with a layered approach where networking is broken down into different layers so we can understand the different responsibilities of each part of the network stack and how they interact with each other.

Transport layer:

- reliable communication over an unreliable network layer
- connection establishment/teardown and handshaking
- congestion and flow control
- multiplexing

Network layer:

- determining "good" paths between two routers
- interconnecting a large number of heterogeneous networks
- managing the complexity of a modern network

Link layer:

- sharing a multiple access channel

Network security:

- condidentiality
- authentication
- mmessage integrity

## Resources behind this note

1. Computer Networking: A Top-Down Approach by Kurose and Ross (9th edition, 2025)
2. Computerphile's series on networking basics with Richard G Clegg: https://www.youtube.com/watch?v=eelvWAURfdI&list=PLzH6n4zXuckpvez0PrA5J2Wv--g_vyVs8&index=5

## Terminology

- Packet/datagram: a unit of data that is transmitted over a network. It contains the source and destination addresses, as well as the data payload.

## OSI ISO model

The OSI ISO model is a conceptual framework used to understand and design network systems. It divides the communication process into seven layers, each with specific functions and responsibilities. The layers are:

7: Application: provides network services to end-users and applications, such as email, web browsing, games, etc.
6: Presentation: responsible for data representation, encryption, compression, etc. (e.g., ASCII, JPEG, SSL/TLS)
5: Session: responsible for establishing, managing, and terminating communication sessions between applications
4: Transport: responsible for reliable data transfers, retransmissions, error handling, etc (TCP, UDP)
3: Network: responsible for routing, path selection, addressing, etc. (IP)
2: Data link: responsible for data delivery between adjacent nodes through network interfaces and firmware (Ethernet, Wi-Fi, etc.)
1: Physical: responsible for the physical transmission of data as binary data over the network medium, such as cables, radio waves, etc.

But when OSI ISO model was being developed during 1970s and 1980s, the TCIP/IP model was also already being developed and implemented by the US Department of Defense for ARPANET and DARPA projects. And because of the practical implementation and widespread adoption of TCP/IP, the OSI ISO model never gained much traction in the real world and is mostly used as a theoretical reference for understanding network communication.

## TCP/IP model

The TCP/IP model is a more practical and widely used model for network communication. It consists of four layers that correspond to the OSI ISO model as follows:

7: Application (combines OSI layers 5, 6, and 7)
4: Transport (corresponds to OSI layer 4)
3: Network (corresponds to OSI layer 3)
2: Link (corresponds to OSI layers 1 and 2)

### Transport layer

The transport layer is responsible for providing reliable data transfer between applications. It ensures that data is delivered to the correct destination, to the correct application, and in the correct order. The two main protocols used in the transport layer are TCP (Transmission Control Protocol) and UDP (User Datagram Protocol).

Ports are used to identify specific applications or services running on a host. They are 16-bit numbers that range from 0 to 65535. Ports 0 to 1023 are known as well-known ports and are reserved for specific services (e.g., HTTP uses port 80, HTTPS uses port 443, FTP uses port 21, etc.). Ports 1024 to 49151 are known as registered ports and can be used by applications that are not well-known. Ports 49152 to 65535 are known as dynamic or private ports and can be used by applications for temporary purposes.

#### UDP

RFC 768. Simplest possible transport protocol which provides data delivery. UDP works with datagrams which contain two parts: a header and a payload. The header contains the source and destination ports, as well as the length of the datagram and a checksum for error detection. The payload contains the actual data being transmitted. UDP is a connectionless protocol, which means that it does not establish a connection before sending data. Not having an established connection means that there is no guarantee of delivery, ordering, or error checking because there is no acknowledgement mechanism for the send to know if the data was received successfully. However, UDP is faster and more efficient than TCP because it has less overhead and does not require the establishment of a connection. It is commonly used for applications that require low latency and can tolerate some loss of data, such as video streaming, online gaming, and voice over IP (VoIP).

#### TCP

RFC 793. When applications between two hosts want to communicate with each other, they need to establish a connection through the internet. What you want is a reliable connection for that communication but the internet is an unreliable network and can lose packets, deliver them out of order etc.

#### Quick UDP Internet Connections (QUIC)

TBW
