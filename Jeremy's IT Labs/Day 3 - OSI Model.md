# Day 3 - OSI Model

### Task
Use Packet Tracer's simulation mode to analyze the traffic being sent throughout the network.

<img src="../Jeremy's IT Labs/images/day3-img1.png" width="600" >

### What I Did

1. **Let network traffic propagate.** In simulation mode, I let the network run and watched the event list fill with traffic.

   <img src="../Jeremy's IT Labs/images/day3-img5.png" width="600" >

2. **Analyzed STP**
   - STP is a Layer 2 protocol, so it BPDU(Bridge Protocol Data Unit) encapsulated in an Ethernet frame.
   - On SW1, the STP BPDU came in on `GigabitEthernet0/1` from SW2. SW1 then sent it out of `FastEthernet0/1` and `GigabitEthernet0/2`.

   <img src="../Jeremy's IT Labs/images/day3-img2.png" width="600" >

   <img src="../Jeremy's IT Labs/images/day3-img3.png" width="600" >

3. **Analyzed OSPF**
   - OSPF is a routing protocol, so its traffic includes Layer 3 information. Routers are Layer 3 devices that communicate using IP.
   - R2 multicast an OSPF Hello packet to `224.0.0.5` out of `GigabitEthernet0/0`.

   <img src="../Jeremy's IT Labs/images/day3-img4.png" width="600" >

4. **Ran `ipconfig /release` and `ipconfig /renew` on PC1** to generate Layer 7 traffic with a DHCP message.

   <img src="../Jeremy's IT Labs/images/day3-img6.png" width="600" >

   <img src="../Jeremy's IT Labs/images/day3-img7.png" width="600" >

   - At Layer 7, a DHCP Discover packet is created. Packet Tracer uses the TCP/IP model, so Layers 5 and 6 are combined with Layer 7.

     <img src="../Jeremy's IT Labs/images/day3-img9.png" width="600" >

   - At Layer 4, the PDU is encapsulated into a UDP segment, which is used to deliver data quickly. The client uses source port 68 and the server uses destination port 67.

     <img src="../Jeremy's IT Labs/images/day3-img8.png" width="600" >
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
