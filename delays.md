## Network Delays
***
### Processing delay
Processing delay is the time it takes for a router to process a packet's header. The switch will perform error checks as well as determine the destination of the packet. Since packet headers are constant size and _only_ 28 bytes, the time it takes to process the header is fairly low. 

However, modern encryption and the need to modify packet contents, make the processing delay dependent on packet size.

### Queuing delay
Queueing delays occur when packets are arriving faster than they can be transmitted (bottlenecking). This usually occurs due to the low bandwidth in the outgoing link.

### Transmission delay
Transmission delays are the amount of time required to push all the bits of a packet onto the wire, meaning it is caused by the data rate of the link.

We can quantify it as DELAY = N-BITSLINK-RATE.

### Propagation delay
Propagation delay is the time taken for a packet to reach it's destination. It is directly proportional to the distance the package must travel.