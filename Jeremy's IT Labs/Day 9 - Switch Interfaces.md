# Day 9 - Switch Interfaces

## Task
Configure the R1, SW1, and SW2 interfaces with a description, speed, and duplex.


## Topology
<img src="../Jeremy's IT Labs/images/day9-img1.png" width="600" >

## What I Did

### 1. Configured PC1, PC2, PC3 and PC4's IP address
All other pc are configured the same way. 

<img src="../Jeremy's IT Labs/images/day9-img3.png" width="600" >

### 2. Configured R1's G0/0 interface
First I set the IP address and activated the interface.
```
R1(config)# int g0/0
R1(config-if)# ip address 172.16.255.254 255.255.0.0
R1(config-if)# no shutdown
```
<img src="../Jeremy's IT Labs/images/day9-img2.png" width="600" >

Added the speed, duplex, and description.
```
R1(config-if)# duplex full
R1(config-if)# speed 1000
R1(config-if)# description ## to SW1 ##
R1(config-if)# no shutdown
```
<img src="../Jeremy's IT Labs/images/day9-img5.png" width="600" >

### 3. Verified R1's interfaces
```
R1(config-if)# do show ip interface brief
```
G0/0 is up/up with the correct IP.

<img src="../Jeremy's IT Labs/images/day9-img6.png" width="600" >

### 4. Added descriptions to R1's unused interfaces
```
R1(config-if)# int range g0/1-2
R1(config-if-range)# description ## not in use ##
```
<img src="../Jeremy's IT Labs/images/day9-img7.png" width="600" >

### 5. Viewed the running config and saved it
```
R1# show running-config
R1# write memory
```
<img src="../Jeremy's IT Labs/images/day9-img8.png" width="400" >

<img src="../Jeremy's IT Labs/images/day9-img9.png" width="600" >

### 6. Configured SW1
Renamed the switch.
```
Switch(config)# hostname SW1
```
<img src="../Jeremy's IT Labs/images/day9-img4.png" width="600" >

Viewed the switch interfaces.
```
SW1> show ip interface brief
```
<img src="../Jeremy's IT Labs/images/day9-img10.png" width="600" >

Set the speed, duplex, and description on the interfaces connected to R1 and SW1. 
```
SW1(config)# int g0/1
SW1(config-if)# speed 1000
SW1(config-if)# duplex full
SW1(config-if)# description ## to R1 ##
SW1(config-if)# int g0/2
SW1(config-if)# speed 1000
SW1(config-if)# duplex full
SW1(config-if)# description ## to SW2 ##
```
<img src="../Jeremy's IT Labs/images/day9-img11.png" width="600" >

Shut down the unused ports and described them.
```
SW1(config-if)# int range f0/3 - 24
SW1(config-if-range)# shutdown
SW1(config-if-range)# description ## not in use ##
```
<img src="../Jeremy's IT Labs/images/day9-img12.png" width="600" >

<img src="../Jeremy's IT Labs/images/day9-img13.png" width="600" >

### 7. Verified SW1 with `show interfaces status`
```
SW1# show interfaces status
```
Fa0/1 and Fa0/2 (end hosts) and Gig0/1 and Gig0/2 are connected. Fa0/3 through Fa0/24 are disabled.

<img src="../Jeremy's IT Labs/images/day9-img14.png" width="600" >

### 9. Configured SW2 the same way as SW1
SW2 got the same configuration as SW1, the only difference is the interface that connects to SW1.
```
SW2(config)# int g0/1
SW2(config-if)# description ## to SW1 ##
```
Verified with `show interfaces status`.
```
SW2# show interfaces status
```
<img src="../Jeremy's IT Labs/images/day9-img16.png" width="600" >

## What I Learned
- Router interfaces start administratively down, so they need `no shutdown` to come up.
- Speed and duplex can be set manually with `speed` and `duplex`, instead of relying on auto-negotiation.
- Descriptions make it easier to tell what each interface connects to.
- Unused switch ports should be shut down with `shutdown`, and `interface range` saves time when configuring many ports.
- `show ip interface brief` and `show interfaces status` verify the configuration.
- Configs need to be saved with `write memory`, or they're lost on reboot.


### Half-Duplex vs Full-Duplex
- **Half-duplex:** send OR receive, not both at the same time. Uses CSMA/CD to detect collisions and recover from them.
- **Full-duplex:** send AND receive at the same time. No collisions.

### Hubs
- Layer 1 devices.
- Send incoming signals out every other port.
- All devices on a hub share one collision domain.

### Switches

- Switches can run in both half-duplex and full-duplex.
- Each switch port is its own collision domain.
- Speed and duplex default to **auto** (auto-negotiated).
- Interfaces come up/up automatically when connected to another device.

| Command | Purpose |
|---------|---------|
| `SW1(config-if)# speed <10 \| 100 \| 1000 \| auto>` | Sets the connection speed |
| `SW1(config-if)# duplex <full \| half \| auto>` | Sets the duplex mode |


### Switch Interface Errors

View with the `show interfaces` command.

| Counter | Triggers when |
|---------|---------------|
| Runts | A frame is smaller than the minimum frame size (64 bytes) |
| Giants | A frame is larger than the maximum frame size (1518 bytes) |
| CRC | An error is found in the FCS (Frame Check Sequence) |
| Frame | A frame has an incorrect format |
| Input errors | Total of all input errors (runts, giants, CRC, frame, and others) |
| Output errors | The switch fails to send a frame because of an error |
