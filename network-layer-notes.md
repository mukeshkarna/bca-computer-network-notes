# Detailed Notes for Computer Networks: Chapter 4 - Network Layer

## 4.1 Functions of Network Layer

The network layer is responsible for source-to-destination delivery of packets across multiple networks. Unlike the data link layer (which handles delivery between directly connected devices), the network layer ensures packets travel from their origin to their final destination, potentially crossing many intermediate networks.

### Key Functions:

1. **Routing:** Determining the optimal path for data to travel from source to destination
   - Involves selecting the best routes through intermediate routers
   - Uses various routing algorithms (distance vector, link state)

2. **Logical Addressing:** Assigning unique addresses to identify devices on the network
   - IP addresses (IPv4, IPv6) provide a network-wide addressing scheme
   - These addresses are independent of physical (MAC) addresses
   - Example: 192.168.1.1 (IPv4) or 2001:0db8:85a3:0000:0000:8a2e:0370:7334 (IPv6)

3. **Internetworking:** Connecting different types of networks together
   - Allows communication between diverse networks with different architectures
   - Handles protocol conversions when necessary

4. **Fragmentation and Reassembly:** Breaking packets into smaller units when necessary
   - Required when a packet must traverse networks with different maximum packet sizes
   - The original packet is reconstructed at the destination

## 4.2 Virtual Circuits and Datagram Subnets

Network layer protocols can operate using two different approaches:

### Datagram Approach (Connectionless):
- Each packet is treated independently
- Packets can take different routes to the same destination
- No setup phase is required before sending data
- Packets may arrive out of order
- Example: Internet Protocol (IP)

**Datagram Operation:**
1. Each packet contains complete addressing information
2. Routers make independent forwarding decisions for each packet
3. No state information is maintained about the connection
4. Highly resilient to network failures

### Virtual Circuit Approach (Connection-oriented):
- A predetermined path is established before data transmission
- All packets follow the same path from source to destination
- Requires setup and teardown phases
- Guarantees packets arrive in order
- Examples: X.25, Frame Relay

**Virtual Circuit Operation:**
1. Setup phase establishes the path and creates entries in switching tables
2. Each packet contains a virtual circuit identifier (VCI) instead of full addressing information
3. Packets follow the predetermined path
4. Connection is terminated when communication is complete

### Comparison:

| Characteristic | Datagram Subnet | Virtual Circuit Subnet |
|----------------|-----------------|------------------------|
| Connection setup | Not required | Required before data transfer |
| Addressing | Each packet contains full source/destination addresses | Packets contain short VC identifier |
| State information | Routers maintain no connection state | Routers maintain connection state |
| Routing | Each packet independently routed | All packets follow same route |
| Failure impact | Highly resilient - alternative routes can be used | Circuit fails if any link in path fails |
| Packet ordering | Not guaranteed | Guaranteed |
| Overhead | Higher per-packet overhead (full addressing) | Lower per-packet overhead after setup |

### Numerical Example - Virtual Circuit:

Consider a network with 4 routers (A, B, C, D) where a virtual circuit is set up from host H1 (connected to router A) to host H2 (connected to router D).

**Setup Phase:**
1. A virtual circuit request is sent from H1 to H2
2. The path chosen is A → B → C → D
3. Virtual circuit tables are created in each router:

**Router A:**
| Incoming Interface | Incoming VCI | Outgoing Interface | Outgoing VCI |
|-------------------|--------------|-------------------|--------------|
| 1 (from H1) | 14 | 2 (to B) | 22 |

**Router B:**
| Incoming Interface | Incoming VCI | Outgoing Interface | Outgoing VCI |
|-------------------|--------------|-------------------|--------------|
| 1 (from A) | 22 | 3 (to C) | 36 |

**Router C:**
| Incoming Interface | Incoming VCI | Outgoing Interface | Outgoing VCI |
|-------------------|--------------|-------------------|--------------|
| 2 (from B) | 36 | 1 (to D) | 18 |

**Router D:**
| Incoming Interface | Incoming VCI | Outgoing Interface | Outgoing VCI |
|-------------------|--------------|-------------------|--------------|
| 3 (from C) | 18 | 2 (to H2) | 42 |

**Data Transfer:**
When H1 sends a packet to H2, it attaches the VCI 14. As the packet travels through the network, each router uses its table to determine the outgoing interface and updates the VCI in the packet header. The packet's VCI changes at each hop, but it follows the predetermined path to its destination.

## 4.3 IPv4 Addresses

IPv4 uses 32-bit addresses, typically written in dotted-decimal notation (e.g., 192.168.1.1).

### IPv4 Address Space:

With 32 bits, IPv4 can theoretically address up to 2^32 (approximately 4.3 billion) devices.

### Classful Addressing:

Originally, IPv4 addresses were divided into five classes:

| Class | First Bits | First Byte Range | Default Mask | Network Bits | Host Bits | Networks Possible | Hosts per Network |
|-------|------------|------------------|--------------|--------------|-----------|-------------------|-------------------|
| A | 0 | 1-127 | 255.0.0.0 | 8 | 24 | 126 | 16,777,214 |
| B | 10 | 128-191 | 255.255.0.0 | 16 | 16 | 16,384 | 65,534 |
| C | 110 | 192-223 | 255.255.255.0 | 24 | 8 | 2,097,152 | 254 |
| D | 1110 | 224-239 | (Multicast) | - | - | - | - |
| E | 1111 | 240-255 | (Experimental) | - | - | - | - |

### Classless Addressing (CIDR):

Classless Inter-Domain Routing (CIDR) was introduced to use address space more efficiently. CIDR notation uses a suffix indicating the number of network bits (e.g., 192.168.1.0/24).

**Benefits of CIDR:**
- More flexible allocation of address space
- Reduces the size of routing tables
- Enables variable-length subnet masks

### Subnetting:

Subnetting allows dividing a network into smaller sub-networks, each with its own network ID.

#### Numerical Example for Subnetting:

Suppose you have the network 192.168.10.0/24 and need to create 4 subnets.

1. Determine how many bits to borrow:
   - For 4 subnets, you need at least 2 bits (2^2 = 4)

2. The new subnet mask:
   - Original mask: 255.255.255.0 (/24)
   - Adding 2 bits: 255.255.255.192 (/26)

3. Calculate subnet information:
   - Subnet 0: 192.168.10.0/26 (Range: 192.168.10.0 - 192.168.10.63)
   - Subnet 1: 192.168.10.64/26 (Range: 192.168.10.64 - 192.168.10.127)
   - Subnet 2: 192.168.10.128/26 (Range: 192.168.10.128 - 192.168.10.191)
   - Subnet 3: 192.168.10.192/26 (Range: 192.168.10.192 - 192.168.10.255)

4. For each subnet:
   - Network ID: First address in range
   - Broadcast address: Last address in range
   - Usable host addresses: All addresses between network ID and broadcast address

### Network Address Translation (NAT):

NAT allows multiple devices on a private network to share a single public IP address.

**Types of NAT:**
1. **Static NAT:** One-to-one mapping between private and public addresses
2. **Dynamic NAT:** Multiple private addresses share a pool of public addresses
3. **Port Address Translation (PAT):** Multiple private addresses share a single public address using different ports

**NAT Table Example:**

| Private IP | Private Port | Public IP | Public Port | Protocol |
|------------|--------------|-----------|-------------|----------|
| 192.168.1.10 | 3333 | 203.0.113.5 | 5555 | TCP |
| 192.168.1.11 | 4444 | 203.0.113.5 | 6666 | TCP |
| 192.168.1.12 | 5555 | 203.0.113.5 | 7777 | UDP |

When a device with private IP 192.168.1.10 initiates a connection from port 3333, NAT translates this to the public IP 203.0.113.5 with port 5555. Return traffic to 203.0.113.5:5555 is translated back to 192.168.1.10:3333.

## 4.4 IPv4 Datagram Format and Fragmentation

### IPv4 Datagram Format:

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             Data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**Field Descriptions:**
- **Version (4 bits):** Indicates protocol version (4 for IPv4)
- **IHL (4 bits):** Internet Header Length in 32-bit words, minimum value is 5 (20 bytes)
- **Type of Service/DSCP (8 bits):** Specifies how the packet should be handled
- **Total Length (16 bits):** Length of the entire datagram (header + data) in bytes
- **Identification (16 bits):** Used to identify fragments of a datagram
- **Flags (3 bits):** Control and identify fragments
  - Bit 0: Reserved, must be 0
  - Bit 1: Don't Fragment (DF)
  - Bit 2: More Fragments (MF)
- **Fragment Offset (13 bits):** Indicates where this fragment belongs in the original datagram
- **Time to Live (8 bits):** Limits the packet's lifetime to prevent infinite loops
- **Protocol (8 bits):** Indicates the next-level protocol (e.g., 6 for TCP, 17 for UDP)
- **Header Checksum (16 bits):** Error detection for the header
- **Source Address (32 bits):** Sender's IP address
- **Destination Address (32 bits):** Recipient's IP address
- **Options (variable):** Additional header fields (rarely used)
- **Padding:** Ensures the header ends on a 32-bit boundary

### Fragmentation:

When a datagram needs to traverse a network with a smaller Maximum Transmission Unit (MTU) than the datagram's size, it must be fragmented.

**Fragmentation Process:**
1. The original datagram is divided into smaller pieces (fragments)
2. Each fragment becomes an independent datagram
3. The Identification field remains the same for all fragments
4. The More Fragments (MF) flag is set for all fragments except the last one
5. The Fragment Offset field indicates where the fragment belongs in the original datagram

**Numerical Example for Fragmentation:**

Assume:
- Original datagram size: 4,000 bytes (3,980 bytes data + 20 bytes header)
- Network MTU: 1,500 bytes
- Original identification value: 12345

**Fragmentation Calculation:**
1. Maximum data per fragment = MTU - IP header size = 1,500 - 20 = 1,480 bytes
2. Number of fragments needed = ceil(original data size / maximum data per fragment) = ceil(3,980 / 1,480) = 3 fragments

**Fragment 1:**
- Total size: 1,500 bytes (1,480 bytes data + 20 bytes header)
- Identification: 12345
- Flags: MF=1 (More Fragments)
- Fragment Offset: 0 (first fragment)

**Fragment 2:**
- Total size: 1,500 bytes (1,480 bytes data + 20 bytes header)
- Identification: 12345
- Flags: MF=1 (More Fragments)
- Fragment Offset: 1,480 / 8 = 185 (measured in 8-byte units)

**Fragment 3:**
- Total size: 1,040 bytes (1,020 bytes data + 20 bytes header)
- Identification: 12345
- Flags: MF=0 (Last Fragment)
- Fragment Offset: 2,960 / 8 = 370 (measured in 8-byte units)

**Note:** Fragment offset is measured in 8-byte units, which is why we divide by 8.

## 4.5 IPv6 Address Structure and Advantages over IPv4

### IPv6 Address Structure:

IPv6 uses 128-bit addresses, typically written as eight groups of four hexadecimal digits, separated by colons (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334).

**Address Representation Rules:**
1. Leading zeros within a group can be omitted: 0001 → 1
2. One or more consecutive groups of zeros can be replaced with "::" (but only once in an address)

**Example:**
- Full address: 2001:0db8:0000:0000:0000:0000:1428:57ab
- Shortened: 2001:db8::1428:57ab

### IPv6 Address Types:

1. **Unicast:** Identifies a single interface
   - **Global Unicast:** Public addresses, globally routable (starts with 2000::/3)
   - **Link-Local:** Used only on a single link (starts with fe80::/10)
   - **Unique Local:** Private addresses, not routable on the internet (starts with fc00::/7)

2. **Multicast:** Identifies a group of interfaces (starts with ff00::/8)

3. **Anycast:** Identifies a set of interfaces, but packets are delivered to the nearest one

### IPv6 Header Format:

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |           Flow Label                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                         Source Address                        +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                      Destination Address                      +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**Field Descriptions:**
- **Version (4 bits):** Set to 6 for IPv6
- **Traffic Class (8 bits):** Used for QoS prioritization
- **Flow Label (20 bits):** Identifies packets belonging to the same flow
- **Payload Length (16 bits):** Length of the payload (excluding header)
- **Next Header (8 bits):** Identifies the type of header immediately following the IPv6 header
- **Hop Limit (8 bits):** Similar to TTL in IPv4
- **Source Address (128 bits):** Sender's IPv6 address
- **Destination Address (128 bits):** Recipient's IPv6 address

### Advantages of IPv6 over IPv4:

1. **Larger Address Space:**
   - IPv4: 2^32 addresses (approximately 4.3 billion)
   - IPv6: 2^128 addresses (approximately 340 undecillion)

2. **Simplified Header:**
   - Fixed length (40 bytes)
   - Fewer fields, improving router processing
   - No header checksum, reducing processing overhead

3. **Improved Security:**
   - IPsec is built-in (optional in IPv4)
   - Better authentication and privacy capabilities

4. **Better Support for QoS:**
   - Traffic class and flow label fields provide better traffic management

5. **No Need for NAT:**
   - Sufficient addresses for all devices
   - End-to-end connectivity is restored

6. **Autoconfiguration:**
   - Stateless address autoconfiguration (SLAAC)
   - No need for DHCP in many cases

7. **More Efficient Routing:**
   - Hierarchical addressing structure
   - Simplified routing tables

8. **Better Multicast Support:**
   - Multicast is a fundamental part of IPv6
   - Improved efficiency for one-to-many communications

## 4.6 Routing Algorithms: Distance Vector Routing, Link State Routing

Routing algorithms determine how routers select paths for forwarding packets.

### Distance Vector Routing:

Based on the Bellman-Ford algorithm, each router maintains a distance vector (table) containing the distance (cost) to each destination and the next hop to reach that destination.

**Algorithm Steps:**
1. Each router initializes its distance vector with 0 cost to itself and infinity to all other destinations
2. Each router shares its distance vector with its neighbors
3. Upon receiving a distance vector from a neighbor, a router updates its own distance vector if a shorter path is found
4. The process continues until the network reaches a stable state

**Bellman-Ford Equation:**
D(x,y) = min{c(x,v) + D(v,y)} for all neighbors v of x

Where:
- D(x,y) is the cost of the least-cost path from node x to node y
- c(x,v) is the cost to reach neighbor v from x
- D(v,y) is the cost of the least-cost path from v to y as reported by v

#### Numerical Example - Distance Vector Routing:

Consider a network with 4 routers (A, B, C, D) with the following direct link costs:
- A to B: 1
- A to C: 3
- B to C: 1
- B to D: 2
- C to D: 1

**Initial Distance Vectors:**

Router A:
| Destination | Cost | Next Hop |
|-------------|------|----------|
| A | 0 | - |
| B | 1 | B |
| C | 3 | C |
| D | ∞ | - |

Router B:
| Destination | Cost | Next Hop |
|-------------|------|----------|
| A | 1 | A |
| B | 0 | - |
| C | 1 | C |
| D | 2 | D |

Router C:
| Destination | Cost | Next Hop |
|-------------|------|----------|
| A | 3 | A |
| B | 1 | B |
| C | 0 | - |
| D | 1 | D |

Router D:
| Destination | Cost | Next Hop |
|-------------|------|----------|
| A | ∞ | - |
| B | 2 | B |
| C | 1 | C |
| D | 0 | - |

**After First Exchange:**

Router A updates its table:
- A → D: min{∞, 1+2, 3+1} = 3 via B

Updated Router A table:
| Destination | Cost | Next Hop |
|-------------|------|----------|
| A | 0 | - |
| B | 1 | B |
| C | 3 | C |
| D | 3 | B |

Similarly, other routers update their tables. After a few iterations, the network converges to a stable state with each router knowing the optimal path to every other router.

### Problems with Distance Vector Routing:

1. **Count to Infinity Problem:**
   - When a link fails, information about the failure propagates slowly
   - Routers may incorrectly increase their distance to a destination incrementally

2. **Solutions to Count to Infinity:**
   - **Split Horizon:** Never send information about a route back to the router that reported it
   - **Split Horizon with Poisoned Reverse:** Send routes back but with an infinite metric
   - **Defining a Maximum Count:** Typically 15 or 16 hops

### Link State Routing:

In link state routing, each router builds a complete map of the network topology and then computes the shortest path to each destination using Dijkstra's algorithm.

**Algorithm Steps:**
1. Each router discovers its neighbors and learns the cost to each neighbor
2. Each router builds a link state packet (LSP) containing this information
3. Each router floods its LSP to all other routers in the network
4. Each router builds a link state database from received LSPs
5. Each router runs Dijkstra's algorithm on this database to compute shortest paths

#### Numerical Example - Link State Routing (Dijkstra's Algorithm):

Using the same network as in the distance vector example:

**Step 1:** Initialize two sets:
- N = {A} (nodes whose least-cost path is known)
- All other nodes in N' (nodes whose least-cost path is not known yet)

Initialize distance vector D:
- D(A) = 0
- D(B) = 1
- D(C) = 3
- D(D) = ∞

**Step 2:** Find node with minimum cost in N', add to N
- Add B to N, N = {A, B}

**Step 3:** Update distances to remaining nodes in N'
- D(C) = min{3, 1+1} = 2 via B
- D(D) = min{∞, 1+2} = 3 via B

**Step 4:** Find node with minimum cost in N', add to N
- Add C to N, N = {A, B, C}

**Step 5:** Update distances to remaining nodes in N'
- D(D) = min{3, 2+1} = 3 (no change)

**Step 6:** Find node with minimum cost in N', add to N
- Add D to N, N = {A, B, C, D}

**Final Shortest Path Tree from A:**
- A to B: Direct link with cost 1
- A to C: Via B with cost 2
- A to D: Via B with cost 3

### Comparison Between Distance Vector and Link State Routing:

| Aspect | Distance Vector | Link State |
|--------|----------------|------------|
| Information shared | Distance vectors (tables) | Link states (network map) |
| Knowledge of topology | Partial (next hop only) | Complete network map |
| Algorithm | Bellman-Ford | Dijkstra |
| Convergence speed | Slower | Faster |
| Message size | Smaller | Larger |
| Processing requirements | Lower | Higher |
| Scalability | Less scalable | More scalable |
| Vulnerability to mistakes | Higher (count-to-infinity) | Lower |
| Examples | RIP | OSPF, IS-IS |

## 4.7 Internet Control Protocols: ARP, RARP, ICMP

### Address Resolution Protocol (ARP):

ARP resolves IP addresses to MAC addresses on a local network.

**ARP Process:**
1. When a host wants to send a packet to an IP address on the local network, it needs the corresponding MAC address
2. The host checks its ARP cache for an existing mapping
3. If no mapping exists, the host broadcasts an ARP request asking "Who has this IP address?"
4. The host with that IP address responds with its MAC address
5. The original host adds this mapping to its ARP cache and sends the packet

**ARP Packet Format:**
- Hardware Type: Specifies the link-layer protocol (1 for Ethernet)
- Protocol Type: Specifies the upper-layer protocol (0x0800 for IPv4)
- Hardware Address Length: Length of hardware addresses (6 bytes for Ethernet)
- Protocol Address Length: Length of protocol addresses (4 bytes for IPv4)
- Operation: 1 for request, 2 for reply
- Sender Hardware Address: MAC address of sender
- Sender Protocol Address: IP address of sender
- Target Hardware Address: MAC address of target (filled in by the reply)
- Target Protocol Address: IP address of target

**Numerical Example - ARP:**

Suppose host A (IP: 192.168.1.5, MAC: 00:1A:2B:3C:4D:5E) wants to send a packet to host B (IP: 192.168.1.10, MAC: 00:6F:7G:8H:9I:0J) on the same network.

1. Host A checks its ARP cache for 192.168.1.10, but no entry is found
2. Host A creates an ARP request:
   - Sender IP: 192.168.1.5
   - Sender MAC: 00:1A:2B:3C:4D:5E
   - Target IP: 192.168.1.10
   - Target MAC: 00:00:00:00:00:00 (unknown)
3. Host A broadcasts this request to all devices (MAC: FF:FF:FF:FF:FF:FF)
4. Host B recognizes its IP in the request and sends an ARP reply:
   - Sender IP: 192.168.1.10
   - Sender MAC: 00:6F:7G:8H:9I:0J
   - Target IP: 192.168.1.5
   - Target MAC: 00:1A:2B:3C:4D:5E
5. Host A receives the reply and updates its ARP cache
6. Host A can now send the packet directly to Host B's MAC address

### Reverse Address Resolution Protocol (RARP):

RARP resolves MAC addresses to IP addresses, primarily used by diskless workstations.

**RARP Process:**
1. A diskless machine broadcasts a RARP request containing its MAC address
2. A RARP server responds with the corresponding IP address
3. The diskless machine can then configure its IP address

### Internet Control Message Protocol (ICMP):

ICMP provides error reporting and network diagnostic functions for IP.

**ICMP Message Format:**
- Type: Identifies the message type
- Code: Provides more specific information about the message type
- Checksum: Error detection
- Variable Part: Depends on the message type

**Common ICMP Message Types:**

| Type | Code | Description |
|------|------|-------------|
| 0 | 0 | Echo Reply (ping response) |
| 3 | 0-15 | Destination Unreachable |
| 5 | 0-3 | Redirect |
| 8 | 0 | Echo Request (ping) |
| 11 | 0-1 | Time Exceeded (used by traceroute) |
| 12 | 0 | Parameter Problem |

#### Numerical Example - ICMP Echo (Ping):

1. Host A sends an ICMP Echo Request to Host B:
   - Type: 8 (Echo Request)
   - Code: 0
   - Identifier: 12345 (arbitrary)
   - Sequence Number: 1
   - Data: Timestamp and optional user data

2. Host B responds with an ICMP Echo Reply:
   - Type: 0 (Echo Reply)
   - Code: 0
   - Identifier: 12345 (same as request)
   - Sequence Number: 1 (same as request)
   - Data: Same as in the request

3. Host A calculates Round Trip Time (RTT) by subtracting the timestamp in the data from the current time

### Ping and Traceroute:

**Ping** uses ICMP Echo messages to test connectivity and measure RTT.

**Traceroute:**
1. Sends packets with incrementing TTL values
2. Each router decrements TTL and returns ICMP Time Exceeded when TTL reaches zero
3. This maps out the path packets take to a destination

## 4.8 Routing Protocols: OSPF, BGP, Unicast, Multicast, and Broadcast

### Open Shortest Path First (OSPF):

OSPF is a link-state routing protocol used for intra-AS (within an Autonomous System) routing.

**Key Features:**
1. Uses Dijkstra's algorithm to calculate shortest paths
2. Supports hierarchical routing through areas
3. Faster convergence than distance vector protocols
4. Authentication for security
5. Support for equal-cost multipath routing

**OSPF Areas:**
- Area 0 (backbone area) connects all other areas
- Reduces routing table sizes and processing load
- Area Border Routers (ABRs) connect areas

**OSPF Packet Types:**
1. Hello: Discovers neighbors and maintains relationships
2. Database Description: Summarizes database contents during synchronization
3. Link State Request: Requests specific link state records
4. Link State Update: Sends requested link state records
5. Link State Acknowledgment: Acknowledges receipt of updates

### Border Gateway Protocol (BGP):

BGP is a path vector routing protocol used for inter-AS routing (between Autonomous Systems).

**Key Features:**
1. Uses TCP for reliable communication
2. Path attributes to select routes
3. Policy-based routing
4. Route filtering capabilities
5. Support for CIDR and route aggregation

**BGP Path Attributes:**
1. AS_PATH: List of ASs the route has traversed
2. NEXT_HOP: Next hop to reach the destination
3. LOCAL_PREF: Preference value for route selection
4. ORIGIN: Origin of the route information

**BGP Route Selection Process:**
1. Prefer routes with highest LOCAL_PREF
2. Prefer routes with shortest AS_PATH
3. Prefer routes with lowest origin type
4. Prefer routes with lowest MED (Multi-Exit Discriminator)
5. Prefer eBGP over iBGP routes
6. Prefer routes with lowest IGP metric to NEXT_HOP
7. Prefer the route learned from the BGP router with lowest router ID

### Unicast Routing:

In unicast routing, packets are sent to a single destination.

**Characteristics:**
- One sender, one receiver
- Each destination has a unique address
- Most common form of IP communication

### Multicast Routing:

Multicast routing enables efficient delivery of packets to multiple destinations simultaneously.

**Characteristics:**
- One sender, multiple receivers
- Receivers join multicast groups
- Traffic is only sent to interested receivers
- More efficient than multiple unicast transmissions

**Multicast Routing Protocols:**
1. **DVMRP (Distance Vector Multicast Routing Protocol):**
DVMRP is one of the first multicast routing protocols developed for IP networks, originally described in RFC 1075.
   - Uses reverse path forwarding


### Key Characteristics:

1. **Protocol Foundation:**
   - Based on Distance Vector routing principles (similar to RIP)
   - Uses the Truncated Reverse Path Broadcasting (TRPB) algorithm with pruning
   - Creates source-based distribution trees

2. **Operation Process:**
   - Maintains its own unicast routing table (separate from the main routing table)
   - Uses this routing table to perform Reverse Path Forwarding (RPF) checks
   - Initially floods multicast packets throughout the network
   - Implements "flood and prune" mechanism to optimize delivery

3. **Dense Mode Behavior:**
   - Assumes group members are densely distributed throughout the network
   - Initially floods multicast traffic everywhere
   - Routers with no downstream members send "prune" messages upstream
   - Pruned interfaces are placed in "pruned state" for a specified time period

4. **Tunneling Capability:**
   - Can tunnel through non-multicast-capable routers using encapsulation
   - Creates "virtual links" across unicast-only networks

### DVMRP Numerical Example:

Consider a network with the following topology:
```
    R1
   /  \
  /    \
 R2    R3
 |      |
 R4    R5
```

- Source S is connected to R1
- Group member G1 is connected to R4
- Group member G2 is connected to R5

**Initial Flooding Stage:**
1. Source S sends a multicast packet to R1
2. R1 forwards the packet to both R2 and R3
3. R2 forwards the packet to R4
4. R3 forwards the packet to R5
5. R4 delivers the packet to group member G1
6. R5 delivers the packet to group member G2

**Pruning Stage (if a router has no group members):**
Let's modify our scenario and say there's no group member at R5:
1. R5 recognizes it has no group members or downstream routers with members
2. R5 sends a prune message to R3 for this source-group pair
3. R3 prunes the interface to R5 and stops forwarding packets on that interface
4. The pruned state has a timeout (typically 2-3 minutes)
5. After the timeout, R3 will resume forwarding to R5, which will re-prune if still necessary

### DVMRP Metrics and Route Calculation:

DVMRP uses hop count as its metric with a maximum of 32 hops (infinity value is 32).

**Example Metrics:**
- Direct connection: 1
- Additional hop: +1 per router
- Tunnel: Usually weighted higher (e.g., +2) to prefer physical links

For a path from R1 → R2 → R4:
- Cost at R1: 0 (to itself)
- Cost at R2: 1 (one hop from R1)
- Cost at R4: 2 (two hops from R1)

### Advantages and Disadvantages:

**Advantages:**
- Relatively simple to implement
- Effective in densely-populated multicast environments
- No need for a rendezvous point (RP)
- Creates shortest path trees

**Disadvantages:**
- Periodic flooding can waste bandwidth
- Doesn't scale well to large networks
- Creates high state maintenance burden on routers
- Not suitable for sparsely distributed group members
- Requires all routers on the network to run DVMRP

### Current Status:

While historically important, DVMRP has largely been replaced by more scalable protocols like PIM-SM for modern networks. However, it remains instructive as the foundation for understanding multicast routing concepts, and some of its mechanisms (like RPF checking) are used in modern protocols.

2. **PIM (Protocol Independent Multicast):**
   - **PIM-DM (Dense Mode):** 
     - Uses flood-and-prune approach
     - Assumes recipients are densely distributed
   - **PIM-SM (Sparse Mode):** 
     - Uses shared distribution trees with rendezvous points
     - More efficient when recipients are sparsely distributed

3. **MOSPF (Multicast OSPF):**
   - Extension of OSPF for multicast
   - Uses existing OSPF infrastructure

**Multicast Distribution Trees:**
1. **Source-Based Trees:** 
   - Separate tree for each source
   - Optimal paths but more state information

2. **Shared Trees:**
   - Single tree shared by all sources
   - Less state information but potentially less efficient paths

#### Numerical Example - Multicast Routing:

Consider a network with routers R1, R2, R3, R4, and R5, with hosts connected to each router:
- Multicast group address: 224.0.0.1
- Source: Host on R1
- Group members: Hosts on R3 and R5

**Using Reverse Path Forwarding (RPF):**
1. R1 sends multicast packets to all neighbors (R2 and R4)
2. R2 verifies that R1 is on shortest path to source (RPF check passes)
   - R2 forwards to R3
3. R3 verifies RPF check and delivers to local member
4. R4 verifies RPF check and forwards to R5
5. R5 delivers to local member

If R5 were to receive the multicast packet from R3 instead of R4, it would fail the RPF check (if R4 is on the shortest path to source) and discard the packet.

### Broadcast Routing:

Broadcast routing delivers packets to all nodes in a network.

**Broadcast Delivery Methods:**
1. **Flooding:** Each router forwards broadcast packets on all interfaces except the one on which it was received
2. **Reverse Path Broadcasting (RPB):** Extends RPF to avoid loops in broadcast
3. **Truncated Reverse Path Broadcasting:** Avoids sending broadcasts to leaf subnets with no members
4. **Reverse Path Multicasting (RPM):** Extends RPB with pruning

### Anycast Routing:

Anycast delivers packets to the "nearest" member of a group of potential receivers.

**Characteristics:**
- Multiple destinations share the same address
- Packets are delivered to the closest destination
- Used for services like DNS root servers and CDNs

### IPv6 Network Address Translation (NAT64):

A technology to allow IPv6 networks to communicate with IPv4 networks.

**Operation:**
1. IPv6 host sends packet to IPv4 destination
2. NAT64 gateway creates mapping
3. NAT64 translates IPv6 packet to IPv4
4. Return traffic is translated back to IPv6

## Additional Network Layer Concepts

### Congestion Control:

Congestion occurs when too many packets overwhelm network resources, leading to degraded performance.

**Congestion Control Methods:**
1. **Open-Loop Control:** Prevents congestion before it happens
   - Careful network design
   - Traffic admission control
   - Traffic policing

2. **Closed-Loop Control:** Reacts to congestion after it's detected
   - Explicit feedback (ECN)
   - Implicit feedback (packet loss, delay)

### Quality of Service (QoS):

QoS refers to the ability to provide different priority to different applications, users, or data flows.

**QoS Parameters:**
1. **Reliability:** Percentage of packets delivered correctly
2. **Delay:** Time taken for packet to reach destination
3. **Jitter:** Variation in delay
4. **Bandwidth:** Data rate available

**QoS Mechanisms:**
1. **Integrated Services (IntServ):** Resource reservation
2. **Differentiated Services (DiffServ):** Packet classification and marking
3. **MPLS (Multiprotocol Label Switching):** Traffic engineering

### Mobile IP:

Mobile IP allows mobile devices to move from one network to another while maintaining their IP address.

**Components:**
1. **Home Agent (HA):** Router on mobile node's home network
2. **Foreign Agent (FA):** Router on visited network
3. **Care-of Address (COA):** Temporary address on visited network

**Operation:**
1. Mobile node registers with foreign agent
2. Foreign agent registers with home agent
3. Home agent tunnels packets to foreign agent
4. Foreign agent delivers packets to mobile node

### Fragmentation Numerical Example:

Let's extend the earlier fragmentation example with specific packet contents:

Assume:
- Original IP datagram: 4000 bytes (20 bytes header + 3980 bytes data)
- ID: 45678
- DF flag: 0
- MTU of next network: 1500 bytes
- Payload content: First 4 bytes are [0xAA, 0xBB, 0xCC, 0xDD]

**Fragmentation Process:**
1. Maximum payload size per fragment = 1500 - 20 = 1480 bytes
2. Number of fragments needed = ceil(3980/1480) = 3

**Fragment 1:**
- Total length: 1500 bytes
- Header length: 20 bytes
- Payload length: 1480 bytes
- ID: 45678
- Flags: MF=1 (more fragments)
- Fragment offset: 0
- Payload starts with: [0xAA, 0xBB, 0xCC, 0xDD]

**Fragment 2:**
- Total length: 1500 bytes
- Header length: 20 bytes
- Payload length: 1480 bytes
- ID: 45678
- Flags: MF=1 (more fragments)
- Fragment offset: 1480/8 = 185
- Payload starts with: bytes 1481-1484 of original data

**Fragment 3:**
- Total length: 1020 bytes
- Header length: 20 bytes
- Payload length: 1000 bytes
- ID: 45678
- Flags: MF=0 (last fragment)
- Fragment offset: 2960/8 = 370
- Payload consists of: bytes 2961-3980 of original data

### Path MTU Discovery:

Path MTU Discovery is a technique to determine the maximum packet size that can be sent along a path without fragmentation.

**Process:**
1. Sender sets the DF (Don't Fragment) flag
2. If a router cannot forward the packet due to MTU constraints, it sends an ICMP "Fragmentation Needed" message
3. The sender reduces the packet size and tries again
4. Process continues until successful transmission

### Comparison of Routing Protocols:

| Protocol | Type | Interior/Exterior | Algorithm | Features | Limitations |
|----------|------|-------------------|-----------|----------|-------------|
| RIP | Distance Vector | Interior | Bellman-Ford | Simple, easy to configure | Limited to 15 hops, slow convergence |
| OSPF | Link State | Interior | Dijkstra | Fast convergence, support for large networks | Complex configuration, higher resource use |
| IS-IS | Link State | Interior | Dijkstra | Efficient, scales well | Complex configuration |
| BGP | Path Vector | Exterior | Best path selection | Policy-based, scalable | Complex configuration, slow convergence |
| EIGRP | Hybrid | Interior | DUAL | Fast convergence, efficient | Cisco proprietary |

### IPv6 Addressing Exercise:

Let's expand on IPv6 addressing with a numerical example:

Given an IPv6 prefix of 2001:db8:1234::/48, create a subnetting plan for an organization with 4 departments, each needing a different-sized subnet:
- IT Department: 1000 addresses
- Sales: 500 addresses
- Engineering: 2000 addresses
- Administration: 200 addresses

**Solution:**
- The /48 prefix gives us 16 bits for subnetting (from bit 48 to bit 64)
- IT Department needs at least 1000 addresses → 10 bits (2^10 = 1024)
- Sales needs at least 500 addresses → 9 bits (2^9 = 512)
- Engineering needs at least 2000 addresses → 11 bits (2^11 = 2048)
- Administration needs at least 200 addresses → 8 bits (2^8 = 256)

**Subnet Assignments:**
- IT Department: 2001:db8:1234:0::/54 (provides 2^10 = 1024 addresses)
- Sales: 2001:db8:1234:400::/55 (provides 2^9 = 512 addresses)
- Engineering: 2001:db8:1234:600::/53 (provides 2^11 = 2048 addresses)
- Administration: 2001:db8:1234:e00::/56 (provides 2^8 = 256 addresses)

### Distance Vector Routing Example with Count-to-Infinity Problem:

Let's explore the count-to-infinity problem with a numerical example:

Consider three routers A, B, and C connected in a line:
```
A --- B --- C
```

Link costs:
- A to B: 1
- B to C: 1

Initial distance vectors:
- A: {A:0, B:1, C:2}
- B: {A:1, B:0, C:1}
- C: {A:2, B:1, C:0}

Now suppose the link between B and C fails:
1. B detects the failure and updates its distance vector: {A:1, B:0, C:∞}
2. B sends its updated distance vector to A
3. Before A receives B's update, it sends its distance vector {A:0, B:1, C:2} to B
4. B sees that A can reach C with cost 2, so B updates its vector: {A:1, B:0, C:3}
5. B sends this update to A
6. A updates its vector to {A:0, B:1, C:4}
7. This process continues, with the cost to C incrementing by 1 each time

This "count to infinity" continues until the cost reaches the defined maximum (typically 16 for RIP, which would consider the destination unreachable).

**Solutions:**
1. **Split Horizon:** A doesn't tell B about routes learned from B
2. **Poisoned Reverse:** A tells B that C is unreachable (infinite cost)
3. **Defining Maximum Metrics:** After a certain threshold (e.g., 16), the destination is considered unreachable

## Summary of Key Network Layer Concepts

1. **Network Layer Functions:**
   - Logical addressing
   - Routing
   - Fragmentation and reassembly
   - Error reporting

2. **IPv4 vs IPv6:**
   - IPv4: 32-bit addresses, complex header, limited addresses
   - IPv6: 128-bit addresses, simplified header, extensive address space

3. **Routing Approaches:**
   - Distance Vector: Based on Bellman-Ford, shares distance information
   - Link State: Based on Dijkstra, shares topology information
   - Path Vector: Used in BGP, incorporates policy decisions

4. **Delivery Modes:**
   - Unicast: One sender, one receiver
   - Multicast: One sender, multiple receivers
   - Broadcast: One sender, all receivers
   - Anycast: One sender, nearest member of a group

5. **Internet Control Protocols:**
   - ARP: Maps IP addresses to MAC addresses
   - ICMP: Error reporting and network diagnostics
   - IGMP: Manages multicast group memberships

The network layer provides the crucial service of getting packets from source to destination across multiple networks, forming the backbone of internet communication.