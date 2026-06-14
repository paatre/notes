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

* **Packet / datagram:** A fundamental unit of data formatted for transmission over a network. It consists of control information (such as source and destination addresses) and user data (the payload).

## Resources behind this note

1. **Book:** *Computer Networking: A Top-Down Approach* by Kurose and Ross (9th edition, 2025)
2. **Video:** [Computerphile's series on networking basics with Richard G Clegg](https://www.youtube.com/watch?v=eelvWAURfdI&list=PLzH6n4zXuckpvez0PrA5J2Wv--g_vyVs8&index=5)
