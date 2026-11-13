# Transport Layer

The **Transport Layer** is the fourth layer of the OSI model and provides end-to-end communication between applications running on different devices.

It operates above the Network Layer and below the Session Layer in the OSI model.

The Transport Layer is responsible for:

* Application-to-application communication.
* Data segmentation and reassembly.
* Port-based process identification.
* Reliable data delivery when required.
* Flow control.
* Congestion control.
* Error detection and recovery mechanisms.

The two primary transport-layer protocols are:

* **TCP (Transmission Control Protocol):** Provides reliable, connection-oriented communication.
* **UDP (User Datagram Protocol):** Provides connectionless, best-effort communication with minimal protocol overhead.

Understanding the Transport Layer is essential for web development, backend engineering, network administration, and distributed systems.

# 🗁 Table of Contents

The Transport Layer section follows this learning sequence.

| File | Topic | Main Focus |
| --- | --- | --- |
| `01-tcp.md` | TCP | Connection-oriented communication and reliability |
| `02-udp.md` | UDP | Connectionless datagram communication |
| `03-ports.md` | Ports | Port numbers and application identification |
| `04-sockets.md` | Sockets | Network communication endpoints |
| `05-tcp-handshake.md` | TCP Handshake | Connection establishment |
| `06-tcp-connection-termination.md` | TCP Connection Termination | Connection closing and TCP states |
| `07-tcp-flow-control.md` | TCP Flow Control | Receiver protection and window management |
| `08-tcp-congestion-control.md` | TCP Congestion Control | Network congestion management |
| `09-tcp-vs-udp.md` | TCP vs UDP | Protocol comparison and use cases |
| `10-common-port-numbers.md` | Common Port Numbers | Frequently used transport-layer ports |

# 🗁 Prerequisites

Before studying this section, you should understand:

* IP addressing.
* IPv4 and IPv6.
* Network-layer packet delivery.
* Subnet masks and CIDR.
* Default gateways.
* Basic client-server architecture.
* The OSI and TCP/IP models.

Recommended previous section:

`07-networking/03-ip-addressing/`

# 🗁 Learning Objectives

After completing this section, you should be able to:

* Explain the purpose of the Transport Layer.
* Understand TCP and UDP architecture.
* Explain the difference between segments and datagrams.
* Understand transport-layer port numbers.
* Identify source and destination ports.
* Understand sockets and socket pairs.
* Explain TCP connection establishment.
* Understand TCP connection termination.
* Explain TCP reliability mechanisms.
* Understand TCP flow control.
* Understand TCP congestion control.
* Compare TCP and UDP.
* Identify common transport-layer ports.
* Troubleshoot transport-layer communication problems.
