# Day 9 - Switch Interfaces

## Task

## What I Did

## What I Learned

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
