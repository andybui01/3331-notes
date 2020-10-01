## Application layer
***

### IPC
Processes talk to each other through IPC. On a single machine this is achievable through shared memory. However, when we do this over a network of computers we have to have some abstractions.

### Sockets
Processes send and receive messages to and from ***sockets***.

Once message is sent from the socket, the OS handles everything else in the network stack (below application) and eventually sends it through the physical layer to the other machine, whose OS deals with the incoming message and allows the other application to access this message through its own socket.

### Addressing
Host device has a unique 32-bit IP address. However this is insufficient to address processes because machines have MANY processes running at the same time. So we use a port number to address individual processes. We then get something like:
IP-ADDRESS:PORT-NUMBER

### Client-Server Architecture
#### Server
- Has well-defined interface to request/respond
- Long lived processes that waits for requests
- Carries out requests after receiving

#### Client
- Short-lived process that makes requests
- The "user-side"
- Initiates the communication

### P2P Architecture
- Peers directly communicate
- Symmetric responsibility

**Pros**
- Scalability: new peers bring new service capacity, as well as service demands
- Speed: parallelism, less contention
- Reliability: Redundancy, fault tolerance
- Geographic distribution

**Cons**
- State uncertainty: no shared memory or clock
- Action uncertainty: mutually conflicting decisions
- Distributed algorithms are complex


