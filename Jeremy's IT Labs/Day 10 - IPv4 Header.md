# Day 10 - IPv4 Header

## What I Learned

| Field | Size |
|---|---|
| Version | 4 bits |
| IHL | 4 bits |
| DSCP | 6 bits |
| ECN | 2 bits |
| Total Length | 16 bits |
| Identification | 16 bits |
| Flags | 3 bits |
| Fragment Offset | 13 bits |
| TTL | 8 bits |
| Protocol | 8 bits |
| Header Checksum | 16 bits |
| Source IP | 32 bits |
| Destination IP | 32 bits |
| Options | 0-320 bits |

### Version
- Shows which IP version is being used, e.g. IPv4 = `0100`
- 4 bits in length

### IHL (Internet Header Length)
- Indicates the length of the IP header so the receiver knows where the header ends and the payload starts
- Each 1 value = 4 bytes
- Minimum value: 5 (20 bytes)
- Maximum value: 15 (60 bytes)
- 4 bits in length

### DSCP (Differentiated Services Code Point)
- Used for quality of service (QoS) by prioritizing delay-sensitive network traffic
- 6 bits in length

### ECN (Explicit Congestion Notification)
- Lets a router signal that the network is getting congested, so the sender can slow down how many packets it sends until the network recovers
- 2 bits in length

### Total Length
- Indicates the total length of the packet (header + payload)
- Minimum value: 20
- Maximum value: 65,535
- 16 bits in length

### Identification
- Used to identify which packet a fragment is part of
- All fragments of the same packet have the same identification
- 16 bits in length

### Flags
- 3 bits in length
- **Reserved** (bit 0)
  - Always 0
  - Only there in case a new flag is created
- **DF (Don't Fragment)** (bit 1)
  - Set to 1 if the packet must not be fragmented
- **MF (More Fragments)** (bit 2)
  - Set to 1 if there are more fragments after this one
  - Set to 0 on the last fragment

### Fragment Offset
- Indicates the position of a fragment within the original packet
- 13 bits in length

### TTL (Time to Live)
- The number of hops a packet can take before it gets dropped
- Prevents packets that can't find their destination from looping and overloading the network
- Each router decreases the TTL by 1. Once it hits 0, the packet is dropped
- The default is commonly 64
- 8 bits in length

### Protocol
- Indicates the protocol in the Layer 4 PDU
- Examples:
  - ICMP = 1
  - TCP = 6
  - UDP = 17
- 8 bits in length

### Header Checksum
- Used to detect errors in the header
- 16 bits in length

### Source IP
- IP address of the sender
- 32 bits in length

### Destination IP
- IP address of the receiver
- 32 bits in length

### Options
- Very rarely used
- Examples: Timestamp and Record Route
- Minimum length: 0 bits
- Maximum length: 320 bits
