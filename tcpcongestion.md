### cost of congestion
**knee**: point after which _throughput increases slowly_ while _delays increase fast_.

**cliff**: point after which _throughput drops to ZERO_ (congestion collapse) and _delays approach infinity_.

### approaches to congestion control
end-to-end congestion control:
- no explicit feedback from network
- congestion inferred from end-system observed loss, delay
- approach taken by TCP

network-assisted congestion control:
- routers provide feedback to end systems
- single bit indicating congestion (SNA, DECbit, TCP/IP ECN, ATM)
- explicit rate for sender to send at

### windows
**Congestion window: CWND**
- How many bytes can be sent without overflowing routers
- Computed by the sender using congestion control algorithm

**Flow control window: Advertised/Receive Window (RWND)**
- How many bytes can be sent without overflowing receiver's buffers
- Determined by the receiver and reported to the sender

**sender-side window = min(CWND, RWND)**

### detecting congestion: inferring loss
**Duplicate ACKS: isolated loss**
Duplicate ACKS indicate network is capable of delivering _some_ segments.

**Timeout: much more serious**
Not enough duplicate ACKS and might have suffered several losses.

### Rate adjustment
**Basic structure**
- upon receipt of ACK: _increase_ rate
- upon detection of loss: _decrease_ rate

**How we increase/decrease the rate depends on the phase of congestion control we're in:**
- discovering available bottleneck bandwidth vs.
- adjusting to bandwidth variations

### TCP slow starts (bandwidth discovery)
When connection begins, increase rates exponentially until first loss event:
- initially `cwnd = 1 MSS`
- double `cwnd` every RTT (full ACKs)
- simpler implementation `cwnd += 1` for each ACK

Therefore the initial rate is slow but it ramps up fast.

### Adjusting to varying bandwidth
Slow start gave an estimate of available bandwidth. Then oscillate around its current value.
- repeated probing (rate increase) and backoff (rate decrease)
- known as congestion avoidance

TCP uses Additive Increase Multiplicative Decrease (AIMD).

### AIMD
Summary: Sender increases transmission rate (window size), probing for usable bandwidth, until another congestion event occurs.

**Additive increase:** increase `cwnd` by 1 MSS every RTT until loss detected.
- For each successful RTT (ACK), `cwnd += 1`

**Multiplicative decrease:** cut `cwnd` in half after loss.

### Slow start vs AIMD
**When does the sender switch from slow start and start Additive Increase (AI)?**
- Introduce a "slow start threshold" (`ssthresh`) (a large value)
- Convert to AI when `cwnd = ssthresh`.
- On timeout, `ssthresh = cwnd/2`

### Implementation
**ACK (new data):**
```python
if cwnd < ssthresh:
    cwnd += 1 # slow start
else:
    cwnd += 1/cwnd # Additive increase (congestion avoidance phase)
```

**duplicate ACK:**
```python
dupACK++
if dupACK == 3: # fast retransmit
    ssthresh = cwnd / 2
    cwnd = cwnd / 2
```

**timeout:**
```python
if timeout:
    ssthresh = cwnd / 2
    cwnd = 1
```

### TCP flavors
**Tahoe:**
- `cwnd = 1` for each **triple dup-ack** _and_ **timeout**

**Reno:**
- `cwnd = 1` on timeout
- `cwnd = cwnd/2` on triple dup-ack

**newReno:**
- Reno + improved fast recovery (SKIPPED)

**SACK: (NOT COVERED)**
- incorporates selective acks
