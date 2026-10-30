# Day 11 - Routing Fundamentals

## Topology
<img src="../Jeremy's IT Labs/images/Day11(L1)-img1.png" width="600" >

## Lab 1 
### Task
Configure static routes on all routers so that PC1 and PC2 can communicate.

### What I Did

# Day 11 - Lab 2

## Task
Troubleshoot each router to fix the misconfiguration.

## What I Did

### R1 Troubleshooting
1. Ran `show ip interface brief` to confirm the interfaces were up and the IP addresses were correct. In this case they were configured correctly.
```
R1(config)# do show ip interface brief
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img2.png" width="600" >

2. Ran `show ip route` to view the routing table and check the routes. This is where the misconfiguration was: the static route to 192.168.3.0/24 had its next hop set to 192.168.12.3 when it should be 192.168.12.2.
```
R1(config)# do show ip route
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img3.png" width="600" >

3. Removed the incorrect route with `no ip route`. It has to be removed first, because adding the correct route without removing the old one would just create a second route.
```
R1(config)# no ip route 192.168.3.0 255.255.255.0 192.168.12.3
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img4.png" width="600" >

4. Added the route back with the correct next hop.
```
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.12.2
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img5.png" width="600" >

### R2 Troubleshooting
1. Ran `show ip interface brief` to confirm everything was correct. It was.
```
R2(config)# do show ip interface brief
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img6.png" width="600" >

2. Ran `show ip route` and saw that the static route to 192.168.3.0/24 (the network PC2 is on) used the wrong exit interface. It was set to G0/0, which leads back toward PC1, instead of G0/1.
```
R2(config)# do show ip route
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img7.png" width="600" >

3. Removed the route with `no ip route`.
```
R2(config)# no ip route 192.168.3.0 255.255.255.0 G0/0
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img8.png" width="600" >

4. Added it back, specifying the correct exit interface.
```
R2(config)# ip route 192.168.3.0 255.255.255.0 G0/1
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img9.png" width="600" >

### R3 Troubleshooting
1. Ran `show ip interface brief` and found that G0/0 had the wrong IP address. It was 192.168.23.3 instead of 192.168.13.3.
```
R3(config)# do show ip interface brief
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img10.png" width="600" >

2. Entered the G0/0 interface and changed the address with the `ip address` command. Unlike routes, an interface IP doesn't need to be removed first. The new address replaces the old one.
```
R3(config)# int g0/0
R3(config-if)# ip address 192.168.13.3 255.255.255.0
R3(config-if)# do show ip interface brief
```
<img src="../Jeremy's IT Labs/images/Day11(L2)-img11.png" width="600" >

### Test Ping
Pinged PC3 (192.168.3.1) from PC1 to confirm the fixes worked.
<img src="../Jeremy's IT Labs/images/Day11(L2)-img12.png" width="600" >

## What I Learned

### Routing Basics
- **Static routes** are configured manually.
- **Dynamic routes** are added automatically by routing protocols like OSPF.
- The **routing table** is where all routes are stored.
  - If a packet's destination isn't in the routing table, the router drops the packet.
- The next router in line toward the destination is called the **next hop**.
- To view the routing table, use `show ip route`.
- If multiple routes match a destination, the router uses the one with the **longest prefix match**.
- Clients must set their **default gateway** to the router's IP to send traffic outside the LAN.

### Connected Routes
- Marked with **C** in the routing table.
- A route to the network an interface is connected to.
- Uses the prefix of the network the interface is connected to.

### Local Routes
- Marked with **L** in the routing table.
- A route to the exact IP address configured on the router's interface.
- Uses a prefix length of **/32**.

### Static Routes
- Marked with **S** in the routing table.
- Specify the exit interface and next-hop IP:
  ```
  R1(config)# ip route <network> <netmask> <exit-interface> <next-hop-ip>
  ```
- Specify the exit interface only:
  ```
  R1(config)# ip route <network> <netmask> <exit-interface>
  ```
- Specify the next-hop IP only:
  ```
  R1(config)# ip route <network> <netmask> <next-hop-ip>
  ```

### Default Routes
- Often used to send packets to the internet.
- Network address: `0.0.0.0/0`
- The `*` code marks a candidate default route. A static default route is shown as `S*`.
- Configure a default route:
  ```
  R1(config)# ip route 0.0.0.0 0.0.0.0 <next-hop-ip>
  ```
