# Day 5-6 - Ethernet LAN Switching

# Day 6 - ARP, ICMP, and the Switch MAC Address Table

## Task
- Analyze the messages sent over the network when a device is pinged
- Analyze the MAC address table on a switch

<img src="../Jeremy's IT Labs/images/day6-img1.png" width="600">

## What I Did

### 1. Pinged PC3 from PC1

<img src="../Jeremy's IT Labs/images/day6-img2.png" width="600">

- PC1 doesn't know PC3's MAC address, so it sends an **ARP request** first
- The switch **floods** the ARP request out every port except the one it came in on
- Devices that don't own 192.168.1.3 (PC2, PC4) drop the frame
- PC3 sends an **ARP reply** back to PC1
- Once PC1 has PC3's MAC address, it sends the **ICMP echo request** (the ping)

<img src="../Jeremy's IT Labs/images/day6-img3.png" width="600">
<img src="../Jeremy's IT Labs/images/day6-img11.png" width="200">

### 2. Analyzed the ARP request and reply frames

**ARP request**

<img src="../Jeremy's IT Labs/images/day6-img4.png" width="400">
<img src="../Jeremy's IT Labs/images/day6-img5.png" width="400">

- Sent to the **broadcast** MAC address `FFFF.FFFF.FFFF` because PC1 doesn't know who owns 192.168.1.3
- Source MAC is PC1 (`00D0.D3AD.9CAB`)
- Target MAC is `0000.0000.0000` because it's the unknown being asked for

**ARP reply**

<img src="../Jeremy's IT Labs/images/day6-img10.png" width="400">

- Sent as a **unicast** straight back to PC1's MAC (`00D0.D3AD.9CAB`)
- Source MAC is PC3 (`0004.9A6E.D870`), which answers PC1's question

<img src="../Jeremy's IT Labs/images/day6-img7.png" width="400">

### 3. Generated traffic to fill the switch MAC address tables
- Sent pings between the PCs so the switches could learn each PC's MAC address

### 4. Viewed the MAC address table on SW2
- Connected to SW2 and ran:

```
SW2> enable
SW2# show mac address-table
```

<img src="../Jeremy's IT Labs/images/day6-img8.png" width="400">


### 5. Cleared the dynamic MAC address table on SW2

```
SW2# clear mac address-table dynamic
SW2# show mac address-table
```

<img src="../Jeremy's IT Labs/images/day6-img9.png" width="400">

- The table is empty after clearing, and the switch relearns addresses as new traffic arrives

## What I Learned

### Ethernet Frame
- **Preamble**
  - Used to synchronize the receiver with the frame
  - Bit pattern `10101010`
  - 7 bytes in length
- **SFD (Start Frame Delimiter)**
  - Marks the end of the preamble and the start of the rest of the frame
  - Bit pattern `10101011`
  - 1 byte in length
- **Destination**
  - MAC address of the receiving device
  - 6 bytes in length
- **Source**
  - MAC address of the sending device
  - 6 bytes in length
- **Type/Length field**
  - If the value is 1500 or less, it indicates the length of the payload
  - If the value is 1536 or more, it indicates the EtherType, for example IPv4 (`0x0800`), IPv6 (`0x86DD`), or ARP (`0x0806`)
  - 2 bytes in length
- **Data (Payload)**
  - The encapsulated Layer 3 packet
  - 46-1500 bytes in length
- **FCS (Frame Check Sequence)**
  - Contains the CRC (Cyclic Redundancy Check), which detects errors in the frame
  - 4 bytes in length

### MAC Address (Media Access Control)
- Used to identify devices within the LAN
- A MAC address is 48 bits (6 bytes)
- The first 3 bytes are the OUI (identifies the manufacturer)
- The last 3 bytes are assigned by the manufacturer to the specific device
- Broadcast MAC address: `FFFF.FFFF.FFFF`

### Ethernet and Switch

#### MAC Address Table
- A switch builds its MAC address table by learning source MAC addresses from incoming frames
- By default, a dynamic entry is removed after 5 minutes of inactivity
- View the table: `show mac address-table`
- Clear dynamic entries: `clear mac address-table dynamic`

#### ARP (Address Resolution Protocol)
- Maps a known IP address to an unknown MAC address
- ARP uses two messages: **ARP request** and **ARP reply**
  - Request: broadcast that finds who owns the IP
  - Reply: unicast response with the MAC address
- View the ARP table on Cisco devices: `show ip arp`
- View the ARP table on end-user devices: `arp -a`
