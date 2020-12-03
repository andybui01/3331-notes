### Wireless link characteristics
**decreased signal strength:** radio signal attenuates as it propagates through _matter_
**interference from other sources:** standardized wireless network frequencies (e.g. 2.4GHz) shared by other devices (e.g. phone); devices (motors) interfere as well
**multipath propagation:** radio signal reflects off objects ground, arriving at destination at slightly different times

### signal-to-noise ratio (SNR)
A larger SNR means its easer to extract signal from noise, a good thing.

BER = Bit Error Rate

Constant physical layer: As we increase power, SNR increases and BER decreases as a result.

Constant SNR: choose a physical layer that meets BER requirement which gives highest throughput. SNR may change with mobility: dynamically adapt physical layer (modulation technique, rates)

### 802.11 (wifi)

A wireless host communicates with base station (access point AP)

Basic service set (BSS) (aka cell) in infrastructure contains: wireless hosts, access point, ad hoc mode (hosts only)

We use CSMA to _avoid_ collisions as detection is very hard! Don't have to detect if we can ensure there are zero collisions.

### 802.11 MAC protocol CSMA/CA
DIFS = DCF Inter Frame Space
SIFS = Short Inter Frame Space

**Sender**
1. If sense channel _idle_ for DIFS then transmit entire frame (no CD)
2. If sense channel _busy_ then:
    - start random backoff time
    - timer counts down while channel idle
    - transmit when timer expires
    - if no ACK, increase random backoff interval, repeat 2

**Receiver**
If frame received OK: return ACK after SIFS (ACK needed due to hidden terminal problem)

**Avoiding collisions**
We allow sender to reserve the channel rather than random access of data frames.
- Sender first transmit small request-to-send (RTS) packets to BS using CSMA
- BS broadcasts clear-to-send CTS in response to RTS
- CTS heard by all nodes
    - only sender transmits as others defer
- RTS and CTS contain the duration for transmitting the subsequent data frame