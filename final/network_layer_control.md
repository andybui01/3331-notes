## Routing protocols
---
### Internet Routing

Works at two levels:
- Each Autonomous System (domain) runs an _intra-domain_ routing protocol that establishes routes within its domain
    - AS - region of network under a single admin entity
    - link state e.g. Open Shortest Path first (OSPF)
    - distance vector e.g. Routing Information Protocol (RIP)
- AS' participate in an inter-domain routing protocol that establishes routes between domains 
    - Path vector e.g. Border Gateway Protocol (BGP)

### AS Routing algorithms
**Link State (Global)**
- Routers maintain cost of each link in the network
- Connectivity/cost changes flooded to all routers
- Converges quickly (less inconsistency, looping, etc.)
- Limited network sizes

Problems:
- Scalability:
    - We need O(VE) messages to flood link state
    - Djikstra is O(N^2)
    - O(E) entries in the Link State topology database
    - O(V) entries in the forwarding table
- Transient Disruptions
    - Some routers know about failure before others
    - The shortest paths are no longer consistent
    - Can cause transient forwarding loops

**Distance Vector (Decentralized)**
- Routers maintain next hop & cost of each destination
- Connectivity/cost changes iteratively propagate from neighbor to neighbor
- Requires multiple rounds to converge
- Scales to large networks

Problems:
- Reacts rapidly to good news, but takes its time on bad news
- Count-to-infinity:
    - Solved by the "poisoned reverse" rule

#### Internet Control Message Protocol (ICMP)
Used to send error messages and operating information indicating success or failure when communicating with another IP address.

