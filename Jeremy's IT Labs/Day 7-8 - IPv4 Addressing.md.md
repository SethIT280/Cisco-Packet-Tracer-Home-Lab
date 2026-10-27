# Day 7-8 - IPv4 Addressing

## Task
Configure R1's interfaces so that all devices can communicate with each other.

## Topology
<img src="../Jeremy's IT Labs/images/day8-img1.png" width="600" >

| Network | Router Interface | Router IP | PC | PC IP |
|---|---|---|---|---|
| 15.0.0.0/8 | G0/0 | 15.255.255.254 | PC1 | 15.0.0.1 |
| 182.98.0.0/16 | G0/1 | 182.98.255.254 | PC2 | 182.98.0.1 |
| 201.191.20.0/24 | G0/2 | 201.191.20.254 | PC3 | 201.191.20.1 |

## What I Did

### 1. Configured the IP addresses on PC1, PC2, and PC3
<img src="../Jeremy's IT Labs/images/day8-img4.png" width="600" >

<img src="../Jeremy's IT Labs/images/day8-img2.png" width="600" >

<img src="../Jeremy's IT Labs/images/day8-img3.png" width="600" >

### 2. Renamed the router with the `hostname` command
```
Router> enable
Router# configure terminal
Router(config)# hostname R1
```
<img src="../Jeremy's IT Labs/images/day8-img5.png" width="600" >

### 3. Viewed R1's interfaces with `show ip interface brief`
```
R1(config)# do show ip interface brief
```
All interfaces were unassigned and administratively down.

<img src="../Jeremy's IT Labs/images/day8-img6.png" width="600" >

### 4. Configured each interface
**G0/0**
```
R1(config)# interface g0/0
R1(config-if)# ip address 15.255.255.254 255.0.0.0
R1(config-if)# no shutdown
```
<img src="../Jeremy's IT Labs/images/day8-img7.png" width="600" >

**G0/1**
```
R1(config)# interface g0/1
R1(config-if)# ip address 182.98.255.254 255.255.0.0
R1(config-if)# no shutdown
```
<img src="../Jeremy's IT Labs/images/day8-img8.png" width="600" >

**G0/2**
```
R1(config)# interface g0/2
R1(config-if)# ip address 201.191.20.254 255.255.255.0
R1(config-if)# no shutdown
```
<img src="../Jeremy's IT Labs/images/day8-img9.png" width="600" >

### 5. Verified the interfaces are up
```
R1(config-if)# do show ip interface brief
```
All three interfaces show `up/up` with the correct IP addresses.

<img src="../Jeremy's IT Labs/images/day8-img10.png" width="600" >

### 6. Pinged PC2 and PC3 from PC1 to test connectivity
Both pings succeeded. The first packet timed out because of the initial ARP request.

<img src="../Jeremy's IT Labs/images/day8-img11.png" width="600" >

## Result
PC1 can reach PC2 (182.98.0.1) and PC3 (201.191.20.1) through R1.

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
