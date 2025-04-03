# Chapter 3: Data Link Layer

## 3.1 Functions of Data Link Layer

The data link layer provides several key functions in the network communication model:

- **Service Interface**: Provides a well-defined service interface to the network layer through framing
- **Error Handling**: Deals with transmission errors that occur in the physical medium
- **Flow Regulation**: Controls the flow of data to prevent fast senders from overwhelming slow receivers
- **Packet Preparation**: Encapsulates packets with headers and trailers to create frames

The Data Link layer frame typically includes:
- **Header**: Contains control information like addressing and is located at the beginning
- **Data**: Contains the packet from the Network layer
- **Trailer**: Contains control information added to the end, often including error detection/correction
- **Flag**: Special bit patterns marking the beginning and end of the frame

The Data Link layer is divided into two sublayers:
1. **MAC (Media Access Control)**: Handles access to the shared medium
2. **LLC (Logical Link Control)**: Handles flow control, error control, and identifying network layer protocols

## 3.2 Data Link Control: Framing, Flow and Error Control

### Framing

Framing is the process of dividing the bit stream from the physical layer into manageable data units called frames. This allows the data link layer to organize and structure data for transmission.

#### Types of Framing

1. **Fixed-size Framing**
   - Uses frames of predetermined, fixed length
   - No need for delimiters as the size itself serves as a boundary marker
   - **Drawback**: Internal fragmentation when data is smaller than frame size
   - **Solution**: Padding (adding extra bits to fill the frame)

2. **Variable-size Framing**
   - Frames can be of different sizes based on data needs
   - Requires methods to identify frame boundaries:
     - **Length Field**: Includes a field specifying the frame length (used in Ethernet/802.3)
     - **End Delimiters**: Special bit patterns marking frame boundaries (used in Token Ring)

When using delimiters, special techniques are needed to prevent data from being mistaken as delimiters:
- **Byte Stuffing**: Extra bytes inserted in the data to differentiate from delimiters
- **Bit Stuffing**: Extra bits inserted to ensure data never matches delimiter patterns

### Flow Control

Flow control refers to mechanisms that prevent a sender from overwhelming a receiver with data sent too quickly. Several approaches are used:

#### Simplest Protocol

- Unidirectional flow (sender to receiver)
- No flow or error control
- Assumes receiver can immediately handle frames
- Inefficient when receiver processing is slower than transmission speed

#### Stop-and-Wait Protocol

- Sender transmits one frame and waits for acknowledgment (ACK)
- Next frame sent only after receiving ACK for previous frame
- Uses timers to handle lost frames or ACKs
- Simple but inefficient due to idle time waiting for acknowledgments

**Process**:
1. Sender transmits a frame
2. Sender starts timer and waits for ACK
3. If ACK received before timeout, sender sends next frame
4. If timeout occurs, sender retransmits the frame

#### Go-Back-N ARQ (Automatic Repeat Request)

- Uses sliding window approach allowing multiple unacknowledged frames
- Sequence numbers used to track frames (modulo-2^m arithmetic)
- Send window size can be up to 2^m-1 frames
- Receive window size is always 1 (only accepts frames in order)

**Key Components**:
- **Send Window**: Defines range of sequence numbers sender can transmit
- **Receive Window**: Defines range of sequence numbers receiver can accept
- **Timers**: Track time since frame transmission
- **Acknowledgments**: Cumulative ACKs indicating next expected frame

**When Error Occurs**:
- Sender must retransmit the frame in error and all subsequent frames
- Less efficient on noisy links due to multiple retransmissions

#### Selective Repeat ARQ

- More efficient for noisy links
- Only retransmits specific frames that are damaged or lost
- Both sender and receiver maintain windows of size 2^(m-1)
- Receiver can accept and buffer out-of-order frames

**Key Difference from Go-Back-N**:
- Receiver accepts out-of-order frames and buffers them
- Only damaged/lost frames are retransmitted
- Window size must be at most half the sequence number space

#### Piggybacking

- Technique to improve efficiency in bidirectional communication
- Combines acknowledgments with data frames
- Reduces overhead by eliminating separate ACK frames
- Delays ACKs slightly to wait for outgoing data frames

### Error Control

Error control involves detecting and potentially correcting errors that occur during transmission.

#### Types of Errors

1. **Single-Bit Error**: Only one bit in a data unit changes from 1 to 0 or 0 to 1
2. **Burst Error**: Multiple consecutive bits are changed, spanning from first corrupted bit to last

#### Error Detection Methods

1. **Simple Parity Check**
   - Adds one extra bit (parity bit) to make the total number of 1's either even (even parity) or odd (odd parity)
   - **Process**:
     - Count number of 1's in data
     - Add parity bit to make total number even or odd depending on scheme
     - Receiver checks if parity matches expected type
   - **Limitation**: Can detect odd number of errors but not even number

   **Example**:  
   For data 1001001 using even parity:
   - Count of 1's = 3 (odd)
   - Add parity bit 1 to make total even
   - Transmitted code: 10010011
   - If single bit changes during transmission, parity violation detected

2. **Checksum**
   - Divides data into segments of equal size (e.g., 8 bits)
   - Adds segments using 1's complement arithmetic
   - Complements the sum to get checksum
   - Transmits checksum along with data
   - Receiver adds all segments including checksum; result should be all 1's if no error

   **Process**:
   1. Divide data into m-bit segments
   2. Add all segments using 1's complement addition
   3. Complement the sum to get checksum
   4. Transmit data with checksum
   5. Receiver adds all segments and checksum
   6. If result is all 0's, data is accepted

   **Example**:  
   For data 10011001 11100010 00100100 10000100 with 8-bit checksum:
   - Sum: 10011001 + 11100010 + 00100100 + 10000100 = 1000100011
   - Wrap around extra bits: 00100011 + 10 = 00100101
   - Checksum (1's complement): 11011010
   - Transmitted data with checksum: original data + 11011010
   - At receiver: sum of all segments + checksum = 11111111
   - Complemented value = 00000000 (indicates no error)

3. **Cyclic Redundancy Check (CRC)**
   - Based on binary division rather than addition
   - Treats bit strings as polynomials with binary coefficients
   - Uses a generator polynomial (divisor) known to both sender and receiver
   - More powerful than parity or checksum for detecting burst errors

   **Process**:
   1. Select a generator polynomial G(x) of degree n
   2. Append n zeros to the data bits
   3. Divide the result by G(x) using modulo-2 division
   4. Replace appended zeros with remainder (CRC)
   5. Transmit data with CRC
   6. Receiver divides received code by same G(x)
   7. If remainder is zero, no error detected

   **Example**:  
   For data 1001 with generator polynomial x³ + x + 1 (1011):
   - Append 3 zeros: 1001000
   - Divide by 1011 using modulo-2 division
   - Remainder: 001
   - Transmitted code: 1001001

   **Properties of Good CRC Polynomials**:
   - Not divisible by x (detects all burst errors equal to polynomial length)
   - Divisible by x+1 (detects all odd number of bit errors)
   - Detects all single-bit errors if contains at least 3 terms
   - Detects most burst errors of length greater than polynomial degree

## 3.3 Error Detection and Correction

While error detection identifies errors, error correction additionally locates and fixes them:

### Hamming Code

Hamming code is a linear error-correcting code that can detect up to two-bit errors and correct single-bit errors:

**Algorithm**:
1. Number bit positions starting from 1 (right to left)
2. Positions that are powers of 2 (1, 2, 4, 8, 16, etc.) are reserved for parity bits
3. Other positions contain data bits
4. Each parity bit checks specific bit positions:
   - Parity bit at position 1 checks all positions with LSB 1 (1, 3, 5, 7, 9, 11, ...)
   - Parity bit at position 2 checks all positions with bit 2 set (2, 3, 6, 7, 10, 11, ...)
   - Parity bit at position 4 checks all positions with bit 3 set (4, 5, 6, 7, 12, 13, ...)
   - Parity bit at position 8 checks all positions with bit 4 set (8, 9, 10, 11, 12, ...)
5. Set each parity bit to make the total number of 1's in its check group even

**Error Detection and Correction**:
1. Receiver calculates parity for each group
2. Creates binary number from parity check results (1 for failed checks, 0 for passed)
3. If result is 0, no error detected
4. If non-zero, the decimal value indicates the bit position in error
5. Flip that bit to correct the error

**Example**:  
For 7-bit data 1011001, using Hamming code:
- Requires 4 parity bits (positions 1, 2, 4, 8)
- Data bits go in positions 3, 5, 6, 7, 9, 10, 11
- Calculate parity bits:
  - P1 checks bits 1, 3, 5, 7, 9, 11: value 0 for even parity
  - P2 checks bits 2, 3, 6, 7, 10, 11: value 1 for even parity
  - P4 checks bits 4, 5, 6, 7: value 1 for even parity
  - P8 checks bits 8, 9, 10, 11: value 0 for even parity
- Transmitted code: 01101001011

If bit 6 changes from 0 to 1 during transmission:
- Parity checks fail for P2 and P4 (positions 2 and 4)
- Binary result: 0110 (decimal 6)
- Error is in bit position 6
- Flip bit 6 to correct error

## 3.4 High-Level Data Link Control (HDLC) & Point-to-Point Protocol (PPP)

### High-Level Data Link Control (HDLC)

HDLC is a bit-oriented protocol for communication over point-to-point and multipoint links:

#### Configurations and Transfer Modes

1. **Normal Response Mode (NRM)**
   - Primary station sends commands, secondary stations respond
   - Used for both point-to-point and multipoint communications

2. **Asynchronous Balanced Mode (ABM)**
   - Balanced configuration where all stations can send commands and respond
   - Used only for point-to-point communications

#### HDLC Frame Format

- **Flag (8 bits)**: Bit pattern 01111110 marking frame boundaries
- **Address (8+ bits)**: Contains receiver address for frames from primary, or primary address for frames from secondary
- **Control (8 or 16 bits)**: Contains flow and error control information
- **Payload (variable)**: Contains data from network layer
- **FCS (16 or 32 bits)**: Frame Check Sequence for error detection (CRC)

#### HDLC Frame Types

1. **Information (I) Frame**
   - Carries user data from network layer
   - Includes flow and error control (piggybacked)
   - Control field format includes N(S) sequence number, N(R) acknowledgment, and P/F bit
   - First bit of control field is 0

2. **Supervisory (S) Frame**
   - Used for flow and error control (no data)
   - First two bits of control field are 10
   - Types include Receive Ready (RR), Receive Not Ready (RNR), and Reject (REJ)

3. **Unnumbered (U) Frame**
   - Used for link management functions
   - May contain information field if required
   - First two bits of control field are 11
   - Used for setup, disconnect, and mode setting

### Point-to-Point Protocol (PPP)

PPP is a byte-oriented protocol used for direct connections between two devices:

#### Characteristics
- Widely used for broadband connections with high speeds
- Used by Internet users connecting to ISPs
- Supports multiple network layer protocols
- Provides authentication capabilities

#### PPP Frame Format

- **Flag (8 bits)**: Pattern 01111110 marking frame boundaries
- **Address (8 bits)**: Set to 11111111 (broadcast address)
- **Control (8 bits)**: Set to constant value 11000000 (no flow control)
- **Protocol (8 or 16 bits)**: Identifies network layer protocol in payload
- **Payload (variable)**: Data from network layer (max 1500 bytes by default)
- **FCS (16 or 32 bits)**: Frame Check Sequence for error detection

#### PPP Components

1. **Encapsulation Component**
   - Encapsulates datagrams for transmission over specified physical layer

2. **Link Control Protocol (LCP)**
   - Establishes, configures, tests, maintains, and terminates links
   - Negotiates options and features between endpoints

3. **Authentication Protocols**
   - **Password Authentication Protocol (PAP)**: Simple authentication using cleartext passwords
   - **Challenge Handshake Authentication Protocol (CHAP)**: More secure, challenge-based authentication

4. **Network Control Protocols (NCPs)**
   - One NCP for each supported network layer protocol
   - Examples include Internet Protocol Control Protocol (IPCP), IPv6CP, AppleTalk Control Protocol

#### PPP State Transition

PPP connection goes through several states:
1. **Dead**: Physical layer not ready
2. **Establish**: LCP configuring link
3. **Authenticate**: Optional authentication process
4. **Network**: NCPs configuring network layer
5. **Terminate**: Link termination

## 3.5 Channel Allocation Problem

When multiple users need to access a shared network channel, allocation methods are required:

### Static Channel Allocation

- Fixed portion of channel bandwidth permanently assigned to each user
- Typically uses Frequency Division Multiplexing (FDM)
- For N users, bandwidth divided into N equal channels
- Simple to implement but inefficient when traffic is bursty

**Efficiency Formula**:
- T = 1/(U*C-L)
- T(FDM) = N*T(1/U(C/N)-L/N)

Where:
- T = mean time delay
- C = channel capacity
- L = arrival rate of frames
- U = bits per frame
- N = number of subchannels

### Dynamic Channel Allocation

- Channels allocated on-demand from a central pool
- Allocation considers parameters to minimize interference
- Optimizes bandwidth usage for better performance
- Types:
  - **Centralized**: Central controller assigns channels
  - **Distributed**: Nodes coordinate among themselves

**Assumptions in Dynamic Allocation**:
1. **Station Model**: Independent frame generation with probability λΔt
2. **Single Channel**: All stations use same channel
3. **Collisions**: Overlapping transmissions cause errors requiring retransmission
4. **Time Model**: Can be slotted or continuous
5. **Carrier Sensing**: Stations may detect when channel is in use

## 3.6 Multiple Access

Multiple access methods allow multiple stations to share transmission medium:

### 1. Random Access Methods

Stations transmit data whenever needed without central coordination:

#### ALOHA

**Pure ALOHA**:
- Stations transmit frames whenever they have data
- If collision occurs, wait random time and retransmit
- **Vulnerable Time**: 2 × Tt (frame transmission time)
- **Efficiency**: S = G × e^(-2G)
- **Maximum Efficiency**: 18.4% when G = 0.5

**Slotted ALOHA**:
- Time divided into discrete slots of length Tt
- Stations can only transmit at beginning of slots
- **Vulnerable Time**: Tt (one time slot)
- **Efficiency**: S = G × e^(-G)
- **Maximum Efficiency**: 36.8% when G = 1

**Advantages**:
- Simple implementation
- Works well for light loads
- Single active node can use full channel bandwidth

**Disadvantages**:
- Poor utilization under heavy load
- Unstable at high loads
- Requires slot synchronization (for slotted ALOHA)

#### Carrier Sense Multiple Access (CSMA)

Stations listen to medium before transmitting to reduce collisions:

**Persistence Methods**:
1. **1-persistent CSMA**:
   - If channel busy, continuously sense until idle, then immediately transmit
   - High collision probability when multiple stations waiting
   - Aggressive approach

2. **Non-persistent CSMA**:
   - If channel busy, wait random time before checking again
   - Reduces collision probability but increases delay
   - Less aggressive, more polite approach

3. **p-persistent CSMA**:
   - If channel idle, transmit with probability p, delay with probability 1-p
   - Compromise between 1-persistent and non-persistent
   - Used for slotted channels

**Vulnerable Time**: Propagation time (Tp)
**Efficiency**: Up to 50-90% depending on implementation

#### CSMA/CD (Collision Detection)

Enhanced CSMA that detects collisions during transmission:

**Process**:
1. Station senses channel before transmitting
2. If channel idle, begin transmission
3. While transmitting, monitor channel for collisions
4. If collision detected:
   - Stop transmission immediately
   - Send jam signal to ensure all stations detect collision
   - Wait random time using binary exponential backoff
   - Try again

**Key Requirements**:
- Minimum frame size must be at least twice the maximum propagation time
- Collision must be detectable during transmission

**Efficiency**: Up to 90% for non-persistent method
**Application**: Used in traditional Ethernet (802.3)

#### CSMA/CA (Collision Avoidance)

Designed for wireless networks where collision detection is difficult:

**Components**:
1. **InterFrame Space (IFS)**: Waiting period after channel becomes idle
2. **Contention Window**: Random backoff time divided into slots
3. **Acknowledgments**: Positive acknowledgment required for successful transmission

**Process**:
1. Station waits for IFS after channel becomes idle
2. Station enters random backoff within contention window
3. If channel remains idle during backoff, transmit
4. If channel becomes busy during backoff, pause timer and resume after channel idle
5. After transmission, wait for acknowledgment
6. If no acknowledgment received, assume collision and retry

**Application**: Used in WiFi (802.11)

### 2. Controlled Access Methods

Stations coordinate to determine transmission rights:

#### Reservation

**Process**:
1. Time divided into intervals
2. Each interval begins with reservation frame containing mini-slots
3. Each station has dedicated mini-slot to make reservations
4. Stations request transmission by marking their mini-slot
5. After reservation frame, stations transmit in order of mini-slots

**Advantages**:
- No collisions once reservations made
- Fair access for all stations
- Good for stable, high-load networks

**Disadvantages**:
- Overhead of reservation slots
- Inefficient for bursty traffic

#### Polling

Central controller (primary station) determines which station can transmit:

**Functions**:
1. **Select**: Primary sends SEL frame to grant permission to receive
2. **Poll**: Primary sends POLL frame to ask if secondary has data to send

**Process**:
1. Primary polls each secondary in sequence
2. Secondary responds with data if available, NAK if not
3. Primary acknowledges received data
4. Primary moves to next secondary

**Efficiency**: T_t/(T_t + T_poll)
Where T_t is transmission time and T_poll is polling overhead

**Advantages**:
- No collisions
- Definite upper bound on access time
- Priorities can be implemented

**Disadvantages**:
- Polling overhead
- Single point of failure (controller)
- Latency increases with number of stations

#### Token Passing

Special frame (token) grants transmission permission:

**Process**:
1. Token circulates around logical ring
2. Station with token can transmit for allowed time
3. After transmission or if no data, station passes token to next station
4. If token lost, recovery mechanisms activate

**Network Arrangements**:
- **Token Ring**: Physical ring topology
- **Token Bus**: Physical bus with logical ring

**Performance**:
- Throughput S = 1/(1 + a/N) for a < 1
- Throughput S = 1/{a(1 + 1/N)} for a > 1
Where N = number of stations, a = T_p/T_t (propagation delay / transmission delay)

**Advantages**:
- Deterministic access (bounded delay)
- Fair access for all stations
- No collisions

**Disadvantages**:
- Token overhead
- Token management complexity
- Sensitivity to failures

### 3. Channelization Methods

Divide channel capacity among stations:

#### Frequency Division Multiple Access (FDMA)

**Characteristics**:
- Divides available bandwidth into frequency bands
- Each station assigned dedicated frequency band
- Guard bands prevent interference between adjacent channels
- Simple implementation but potentially wasteful

**Applications**:
- AM/FM radio broadcasting
- First-generation cellular systems
- Cable TV systems

#### Time Division Multiple Access (TDMA)

**Characteristics**:
- Divides time into slots assigned to different stations
- Each station transmits only during its assigned slot
- Requires synchronization between stations
- Makes efficient use of bandwidth

**Types**:
- **Synchronous TDMA**: Fixed allocation of time slots
- **Statistical TDMA**: Dynamic allocation based on need

**Applications**:
- Digital cellular systems
- Satellite communication
- Digital telephony

#### Code Division Multiple Access (CDMA)

**Characteristics**:
- All stations use entire bandwidth simultaneously
- Each station assigned unique code sequence
- Codes are orthogonal to minimize interference
- Based on spread spectrum technology

**Process**:
1. Each bit multiplied by station's unique code sequence (chips)
2. Receiver uses same code to extract original data
3. Other transmissions appear as low-level noise

**Advantages**:
- Resistant to jamming and interference
- Secure transmission
- Soft capacity (gradual degradation under load)

**Applications**:
- 3G cellular systems
- GPS
- Military communications

## 3.7 Wired LAN: Ethernet Standards and FDDI

### Ethernet (IEEE 802.3)

The most widely deployed LAN technology:

#### Ethernet Naming Convention

Format: [Speed][BASE][Medium]

Examples:
- **10BASE5**: 10 Mbps over thick coaxial cable (500m)
- **10BASE2**: 10 Mbps over thin coaxial cable (185m)
- **10BASE-T**: 10 Mbps over twisted pair
- **100BASE-TX**: 100 Mbps over twisted pair
- **1000BASE-T**: 1 Gbps over twisted pair
- **10GBASE-T**: 10 Gbps over twisted pair

#### Ethernet Frame Format

- **Preamble (7 bytes)**: Alternating 1s and 0s for synchronization
- **Start Frame Delimiter (1 byte)**: 10101011 marking frame start
- **Destination Address (6 bytes)**: MAC address of intended recipient
- **Source Address (6 bytes)**: MAC address of sender
- **Type/Length (2 bytes)**: Protocol type or frame length
- **Data (46-1500 bytes)**: Actual payload data
- **FCS (4 bytes)**: CRC for error detection

#### Ethernet Evolution

- **Standard Ethernet**: 10 Mbps, CSMA/CD
- **Fast Ethernet**: 100 Mbps, CSMA/CD maintained
- **Gigabit Ethernet**: 1 Gbps, optional CSMA/CD
- **10 Gigabit Ethernet**: 10 Gbps, no CSMA/CD (full-duplex only)

### Fiber Distributed Data Interface (FDDI)

High-speed fiber optic LAN technology:

#### Characteristics

- ANSI and ISO standard for fiber optic networks
- 100 Mbps data rate over fiber optic cable
- Dual counter-rotating rings for redundancy
- Token passing access method
- Maximum network diameter of 200 kilometers
- Support for thousands of connected devices

#### Frame Format

- **Preamble (1 byte)**: For synchronization
- **Start Delimiter (1 byte)**: Marks frame beginning
- **Frame Control (1 byte)**: Specifies frame type (data or control)
- **Destination Address (2-6 bytes)**: Address of recipient
- **Source Address (2-6 bytes)**: Address of sender
- **Payload (variable)**: Data from network layer
- **Checksum (4 bytes)**: For error detection
- **End Delimiter (1 byte)**: Marks frame end

#### Applications

- Backbone networks for large organizations
- Metropolitan area networks
- High-speed interconnections between buildings
- Environments requiring high reliability and fault tolerance

## 3.8 Wireless LAN: IEEE 802.11x and Bluetooth Standards

### IEEE 802.11 (Wi-Fi)

Standard family for wireless local area networks:

#### MAC Sublayers

1. **Distributed Coordination Function (DCF)**
   - Uses CSMA/CA as access method
   - Offers asynchronous service
   - Fundamental access method in all 802.11 networks

2. **Point Coordination Function (PCF)**
   - Built on top of DCF
   - Uses centralized, contention-free polling
   - Offers both asynchronous and time-bounded service
   - Optional feature rarely implemented

#### 802.11 Frame Format

- **Frame Control (2 bytes)**: Defines frame type and control information
- **Duration/ID (2 bytes)**: Sets duration for NAV (Network Allocation Vector)
- **Addresses (6 bytes each)**: Four address fields for source, destination, and base stations
- **Sequence Control (2 bytes)**: For fragment reassembly and duplicate detection
- **Frame Body (0-2312 bytes)**: Contains actual data
- **FCS (4 bytes)**: For error detection

#### IEEE 802.11 Standards

- **802.11a**: 5 GHz, up to 54 Mbps
- **802.11b**: 2.4 GHz, up to 11 Mbps
- **802.11g**: 2.4 GHz, up to 54 Mbps (backward compatible with 802.11b)
- **802.11n**: 2.4/5 GHz, up to 600 Mbps, MIMO technology
- **802.11ac**: 5 GHz, up to 7 Gbps, wider channels, more spatial streams
- **802.11ax (Wi-Fi 6)**: Improved efficiency, higher throughput in dense environments

### Bluetooth (IEEE 802.15)

Short-range wireless technology for personal area networks:

#### Characteristics

- 2.4 GHz ISM band divided into 79 channels of 1 MHz each
- Frequency-hopping spread spectrum (1600 hops/second)
- Range: 1-100 meters depending on device class
- Low power consumption
- Ad-hoc network (automatic discovery and connection)

#### Device Classes

Based on power and range:
1. **Class 1**: 100 mW, ~100 meters range
2. **Class 2**: 2.5 mW, ~10 meters range
3. **Class 3**: 1 mW, ~1 meter range

#### Protocol Architecture

Layered architecture with:
- **Core Protocols**: Radio, Baseband, Link Manager, L2CAP, SDP
- **Cable Replacement Protocol**: RFCOMM
- **Telephony Control Protocol**: TCS BIN
- **Adopted Protocols**: TCP/IP, OBEX, WAP

#### Network Topologies

1. **Piconet**
   - Basic Bluetooth network with one master and up to 7 active slaves
   - Master controls timing and frequency hopping
   - Slaves synchronize with master clock

2. **Scatternet**
   - Multiple interconnected piconets
   - Devices can participate in multiple piconets
   - Allows extending network range and capacity

#### Security Features

- Authentication using challenge-response
- Encryption for privacy
- Key management

## 3.9 Token Bus, Token Ring and Virtual LAN

### Token Bus (IEEE 802.4)

Token-passing protocol on a physical bus topology:

#### Characteristics

- Uses token-passing access method on bus topology
- Logical ring on a physical bus
- Only token holder can transmit
- Token passed to "next" logical station (not physically adjacent)
- Maximum token holding time ensures fair access

#### Operation

1. Station receives token
2. Transmits frames if needed (up to maximum time)
3. Passes token to next logical station
4. If station fails, token management protocols recover

#### Applications

- Industrial control systems
- Manufacturing automation
- Factory networking
- Process control environments

### Token Ring (IEEE 802.5)

Token-passing protocol on a physical ring topology:

#### Characteristics

- Data travels unidirectionally around the ring
- Only token holder can transmit
- Token released after frame transmission
- Physical star, logical ring implementation common

#### Operation

1. Station receives token, changes it to "busy" state
2. Transmits frames onto the ring
3. Receiving station copies frame but keeps it on ring
4. Originating station removes its frame from ring
5. Inserts free token for next station

#### Features

- Priority mechanism allows higher priority traffic
- Early token release for better throughput
- Active monitor for token management
- Automatic fault detection and recovery

### Virtual LAN (VLAN)

Logical segmentation of network independent of physical location:

#### Types

1. **Static VLAN (Port-based)**
   - Port on switch manually assigned to VLAN
   - Device connected to port automatically joins that VLAN
   - Most common implementation
   - Configuration changes require administrator intervention

2. **Dynamic VLAN**
   - Membership based on MAC address or user identity
   - VLAN Membership Policy Server (VMPS) tracks assignments
   - Devices retain VLAN membership regardless of physical location
   - More flexible but more complex to administer

#### Benefits

- **Simplified Administration**: Easier moves, adds, changes without physical recabling
- **Traffic Control**: Broadcasts confined to VLAN members
- **Security Enhancement**: Logical isolation of sensitive systems
- **Workgroup Creation**: Group users by function rather than location
- **Reduced Broadcast Domains**: Smaller, more manageable broadcast domains

#### Components

- **Switches**: Provide logical segmentation
- **Routers/Layer 3 Switches**: Enable inter-VLAN communication
- **VLAN Trunking**: Carries multiple VLANs over single link
- **VLAN Tagging (802.1Q)**: Identifies VLAN membership in frame header

#### Considerations

- **Management Complexity**: Requires careful planning and documentation
- **Router Dependence**: Inter-VLAN communication requires routing
- **Implementation Cost**: Requires VLAN-capable switches
- **Scalability Limits**: Number of VLANs supported by switches