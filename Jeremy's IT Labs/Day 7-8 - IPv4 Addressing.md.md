## Day 7-8 - IPv4 Addressing

### Task

### What I Did

### What I Learned

#### IP Address Classes

| Class | First Bits | Default Mask | First Octet Range | Networks | Hosts per Network | Purpose |
|-------|-----------|--------------|-------------------|----------|-------------------|---------|
| A | `0xxxxxxx` | /8 (255.0.0.0) | 0 - 127 | 128 | 16,777,214 | Unicast |
| B | `10xxxxxx` | /16 (255.255.0.0) | 128 - 191 | 16,384 | 65,534 | Unicast |
| C | `110xxxxx` | /24 (255.255.255.0) | 192 - 223 | 2,097,152 | 254 | Unicast |
| D | `1110xxxx` | N/A | 224 - 239 | N/A | N/A | Multicast |
| E | `1111xxxx` | N/A | 240 - 255 | N/A | N/A | Experimental |

#### Special Addresses

- **Network address:** all host bits set to 0 (example: `x.x.x.0` in a /24)
- **Broadcast address:** all host bits set to 1 (example: `x.x.x.255` in a /24)
- **Loopback address:** `127.x.x.x`
  - Traffic sent to this address loops back to the sending device.

#### Calculating Hosts

Number of usable hosts, where `n` is the number of host bits:

```
(2^n) - 2
```

The 2 subtracted are the network address and the broadcast address.

### Router CLI

| Command | Purpose |
|---------|---------|
| `R1(config-if)# description <text>` | Adds a description to the interface |
| `R1(config-if)# ip address <ip-address> <subnet-mask>` | Sets the IP address of the interface |
| `R1(config-if)# no shutdown` | Enables the interface |
| `R1# show interfaces description` | Shows the descriptions set on each interface |
| `R1# show ip interface brief` | Shows interface name, IP address, method used to set the IP, status, and protocol |

**Notes**
- The address entered on the interface is typically used as the default gateway for hosts on that network.
- In `show ip interface brief`:
  - **Status** = Layer 1 status
  - **Protocol** = Layer 2 status
- Router interfaces are administratively down/down by default until you enter `no shutdown`.
