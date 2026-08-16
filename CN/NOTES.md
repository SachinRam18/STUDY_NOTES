# COMPUTER NETWORKS — COMPLETE NOTES

> **Covers:** College Exams | Placement Preparation | Technical Interviews | Quick Revision

---

# SECTION 1 — NETWORKING FUNDAMENTALS

---

## 1. Computer Network

### Definition

A **computer network** is a collection of **two or more devices (computers, servers, phones)** connected together to **share data and resources**.

### Need / Purpose

- **Resource Sharing** → Share printers, files, internet connections.
- **Communication** → Email, messaging, video calls.
- **Data Storage** → Centralized servers; remote storage.
- **Cost Reduction** → Share expensive hardware across users.

### Types of Networks

| Type | Full Form | Coverage | Example |
|------|-----------|----------|---------|
| **PAN** | Personal Area Network | ~10 meters | Bluetooth, USB |
| **LAN** | Local Area Network | Building / Campus | Office Wi-Fi, Ethernet |
| **MAN** | Metropolitan Area Network | City | Cable TV network |
| **WAN** | Wide Area Network | Country / World | Internet, leased lines |

### In One Line

> **A computer network connects devices to share data, resources, and services.**

---

## 2. Internet

### Definition

The **Internet** is a **global network of interconnected networks** that uses the **TCP/IP protocol suite** to communicate.

### Key Points

- Made up of millions of private, public, academic, and government networks.
- Data travels as **packets** across routers.
- **WWW (World Wide Web)** is a service on the Internet — not the Internet itself.
- Governed by standards from **IETF, ICANN, IEEE**.

### In One Line

> **The Internet is the global network of networks communicating via TCP/IP.**

---

## 3. Client and Server

### Definition

- **Client** → A device or program that **requests** a service or resource.
- **Server** → A device or program that **provides** a service or resource.

### Client-Server Model

```text
Client                    Server
  │──── Request (e.g., HTTP GET) ────→│
  │←─── Response (e.g., HTML page) ───│
```

### Key Points

- **Client** initiates communication; **Server** listens and responds.
- A single machine can act as both client and server.
- **P2P (Peer-to-Peer)** → Every node acts as both client and server. Example: BitTorrent.

### In One Line

> **Client requests, server responds — the fundamental model of networked communication.**

---

## 4. Network Protocol

### Definition

A **network protocol** is a **set of rules and conventions** that define how data is **formatted, transmitted, received, and acknowledged** between devices.

### Why Needed

- Without protocols, devices from different manufacturers cannot communicate.
- Protocols define **format, timing, sequencing, and error handling**.

### Examples

| Protocol | Purpose |
|----------|---------|
| **HTTP/HTTPS** | Web browsing |
| **TCP** | Reliable data transfer |
| **UDP** | Fast, unreliable transfer |
| **IP** | Addressing and routing |
| **DNS** | Domain name to IP mapping |
| **SMTP** | Email sending |
| **FTP** | File transfer |

### In One Line

> **A protocol is a set of rules that allows devices to communicate — the language of networks.**

---

## 5. Packet

### Definition

A **packet** is a small unit of data transmitted over a network. Large data is **broken into packets**, sent independently, and **reassembled at the destination**.

### Packet Structure

```text
┌────────────┬──────────────┬────────────┐
│  Header    │   Payload    │  Trailer   │
│(src/dst IP,│ (actual data)│ (checksum) │
│ seq. no.)  │              │            │
└────────────┴──────────────┴────────────┘
```

### Key Points

- **Packet switching** → Packets take independent paths; reassembled at destination.
- **Header** → Contains source/destination address, sequence number, protocol info.
- **Payload** → Actual data being transmitted.
- Smaller packets → Better error detection; less retransmission overhead.

### In One Line

> **A packet is a small chunk of data with a header (addressing info) and payload (actual data) sent across a network.**

---

## 6. Port

### Definition

A **port** is a **logical endpoint** for communication, used to identify a specific **process or service** on a host. Identified by a 16-bit number (0–65535).

### Port Ranges

| Range | Type | Description |
|-------|------|-------------|
| 0–1023 | **Well-known ports** | Reserved for standard services |
| 1024–49151 | **Registered ports** | Assigned by IANA to applications |
| 49152–65535 | **Dynamic/Ephemeral ports** | Used by clients temporarily |

### Common Port Numbers

| Port | Protocol |
|------|---------|
| 20, 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 3389 | RDP |

### In One Line

> **A port is a logical endpoint that identifies a specific service on a host — IP identifies the host, port identifies the service.**

---

## 7. Socket

### Definition

A **socket** is an **endpoint for communication** between two programs over a network, identified by the combination of **IP address + Port number + Protocol**.

### Socket = IP Address + Port + Protocol

```text
Socket: 192.168.1.10 : 443 : TCP
        └──────────┘   └─┘   └──┘
         IP Address    Port  Protocol
```

### Key Points

- A **connection** is uniquely identified by a pair of sockets (client socket + server socket).
- **Stream socket (TCP)** → Reliable, connection-oriented.
- **Datagram socket (UDP)** → Unreliable, connectionless.
- In programming: `socket()`, `bind()`, `listen()`, `accept()`, `connect()`, `send()`, `recv()`.

### In One Line

> **A socket = IP + Port + Protocol — the unique endpoint through which two processes communicate.**

---

## 8. IP Address

### Definition

An **IP (Internet Protocol) address** is a **unique numerical label** assigned to every device on a network, used for **identification and routing**.

### IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Format | 32-bit, dotted decimal (e.g., 192.168.1.1) | 128-bit, hexadecimal (e.g., 2001:db8::1) |
| Total addresses | ~4.3 billion | ~3.4 × 10³⁸ |
| Notation | 4 octets (0–255) | 8 groups of 4 hex digits |
| Header size | 20–60 bytes | Fixed 40 bytes |
| NAT needed | Yes (address exhaustion) | No |

### In One Line

> **An IP address uniquely identifies a device on a network; IPv4 is 32-bit, IPv6 is 128-bit.**

---

## 9. MAC Address

### Definition

A **MAC (Media Access Control) address** is a **hardware address** permanently assigned to a network interface card (NIC) by the manufacturer. Used for communication within the same network (Layer 2).

### Format

```text
AA:BB:CC:DD:EE:FF   (6 bytes = 48 bits, hexadecimal)
└────────┘ └────────┘
 OUI (manufacturer) Device-specific
```

### MAC vs IP

| Feature | MAC Address | IP Address |
|---------|-------------|------------|
| Layer | Data Link (Layer 2) | Network (Layer 3) |
| Scope | Local network (LAN) | Global (Internet) |
| Assigned by | Manufacturer (NIC) | Network admin / DHCP |
| Changes | No (burned-in) | Yes (dynamic) |
| Format | 48-bit hex | 32-bit (IPv4) or 128-bit (IPv6) |

### In One Line

> **MAC address is a permanent hardware address used for local network communication; IP is used for routing across networks.**

---

## 10. Bandwidth

### Definition

**Bandwidth** is the **maximum amount of data** that can be transmitted over a network connection in a given time period. Measured in **bps (bits per second)**.

### Key Terms

- **Bandwidth** → Maximum theoretical capacity (e.g., 100 Mbps Ethernet).
- **Throughput** → Actual data rate achieved in practice (always ≤ bandwidth).
- **Goodput** → Useful application-level throughput (excluding protocol overhead).

### Units

| Unit | Value |
|------|-------|
| 1 Kbps | 1,000 bits/sec |
| 1 Mbps | 1,000,000 bits/sec |
| 1 Gbps | 1,000,000,000 bits/sec |

### In One Line

> **Bandwidth is the maximum data capacity of a link; throughput is what you actually get.**

---

## 11. Latency

### Definition

**Latency** is the **time delay** between sending data and receiving it at the destination. Also called **delay** or **ping**.

### Components of Latency

- **Propagation delay** → Time for signal to travel the physical medium (speed of light).
- **Transmission delay** → Time to push all packet bits onto the wire = Packet size / Bandwidth.
- **Processing delay** → Time routers take to process packet headers.
- **Queuing delay** → Time packet waits in router queue.

```
Total Latency = Propagation + Transmission + Processing + Queuing
```

### In One Line

> **Latency is the total delay for data to travel from source to destination — lower is better.**

---
---

# SECTION 2 — NETWORK MODELS

---

## 12. OSI Model

### Definition

The **OSI (Open Systems Interconnection) Model** is a **conceptual 7-layer framework** that standardizes how different network systems communicate.

### The 7 Layers

```text
Layer 7 — Application   (HTTP, FTP, SMTP, DNS)
Layer 6 — Presentation  (Encryption, Compression, Encoding)
Layer 5 — Session       (Session management, Authentication)
Layer 4 — Transport     (TCP, UDP — end-to-end communication)
Layer 3 — Network       (IP — routing, logical addressing)
Layer 2 — Data Link     (MAC — frames, local delivery, error detection)
Layer 1 — Physical      (Cables, signals, bits)
```

> **Mnemonic:** "**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing" (top to bottom)

### Layer Functions

| Layer | Name | PDU | Key Protocols | Job |
|-------|------|-----|---------------|-----|
| 7 | Application | Data | HTTP, FTP, DNS, SMTP | User interface; application services |
| 6 | Presentation | Data | SSL/TLS, JPEG, MPEG | Format, encrypt, compress data |
| 5 | Session | Data | NetBIOS, RPC | Establish, manage, terminate sessions |
| 4 | Transport | Segment | TCP, UDP | End-to-end delivery, reliability |
| 3 | Network | Packet | IP, ICMP, OSPF | Logical addressing, routing |
| 2 | Data Link | Frame | Ethernet, ARP, PPP | Physical addressing (MAC), framing |
| 1 | Physical | Bit | Ethernet cable, Wi-Fi | Transmit raw bits over medium |

### In One Line

> **OSI Model is a 7-layer reference framework: Physical → Data Link → Network → Transport → Session → Presentation → Application.**

---

## 13. Physical Layer

### Definition

The **Physical Layer (Layer 1)** is responsible for **transmitting raw bits** over a physical medium (cable, fiber, wireless).

### Key Points

- Deals with **electrical signals, light pulses, or radio waves**.
- Defines: connector types, pin layout, cable type, voltage levels, data rates.
- **PDU:** Bit.
- Examples: Ethernet cables (Cat5e, Cat6), fiber optic, Wi-Fi radio signals, hubs.
- Devices: Hub, Repeater, Modem.

### In One Line

> **Physical layer transmits raw bits — it's the hardware and signal level of networking.**

---

## 14. Data Link Layer

### Definition

The **Data Link Layer (Layer 2)** is responsible for **node-to-node delivery** of frames within the same network, using **MAC addresses**.

### Key Functions

- **Framing** → Wraps packets into frames.
- **Physical Addressing** → Uses MAC address for local delivery.
- **Error Detection** → CRC (Cyclic Redundancy Check) in frame trailer.
- **Flow Control** → Manages data rate between sender and receiver.
- **Access Control** → MAC protocols (CSMA/CD for Ethernet).

### Sub-layers

- **LLC (Logical Link Control)** → Error control, flow control.
- **MAC (Media Access Control)** → Physical addressing, media access.

### Devices: Switch, Bridge.

### In One Line

> **Data Link layer delivers frames within a LAN using MAC addresses and detects errors with CRC.**

---

## 15. Network Layer

### Definition

The **Network Layer (Layer 3)** is responsible for **logical addressing (IP)** and **routing** packets from source to destination across multiple networks.

### Key Functions

- **Logical Addressing** → Assigns and uses IP addresses.
- **Routing** → Determines best path for packets.
- **Packet Forwarding** → Moves packets hop-by-hop toward destination.
- **Fragmentation** → Splits packets if too large for the next network's MTU.

### Key Protocols

- **IP (IPv4/IPv6)** → Addressing and delivery.
- **ICMP** → Error reporting (used by `ping`).
- **OSPF, BGP, RIP** → Routing protocols.

### Devices: Router, Layer 3 Switch.

### In One Line

> **Network layer handles IP addressing and routing — getting packets from source to destination across networks.**

---

## 16. Transport Layer

### Definition

The **Transport Layer (Layer 4)** provides **end-to-end communication** between applications on different hosts, handling reliability, flow control, and multiplexing.

### Key Functions

- **Segmentation** → Breaks data into segments.
- **Port-based Multiplexing** → Multiple apps use network simultaneously via ports.
- **Reliability (TCP)** → Acknowledgements, retransmission, sequencing.
- **Flow Control** → Prevents sender from overwhelming receiver.
- **Congestion Control** → Prevents network overload.

### Key Protocols: **TCP** (reliable), **UDP** (fast).

### In One Line

> **Transport layer delivers data end-to-end between applications using ports, with TCP (reliable) or UDP (fast).**

---

## 17. Application Layer

### Definition

The **Application Layer (Layer 7)** provides **network services directly to user applications** — the layer humans interact with.

### Key Protocols

| Protocol | Purpose |
|----------|---------|
| **HTTP/HTTPS** | Web browsing |
| **DNS** | Domain name resolution |
| **SMTP/IMAP/POP3** | Email |
| **FTP/SFTP** | File transfer |
| **SSH** | Secure remote login |
| **DHCP** | IP address assignment |
| **Telnet** | Unsecure remote login |

### In One Line

> **Application layer is where user-facing protocols live — HTTP, DNS, FTP, SMTP, SSH.**

---

## 18. TCP/IP Model

### Definition

The **TCP/IP Model** (also called the **Internet Model**) is the **practical 4-layer model** used by the actual Internet.

### The 4 Layers

```text
Layer 4 — Application     (HTTP, FTP, DNS, SMTP, SSH)
Layer 3 — Transport        (TCP, UDP)
Layer 2 — Internet         (IP, ICMP, ARP)
Layer 1 — Network Access   (Ethernet, Wi-Fi, physical hardware)
```

### In One Line

> **TCP/IP is the 4-layer practical model that powers the Internet: Network Access → Internet → Transport → Application.**

---

## 19. OSI vs TCP/IP

| Feature | OSI Model | TCP/IP Model |
|---------|-----------|--------------|
| Layers | 7 | 4 |
| Purpose | Conceptual reference | Practical implementation |
| Usage | Teaching, troubleshooting | Actual Internet |
| Transport protocols | Defined generically | TCP and UDP |
| Application layers | Session + Presentation + Application | Combined into Application |
| Developed by | ISO | ARPANET / DoD |
| Protocol independence | Protocol-independent model | Built around TCP/IP |

### Mapping

```text
OSI                     TCP/IP
Application    ┐
Presentation   ├────→   Application
Session        ┘
Transport      ─────→   Transport
Network        ─────→   Internet
Data Link      ┐
Physical       ├────→   Network Access
```

### In One Line

> **OSI = 7-layer reference model; TCP/IP = 4-layer practical model; TCP/IP collapses OSI's top 3 and bottom 2 layers.**

---

## 20. Encapsulation & Decapsulation

### Encapsulation

**Adding headers (and trailers) at each layer** as data travels down the sender's protocol stack.

```text
Application:  [Data]
Transport:    [TCP Header | Data]               → Segment
Network:      [IP Header | TCP Header | Data]   → Packet
Data Link:    [Frame Header | Packet | FCS]     → Frame
Physical:     Bits on wire
```

### Decapsulation

**Removing headers at each layer** as data travels up the receiver's protocol stack — reverse of encapsulation.

### In One Line

> **Encapsulation wraps data with headers at each layer going down; decapsulation unwraps them going up.**

---
---

# SECTION 3 — DATA LINK & LOCAL NETWORKING

---

## 21. MAC Address (Detail)

### Definition

A **MAC (Media Access Control) address** is a **48-bit (6-byte) unique hardware address** burned into every NIC, used for local network communication.

### Key Points

- First 3 bytes = **OUI** (Organizationally Unique Identifier — manufacturer ID).
- Last 3 bytes = Device-specific identifier.
- Written as: `AA:BB:CC:DD:EE:FF` or `AA-BB-CC-DD-EE-FF`.
- **ARP** maps IP addresses to MAC addresses on a LAN.

### In One Line

> **MAC address = unique 48-bit NIC hardware address used for local delivery within a LAN.**

---

## 22. Ethernet

### Definition

**Ethernet** is the most widely used **wired LAN technology** (IEEE 802.3 standard) that defines how data is transmitted over cables using frames.

### Key Points

- Uses **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection) for media access.
- Data sent as **frames** with source/destination MAC addresses.
- Speeds: 10 Mbps → 100 Mbps → 1 Gbps → 10 Gbps → 100 Gbps.
- Cable types: Cat5e (100m, 1 Gbps), Cat6 (100m, 10 Gbps), Fiber optic.

### In One Line

> **Ethernet is the standard wired LAN technology using frames, MAC addresses, and CSMA/CD.**

---

## 23. Frame

### Definition

A **frame** is the **Layer 2 PDU (Protocol Data Unit)** — a packet of data with a header containing MAC addresses and a trailer with error detection.

### Ethernet Frame Structure

```text
┌──────────┬──────────┬──────┬──────────────┬─────────┐
│ Preamble │ Dest MAC │ Src  │   Payload    │   FCS   │
│ (8 bytes)│ (6 bytes)│ MAC  │ (46–1500 B)  │(4 bytes)│
│          │          │(6 B) │              │         │
└──────────┴──────────┴──────┴──────────────┴─────────┘
```

- **Preamble** → Synchronization.
- **Destination / Source MAC** → Who it's for / from.
- **Payload** → Encapsulated packet (IP packet).
- **FCS (Frame Check Sequence)** → CRC for error detection.

### In One Line

> **A frame is the Layer 2 data unit with source/destination MAC addresses and CRC error check.**

---

## 24. ARP (Address Resolution Protocol)

### Definition

**ARP** is a protocol that **resolves an IP address to a MAC address** within a local network (LAN).

### How ARP Works

```text
1. Host A wants to send data to IP 192.168.1.5
2. A checks ARP cache — not found
3. A broadcasts ARP Request: "Who has 192.168.1.5?"
4. Host B (192.168.1.5) replies with its MAC address (unicast)
5. A updates its ARP cache and sends the frame
```

### Key Terms

- **ARP Cache** → Local table storing IP→MAC mappings (temporary).
- **ARP Request** → Broadcast asking for MAC of a given IP.
- **ARP Reply** → Unicast response with the MAC address.
- **Gratuitous ARP** → A host announces its own IP→MAC mapping.

### In One Line

> **ARP maps IP addresses to MAC addresses so that frames can be delivered correctly within a LAN.**

---

## 25. Hub

### Definition

A **hub** is a Layer 1 networking device that **broadcasts all incoming data to every connected device**, regardless of the destination.

### Key Points

- **Dumb device** — no intelligence; no MAC table.
- All devices share the same **collision domain**.
- Uses **CSMA/CD** to handle collisions.
- Replaced by switches in modern networks.
- **Half-duplex** operation.

### In One Line

> **A hub broadcasts data to all ports — simple, dumb, and creates collision/broadcast domains.**

---

## 26. Switch

### Definition

A **switch** is a Layer 2 networking device that **forwards frames only to the correct destination port** using a **MAC address table**.

### How a Switch Works

```text
1. Frame arrives at port 1 with Src=AA:BB, Dst=CC:DD
2. Switch learns: AA:BB is on port 1 (adds to MAC table)
3. Switch looks up CC:DD in MAC table
4. If found → forwards to that port only (unicast)
5. If not found → floods to all ports except incoming (unknown unicast)
```

### Key Points

- Each port is its own **collision domain** (full-duplex support).
- All ports share one **broadcast domain** (unless VLANs used).
- **MAC address table** (CAM table) stores port-to-MAC mappings.
- More efficient and secure than hubs.

### In One Line

> **A switch intelligently forwards frames to specific ports using a MAC table — eliminates collisions.**

---

## 27. Router

### Definition

A **router** is a Layer 3 networking device that **routes packets between different networks** using **IP addresses** and a **routing table**.

### How a Router Works

```text
1. Packet arrives with destination IP
2. Router checks routing table for best match (longest prefix match)
3. Forwards packet out the appropriate interface toward destination
4. Decrements TTL; discards if TTL = 0
```

### Key Points

- Connects different **networks** (LANs, WANs).
- Each interface is a separate **broadcast domain**.
- Uses **routing protocols** (OSPF, BGP, RIP) to build routing table.
- Performs **NAT** to translate private/public IPs.

### In One Line

> **A router routes packets between different networks using IP addresses and a routing table.**

---

## 28. Hub vs Switch

| Feature | Hub | Switch |
|---------|-----|--------|
| OSI Layer | Layer 1 (Physical) | Layer 2 (Data Link) |
| Intelligence | None | MAC address table |
| Forwarding | Broadcasts to all ports | Forwards to specific port |
| Collision domain | One for all ports | One per port |
| Broadcast domain | One | One (unless VLANs) |
| Speed | Half-duplex | Full-duplex |
| Efficiency | Low | High |
| Security | Low | Higher |
| Used today | Rarely | Yes, universally |

### In One Line

> **Hub = broadcast to all; Switch = forward to correct port using MAC table — switches replaced hubs.**

---

## 29. Switch vs Router

| Feature | Switch | Router |
|---------|--------|--------|
| OSI Layer | Layer 2 | Layer 3 |
| Address used | MAC address | IP address |
| Connects | Devices in same network | Different networks |
| Broadcast domain | One (unless VLANs) | Separates broadcast domains |
| Routing table | No | Yes |
| NAT | No | Yes |
| Primary job | Frame switching | Packet routing |

### In One Line

> **Switch connects devices within a LAN using MAC; Router connects networks using IP.**

---

## 30. Broadcast, Unicast & Multicast

| Type | Description | Destination |
|------|-------------|-------------|
| **Unicast** | One sender → one specific receiver | Specific MAC / IP |
| **Broadcast** | One sender → all devices in network | FF:FF:FF:FF:FF:FF (MAC) / 255.255.255.255 (IP) |
| **Multicast** | One sender → group of interested receivers | Multicast group address |
| **Anycast** | One sender → nearest of a group | Used in IPv6, DNS |

### In One Line

> **Unicast=one-to-one; Broadcast=one-to-all; Multicast=one-to-group.**

---
---

# SECTION 4 — IP ADDRESSING & ROUTING

---

## 31. IPv4

### Definition

**IPv4** is the fourth version of the Internet Protocol, using **32-bit addresses** written as **four decimal octets** separated by dots.

### Format

```text
192  .  168  .   1  .   1
 ↑        ↑       ↑      ↑
8 bits  8 bits  8 bits  8 bits  = 32 bits total
Range: 0–255 per octet
```

### IPv4 Address Classes

| Class | Range | Default Subnet Mask | Use |
|-------|-------|---------------------|-----|
| A | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 (/8) | Large organizations |
| B | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 (/16) | Medium organizations |
| C | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 (/24) | Small networks |
| D | 224.0.0.0 – 239.255.255.255 | — | Multicast |
| E | 240.0.0.0 – 255.255.255.255 | — | Experimental |

### In One Line

> **IPv4 is a 32-bit addressing scheme using dotted decimal notation; provides ~4.3 billion addresses.**

---

## 32. IPv6 Basics

### Definition

**IPv6** is the sixth version of the Internet Protocol, using **128-bit addresses** to solve IPv4 address exhaustion.

### Format

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
(8 groups of 4 hex digits, separated by colons)
Shortened: 2001:db8:85a3::8a2e:370:7334
```

### IPv6 Key Features

- **Huge address space:** 2¹²⁸ ≈ 3.4 × 10³⁸ addresses.
- **No NAT needed** (enough addresses for all devices).
- **Built-in IPSec** (security).
- **Simplified header** (fixed 40 bytes).
- **No broadcast** → uses multicast and anycast instead.
- **Auto-configuration** (SLAAC — Stateless Address Autoconfiguration).

### In One Line

> **IPv6 is 128-bit addressing solving IPv4 exhaustion; supports auto-configuration and built-in security.**

---

## 33. Public IP vs Private IP

| Feature | Public IP | Private IP |
|---------|-----------|------------|
| Assigned by | ISP (Internet Service Provider) | Router (DHCP) |
| Accessible from | Anywhere on Internet | Only within local network |
| Unique globally | Yes | No (same ranges reused everywhere) |
| Cost | Paid (limited) | Free (unlimited internally) |

### Private IP Ranges (RFC 1918)

| Class | Private Range |
|-------|--------------|
| A | 10.0.0.0 – 10.255.255.255 |
| B | 172.16.0.0 – 172.31.255.255 |
| C | 192.168.0.0 – 192.168.255.255 |

### In One Line

> **Public IPs are globally routable; private IPs are local-only and translated to public via NAT.**

---

## 34. Static IP vs Dynamic IP

| Feature | Static IP | Dynamic IP |
|---------|-----------|------------|
| Assigned | Manually, fixed permanently | Automatically by DHCP server |
| Changes | Never | Changes on reconnect / lease expiry |
| Best for | Servers, printers, DNS | End-user devices (laptops, phones) |
| Cost | More expensive | Usually included |
| Management | Manual configuration | Automatic |

### In One Line

> **Static IP is manually assigned and fixed; dynamic IP is automatically assigned by DHCP and can change.**

---

## 35. Loopback Address

### Definition

The **loopback address** is a special IP address that **routes traffic back to the same device** without going through any network — used for testing.

### Key Points

- **IPv4 loopback:** `127.0.0.1` (entire 127.0.0.0/8 block reserved).
- **IPv6 loopback:** `::1`.
- Hostname: `localhost`.
- Used to test if the network stack on the local machine is working.
- Pinging `127.0.0.1` tests the local TCP/IP stack.

### In One Line

> **127.0.0.1 (localhost) is the loopback address — traffic sent here stays on the same machine.**

---

## 36. Subnet Mask

### Definition

A **subnet mask** is a 32-bit number that **divides an IP address into network and host portions** by masking bits.

### How It Works

```text
IP Address:   192.168.1.100  =  11000000.10101000.00000001.01100100
Subnet Mask:  255.255.255.0  =  11111111.11111111.11111111.00000000
                                └────────────────────────┘ └────────┘
                                      Network portion       Host portion

Network:  192.168.1.0
Host:     .100
```

### In One Line

> **Subnet mask identifies which bits of an IP address are the network part and which are the host part.**

---

## 37. CIDR (Classless Inter-Domain Routing)

### Definition

**CIDR** is a method for **allocating IP addresses** and **routing** that replaces the old class-based system, using a **prefix length** notation.

### CIDR Notation

```text
192.168.1.0/24
             ↑
       24 bits = network portion → 8 bits for hosts → 2⁸ - 2 = 254 usable hosts
```

### Common CIDR Values

| CIDR | Subnet Mask | Hosts |
|------|-------------|-------|
| /8 | 255.0.0.0 | 16,777,214 |
| /16 | 255.255.0.0 | 65,534 |
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /30 | 255.255.255.252 | 2 |

### In One Line

> **CIDR uses /prefix notation to flexibly allocate IP address blocks without class constraints.**

---

## 38. Subnetting

### Definition

**Subnetting** is the process of **dividing a large network into smaller sub-networks (subnets)** to improve performance and security.

### How to Subnet

**Example:** Divide 192.168.1.0/24 into 4 subnets.

- Need 4 subnets → borrow 2 bits → /26 (256 addresses → 4 × 64 = 64 addresses each).

| Subnet | Network Address | Broadcast | Usable Range | Hosts |
|--------|----------------|-----------|--------------|-------|
| 1 | 192.168.1.0 | 192.168.1.63 | .1 – .62 | 62 |
| 2 | 192.168.1.64 | 192.168.1.127 | .65 – .126 | 62 |
| 3 | 192.168.1.128 | 192.168.1.191 | .129 – .190 | 62 |
| 4 | 192.168.1.192 | 192.168.1.255 | .193 – .254 | 62 |

### Formula

```
Number of subnets   = 2^(borrowed bits)
Hosts per subnet    = 2^(host bits) - 2   (subtract network + broadcast)
Block size          = 256 - last octet of subnet mask
```

### In One Line

> **Subnetting divides a network into smaller subnets by borrowing host bits — improves security and reduces broadcast traffic.**

---

## 39. Network Address & Broadcast Address

### Network Address

- **First address** in a subnet.
- Identifies the **subnet itself** — not assignable to any host.
- Example: 192.168.1.0 in a /24 subnet.

### Broadcast Address

- **Last address** in a subnet.
- Sends data to **all hosts** in that subnet simultaneously.
- Example: 192.168.1.255 in a /24 subnet.

### Usable Hosts = Total addresses − 2 (network + broadcast)

### In One Line

> **Network address = first (identifies subnet); Broadcast = last (sends to all hosts); both are non-assignable.**

---

## 40. Default Gateway

### Definition

A **default gateway** is the **router's IP address** on the local network that devices use to send traffic **destined for outside the local network**.

### Key Points

- If the destination IP is **not in the local subnet** → send to default gateway.
- Usually the **first or last usable IP** in a subnet (e.g., 192.168.1.1).
- Without a gateway, a device can only communicate within its own subnet.

### In One Line

> **Default gateway is the local router's IP — all traffic for external networks is sent through it.**

---

## 41. Routing

### Definition

**Routing** is the process of **selecting the best path** for packets to travel from source to destination across multiple networks.

### Routing Process

```text
1. Packet arrives at router with destination IP
2. Router looks up routing table (longest prefix match)
3. Forwards to next-hop router or directly to destination
4. Repeats at each router until destination reached
```

### Key Terms

- **Next hop** → IP address of the next router toward the destination.
- **Metric** → Cost of a route (hop count, bandwidth, delay).
- **TTL (Time to Live)** → Decremented at each hop; packet discarded when TTL=0.

### In One Line

> **Routing selects the best path for packets across networks using a routing table.**

---

## 42. Routing Table

### Definition

A **routing table** is a **database in a router** that stores information about known network paths used to forward packets.

### Routing Table Entry

| Network | Subnet Mask | Next Hop | Interface | Metric |
|---------|-------------|----------|-----------|--------|
| 192.168.1.0 | /24 | — | eth0 | 0 |
| 10.0.0.0 | /8 | 192.168.1.1 | eth1 | 1 |
| 0.0.0.0 | /0 | 203.0.113.1 | eth2 | 10 |

- **0.0.0.0/0** → Default route (used when no specific match).
- **Longest prefix match** → Most specific route wins.

### In One Line

> **Routing table stores network-to-next-hop mappings; longest prefix match determines the forwarding path.**

---

## 43. Static vs Dynamic Routing

| Feature | Static Routing | Dynamic Routing |
|---------|---------------|-----------------|
| Configuration | Manually by admin | Automatically by routing protocols |
| Updates | Manual | Automatic (protocols exchange info) |
| Overhead | Low (no protocol traffic) | Higher (routing protocol messages) |
| Scalability | Poor (hard for large networks) | Good (auto-adapts) |
| Fault tolerance | None (manual fix required) | Good (reroutes around failures) |
| Examples | Small networks, specific routes | OSPF, RIP, BGP, EIGRP |

### In One Line

> **Static routing is manually configured and predictable; dynamic routing auto-updates using routing protocols.**

---

## 44. NAT (Network Address Translation)

### Definition

**NAT** is a technique where a **router replaces private IP addresses with a public IP** when forwarding packets to the Internet, and reverses the mapping for replies.

### How NAT Works

```text
Private Network             Router (NAT)              Internet
192.168.1.10  ──→  192.168.1.10:5000 → 203.0.113.1:5000 ──→  Web Server
                   [NAT Table entry]
192.168.1.10:5000 ↔ 203.0.113.1:5000
```

### Types of NAT

- **SNAT (Static NAT)** → 1-to-1 mapping (one private to one public).
- **DNAT (Dynamic NAT)** → Many-to-pool mapping.
- **PAT / Masquerading (NAPT)** → Many-to-one; uses port numbers to distinguish connections.

### Benefits

- **Conserves IPv4 addresses** → Many devices share one public IP.
- **Security** → Internal IPs hidden from Internet.

### In One Line

> **NAT translates private IPs to a public IP for Internet access, conserving IPv4 addresses.**

---
---

# SECTION 5 — TRANSPORT LAYER

---

## 45. TCP (Transmission Control Protocol)

### Definition

**TCP** is a **connection-oriented, reliable transport protocol** that guarantees data delivery in order, without duplicates, and without errors.

### Key Features

- **Connection-oriented** → Must establish connection before data transfer (3-way handshake).
- **Reliable** → Acknowledgements, retransmission on loss.
- **Ordered** → Sequence numbers ensure correct order.
- **Error checked** → Checksum on every segment.
- **Flow control** → Sliding window.
- **Congestion control** → Slow start, congestion avoidance.

### In One Line

> **TCP is reliable, ordered, connection-oriented delivery — ensures data arrives correctly, in order.**

---

## 46. UDP (User Datagram Protocol)

### Definition

**UDP** is a **connectionless, unreliable transport protocol** that sends datagrams with no guarantee of delivery, order, or error recovery.

### Key Features

- **Connectionless** → No handshake; send and forget.
- **Unreliable** → No ACKs, no retransmission.
- **No ordering** → Datagrams may arrive out of order.
- **Fast & Low overhead** → Minimal header (8 bytes vs TCP's 20+ bytes).
- **Best for:** Streaming, gaming, DNS, VoIP, DHCP.

### In One Line

> **UDP is fast, connectionless, and unreliable — used when speed matters more than delivery guarantee.**

---

## 47. TCP vs UDP

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable (ACK + retransmit) | Unreliable (no ACK) |
| Ordering | Ordered (sequence numbers) | No ordering |
| Speed | Slower (overhead) | Faster |
| Header size | 20–60 bytes | 8 bytes |
| Flow control | Yes (sliding window) | No |
| Congestion control | Yes | No |
| Use cases | HTTP, FTP, SMTP, SSH | DNS, DHCP, VoIP, Video, Gaming |
| Error recovery | Yes | No (app handles if needed) |

### In One Line

> **TCP = reliable but slow; UDP = fast but unreliable — choose based on application need.**

---

## 48. Port Numbers

### Definition

**Port numbers** identify specific **applications or services** on a host. Combined with an IP address, they form a socket.

### Key Well-Known Ports

| Port | Protocol | Service |
|------|----------|---------|
| 20, 21 | TCP | FTP (data/control) |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67/68 | UDP | DHCP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 143 | TCP | IMAP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |

### In One Line

> **Port numbers identify services on a host; well-known ports (0–1023) are reserved for standard services.**

---

## 49. Connection-Oriented vs Connectionless

| Feature | Connection-Oriented (TCP) | Connectionless (UDP) |
|---------|--------------------------|----------------------|
| Setup | Handshake required | No setup |
| State | Maintains connection state | No state |
| Reliability | Guaranteed delivery | Best-effort |
| Overhead | High | Low |
| Examples | TCP, ATM | UDP, IP |

### In One Line

> **Connection-oriented (TCP) establishes a dedicated path before transfer; connectionless (UDP) just sends.**

---

## 50. Flow Control & Congestion Control

### Flow Control

- **Purpose:** Prevents the **sender from overwhelming the receiver's buffer**.
- **Mechanism:** Receiver advertises a **receive window (rwnd)** — max data it can accept.
- Sender limits unacknowledged data to ≤ rwnd.

### Congestion Control

- **Purpose:** Prevents the **sender from overwhelming the network** (routers/links).
- **Mechanism:** TCP maintains a **congestion window (cwnd)**.
- **Slow Start** → cwnd starts small; doubles every RTT.
- **Congestion Avoidance** → After threshold: cwnd increases linearly.
- **Fast Retransmit** → 3 duplicate ACKs → retransmit without waiting for timeout.

### In One Line

> **Flow control protects the receiver's buffer; congestion control protects the network — both use window sizes.**

---

## 51. Sliding Window

### Definition

**Sliding window** is a **flow control mechanism** that allows the sender to have **multiple unacknowledged packets** in transit simultaneously, improving throughput.

### How It Works

```text
Sender window size = 4 (can send 4 packets before waiting for ACK)

Sent & ACKed | Sent, not ACKed | Can send | Cannot send yet
─────────────┼─────────────────┼──────────┼──────────────
  1  2  3    │   4  5  6  7    │  8  9    │  10  11  12
             └─────────────────┘ (window)
As ACK arrives, window "slides" forward
```

### Key Points

- **Window size** determines throughput.
- **Go-Back-N** → On error, retransmit from error onwards.
- **Selective Repeat** → Retransmit only the lost packet.

### In One Line

> **Sliding window allows multiple packets in flight; window slides forward as ACKs arrive — maximizes throughput.**

---
---

# SECTION 6 — TCP CONNECTION & RELIABILITY

---

## 52. TCP 3-Way Handshake

### Definition

The **TCP 3-Way Handshake** is the process TCP uses to **establish a connection** between client and server before data transfer begins.

### Steps

```text
Client                              Server
  │──── SYN (Seq=x) ──────────────→ │   Step 1: Client sends SYN
  │                                  │
  │←─── SYN-ACK (Seq=y, Ack=x+1) ──│   Step 2: Server replies SYN-ACK
  │                                  │
  │──── ACK (Ack=y+1) ────────────→ │   Step 3: Client confirms
  │                                  │
  │ ←── Data Transfer ─────────────→│   Connection ESTABLISHED
```

### Purpose of Each Step

- **SYN** → Client requests connection; sends initial sequence number.
- **SYN-ACK** → Server acknowledges; sends its own sequence number.
- **ACK** → Client acknowledges server's sequence number.

### In One Line

> **TCP 3-Way Handshake (SYN → SYN-ACK → ACK) establishes a reliable connection before data transfer.**

---

## 53. SYN, SYN-ACK, ACK

| Flag | Meaning | Direction | Purpose |
|------|---------|-----------|---------|
| **SYN** | Synchronize | Client → Server | Initiate connection; share initial seq number |
| **SYN-ACK** | Sync + Acknowledge | Server → Client | Accept connection; share server's seq number |
| **ACK** | Acknowledge | Client → Server | Confirm server's seq number; connection ready |

### In One Line

> **SYN=start, SYN-ACK=accept, ACK=confirm — three flags that establish a TCP connection.**

---

## 54. TCP 4-Way Termination

### Definition

**TCP 4-Way Termination** is the process of **gracefully closing a TCP connection** using four steps (each side closes independently).

### Steps

```text
Client                              Server
  │──── FIN ────────────────────→ │   Step 1: Client done sending; sends FIN
  │←─── ACK ───────────────────── │   Step 2: Server ACKs the FIN
  │ (Server may still send data)   │
  │←─── FIN ───────────────────── │   Step 3: Server done; sends FIN
  │──── ACK ────────────────────→ │   Step 4: Client ACKs; enters TIME_WAIT
```

### Why 4 Steps (not 3)?

- Each direction closes **independently**.
- Server may have more data to send after receiving client's FIN.

### In One Line

> **TCP closes with 4 steps (FIN → ACK → FIN → ACK) since each side terminates independently.**

---

## 55. Sequence Number & Acknowledgement Number

### Sequence Number

- A **32-bit number** in the TCP header that **identifies the byte position** of the first byte in the segment.
- Ensures data is reassembled in the correct order.

### Acknowledgement Number

- Tells the sender the **next byte expected** from it.
- `ACK number = last received byte + 1` = "send me from this byte next."

### Example

```text
Sender sends Seq=100, 10 bytes of data (bytes 100–109)
Receiver replies: ACK=110 (next expected byte)
Sender now knows bytes 100–109 were received correctly
```

### In One Line

> **Sequence number labels data bytes; ACK number tells sender the next expected byte — together they ensure ordered, reliable delivery.**

---

## 56. Retransmission & Timeout

### Retransmission

- If an ACK is **not received** within a timeout period, TCP **retransmits** the unacknowledged segment.
- Also triggered by **3 duplicate ACKs** (fast retransmit).

### Timeout (RTO — Retransmission Timeout)

- Calculated dynamically based on **RTT (Round Trip Time)**.
- `RTO = EstimatedRTT + 4 × DevRTT` (Jacobson/Karels algorithm).
- **Too short** → Unnecessary retransmissions.
- **Too long** → Slow recovery from loss.

### In One Line

> **Retransmission recovers lost segments — triggered by timeout or 3 duplicate ACKs.**

---

## 57. TCP Connection States

| State | Description |
|-------|-------------|
| **CLOSED** | No connection; initial state |
| **LISTEN** | Server waiting for incoming connection requests |
| **SYN_SENT** | Client sent SYN; waiting for SYN-ACK |
| **SYN_RECEIVED** | Server received SYN; sent SYN-ACK |
| **ESTABLISHED** | Connection open; data transfer in progress |
| **FIN_WAIT_1** | Client sent FIN; waiting for ACK |
| **FIN_WAIT_2** | Client received ACK; waiting for server's FIN |
| **CLOSE_WAIT** | Server received FIN; waiting for app to close |
| **TIME_WAIT** | Client sent final ACK; waiting 2×MSL before closing |
| **LAST_ACK** | Server waiting for final ACK from client |

### In One Line

> **TCP goes through multiple states: LISTEN → ESTABLISHED (data) → FIN_WAIT → TIME_WAIT → CLOSED.**

---

## 58. TIME_WAIT & CLOSE_WAIT

### TIME_WAIT

- State entered by the **active closer** (usually client) after sending final ACK.
- Lasts **2 × MSL (Maximum Segment Lifetime)** ≈ 60–120 seconds.
- **Why:** Ensures the final ACK reached server; allows old duplicate packets to expire.

### CLOSE_WAIT

- State entered by **passive closer** (usually server) after receiving FIN.
- Waits for the **application to call close()**.
- If many sockets stuck in CLOSE_WAIT → application bug (not calling close).

### In One Line

> **TIME_WAIT is the client's safety wait after closing; CLOSE_WAIT is the server waiting for the app to close its socket.**

---

## 59. TCP Flow Control

### Definition

**TCP Flow Control** ensures the sender doesn't **overwhelm the receiver's buffer** by limiting how much data can be in transit.

### Mechanism: Receive Window (rwnd)

- Receiver advertises **rwnd** (free space in its buffer) in every ACK.
- Sender limits unacknowledged data to ≤ rwnd.
- If rwnd = 0 → sender stops (sends probes to check when receiver is ready).

```text
Sender ──→ up to rwnd bytes unacknowledged ──→ Receiver
           ←── ACK with new rwnd value ─────
```

### In One Line

> **TCP flow control uses the receiver's advertised window (rwnd) to prevent buffer overflow at the receiver.**

---

## 60. TCP Congestion Control

### Definition

**TCP Congestion Control** prevents the sender from **overloading the network** by dynamically adjusting the sending rate based on network conditions.

### Phases

```text
1. Slow Start:
   cwnd = 1 MSS
   Double cwnd each RTT (exponential growth)
   Until cwnd reaches ssthresh

2. Congestion Avoidance:
   Increase cwnd by 1 MSS per RTT (linear)
   Until packet loss detected

3. On Packet Loss:
   (a) Timeout → ssthresh = cwnd/2; cwnd = 1 (restart slow start)
   (b) 3 dup ACKs (Fast Recovery) → ssthresh = cwnd/2; cwnd = ssthresh
```

### In One Line

> **TCP congestion control uses slow start (exponential) then congestion avoidance (linear) to find the network's capacity without causing collapse.**

---
---

# SECTION 7 — APPLICATION-LEVEL NETWORKING

---

## 61. DNS (Domain Name System)

### Definition

**DNS** is a **hierarchical distributed naming system** that translates **human-readable domain names** (e.g., google.com) to **IP addresses** (e.g., 142.250.182.46).

### How DNS Resolution Works

```text
Browser: "What is the IP of www.google.com?"
    ↓
1. Check local DNS cache
2. Ask Recursive Resolver (ISP's DNS)
3. Resolver asks Root DNS Server (knows TLD servers)
4. Root replies: "Ask .com TLD server"
5. Resolver asks .com TLD server
6. TLD replies: "Ask google.com's authoritative NS"
7. Resolver asks google.com's authoritative server
8. Gets IP → returns to browser → browser connects
```

### Key DNS Record Types

| Record | Purpose |
|--------|---------|
| **A** | Domain → IPv4 address |
| **AAAA** | Domain → IPv6 address |
| **CNAME** | Alias → canonical domain name |
| **MX** | Mail exchange server for domain |
| **NS** | Authoritative name servers |
| **PTR** | Reverse DNS (IP → domain) |
| **TXT** | Text records (SPF, DKIM, verification) |

### In One Line

> **DNS translates domain names to IPs; resolution goes from local cache → recursive resolver → root → TLD → authoritative DNS.**

---

## 62. DHCP (Dynamic Host Configuration Protocol)

### Definition

**DHCP** is a network protocol that **automatically assigns IP addresses** and other network configuration to devices when they join a network.

### What DHCP Assigns

- IP address, Subnet mask, Default gateway, DNS server addresses, Lease time.

### DHCP Process (DORA)

```text
Client                          DHCP Server
  │──── DISCOVER (broadcast) ──→ │   "Anyone have an IP for me?"
  │←─── OFFER ─────────────────── │   "Here's 192.168.1.50"
  │──── REQUEST (broadcast) ──→  │   "I'll take 192.168.1.50"
  │←─── ACK ───────────────────── │   "It's yours for 24 hours"
```

- **D**iscover → **O**ffer → **R**equest → **A**cknowledge

### In One Line

> **DHCP automatically assigns IP configuration via DORA — Discover, Offer, Request, Acknowledge.**

---

## 63. HTTP (HyperText Transfer Protocol)

### Definition

**HTTP** is an **application-layer protocol** used for **transferring web pages** between a client (browser) and a web server.

### Key Points

- **Stateless** → Each request is independent; server remembers nothing.
- **Request-Response** model.
- Default port: **80**.
- Uses **TCP** as transport.
- Text-based protocol (human-readable headers).

### HTTP Request Structure

```text
GET /index.html HTTP/1.1
Host: www.example.com
Accept: text/html
User-Agent: Mozilla/5.0
```

### HTTP Response Structure

```text
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1024

<html>...</html>
```

### In One Line

> **HTTP is the stateless request-response protocol for web communication on port 80.**

---

## 64. HTTPS

### Definition

**HTTPS (HTTP Secure)** is HTTP with **SSL/TLS encryption** — ensuring data confidentiality, integrity, and server authentication.

### How HTTPS Works

```text
1. Client connects to server on port 443
2. TLS Handshake: negotiate cipher suite; server sends certificate
3. Client verifies certificate with CA
4. Session key established
5. All HTTP data encrypted with session key
```

### HTTP vs HTTPS

| Feature | HTTP | HTTPS |
|---------|------|-------|
| Port | 80 | 443 |
| Encryption | No | Yes (TLS) |
| Security | None | Encrypted + Authenticated |
| Certificate | Not required | Required (from CA) |
| URL prefix | http:// | https:// |

### In One Line

> **HTTPS = HTTP + TLS encryption; provides confidentiality, integrity, and authentication on port 443.**

---

## 65. HTTP Methods

| Method | Purpose | Has Body? | Idempotent? | Safe? |
|--------|---------|-----------|-------------|-------|
| **GET** | Retrieve a resource | No | Yes | Yes |
| **POST** | Create a new resource | Yes | No | No |
| **PUT** | Replace an entire resource | Yes | Yes | No |
| **PATCH** | Partially update a resource | Yes | No | No |
| **DELETE** | Delete a resource | Optional | Yes | No |
| **HEAD** | Like GET but no body in response | No | Yes | Yes |
| **OPTIONS** | List supported methods | No | Yes | Yes |

- **Idempotent** → Calling multiple times gives same result.
- **Safe** → Doesn't modify server state.

### In One Line

> **GET=read, POST=create, PUT=replace, PATCH=update, DELETE=remove — the standard HTTP methods.**

---

## 66. HTTP Status Codes

| Range | Category | Common Codes |
|-------|----------|--------------|
| **1xx** | Informational | 100 Continue |
| **2xx** | Success | 200 OK, 201 Created, 204 No Content |
| **3xx** | Redirection | 301 Moved Permanently, 302 Found, 304 Not Modified |
| **4xx** | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 429 Too Many Requests |
| **5xx** | Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

### In One Line

> **2xx=success, 3xx=redirect, 4xx=client error, 5xx=server error — five classes of HTTP status codes.**

---

## 67. HTTP Headers

### Definition

**HTTP headers** are **key-value pairs** sent in requests and responses, providing metadata about the message.

### Common Request Headers

| Header | Purpose |
|--------|---------|
| `Host` | Target server domain |
| `Authorization` | Credentials (e.g., Bearer token) |
| `Content-Type` | Media type of request body |
| `Accept` | Media types client accepts |
| `Cookie` | Session cookies sent to server |
| `User-Agent` | Client browser/OS info |

### Common Response Headers

| Header | Purpose |
|--------|---------|
| `Content-Type` | Media type of response body |
| `Set-Cookie` | Server sets a cookie on client |
| `Cache-Control` | Caching instructions |
| `Location` | Redirect target URL (3xx) |
| `WWW-Authenticate` | Auth challenge (401) |

### In One Line

> **HTTP headers carry metadata about requests and responses — authentication, content type, caching, cookies.**

---

## 68. Cookies & Sessions

### Cookies

- **Small pieces of data** stored by the **browser**, sent with every request to the same origin.
- Set by server via `Set-Cookie` response header.
- Used for: session management, personalization, tracking.
- **Persistent cookies** → Have an expiry date.
- **Session cookies** → Deleted when browser closes.

### Sessions

- **Server-side storage** of user state.
- Browser stores only a **Session ID** (in a cookie).
- Server uses Session ID to look up full session data.

### Cookie vs Session

| Feature | Cookie | Session |
|---------|--------|---------|
| Storage | Client (browser) | Server |
| Security | Less secure (client-side) | More secure (server-side) |
| Size limit | ~4KB | Larger (server storage) |
| Expiry | Can be long-lived | Usually expires on close |

### In One Line

> **Cookies store data on the browser; sessions store data on the server with a cookie holding only the session ID.**

---

## 69. HTTP/1.1 vs HTTP/2 vs HTTP/3

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---------|----------|--------|--------|
| Year | 1997 | 2015 | 2022 |
| Transport | TCP | TCP | QUIC (UDP-based) |
| Multiplexing | No (one request/connection) | Yes (multiple streams) | Yes |
| Header compression | No | HPACK compression | QPACK |
| Server push | No | Yes | Yes |
| HOL blocking | Yes (TCP) | Yes (TCP, not HTTP) | No (QUIC) |
| TLS | Optional | Required (de facto) | Built-in (required) |

### Key Improvements

- **HTTP/2** → Multiplexing eliminates HTTP-level head-of-line blocking.
- **HTTP/3** → Uses QUIC (UDP) to eliminate TCP head-of-line blocking; faster connection setup.

### In One Line

> **HTTP/1.1=one-at-a-time; HTTP/2=multiplexed over TCP; HTTP/3=multiplexed over QUIC(UDP) with built-in TLS.**

---
---

# SECTION 8 — NETWORK SECURITY BASICS

---

## 70. HTTP vs HTTPS

*(See Topic 64 for full comparison.)*

| Feature | HTTP | HTTPS |
|---------|------|-------|
| Encryption | No | Yes (TLS) |
| Port | 80 | 443 |
| Data visible to ISP | Yes | No |
| MITM attack risk | High | Low |
| Certificate | Not required | Required |

### In One Line

> **HTTP is plaintext; HTTPS is encrypted HTTP using TLS — always use HTTPS for sensitive data.**

---

## 71. SSL/TLS Basics

### Definition

- **SSL (Secure Sockets Layer)** → Original protocol; now deprecated (SSL 3.0 vulnerable).
- **TLS (Transport Layer Security)** → Modern, secure replacement for SSL.
- Provides: **Encryption** (confidentiality), **Integrity** (tamper detection), **Authentication** (verify server identity).

### TLS Handshake (Simplified)

```text
1. Client Hello → TLS version, supported cipher suites, random number
2. Server Hello → chosen cipher, server's SSL certificate, random number
3. Client verifies certificate with CA
4. Key Exchange → agree on session key (using asymmetric crypto)
5. Session established → all data encrypted with symmetric session key
```

### TLS Versions

| Version | Status |
|---------|--------|
| SSL 3.0 | Deprecated (insecure) |
| TLS 1.0, 1.1 | Deprecated |
| **TLS 1.2** | Widely used |
| **TLS 1.3** | Current standard; faster, more secure |

### In One Line

> **TLS provides encrypted, authenticated communication — used by HTTPS; consists of handshake + encrypted data transfer.**

---

## 72. Symmetric Encryption

### Definition

**Symmetric encryption** uses the **same key for both encryption and decryption**.

### Key Points

- **Fast** → Efficient for bulk data encryption.
- **Problem:** Key distribution — how to securely share the key?
- Used for: encrypting actual data after TLS handshake.
- Examples: **AES (Advanced Encryption Standard)**, DES, 3DES, ChaCha20.

```text
Plaintext ──[Key]──→ Ciphertext ──[Key]──→ Plaintext
           Encrypt                Decrypt
           (same key both ways)
```

### In One Line

> **Symmetric encryption uses one key for encrypt + decrypt — fast but requires secure key sharing.**

---

## 73. Asymmetric Encryption

### Definition

**Asymmetric encryption** uses a **key pair — public key (encrypt) and private key (decrypt)** — mathematically linked but computationally infeasible to derive one from the other.

### Key Points

- **Public key** → Can be shared openly; used to encrypt.
- **Private key** → Kept secret by owner; used to decrypt.
- **Slow** → Not used for bulk data; used for key exchange and signatures.
- Examples: **RSA**, ECC, Diffie-Hellman.

```text
Sender:  Plaintext ──[Public Key]──→ Ciphertext ──→ Receiver
Receiver: Ciphertext ──[Private Key]──→ Plaintext
```

### Use Cases

- **TLS key exchange** → Agree on symmetric key securely.
- **Digital signatures** → Sign with private key; verify with public key.
- **SSH** → Authenticate without passwords.

### In One Line

> **Asymmetric encryption uses public key to encrypt, private key to decrypt — secure key exchange without sharing a secret.**

---

## 74. Public Key & Private Key

| Feature | Public Key | Private Key |
|---------|------------|-------------|
| Sharing | Share with anyone | Never share; keep secret |
| Used for | Encryption, verifying signatures | Decryption, creating signatures |
| Generated | Together as a pair | Together as a pair |
| If compromised | Can be revoked; get new cert | All security is lost — regenerate |

### Digital Signature (Signing)

```text
Sender:   Hash of data ──[Private Key]──→ Signature (sent with data)
Receiver: Signature ──[Public Key]──→ Hash → compare with received data hash
→ If match: authentic + unmodified
```

### In One Line

> **Public key is shared freely for encryption/verification; private key is secret for decryption/signing.**

---

## 75. Digital Certificate

### Definition

A **digital certificate** (X.509 certificate) is an **electronic document** that **binds a public key to an identity** (domain, organization), signed by a trusted **Certificate Authority (CA)**.

### Certificate Contents

```text
┌────────────────────────────────┐
│ Subject: www.google.com        │
│ Public Key: [RSA 2048-bit key] │
│ Issuer: DigiCert Inc.          │
│ Valid From: 2024-01-01         │
│ Valid To: 2025-01-01           │
│ Signature: [CA's digital sig]  │
└────────────────────────────────┘
```

### In One Line

> **A digital certificate ties a public key to a domain/identity, signed by a trusted CA to prevent impersonation.**

---

## 76. Certificate Authority (CA)

### Definition

A **Certificate Authority (CA)** is a **trusted third-party organization** that **issues, validates, and revokes** digital certificates.

### How It Works

```text
1. Website owner creates key pair; sends Certificate Signing Request (CSR) to CA
2. CA verifies domain ownership
3. CA signs certificate with its private key
4. Browser trusts CAs in its "root store" (pre-installed)
5. Browser verifies certificate signature using CA's public key
```

### CA Hierarchy

```text
Root CA (most trusted, offline)
    ↓
Intermediate CA (signs end-entity certs)
    ↓
End-Entity Certificate (your website cert)
```

### Well-Known CAs

DigiCert, Let's Encrypt (free), GlobalSign, Comodo, Sectigo.

### In One Line

> **CA is the trusted authority that signs digital certificates — browsers trust CAs to verify website identity.**

---

## 77. Firewall

### Definition

A **firewall** is a **network security device (hardware or software)** that **monitors and controls incoming and outgoing network traffic** based on predefined security rules.

### Types of Firewalls

| Type | Description |
|------|-------------|
| **Packet Filter** | Inspects IP/port headers; stateless; fast but limited |
| **Stateful Firewall** | Tracks connection state; smarter filtering |
| **Application Firewall (WAF)** | Inspects application-layer data (HTTP content) |
| **Next-Gen Firewall (NGFW)** | DPI + IDS/IPS + app awareness + user identity |

### Firewall Rules

```text
Allow TCP from 192.168.1.0/24 to any port 80
Allow TCP from 192.168.1.0/24 to any port 443
DENY all other inbound traffic
```

### In One Line

> **A firewall filters traffic by rules — allowing legitimate traffic and blocking threats.**

---

## 78. VPN (Virtual Private Network)

### Definition

A **VPN** creates an **encrypted tunnel** over a public network (Internet), allowing remote users or offices to communicate as if on a **private network**.

### How VPN Works

```text
User Device ──[Encrypted Tunnel]──→ VPN Server ──→ Internet
            (public Internet used, but traffic is encrypted)
```

### Types

| Type | Use Case |
|------|---------|
| **Remote Access VPN** | Individual user connects to corporate network from home |
| **Site-to-Site VPN** | Connects two office networks permanently |
| **SSL/TLS VPN** | Browser-based; no special client needed |

### VPN Protocols

- **OpenVPN** → Open-source; very secure.
- **WireGuard** → Modern, fast, minimal.
- **IPSec** → Standard; used in site-to-site.
- **L2TP/IPSec** → L2TP tunneling + IPSec encryption.

### Benefits

- **Privacy** → ISP and others can't see traffic contents.
- **Security** → Encrypted, even on public Wi-Fi.
- **Access** → Bypass geo-restrictions; access corporate resources.

### In One Line

> **VPN creates an encrypted tunnel over the Internet — provides privacy, security, and remote access to private networks.**

---

## 79. Proxy

### Definition

A **proxy server** is an **intermediary server** that sits between clients and the Internet, forwarding requests on behalf of clients.

### How It Works

```text
Client ──→ Proxy Server ──→ Internet (Web Server)
                         ←── Response ──→ Client
```

### Types of Proxies

| Type | Description |
|------|-------------|
| **Forward Proxy** | Client-side; hides client from Internet; caching, filtering |
| **Reverse Proxy** | Server-side; hides server from Internet; load balancing, SSL termination |
| **Transparent Proxy** | Client doesn't know it's proxied; used by ISPs for caching |
| **SOCKS Proxy** | Works at socket level; handles any protocol |

### Proxy vs VPN

| Feature | Proxy | VPN |
|---------|-------|-----|
| Encryption | Usually no | Yes |
| OS-level | No (app-specific) | Yes (all traffic) |
| Speed | Faster | Slightly slower |
| Privacy | Partial | Full |

### In One Line

> **A proxy forwards requests between client and server — forward proxy hides clients; reverse proxy protects servers.**

---
---

# QUICK REVISION TABLE — ALL TOPICS

| # | Topic | Key Point |
|---|-------|-----------|
| 1 | Computer Network | Devices connected to share data/resources |
| 2 | Internet | Global network of networks using TCP/IP |
| 3 | Client and Server | Client requests; server responds |
| 4 | Network Protocol | Rules for communication format and timing |
| 5 | Packet | Data broken into header + payload chunks |
| 6 | Port | Logical endpoint identifying a service (0–65535) |
| 7 | Socket | IP + Port + Protocol = communication endpoint |
| 8 | IP Address | Unique numerical ID for network device |
| 9 | MAC Address | 48-bit hardware NIC address for LAN delivery |
| 10 | Bandwidth | Max data capacity; throughput is actual rate |
| 11 | Latency | Propagation + Transmission + Processing + Queuing delay |
| 12 | OSI Model | 7 layers: Physical→DataLink→Network→Transport→Session→Presentation→Application |
| 13 | Physical Layer | Layer 1; transmits raw bits; hubs, cables |
| 14 | Data Link Layer | Layer 2; frames, MAC, CRC, switches |
| 15 | Network Layer | Layer 3; IP, routing, packets, routers |
| 16 | Transport Layer | Layer 4; TCP/UDP, ports, end-to-end delivery |
| 17 | Application Layer | Layer 7; HTTP, DNS, FTP, SMTP |
| 18 | TCP/IP Model | 4 layers: Network Access→Internet→Transport→Application |
| 19 | OSI vs TCP/IP | OSI=7 layer reference; TCP/IP=4 layer practical |
| 20 | Encapsulation | Add headers at each layer going down |
| 21 | MAC Address | First 3 bytes=manufacturer; last 3=device; ARP maps IP→MAC |
| 22 | Ethernet | Wired LAN standard; frames; CSMA/CD; IEEE 802.3 |
| 23 | Frame | Layer 2 PDU; src/dst MAC + payload + FCS |
| 24 | ARP | Resolves IP→MAC via broadcast request + unicast reply |
| 25 | Hub | Layer 1; broadcasts to all; one collision domain |
| 26 | Switch | Layer 2; MAC table; forwards to specific port; one per collision domain |
| 27 | Router | Layer 3; routes between networks using IP and routing table |
| 28 | Hub vs Switch | Hub=broadcast,dumb; Switch=forward,smart |
| 29 | Switch vs Router | Switch=MAC/LAN; Router=IP/between networks |
| 30 | Broadcast/Unicast/Multicast | One-to-all / One-to-one / One-to-group |
| 31 | IPv4 | 32-bit, dotted decimal, 4.3B addresses, 5 classes |
| 32 | IPv6 | 128-bit, hex, 3.4×10³⁸ addresses, no NAT needed |
| 33 | Public vs Private IP | Public=globally routable; private=local, RFC 1918 ranges |
| 34 | Static vs Dynamic IP | Static=manual,fixed; Dynamic=DHCP,changes |
| 35 | Loopback | 127.0.0.1 (localhost); tests local TCP/IP stack |
| 36 | Subnet Mask | Separates network and host bits of an IP |
| 37 | CIDR | /prefix notation; replaces classful addressing |
| 38 | Subnetting | Borrow host bits; 2^n subnets; 2^h−2 hosts |
| 39 | Network/Broadcast Address | First=network; Last=broadcast; both non-assignable |
| 40 | Default Gateway | Router IP; used for traffic outside local subnet |
| 41 | Routing | Select best path for packets across networks |
| 42 | Routing Table | Network→next-hop mappings; longest prefix match |
| 43 | Static vs Dynamic Routing | Static=manual; Dynamic=auto via OSPF/BGP/RIP |
| 44 | NAT | Translates private→public IP; PAT uses ports for many-to-one |
| 45 | TCP | Reliable, ordered, connection-oriented; ACK + retransmit |
| 46 | UDP | Fast, connectionless, unreliable; 8-byte header |
| 47 | TCP vs UDP | TCP=reliable,slow; UDP=fast,unreliable |
| 48 | Port Numbers | 0–1023=well-known; 1024–49151=registered; 49152+=ephemeral |
| 49 | Connection vs Connectionless | TCP=handshake,state; UDP=no setup,stateless |
| 50 | Flow/Congestion Control | Flow=protect receiver buffer; Congestion=protect network |
| 51 | Sliding Window | Multiple packets in flight; window slides on ACK |
| 52 | 3-Way Handshake | SYN→SYN-ACK→ACK establishes TCP connection |
| 53 | SYN/SYN-ACK/ACK | Start/Accept/Confirm TCP connection flags |
| 54 | 4-Way Termination | FIN→ACK→FIN→ACK closes TCP connection |
| 55 | Seq/ACK Number | Seq=byte position; ACK=next expected byte |
| 56 | Retransmission | Triggered by timeout or 3 dup ACKs |
| 57 | TCP States | LISTEN→ESTABLISHED→FIN_WAIT→TIME_WAIT→CLOSED |
| 58 | TIME_WAIT/CLOSE_WAIT | TIME_WAIT=client waits 2×MSL; CLOSE_WAIT=server waits for app |
| 59 | TCP Flow Control | rwnd limits sender to receiver's buffer capacity |
| 60 | TCP Congestion Control | Slow start (exponential) → congestion avoidance (linear) |
| 61 | DNS | Translates domain names to IPs; DORA: Root→TLD→Authoritative |
| 62 | DHCP | Auto IP assignment via DORA (Discover→Offer→Request→ACK) |
| 63 | HTTP | Stateless request-response web protocol; port 80 |
| 64 | HTTPS | HTTP + TLS encryption; port 443; certificate required |
| 65 | HTTP Methods | GET=read, POST=create, PUT=replace, PATCH=update, DELETE=remove |
| 66 | HTTP Status Codes | 2xx=OK, 3xx=redirect, 4xx=client error, 5xx=server error |
| 67 | HTTP Headers | Metadata in requests/responses; Content-Type, Authorization, Cookie |
| 68 | Cookies & Sessions | Cookie=browser storage; Session=server storage with cookie ID |
| 69 | HTTP/1.1 vs /2 vs /3 | 1.1=serial; 2=multiplexed/TCP; 3=multiplexed/QUIC(UDP) |
| 70 | HTTP vs HTTPS | HTTP=plaintext port 80; HTTPS=encrypted port 443 |
| 71 | SSL/TLS | TLS secures connections: handshake→session key→encrypted data |
| 72 | Symmetric Encryption | One key for both encrypt+decrypt; AES; fast |
| 73 | Asymmetric Encryption | Public=encrypt; Private=decrypt; RSA; slow; key exchange |
| 74 | Public/Private Key | Public=shared freely; Private=never share; used for crypto+signing |
| 75 | Digital Certificate | Binds public key to identity; signed by CA (X.509) |
| 76 | Certificate Authority | Trusted org that signs/issues/revokes certificates |
| 77 | Firewall | Filters traffic by rules; packet/stateful/application/NGFW |
| 78 | VPN | Encrypted tunnel over Internet; remote access + privacy |
| 79 | Proxy | Intermediary; forward=hide client; reverse=protect server |

---

*Notes prepared for: College Exams | Placement Preparation | Technical Interviews | Quick Revision*
