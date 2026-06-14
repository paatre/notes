# Networking basics

Networking covers a wide variety of topics. This note focuses on the foundational concepts and core issues of networking. 

We approach these topics using a **layered model**, breaking the network stack down into distinct layers to understand their individual responsibilities and how they interact with each other.

---

## Primary principles of networking

Before diving into the models, here is a snapshot of what each primary layer is trying to solve:

* **Transport layer:** Achieves *reliable* communication over an *unreliable* network layer. Handles connection handshakes, congestion/flow control, and multiplexing.
* **Network layer:** Determines "good" paths between routers, interconnects massive numbers of heterogeneous networks, and manages modern routing complexity.
* **Link layer:** Manages how multiple devices share a single physical communication channel.
* **Network security:** Ensures confidentiality, authentication, and message integrity across all interactions.

---

## How the internet works: a high-level overview

Internet is a global network of interconnected networks, and each network is made up of devices, hosts or end systems, that communicate with each other on the network and across the internet. The ends systems are connected together by a network of **communication links** and **packet switches**, and these links transmit data with the transimission rate which is measured in bits per second (bps).

The data that is transmitted is organized and segmented into smaller chunks called **packets**. Each packet contains header and the data segment called a payload. After the packet is received at the destination, the packet is reassembled into the original data.

A packet switch receives a packet through an incoming communication link and forwards the packet to an outgoing communication link. The two most prominent types of packet switches are **routers** and **link-layer switches**. Link-layer switches are typically used in access networks to connect end system to the network, while routers are typically used in the netowork core to interconnect different networks and forward packets across the internet. The sequence of communication links and packet switches that a packet traverses from source to destination is called a **route* or a **path**.

End systems access the internet through Internet Service Providers (ISPs) which provide connectivity to the internet. There are different types of ISPs, such as **Tier 1** (global backbone providers), **Tier 2** (regional providers), and **Tier 3** (local providers). Each ISP is like a local netowork that connects to other ISPs on the same tier or higher tiers depending where the data is going.

Devices on the internet run **protocols** that control the sending and receiving of data within the internet. The **Transmission Control Protocol (TCP)** and the **Internet Protocol (IP)** are two of the most important protocols in the internet. The IP protocol specifies the format of the packets that are sent and received among routers and end systems. The internet's principal protocols are collectively known as **TCP/IP**. 

It's important that every device on the internet can interoperate. This is achieved with standards. **Internet standards** are developed and published by the Internet Engineering Task Force (IETF) in the form of **request for comments (RFC)** documents. They tend to be technical and detailed, but they are the definitive source for how protocols and systems should work. They define protocols such as TCP, IP, HTTP and SMTP.

## Architectural models: OSI vs. TCP/IP

To design and understand network systems, we use conceptual frameworks. The two most prominent are the theoretical **OSI model** (7 layers) and the practical **TCP/IP model** (4 layers).

| OSI layer | OSI name | Main responsibility | Example protocols / standards | Corresponding TCP/IP layer |
| :---: | :--- | :--- | :--- | :--- |
| **7** | Application | Network services for end-user apps | HTTP, SMTP, DNS | Layer 4: **Application** *(Combines OSI 5, 6, 7)* |
| **6** | Presentation | Data formatting, encryption, compression | ASCII, JPEG, SSL/TLS | Layer 4: **Application** |
| **5** | Session | Managing communication sessions | NetBIOS, RPC | Layer 4: **Application** |
| **4** | Transport | End-to-end data transfer, error handling | **TCP, UDP** | Layer 3: **Transport** |
| **3** | Network | Routing, path selection, global addressing | **IP** | Layer 2: **Network** |
| **2** | Data link | Local node-to-node delivery, framing | Ethernet, Wi-Fi | Layer 1: **Link** *(Combines OSI 1, 2)* |
| **1** | Physical | Raw binary transmission over physical media | Cables, Radio waves | Layer 1: **Link** |



> **A historical note:**
> While the OSI model was being developed by committee in the 1970s and 1980s, the **TCP/IP model** was already being built and deployed by the US Department of Defense for ARPANET/DARPA projects. Because of its real-world implementation and rapid adoption, TCP/IP became the internet standard. Today, the OSI model is used primarily as a theoretical reference.

---

## TCP/IP model: deep dive

### Transport layer

The transport layer provides data transfer between applications running on different hosts. It ensures data reaches the correct destination application, in the correct order, and free of errors.

#### Port addressing
Ports are 16-bit numbers (ranging from `0` to `65535`) used to identify specific application processes running on a host.

| Port range | Classification | Purpose / examples |
| :--- | :--- | :--- |
| **0 – 1023** | Well-known ports | Reserved for standard services (e.g., HTTP: `80`, HTTPS: `443`, FTP: `21`) |
| **1024 – 49151** | Registered ports | Used by third-party applications and user-installed software |
| **49152 – 65535** | Dynamic / private | Temporary ports allocated on-the-fly by the OS for outbound client connections |

---

### Transport protocols

#### 1. UDP (User Datagram Protocol) — *RFC 768*

UDP is the simplest possible transport protocol. It operates using **datagrams** and provides a lightweight, "best-effort" delivery service.

* **The UDP header:** A minimal 8-byte header containing four fields: **source port**, **destination port**, **length**, and a **checksum** (used for basic error detection).
* **Connectionless nature:** UDP transmits data instantly without performing an initial handshake to establish a formal connection state.
* **The structural tradeoff:** Because it lacks an acknowledgement mechanism, UDP offers **no guarantees** regarding delivery confirmation, packet packet ordering, or error recovery. If a checksum fails at the destination, the packet is simply dropped. However, bypassing this overhead makes UDP exceptionally fast and low-latency.
* **Common uses:** Video streaming, online gaming, and Voice over IP (VoIP)—scenarios where immediate delivery matters more than an occasional lost frame.

#### 2. TCP (Transmission Control Protocol) — *RFC 793*

When applications on different hosts communicate, they typically require an assured, reliable connection. However, the underlying network layer (IP) only offers a best-effort delivery service that can drop, delay, or duplicate packets. TCP acts as an intelligent layer built over this unreliable foundation to provide an ordered, error-checked data stream.

*(Section details to be written)*

#### 3. QUIC (Quick UDP Internet Connections)
*(Section details to be written)*

---

## Terminology

* Host / end system: a device that is connected to a network and can send or receive data (e.g., computers, smartphones, servers).
* **Packet / datagram:** A fundamental unit of data formatted for transmission over a network. It consists of control information (such as source and destination addresses) and user data (the payload). Packet is usually talked in the transport layer, while datagram is usually talked in the network layer.

## Resources behind this note

1. **Book:** *Computer Networking: A Top-Down Approach* by Kurose and Ross (9th edition, 2025)
2. **Video:** [Computerphile's series on networking basics with Richard G Clegg](https://www.youtube.com/watch?v=eelvWAURfdI&list=PLzH6n4zXuckpvez0PrA5J2Wv--g_vyVs8&index=5)
