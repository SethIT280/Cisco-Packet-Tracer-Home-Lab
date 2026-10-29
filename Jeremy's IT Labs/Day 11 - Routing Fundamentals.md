# Day 11 - Routing Fundamentals

## Task
<!-- Add today's task here -->

## What I Did
<!-- Add what you did here (labs, Packet Tracer, videos, etc.) -->

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
