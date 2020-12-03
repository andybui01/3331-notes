### Link layer services
**framing, link access:**
- encapsulate datagram into frame, adding header, trailer
- channel access if shared medium
- mac addresses used in frame headers to identify source and destination

**flow control:**
- pacing between adjacent sending and receiving nodes

**error detection:**
- errors caused by signal attenuation, "noise"
- receiver detects presence of errors, signals the sender for retransmission or drops frame

**error correction:**
- receiver identifies (and corrects) bit error(s) without resorting to retransmission

**half-duplex and full-duplex:**
- with half duplex, nodes at both ends of link can transmit, but not at same time

### Where is the link layer implemented?
- every host
- implemented in an adaptor (network interface card NIC) or on a chip
    - ethernet card
    - 802.11 card
    - ethernet chipset
    - implements link and physical layer
- attaches into host's system buses (PCI)

### Adaptors communicating
**sending side:**
- encapsulates datagram in frame
- adds error checking bits, rdt, flow control, etc.

**receiving side:**
- looks for errors, rdt, flow control, etc.
- extracts datagram, passes to upper layer at receiving side

### Framing
The physical layer talks in terms of bits. So how do we identify frames within the sequence of bits?

The start of frame is recognized by identifying:
- preamble: seven bytes with pattern `0b10101010`
- start of frame delimiter (SFD): 0b10101011

Structure (bytes):
- preamble (7)
- SFD (1)
- destination mac (6)
- source mac (6)
- type/length (2 bytes)
- payload (46-1500)
- FCS/CRC (4)
- inter frame gap (12 bytes)

### Error detection
**Simple parity**
For every d-bits (e.g. d=7) add a parity bit, 1 if number of 1s is odd, 0 if even.

|Message chunk|Parity bit|
|---|---|
|0b00**1**0**11**0|1|
|0b**11**0**11**00|0|
|0b0**11**00**1**0|1|

result: 0b0010110**1**1101100**0**0110010**1**

Receiver checks, and if a bit is flipped, they'll detect it (but won't know which one to correct)

cost: one extra bit for every _d_ bits

**Two dimensional parity**
Add an extra parity byte, which computes the parity on the columns.
||Message chunk|Parity bit|
|---|---|---|
||0b00**1**0**11**0|1|
||0b**11**0**11**00|0|
||0b0**11**00**1**0|1|
|parity byte|0b1001000|0|

In practice, however, bit errors occur in bursts, and we're willing to trade computational complexity for space efficiency. This means we need better _hardware_ to interface with the network and do the computation there.

**Cyclic Redundancy Check**
- We have data bits **D**
- choose r+1 bit pattern G that is known to both endpoints
- choose r-bits **R** such that `<D,R>` divide by **G** (modulo 2)
    - receiver knows **G**, and does division, if non-zero remainder then there is an error
    - can detect all burst errors less than r+1 bits

Sender operation:
- Extend **D** data bits with **R** zeros
- Divide by generator **G**
- Keep remainder
- **R** = remainder

Receiver procedure:
- Divide received frame <D, R> by G and check for zero remainder

`<D, R> = (D << r) ^ R`

### MAC protocols

**Three broad classes**
channel partitioning:
- divide channel into smaller pieces (time slots, frequency, code)
- allocate piece to node for exclusive use

random access:
- channel not divided, allow collisions
- "recover" from collisions

taking turns:
- nodes take turns, but nodes with more to send can take longer turns

### channel partitioning

**time division multiple access (TDMA)**
- access to channel in "rounds"
- each station gets fixed length slot (length = pkt transmission time) in each round
- unused slots go idle

**FDMA**
- channel divided into frequency bands
- each station assigned fixed frequency band
- unused transmission time in frequency bands go idle

### random access protocols
When node has a packet to send, we transmit at full channel data rate **R**. If there are two or more transmitting nodes then there's a "collision". Random access MAC protocol specifies how to detect and recover from collisions (via delayed retransmissions).

Examples: slotted ALOHA, ALOHA, CSMA

**Slotted ALOHA**
When a node obtains fresh frame, it transmits in the next slot
- if no collision: node can send new frame in next slot
- if collision: node retransmits frame in each subsequent slot with probability _p_ until success

pros:
- single node can transmit continuously at full rate
- highly decentralized: only slots in nodes need to be in sync
- simple

cons:
- collisions, wasting slots
- idle/empty slots
- nodes may be able to detect collision in less than time to transmit packet
- clock sync

**CSMA (carrier sense multiple access)**
Listen before transmit. However, there are issues related to propagation delays...

Collisions are detected within a short amount of time, and the colliding transmissions are aborted, reducing channel wastage. This is easy in wired LAN as we can measure signal strength but is harder to do with wireless LANs.

**Minimum frame size**

**Why enforce a minimum frame size?** So we can give a host enough time to detect collisions.

In Ethernet, minimum packet size = 64 bytes (meaning minimum 46 bytes of _payload_).

**CSMA/CD Algorithm**
1. NIC receives datagram from network layer and creates frame
2. if NIC senses channel idle, starts frame transmission. If NIC sense channel busy, wait until channel idle to transmit
3. If NIC transmits entire frame without detecting another transmission, NIC is done with frame
4. If NIC detects another transmission while transmitting, aborts and sends jam signal
5. After aborting, NIC enters binary backoff:
    - after *m*th collision, NIC chooses _K_ at random from {0,1,2,...,2^m - 1} NIC waits 512K bit times, returns to step _2_.
    - Longer backoff interval with more collisions

### taking turns MAC protocols
Control token is passed from one node to next.

---
**MAC address**
48-bit hard coded addresses allocated by IEEE. Manufacturers buy a portion of MAC address space to assure uniqueness.

They are hard coded into the ROM of an adapter.

How to determine interface's MAC address from its IP address? **ARP!**

**address resolution protocol (ARP)**

ARP table: Each node (host or router) has a table that maps IP-MAC with a TTL of around 20 minutes.

### Ethernet: unreliable, connectionless
**connectionless:** no handshaking between sending and receiving NICs
**unreliable:** receiving NIC does not send acks or nacks to sending NIC.Data in dropped frames recovered only if initial sender uses higher layer rdt (e.g. TCP), otherwise dropped data lost.

Uses CSMA/CD with binary backoff.

### Ethernet Switch
Link layer device that stores and forwards Ethernet frames. It examines incoming frames' MAC address, selectively forwarding frame to one-or-more outgoing links when frame is to be forwarded on segment.

Hosts are unaware of presence of switches.

