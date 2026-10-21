# Day 5-6 - Ethernet LAN Switching

## Task

## What I Did

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
