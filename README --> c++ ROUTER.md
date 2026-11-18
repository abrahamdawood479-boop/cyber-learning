# Custom C++ Network Router & Security Kernel (In Progress)

A modular, from-scratch network router and security engine written in C++ using POSIX sockets (`<arpa/inet.h>`). This project implements core OSI Layer 2–4 networking protocols, packet header structures, and security subsystems at a low level without relying on high-level networking abstractions.

> **Project Status:** Active Development — Currently building out the **DHCP Server Engine (RFC 2131)** and UDP broadcast socket pipeline.

---

## Architecture & Subsystems

The router kernel is designed around modular subsystems that boot and operate concurrently (multithreading planned):

* **DHCP Server Engine (`run_dhcp_server`):** Implements the BOOTP/DHCP packet frame (`op`, `htype`, `hlen`, `hops`, `xid`, `ciaddr`, `yiaddr`, `siaddr`, `giaddr`, `chaddr`, `magic_cookie`, and `options`), UDP port `67`/`68` broadcast handling, IPv4 address pool management (`192.168.1.50` – `192.168.1.100`), and the DORA (Discover, Offer, Request, Acknowledge) state machine.
* **Routing & Switching Engines (`run_router`, `run_switch`):** Layer 3 packet forwarding and Layer 2 MAC switching foundations.
* **Address Resolution & Translation (`run_arp_table`, `run_nat`):** Hardware MAC-to-IP cache mapping and Network Address Translation.
* **Security & Threat Monitoring (`run_firewall`, `run_ids`, `run_ips`):** Stateful packet filtering, Intrusion Detection, and Intrusion Prevention hooks.

---

## Development Roadmap

### Phase 1: Core DHCP & Socket Pipeline *(Current Focus)*
- [x] RFC-compliant DHCP/BOOTP packet structure (`CONFIGURATION_ENGINE_CONFIGS`) and protocol constants (`DHCP_BOOTREPLY`, `DHCP_BROADCAST`, Ports `67`/`68`)
- [x] IPv4 address pool range definitions (`0xC0A80132` – `0xC0A80164`) and 24-hour (`86400s`) lease configuration
- [x] UDP socket initialization with `SO_BROADCAST` permissions and packet receive loop (`recvfrom`)
- [ ] Complete DORA packet serialization (Offer, Request, Acknowledge) and lease persistence database

### Phase 2: Core Routing & Layer 2/3 Mechanics
- [ ] ARP cache table resolution (`run_arp_table`)
- [ ] Layer 2 MAC address table & frame switching (`run_switch`)
- [ ] NAT translation table for LAN-to-WAN traffic (`run_nat`)
- [ ] Static & dynamic IP routing table lookups (`run_router`)

### Phase 3: Security & Concurrency
- [ ] Multithreaded subsystem execution (`std::thread`) for concurrent packet inspection
- [ ] Stateful firewall rule filtering and IDS/IPS anomaly signatures

---

## Key Concepts Demonstrated
* **Network Protocols & Byte Ordering:** Manual handling of network byte order (`htons`, `inet_pton`), hexadecimal IP representations, and broadcast flags (`0x8000`).
* **Low-Level Socket Programming:** Raw Linux/POSIX datagram (`SOCK_DGRAM`) socket creation, option flags (`setsockopt`), and buffer management.
* **Systems & Security Design:** Separation of control plane and security monitoring layers (Firewall, IDS, IPS).
