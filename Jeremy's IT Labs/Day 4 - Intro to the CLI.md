# Day 4 - Intro to the CLI

## Task
Configure basic device security using the Cisco IOS CLI.

<img src="../Jeremy's IT Labs/images/day4-img1.png" width="600" alt="Complete DHCP configuration wizard">

## What I Did
1. Entered global configuration mode and changed the hostname to `SW1`
```
   Switch> enable
   Switch# configure terminal
   Switch(config)# hostname SW1
```
<img src="../Jeremy's IT Labs/images/day4-img4.png" width="600" alt="Complete DHCP configuration wizard">

2. Set an unencrypted (plaintext) password with the `enable password` command
```
   SW1(config)# enable password <password>
```
<img src="../Jeremy's IT Labs/images/day4-img5.png" width="600" alt="Complete DHCP configuration wizard">
<img src="../Jeremy's IT Labs/images/day4-img6.png" width="600" alt="Complete DHCP configuration wizard">
<img src="../Jeremy's IT Labs/images/day4-img7.png" width="600" alt="Complete DHCP configuration wizard">


3. Encrypted the plaintext passwords with `service password-encryption`
```
   SW1(config)# service password-encryption
```
<img src="../Jeremy's IT Labs/images/day4-img8.png" width="600" alt="Complete DHCP configuration wizard">
<img src="../Jeremy's IT Labs/images/day4-img9.png" width="600" alt="Complete DHCP configuration wizard">

4. Set an MD5-hashed password with `enable secret`
```
   SW1(config)# enable secret <password>
```
<img src="../Jeremy's IT Labs/images/day4-img10.png" width="600" alt="Complete DHCP configuration wizard">
<img src="../Jeremy's IT Labs/images/day4-img11.png" width="600" alt="Complete DHCP configuration wizard">


5. Saved the running configuration to the startup configuration
```
   SW1# write memory
```
<img src="../Jeremy's IT Labs/images/day4-img12.png" width="600" alt="Complete DHCP configuration wizard">


## What I Learned

### Commands
| Command | Purpose |
|---|---|
| `enable` | Enter privileged EXEC mode |
| `write memory` | Save running-config to startup-config |
| `show startup-config` | View the saved configuration |
| `no <command>` | Remove or negate a configuration |
| `exit` | Go back one mode level |
| `?` | View available commands |

### Console Connection
- Use a rollover cable or a USB console cable to connect to network devices CLI.

### Cisco Console Port Settings
| Setting | Value |
|---|---|
| Speed | 9600 bits/s |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |

### Password Security
- `enable password` stores the password in plaintext.
- `service password-encryption` uses **type 7** encryption (weak and easily reversible).
- `enable secret` uses **type 5** (MD5) hashing, which is stronger.
-  , `enable secret` takes priority even if `service password-encryption` has been set.
