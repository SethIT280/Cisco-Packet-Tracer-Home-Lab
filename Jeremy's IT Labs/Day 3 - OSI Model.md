# Day 3 - OSI Model

## Task
Analyze traffic being sent throughout the network.

## What I Did

## What I Learned

### OSI / TCP/IP Model

| Layer | Name | PDU | Key Points |
|-------|------|-----|------------|
| 7 | Application | Data | HTTP/HTTPS, DNS |
| 6 | Presentation | Data | Encryption/decryption (TLS/SSL), data translation |
| 5 | Session | Data | Opens, manages, and closes communication between nodes |
| 4 | Transport | Segment | TCP/UDP, ports |
| 3 | Network | Packet | IP addresses, routing, routers |
| 2 | Data Link | Frame | MAC addresses, switches, ARP |
| 1 | Physical | Bits | Ethernet, radio waves |

### Layer Details

- **Application (Layer 7)**
  - Interacts with the application the user is using, like Firefox
  - Holds protocols like HTTP and HTTPS
  - Identifies communication partners using things like DNS
  - Often grouped together with the Presentation and Session layers (5-7) in the TCP/IP model
- **Presentation (Layer 6)**
  - Handles encryption and decryption of data using TLS/SSL
  - Handles translation of that data
- **Session (Layer 5)**
  - Handles opening, maintaining, and closing communication between two nodes
  - Synchronizes communication using things like session tokens
- **Transport (Layer 4)**
  - Delivers data to the correct location/port
  - Uses protocols like TCP and UDP
- **Network (Layer 3)**
  - Delivers a packet to the correct destination using routing and IP addresses
  - Routers operate at this layer
- **Data Link (Layer 2)**
  - Focuses on MAC addresses and frames within the local network
  - Switches operate at this layer
  - ARP request and reply is used to find device MAC addresses
- **Physical (Layer 1)**
  - Transfers bits through physical media like Ethernet and radio waves

### Encapsulation and Decapsulation

This whole process relies on encapsulation to work: the sending host encapsulates the PDU, and the receiving host decapsulates it.

**Encapsulation (sender)**

```
Data
L4 Header | Data
L3 Header | L4 Header | Data
L2 Header | L3 Header | L4 Header | Data | L2 Trailer
Transmission through bits (101100)
```

**Decapsulation (receiver)**

```
Receives bits (101100)
L2 Header | L3 Header | L4 Header | Data | L2 Trailer
L3 Header | L4 Header | Data
L4 Header | Data
Data
```
