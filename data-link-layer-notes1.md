# CHAPTER 3: DATA LINK LAYER

## 3.1 Functions of Data Link Layer

The Data Link Layer is the second layer in the OSI model, situated between the Physical Layer and the Network Layer. It transforms the raw bit stream from the physical layer into a reliable link and handles data transfer between network nodes.

### Key Functions:

1. **Framing**: Divides the bit stream into manageable data units called frames
2. **Physical Addressing**: Adds header with physical (MAC) addresses 
3. **Flow Control**: Prevents overwhelming receivers with too much data
4. **Error Control**: Detects and retransmits damaged or lost frames
5. **Access Control**: Determines which device can use the shared medium

### Data Link Layer Structure:

The IEEE has divided the data link layer into two sublayers:

![Data Link Layer Sublayers](https://i.imgur.com/MFnBxgE.png)

1. **Logical Link Control (LLC) Sublayer**:
   - Provides interface to network layer
   - Performs flow and error control
   - Independent of transmission medium
   - Defined by IEEE 802.2 standard

2. **Media Access Control (MAC) Sublayer**:
   - Responsible for physical addressing (MAC addresses)
   - Controls access to shared media
   - Specific to transmission medium
   - Defined by various IEEE standards (802.3, 802.11, etc.)

### LLC Sublayer in Detail:

The LLC sublayer provides a uniform interface to the network layer regardless of the underlying MAC sublayer. An LLC header includes:

- **DSAP (Destination Service Access Point)**: 1 byte identifying the receiving upper-layer process
- **SSAP (Source Service Access Point)**: 1 byte identifying the sending upper-layer process
- **Control**: Field containing control information (1 or 2 bytes)

LLC supports three types of frames:
1. **Information (I) frames**: Carry upper-layer data with control information
2. **Supervisory (S) frames**: Provide control functions without data
3. **Unnumbered (U) frames**: Used for control purposes with or without data

### MAC Sublayer in Detail:

The MAC sublayer handles:

- **Physical addressing**: Manages 48-bit MAC addresses of devices
- **Frame delimiting**: Marks beginning and end of frames
- **Error detection**: Verifies frame integrity using CRC
- **Media access control**: Determines when devices can transmit on shared media

Different MAC protocols are used depending on network type:
- Ethernet: CSMA/CD
- Wi-Fi: CSMA/CA
- Token Ring: Token passing

## 3.2 Data Link Control: Framing, Flow and Error Control

### Framing

Framing is the process of dividing the bit stream from the physical layer into data units called frames and adding a header and a trailer to each unit.

#### Parts of a Frame:

![Basic Frame Structure](/images/Screenshot%202025-04-04%20at%2005.53.47.png)

- **Header**: Contains control information such as addresses and sequence numbers
- **Payload**: Contains the actual data from the network layer
- **Trailer**: Contains error detection and control information
- **Flag**: Special bit patterns marking the beginning and end of the frame

#### Types of Framing:

1. **Fixed-Size Framing**:
   - All frames have the same size
   - No need for explicit frame delimiters
   - Easier to process
   - Examples: ATM cells (53 bytes)
   - **Drawback**: Internal fragmentation when data is smaller than frame size
   - **Solution**: Padding (adding extra bits to fill the frame)

2. **Variable-Size Framing**:
   - Frames can have different sizes
   - Requires methods to identify frame boundaries:
     
   a. **Length-Based Framing**:
   - Includes field specifying frame length
   - Used in Ethernet (802.3)
   - Example: If length field shows 1500, receiver counts 1500 bytes after header
   - **Problem**: If length field corrupted, frame boundaries lost

   b. **Character/Byte-Based Framing**:
   - Uses special characters as delimiters
   - Used in protocols like PPP
   - **Problem**: Delimiter might appear in data
   - **Solution**: Byte stuffing

   c. **Bit-Oriented Framing**:
   - Uses special bit patterns as delimiters
   - Used in protocols like HDLC
   - **Problem**: Delimiter might appear in data
   - **Solution**: Bit stuffing

#### Byte Stuffing:

Byte stuffing (also called character stuffing) is used in byte-oriented protocols to ensure that delimiter bytes in the data don't interfere with framing.

**Process**:
1. Choose an escape character (ESC) different from delimiter
2. When delimiter appears in data, insert ESC before it
3. If ESC appears in data, insert another ESC before it
4. Receiver removes inserted ESC characters

![Byte Stuffing Example](/images/Byte%20stuffing.jpeg)

**Example**:
- Delimiter: FLAG (01111110)
- Escape character: ESC (00011011)
- Original data: A•B•FLAG•C•ESC•D (where • represents any byte)
- Transmitted data: A•B•ESC•FLAG•C•ESC•ESC•D
- Receiver removes escape characters to recover original data

#### Bit Stuffing:

Bit stuffing is used in bit-oriented protocols to prevent bit patterns in the data from being mistaken for delimiters.

**Process**:
1. Choose a bit pattern as delimiter (e.g., 01111110)
2. If five consecutive 1s appear in data, insert a 0 after them
3. Receiver removes the inserted 0s

![Bit Stuffing Example](/images/Bit%20oriented%20Protocol.jpeg)

![Bit Stuffing Example](/images/framing_Bit%20oriented%20Protocolbit%20stuffing.jpeg)


**Example**:
- Delimiter: 01111110
- Original data: 01001111110110
- Transmitted data: 0100111110110 (0 inserted after five 1s)
- Receiver removes the inserted 0 to recover original data

### Flow Control

Flow control is the process of managing the rate of data transmission between two nodes to prevent a fast sender from overwhelming a slow receiver.


- speed matching mechanism
- Flow control coordinates the amount of data that can be sent before receiving an ACK
- A set of procedures that tells the sender how much data it can transmit before it must wait for an ACK from the receiver
- Receiver has a limited speed at which it can process incoming data and a limited amount of memory in which to store incoming data
- Receiver must inform the sender before the limits are reached and request that transmitter to send fewer frames or stop temporarily

#### Flow Control Mechanisms:

Flow control methods can be categorized based on channel conditions:

1. **Noiseless Channel Protocols**:
   - Simplest Protocol (no flow control)
   - Stop-and-Wait Protocol

2. **Noisy Channel Protocols**:
   - Stop-and-Wait ARQ
   - Go-Back-N ARQ
   - Selective Repeat ARQ

#### Simplest Protocol (No Flow Control):

- Unidirectional data flow (sender to receiver)
- Sender transmits frames without considering receiver status
- No acknowledgment, flow control, or error control
- Assumes receiver can process frames at any rate
- Unrealistic for practical networks

![Simplest Protocol](/images/simplest%20protocol.jpeg)

![Simplest Protocol](/images/simplest%20protocol_communication.jpeg)

#### Stop-and-Wait Protocol (For Noiseless Channels):

- Sender transmits one frame and waits for receiver to be ready
- Receiver sends acknowledgment when ready for next frame
- Sender waits until acknowledgment is received
- Simple but inefficient (low channel utilization)

![Stop-and-Wait Protocol](/images/stopandwait_protocol.jpeg)

![Stop-and-Wait Protocol](/images/stopandwait_protocolCommunication.jpeg)

**Example**:
- Sender sends Frame 0
- Receiver processes Frame 0, then sends ACK
- Sender receives ACK, sends Frame 1
- Process continues

**Efficiency Calculation**:
- Let Tt = transmission time for a frame
- Let Tp = propagation time (one-way)
- Total cycle time = Tt + 2Tp + Tack (≈ Tt + 2Tp if Tack is small)
- Utilization = Tt / (Tt + 2Tp)

**Numerical Example**:
- Frame size = 1000 bits
- Bandwidth = 1 Mbps
- Distance = 1000 km
- Propagation speed = 2 × 10⁸ m/s
- Tt = 1000 bits / 1 Mbps = 1 ms
- Tp = 1000 km / (2 × 10⁸ m/s) = 5 ms
- Utilization = 1 / (1 + 2×5) = 1/11 = 9.09%

#### Stop-and-Wait ARQ (For Noisy Channels):

ARQ (Automatic Repeat reQuest) is used when the channel has a possibility of errors.

**Mechanism**:
- Adds error detection and retransmission to Stop-and-Wait
- Sender maintains a copy of sent frame until positive ACK received
- Uses sequence numbers (typically 0 and 1 alternating)
- Receiver sends ACK if frame received correctly, NAK if error detected
- Timer mechanism handles lost frames

![Stop-and-Wait ARQ](/images/stopandwait%20automatic%20repeat%20request.jpeg)

**Example Scenarios**:
1. **Normal Operation**:
   - Sender sends Frame 0, starts timer
   - Receiver receives correct frame, sends ACK 1 (expecting Frame 1 next)
   - Sender receives ACK 1, sends Frame 1
   - Process continues with alternating sequence numbers

2. **Lost Frame**:
   - Sender sends Frame 0, starts timer
   - Frame is lost, no response from receiver
   - Sender's timer expires, retransmits Frame 0
   - Process continues normally

3. **Lost ACK**:
   - Sender sends Frame 0, starts timer
   - Receiver receives frame, sends ACK 1
   - ACK is lost
   - Sender's timer expires, retransmits Frame 0
   - Receiver receives duplicate Frame 0, resends ACK 1
   - Sender receives ACK 1, sends Frame 1

#### Pipelining and Sliding Window

To improve efficiency, we can send multiple frames before waiting for acknowledgments. This is called pipelining, implemented using sliding window protocols.

**Benefits**:
- Higher channel utilization
- Better throughput
- Optimal use of bandwidth

![Go-Back-N ARQ](/images/gobackn%20automatic%20repeat%20request.jpeg)

#### Go-Back-N ARQ:

In Go-Back-N ARQ, the sender can have up to N unacknowledged frames outstanding.

**Mechanism**:
- Uses sequence numbers modulo-2ᵐ (m is number of bits in sequence field)
- Sender window size: 2ᵐ-1
- Receiver window size: 1 (accepts only in-sequence frames)
- Cumulative acknowledgments (ACK n means all frames up to n-1 received)
- If error detected, receiver discards all subsequent frames
- After timeout, sender retransmits all unacknowledged frames

![Go-Back-N ARQ](/images/gobackn%20automatic%20repeat%20request.jpeg)
![Selective Repeat ARQ](/images/gobackn%20automatic%20repeat%20request_receiving%20window.jpeg)

![Selective Repeat ARQ](/images/gobackn%20automatic%20repeat%20request_Design.jpeg)

![Selective Repeat ARQ](/images/gobackn%20automatic%20repeat%20request_send%20window.jpeg)
**Example Scenario**:
1. **Normal Operation**:
   - Window size = 4 (sequence numbers 0-7, 3-bit field)
   - Sender sends frames 0, 1, 2, 3
   - Receiver acknowledges each frame (ACK 1, 2, 3, 4)
   - Sender slides window forward, sends frames 4, 5, 6, 7
   - Process continues

2. **Error and Recovery**:
   - Window size = 4, sender sends frames 0, 1, 2, 3
   - Frame 1 is lost or damaged
   - Receiver accepts frame 0, discards frames 2 and 3 (out of sequence)
   - Receiver sends ACK 1 (requesting frame 1)
   - Sender times out, retransmits frames 1, 2, 3
   - Process continues normally

**Efficiency Calculation**:
- Utilization approaches 1 (100%) as window size increases and error rate decreases
- Efficiency reduced by retransmission of correctly received frames after an error

#### Selective Repeat ARQ:

Selective Repeat ARQ improves efficiency by only retransmitting frames that are actually lost or damaged.

**Mechanism**:
- Uses sequence numbers modulo-2ᵐ
- Sender window size: 2ᵐ⁻¹
- Receiver window size: 2ᵐ⁻¹
- Individual acknowledgments for each frame
- Receiver accepts and buffers out-of-sequence frames
- Sender retransmits only specific frames that are not acknowledged

![Selective Repeat ARQ](/images/Selective%20Repeat%20Automatic%20Repeat%20Request_send%20window.jpeg)

![Selective Repeat ARQ](/images/Selective%20Repeat%20Automatic%20Repeat%20Request_send%20window.jpeg)

![Selective Repeat ARQ](/images/selective%20repeat%20automatic%20repeat%20request_design.jpeg)

**Example Scenario**:
1. **Normal Operation**:
   - Window size = 4 (sequence numbers 0-7, 4-bit field)
   - Sender sends frames 0, 1, 2, 3
   - Receiver acknowledges each frame individually
   - Sender slides window forward for each ACK

2. **Error and Recovery**:
   - Window size = 4, sender sends frames 0, 1, 2, 3
   - Frame 1 is lost or damaged
   - Receiver accepts frames 0, 2, 3 (buffers 2 and 3)
   - Receiver sends ACKs for frames 0, 2, 3
   - Sender times out for frame 1, retransmits only frame 1
   - Receiver accepts frame 1, delivers frames 0-3 in order
   - Process continues normally

**Why Window Size Must Be ≤ 2ᵐ⁻¹**:
- To avoid ambiguity in sequence numbers
- Example: With m=3 (sequence numbers 0-7):
  - If window size > 4, sequence numbers could be ambiguous
  - Sender might not know if ACK 0 refers to first or second occurrence of sequence 0

#### Piggybacking

- Technique to improve efficiency in bidirectional communication
- Combines acknowledgments with data frames
- Reduces overhead by eliminating separate ACK frames
- Delays ACKs slightly to wait for outgoing data frames

![Types of Errors](/images/piggybacking_protocol.jpeg)

### Error Control

Error control in the data link layer involves both error detection and correction. Let's look at the different methods:

#### Types of Errors:

1. **Single-Bit Error**: Only one bit in the data unit changes
2. **Burst Error**: Multiple consecutive bits change, from first corrupted bit to last

![Types of Errors](/images/Screenshot%202025-04-09%20at%2022.09.59.png)

#### Error Detection Methods:

1. **Parity Check**:
   - **Simple Parity**: Adds one bit to make total number of 1s even or odd
      
      - Receiver accepts data with even num ber of 1's
      - When there are odd number of 1's in sender, then parity checker will append 1 to the data to make the count of 1's even
      - If there were any transmission errors due to which the count of 1's becomes odd then receiver will reject

      **Performance of Parity Check**
      - It can detect single bit error
      - It can detect burst error only if the number of errors is odd


      11100001 -> 10100001 -> receiver rejects

      11100001 -> 10100101 -> receiver accepts

![Parity Check](/images/parity%20check.png)

   - **Two-dimensional Parity**: Creates parity bits for rows and columns
      
      - A block of bits is organized in rows and columns
      - The parity bit is calculated for each column and sent along with the data
      - The block of parity acts as the redundant bits 

      **Performance**
      - Increases the likelihood of detecting burst errors
      - If 2 bits in one data units are damaged and two bits in exactly the same position in another data units are also damaged, the Redundancy checker will not detect an error

![Parity Check](/images/LRC%20calculation.png)

![Parity Check](/images/data%20after%20lrc.png)

**Example** (Simple Parity):
- Data: 1001001 (three 1s)
- Even parity bit: 1 (to make total number of 1s even)
- Transmitted code: 10010011
- Can detect odd number of errors

2. **Checksum**:
   - Divides data into k segments of m bits
   - Adds segments using 1's complement arithmetic
   - Complements sum to get checksum
   - Appends checksum to data

![Checksum Process](/images/checksum.png)

**Example**:
- Data: 10011001 11100010 00100100 10000100
- Using 8-bit checksum:
  - Sum: 10011001 + 11100010 + 00100100 + 10000100 = 1000100011
  - Wrap extra bits: 00100011 + 10 = 00100101
  - Checksum (complement): 11011010
  - Transmitted: original data + 11011010
  - Verification: sum all segments + checksum = 11111111 → complement = 00000000 (no error)

**Operation on Sender side**
- Break the original message into `k` number of blocks with n bits in each block
- Sum all the k data blocks
- Add the carry to the sum, if any
- Do 1's complement to the sum = checksum
  ![Checksum Process](/images/checksum%20sender.png)

**Operation on Receiver side**
- Collect all the data blocks including the checksum
- Sum all the data blocks and checksum
- If the result is all 1's Accept, else Reject


**Performance**
- The checksum detects all errors involving and odd number of bits
- It detects most errors involving an even number of bits
- If one or more bits of a segments are damaged and the corresponding bit or bits of opposite value in a second segment are also damaged, the sum of those columns will not change and the receiver will not detect the errors

  ![Checksum Process](/images/checksum%20receiver.png)

3. **Cyclic Redundancy Check (CRC)**:
   - Based on binary division
   - Uses generator polynomial G(x)
   - More powerful than parity or checksum for burst errors

**CRC Process**:
1. Choose generator polynomial G(x) of degree n
2. Append n zeros to data bits
3. Divide using modulo-2 division (XOR)
4. Remainder is the CRC
5. Replace appended zeros with CRC
6. Transmit data + CRC

**Example**:
- Data: 101100
- Generator polynomial G(x) = x³ + x + 1 → 1011
- Append 3 zeros: 101100000
- Division process:
  ```
  1011 ) 101100000
          1011
          ----
           0010
           0000
           ----
            0100
            0000
            ----
             1000
             1011
             ----
              0110
              0000
              ----
               1100
               1011
               ----
                1110
                1011
                ----
                 101
  ```
- CRC = 101
- Transmitted: 101100101

### CRC Sender process
![CRC Process](/images/crc%20sender.png)

### CRC Receiver process
![CRC Process](/images/crc%20receiver.png)

**Properties of Good CRC Polynomials**:
- Not divisible by x (detects burst errors of length equal to polynomial length)
- Divisible by x+1 (detects odd number of bit errors)
- Contains at least three terms (detects all double-bit errors)

## 3.3 Error Detection and Correction

While error detection identifies errors, error correction can also locate and fix them.

### Hamming Code

Hamming code is a linear error-correcting code that can detect up to two-bit errors and correct single-bit errors.

**Hamming Code Algorithm**:
1. Number bit positions starting at 1
2. Positions that are powers of 2 (1, 2, 4, 8, 16...) are parity bits
3. Other positions contain data
4. Each parity bit covers specific bit positions:
   - P1 (position 1): checks bits 1, 3, 5, 7, 9, 11...
   - P2 (position 2): checks bits 2, 3, 6, 7, 10, 11...
   - P4 (position 4): checks bits 4, 5, 6, 7, 12, 13...
   - P8 (position 8): checks bits 8, 9, 10, 11, 12...
5. Each parity bit is set to make the parity of its group even (or odd)

![Hamming Code Bit Positions](https://i.imgur.com/9nfvqRT.png)

**Error Detection and Correction**:
1. Calculate all parity bits
2. If any parity check fails, there's an error
3. The binary number formed by the failed parity checks (1=failed, 0=passed) indicates the error position
4. Flip that bit to correct the error

**Example**:
For 7-bit data 1011001:

1. Need 4 parity bits (positions 1, 2, 4, 8) as 2³ < 11 ≤ 2⁴
2. Place data bits in non-parity positions: _ _ 1 _ 0 1 1 _ 0 0 1
3. Calculate parity bits (using even parity):
   - P1: checks bits 1, 3, 5, 7, 9, 11 → 1+0+1+0+1 = 3 (odd) → P1 = 1
   - P2: checks bits 2, 3, 6, 7, 10, 11 → 1+1+1+0+1 = 4 (even) → P2 = 0
   - P4: checks bits 4, 5, 6, 7 → 0+1+1 = 2 (even) → P4 = 0
   - P8: checks bits 8, 9, 10, 11 → 0+0+1 = 1 (odd) → P8 = 1
4. Complete code: 10100110001

If bit 6 changes to 0 during transmission:
1. Received code: 10100010001
2. Parity checks:
   - P1: 1+1+0+1+0+1 = 4 (even) → Passes
   - P2: 0+1+0+1+0+1 = 3 (odd) → Fails
   - P4: 0+0+0+1 = 1 (odd) → Fails
   - P8: 1+0+0+1 = 2 (even) → Passes
3. Error position: 0110 (binary) = 6 (decimal)
4. Bit 6 is flipped to correct the error

## 3.4 High-Level Data Link Control (HDLC) & Point-to-Point Protocol (PPP)

### High-Level Data Link Control (HDLC)

HDLC is a bit-oriented protocol for communication over point-to-point and multipoint links.

**Characteristics**:
- Bit-oriented (treats frames as sequences of bits)
- Supports full-duplex communication
- Provides both connection-oriented and connectionless service
- Uses bit stuffing for transparency

#### HDLC Configurations:

1. **Normal Response Mode (NRM)**:
   - Primary station controls secondary stations
   - Secondary can only transmit when polled by primary
   - Used in multipoint configurations

![HDLC Frame Format](/images/nrm.png)


2. **Asynchronous Balanced Mode (ABM)**:
   - Both stations act as primary and secondary
   - Either station can initiate transmission
   - Used in point-to-point configurations

![HDLC Frame Format](/images/abm.png)


3. **Asynchronous Response Mode (ARM)**:
   - Secondary can initiate transmission without permission
   - Primary still retains control
   - Rarely used today

<!-- ![HDLC Configurations](https://i.imgur.com/KlPUPFs.png) -->

#### HDLC Frame Format:

![HDLC Frame Format](/images/hdlc%20frame.png)

- **Flag (8 bits)**: 01111110, marks beginning and end of frame
- **Address (8+ bits)**: Identifies secondary station
- **Control (8 or 16 bits)**: Defines frame type and functions
- **Information (variable)**: Contains actual data
- **FCS (16 or 32 bits)**: Frame Check Sequence for error detection

#### HDLC Frame Types:

![HDLC Frame Format](/images/hdlc%20frame%20types.png)

1. **Information (I) Frames**:
   - Carry user data
   - Provide flow and error control
   - Control field format: 0 N(S) P/F N(R)
     - N(S): Sender's sequence number
     - N(R): Expected sequence number of next frame
     - P/F: Poll/Final bit

2. **Supervisory (S) Frames**:
   - Provide control functions without user data
   - Control field format: 10 Type P/F N(R)
   - Types:
     - Receive Ready (RR): Positive acknowledgment
     - Receive Not Ready (RNR): Temporary busy condition
     - Reject (REJ): Negative acknowledgment, go-back-N
     - Selective Reject (SREJ): Selective retransmission

3. **Unnumbered (U) Frames**:
   - Used for control purposes
   - Control field format: 11 M P/F M
   - M bits specify function (e.g., SABM, DISC, UA, FRMR)

![HDLC types](/images/hdlc%20type2.png)

### Point-to-Point Protocol (PPP)

PPP is a data link protocol used for direct connections between two nodes, commonly used for Internet dial-up connections.

**Characteristics**:
- Byte-oriented protocol
- Supports multiple network layer protocols
- Provides authentication
- Simpler than HDLC, with limited error control

#### PPP Architecture:

PPP consists of three main components:
1. **Encapsulation**: HDLC protocol for encapsulating datagrams over point-to-point links
2. **Link Control Protocol (LCP)**: Establishes, configures, and tests the data-link connection
3. **Network Control Protocols (NCPs)**: Family of Network Control Protocols (NCPs) to establish and configure different network layer protocols (IPv4, IPv6, AppleTalk, Novell IPX, and SNA Control Protocol)

![PPP Frame Format](/images/ppp%20components.png)

#### PPP Frame Format:

![PPP Architecture](/images/ppp%20frame.png)

- **Flag (8 bits)**: 01111110, marks beginning and end of frame
- **Address (8 bits)**: Always 11111111 (broadcast)
- **Control (8 bits)**: Always 00000011 (unnumbered frame)
- **Protocol (8 or 16 bits)**: Identifies network layer protocol
- **Information (variable)**: Data from network layer
- **FCS (16 or 32 bits)**: Frame Check Sequence for error detection

#### PPP Operation Phases:

PPP goes through several phases during operation:

1. **Dead**: Link not ready
2. **Establish**: LCP negotiates options
3. **Authenticate**: Optional authentication using PAP or CHAP
4. **Network**: NCP configures network layer protocols
5. **Terminate**: Link termination

![PPP Phases](/images/ppp%20operation%20phases.png)

#### PPP Authentication Protocols:

1. **Password Authentication Protocol (PAP)**:
   - Simple two-step authentication
   - Sends username/password in cleartext (insecure)
   - No protection against replay attacks

![PPP Phases](/images/pap%20authentication.png)

![PPP Phases](/images/pap%20process.png)

2. **Challenge Handshake Authentication Protocol (CHAP)**:
   - Three-way handshake:
     1. Authenticator sends challenge
     2. Peer responds with value calculated using hash function
     3. Authenticator verifies response
   - More secure than PAP
   - Periodic verification during connection

![PPP Phases](/images/chap%20authentication.png)

![PPP Phases](/images/chap%20process%201.png)
![PPP Phases](/images/chap%20process%202.png)


## 3.5 Channel Allocation Problem

Channel allocation refers to methods for sharing a common channel among multiple users.

### Static Channel Allocation

In static allocation, the channel is divided into fixed portions for each user.

**Mechanism**:
- For N users, channel divided into N equal parts
- Each user assigned fixed portion regardless of activity
- Typically uses Frequency Division Multiplexing (FDM)
- Simple but inefficient when user demand varies

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

Dynamic allocation assigns channel capacity based on demand.

**Mechanisms**:
- Allocates bandwidth on-demand
- No channel bandwidth wasted
- More complex implementation
- Types:
  - Centralized: Central controller assigns bandwidth
  - Distributed: Nodes coordinate among themselves

**Assumptions**:
1. Station Model: Independent frame generation
2. Single Channel: All stations use same channel
3. Collision: Overlapping transmissions cause errors
4. Time: Slotted or continuous
5. Carrier Sensing: May be present or absent

## 3.6 Multiple Access

Multiple access protocols allow multiple stations to share a medium. If there is a dedicated link between the sender and the receiver then data link control layer is sufficient,however if there is one dedicated link present then multiple stations can access the channel simultaneously.

Hence, multiple access protocols are required to decrease collision and avoid crosstalk.

![Pure ALOHA](/images/multiple%20access%20protocol.png)
### 1. Random Access Methods

- Stations transmit whenever they have data without central coordination.
- In this, All stations have same priority, that is not station has more priority than another station. Any station can send data depending on mediums state (Idle or Busy)
- Each station has the right to the medium without being controlled by any other station.
- If more than one station tries to send, there is an access conflict (Collision) and frames will be either destroyed or modified

### To avoid access conflict, each station follows a procedure
- When can the station access the medium?
- What can the station do if the medium is busy?
- How can the station determine the success or failure of the transmission?
- What can the station do if there is an access conflict?

#### ALOHA

The original random access protocol developed at the University of Hawaii. It was actually designed for WLAN but it is also applicable for shared medium.
In this, multiple stations can transmit data at the same time and can hence lead to collision and data being garbled.

**Pure ALOHA**:

* Pure ALOHA allows stations to transmit whenever they have data to be sent.
* When a station sends data it waits for an acknowledgement.
* If the acknowledgement doesn't come within the allotted time then the station waits for a random amount of time called __back-off time (Tb)__ and re-sends the data.
* Since different stations wait for different amount of time, the probability of further collision decreases.
* The throughput of pure aloha is maximized when frames are of uniform length.
* Whenever two frames try to occupy the channel at the same time, there will be a collision and both will be garbled.
* If the first bit of a new frame overlaps with just the last bit of a frame almost finished, both frames will be totally destroyed and both will have to be retransmitted later

- If collision occurs, retry after random time
- Vulnerable period: 2 × frame transmission time
- Throughput: S = G × e^(-2G)
- Maximum throughput: 18.4% when G = 0.5

![Pure ALOHA](/images/pure%20aloha.png)

![Pure ALOHA](/images/pure%20aloha%20example.png)


**Slotted ALOHA**:
* It was developed just to improve the efficiency of pure aloha as the chances for collision in pure aloha are high.
* The time of the shared channel is divided into discrete time intervals called slots.
* Sending of data is allowed only at the beginning of these slots.
* If a station misses out the allowed time, it must wait for the next slot. This reduces the probability of collision.

- Time divided into discrete slots equal to frame transmission time
- Stations can only begin transmission at slot boundaries
- Vulnerable period: 1 frame transmission time
- Throughput: S = G × e^(-G)
- Maximum throughput: 36.8% when G = 1

![Slotted ALOHA](/images/slotted%20aloha%20slot.png)

![Slotted ALOHA](/images/slotted%20aloha.png)

 **Pure and Slotted differences**
![Slotted ALOHA](/images/pure-slotted%20diff.png)

#### Carrier Sense Multiple Access (CSMA)

CSMA improves on ALOHA by listening to the channel before transmitting.

**Persistence Methods**:

1. **1-persistent CSMA**:
   - If channel idle, transmit immediately
   - If channel busy, continuously sense until idle, then transmit immediately
   - High collision probability with multiple waiting stations

* Before sending the data, the station first listens to the channel to see if anyone else is transmitting the data at that moment.
* If the channel is idle, the station transmits a frame.
* If busy, then it senses the transmission medium continuously until it becomes idle.
* Since the station transmits the frame with the probability of 1 when the carrier or channel is idle, this scheme of CSMA is called as 1-Persistent CSMA.
* The propagation delay has an important effect on the performance of the protocol.
* The longer the propagation delay, the more important this effect becomes, and the worse the performance of the protocol.

2. **Non-persistent CSMA**:
   - If channel idle, transmit immediately
   - If channel busy, wait random time before checking again
   - Reduces collisions but increases delay

* Before sending, a station senses the channel. If no one else is sending, the station begins doing so itself.
* However, if the channel is already in use, the station does not continually sense it for the purpose of seizing it immediately upon detecting the end of the previous transmission.
* Instead, it waits a random period of time and then repeats the algorithm. Consequently, this algorithm leads to better channel utilization but longer delays than 1-persistent CSMA. 

3. **p-persistent CSMA**:
   - If channel idle, transmit with probability p, delay with probability 1-p
   - If channel busy, wait until idle and apply above rule
   - Compromise between 1-persistent and non-persistent

* It applies to slotted channels.
* When a station becomes ready to send, it senses the channel.
* If it is idle, it transmits with a probability P.
* With a probability Q=1-P, it defers until the next slot.
* If that slot is also idle, it either transmits or defers again, with probabilities P and Q.
* This process is repeated until either the frame has been transmitted or another station has begun transmitting.
* In the latter case, the unlucky station acts as if there had been a collision (i.e., it waits a random time and starts again).
* If the station initially senses the channel busy, it waits until the next slot and applies the above

![CSMA Persistence Methods](/images/persistent.png)

**Vulnerable Time**: Propagation time (Tp)
**Performance**: Much better than ALOHA, but collisions still possible

#### CSMA/CD (Collision Detection)

CSMA/CD extends CSMA by detecting collisions during transmission.
* If two stations sense the channel to be idle and begin transmitting simultaneously, they will both detect the collision almost immediately.
* Rather than finish transmitting their frames, which are irretrievably garbled anyway, they should abruptly stop transmitting as soon as the collision is detected.
* Quickly terminating damaged frames saves time and bandwidth.
* This protocol, known as CSMA/CD (CSMA with Collision Detection) is widely used on LANs in the MAC sublayer.
* Access method used by Ethernet: CSMA/CD.
* At the point marked t0 a station has finished transmitting its frame.
* Any other station having a frame to send may now attempt to do so. If two or more stations decide to transmit simultaneously, there will be a collision.
* Collisions can be detected by looking at the power or pulse width of the received signal and comparing it to the transmitted signal.
* After a station detects a collision, it aborts its transmission, waits a random period of time, and then tries again, assuming that no other station has started transmitting in the meantime.
* Therefore, model for CSMA/CD will consist of alternating contention and transmission periods, with idle periods occurring when all stations are quiet.

**Process**:
1. Station listens before transmitting
2. If channel idle, begins transmission
3. While transmitting, monitors for collisions
4. If collision detected:
   - Aborts transmission immediately
   - Sends jam signal (ensures all stations detect collision)
   - Waits random time using binary exponential backoff
   - Tries again

![CSMA/CD Process](/images/csma%20cd.png)

**Key Requirement**:
- Minimum frame time ≥ 2 × maximum propagation time
- Ensures collisions are detected before transmission completes

**Example**: Ethernet (IEEE 802.3)
**Performance**: Up to 90% utilization possible

#### CSMA/CA (Collision Avoidance)

CSMA/CA is designed for wireless networks where collision detection is difficult.
* Carrier-sense multiple access with collision avoidance (CSMA/CA) is a network multiple access method in which carrier sensing is used, but nodes attempt to avoid collisions by beginning transmission only after the channel is sensed to be "idle".
* It is particularly important for wireless networks, where the collision detection of the alternative CSMA/CD is not possible due to wireless transmitters desensing their receivers during packet transmission.
* CSMA/CA is unreliable due to the hidden node problem and exposed terminal problem.
* Solution: RTS/CTS exchange.
* CSMA/CA is a protocol that operates in the Data Link Layer (Layer 2) of the OSI model.

**Components**:
1. **InterFrame Space (IFS)**: Waiting period after channel becomes idle
2. **Contention Window**: Randomized backoff time
3. **Acknowledgments**: Positive acknowledgment required for successful transmission

**Process**:
1. Station waits for DIFS (Distributed InterFrame Space) after channel idle
2. Enters random backoff within contention window
3. If channel remains idle during backoff, transmits
4. If channel becomes busy during backoff, pauses timer
5. After transmission, receiver waits SIFS (Short IFS) then sends ACK
6. If no ACK received, assumes collision and retries

**Example**: Wi-Fi (IEEE 802.11)

### 2. Controlled Access Methods

In controlled access, stations coordinate to determine which station can transmit.

#### Reservation

Stations reserve transmission time in advance.
* A station need to make a reservation before sending data.
* In each interval, a reservation frame precedes the data frames sent in that interval.
* If there are N stations in the system, there are exactly N reservation minislots in the reservation frame.
* Each minislot belongs to a station.
* When a station needs to send a data frame, it makes a reservation in its own minislot.
* The stations that have made reservations can send their data frames after the reservation frame.
**Process**:
1. Time divided into intervals
2. Each interval begins with reservation frame containing reservation slots
3. Each station has assigned reservation slot
4. Stations request transmission by marking their slot
5. Stations transmit data in order of reservation

![Reservation Process](/images/reservation.png)

**Advantages**: No collisions once reservations are made
**Disadvantages**: Overhead of reservation slots

#### Polling

A central controller (primary station) determines which station can transmit.

**Functions**:
1. **Select**: If the primary wants to send data, it tells the secondary to get ready to receive.
2. **Poll**: If the primary wants to receive data, it asks the secondaries if they have anything to send.

* The polling protocol requires one of the nodes to be designated as a
Master node (Primary station).
* The master node polls each of the nodes in a round-robin fashion.
* In particular, the master node first sends a message to node 1, saying that it (node 1) can transmit up to some maximum number of frames.
* After node 1 transmits some frames, the master node tells node 2 it (node 2) can transmit up to the maximum number of frames.
* The master node can determine when a node has finished sending its frames by observing the lack of a signal on the channel.

* The procedure continues in this manner, with the master node polling each of the nodes in a cyclic manner.
* The polling protocol eliminates the collision.
* This allows polling to achieve a much higher efficiency.
* The first drawback is that the protocol introduces a polling delay-the amount of time required to notify a node that it can transmit.
* The second drawback, which is potentially more serious, is that if the master node fails, the entire channel becomes inoperative.

**Process**:
1. Primary polls each secondary in sequence
2. Secondary responds with data if available, NAK if not
3. After receiving data, primary acknowledges
4. Primary moves to next secondary

![Polling Process](/images/polling.png)

**Efficiency**: T_t/(T_t + T_poll)
Where T_t is transmission time and T_poll is polling overhead

**Advantages**: No collisions, supports priorities
**Disadvantages**: Polling overhead, single point of failure

#### Token Passing

A special frame (token) grants permission to transmit.
* A station is authorized to send data when it receives a special frame called a token.
* Here there is no master node.
* A small, special-purpose frame known as a token is exchanged among the nodes in some fixed order.
* When a node receives a token, it holds onto the token only if it has some frames to transmit; otherwise, it immediately forwards the token to the next node.
* If a node does have frames to transmit when it receives the token, it sends up to a maximum number of frames and then forwards the token to the next node.
* Token passing is decentralized and highly efficient. But it has problems as well.
* For example, the failure of one node can crash the entire channel. Or if a node accidentally neglects to release the token, then some recovery procedure must be invoked to get the token back in circulation.
**Process**:
1. Token circulates among stations in logical order
2. Station with token can transmit for limited time
3. After transmission or if no data, passes token to next station
4. If token lost, recovery mechanisms activate

![Token Passing](/images/token%20passing.png)

**Network Arrangements**:
- **Token Ring**: Physical ring topology
- **Token Bus**: Physical bus with logical ring

![Token Ring vs Token Bus](/images/token%20passin%202.png)

**Performance**:
- For a < 1: S = 1/(1 + a/N)
- For a > 1: S = 1/{a(1 + 1/N)}

Where:
- S = throughput
- N = number of stations
- a = ratio of propagation delay to transmission time

**Advantages**: Deterministic access, fair, no collisions
**Disadvantages**: Token overhead, complex recovery

### 3. Channelization Methods

Channelization is a multiple-access method in which the available bandwidth of a link is shared in time, frequency, or through code, between different stations.

**Multiplexing**
* Multiplexing in computer networking means multiple signals are combined together thus travel simultaneously in a shared medium.
* Multiplexing = Sharing the bandwidth.

#### Frequency Division Multiple Access (FDMA)

FDMA divides the frequency spectrum into bands for different stations.
* In FDMA, the available bandwidth of the common channel is divided into bands that are separated by guard bands.
* The available bandwidth is shared by all stations.
* The FDMA is a data link layer protocol that uses FDM at the physical layer.

**Characteristics**:
- Each station assigned fixed frequency band
- Bands separated by guard bands to prevent interference
- Station has exclusive use of its band
- Simple implementation

![FDMA](/images/fdma1.png)

![FDMA](/images/fdma.png)

**Applications**:
- AM/FM radio broadcasting
- First-generation cellular systems
- Cable TV systems

#### Time Division Multiple Access (TDMA)

TDMA divides time into slots for different stations.
* In TDMA, the bandwidth is just one channel that is time shared between different stations.
* The entire bandwidth is just one channel.
* Stations share the capacity of the channel in time

**Characteristics**:
- Time divided into slots, assigned to stations
- Each station transmits only during its slots
- Requires synchronization
- More efficient use of bandwidth than FDMA

![TDMA](/images/tdma.png)

**Types**:
- **Synchronous TDMA**: Fixed allocation of time slots
- **Statistical TDMA**: Dynamic allocation based on demand

**Applications**:
- Digital cellular systems
- Satellite communication
- Digital telephony

#### Code Division Multiple Access (CDMA)

CDMA allows multiple stations to use the entire bandwidth simultaneously through code division.
* In CDMA, one channel carries all transmissions simultaneously.
* CDMA differs from FDMA because only one channel occupies the entire bandwidth of the link.
* It differs from TOMA because all stations can send data simultaneously; there is no time

The assigned codes have two properties:
1. If we multiply each code by another, we get O.
2. If we multiply each code by itself, we get 4 (the number of stations).
Example:

Data = (d₁.c₁ + d₂.c₂ + d₃.c₃+ d₄.c₄)Xc₁ = 4 x d₁

**Characteristics**:
- Based on spread spectrum technology
- Each station assigned unique code sequence (chip sequence)
- All stations transmit on same channel simultaneously
- Receiver extracts signal using code correlation

![CDMA](/images/cdma.png)

**Process**:
1. Each bit multiplied by station's chip sequence
2. Receiver multiplies received signal by same chip sequence
3. Original data recovered through integration and threshold detection

**Advantages**:
- Resistant to jamming and interference
- Secure transmission
- Soft capacity limits

**Applications**:
- 3G cellular systems
- GPS
- Military communications

## 3.7 Wired LAN: Ethernet Standards and FDDI

### Ethernet (IEEE 802.3)

Ethernet is the most widely deployed LAN technology, operating primarily at the data link and physical layers.

#### Ethernet Naming Convention

Ethernet versions are named according to the format:
[Speed][BASE][Medium]

Common standards include:
- **10BASE5**: 10 Mbps over thick coaxial cable (500m) - "Thicknet"
- **10BASE2**: 10 Mbps over thin coaxial cable (185m) - "Thinnet"
- **10BASE-T**: 10 Mbps over twisted pair
- **100BASE-TX**: 100 Mbps over twisted pair (Fast Ethernet)
- **1000BASE-T**: 1 Gbps over twisted pair (Gigabit Ethernet)
- **10GBASE-T**: 10 Gbps over twisted pair

![Ethernet Evolution](https://i.imgur.com/VnIX0Qc.png)

#### Ethernet Frame Format

![Ethernet Frame Format](https://i.imgur.com/7OA7LJ4.png)

- **Preamble (7 bytes)**: Alternating 1s and 0s for synchronization
- **Start Frame Delimiter (1 byte)**: 10101011 marking frame start
- **Destination Address (6 bytes)**: MAC address of intended recipient
- **Source Address (6 bytes)**: MAC address of sender
- **Type/Length (2 bytes)**: Protocol type or frame length
- **Data (46-1500 bytes)**: Actual payload data (minimum 46 bytes)
- **FCS (4 bytes)**: CRC for error detection

#### Ethernet Operation

Traditional Ethernet operates using CSMA/CD:
1. Station listens for carrier (carrier sense)
2. If medium is idle, transmits immediately
3. If collision detected, sends jam signal
4. Backs off using binary exponential backoff algorithm
5. Retries transmission

**Collision Domain**:
- Network segment where collisions can occur
- Limited by repeaters, bridges, switches, routers

**Performance**:
- Original shared Ethernet: ~30-40% utilization under load
- Switched Ethernet: Nearly 100% utilization possible

#### Modern Ethernet Developments

1. **Switched Ethernet**:
   - Dedicated links between stations and switch
   - Full-duplex operation (simultaneous send/receive)
   - No collisions, no CSMA/CD needed
   - Higher throughput

![Switched Ethernet](https://i.imgur.com/i9LnP1I.png)

2. **Fast Ethernet (100 Mbps)**:
   - 100BASE-TX, 100BASE-FX
   - Compatible with 10 Mbps Ethernet
   - Same frame format and MAC

3. **Gigabit Ethernet (1 Gbps)**:
   - 1000BASE-T, 1000BASE-SX, 1000BASE-LX
   - Carrier extension and frame bursting
   - Full-duplex operation predominant

4. **10 Gigabit Ethernet (10 Gbps)**:
   - 10GBASE-T, 10GBASE-SR, 10GBASE-LR
   - Full-duplex only, no CSMA/CD
   - Focus on fiber optic and enhanced twisted pair

### Fiber Distributed Data Interface (FDDI)

FDDI is a high-speed fiber optic token ring network technology.

**Characteristics**:
- 100 Mbps data rate
- Fiber optic medium
- Dual counter-rotating rings for redundancy
- Maximum network diameter of 200 kilometers
- Support for thousands of stations

![FDDI Dual Ring](https://i.imgur.com/a5ysFRM.png)

#### FDDI Architecture

FDDI uses a dual ring architecture:
- **Primary Ring**: Main data path, normally active
- **Secondary Ring**: Backup path, activates if primary fails
- **Dual Attachment Stations (DAS)**: Connect to both rings
- **Single Attachment Stations (SAS)**: Connect to primary ring only

**Ring Recovery**:
- If link or station fails, rings reconfigure
- Creates single functional ring
- Self-healing capability

#### FDDI Frame Format

![FDDI Frame Format](https://i.imgur.com/ZNuxRQY.png)

- **Preamble (1 byte)**: For synchronization
- **Start Delimiter (1 byte)**: Marks frame beginning
- **Frame Control (1 byte)**: Specifies frame type
- **Destination Address (2-6 bytes)**: Address of recipient
- **Source Address (2-6 bytes)**: Address of sender
- **Data (variable)**: Actual payload
- **FCS (4 bytes)**: CRC for error detection
- **End Delimiter (1 byte)**: Marks frame end

#### FDDI Operation

FDDI uses a timed token rotation protocol:
1. Token circulates around ring
2. Station captures token to transmit
3. Multiple frames may be sent up to token holding time (THT)
4. Token released after transmission
5. Target token rotation time (TTRT) negotiated

**Applications**:
- Backbone networks
- High-speed interconnection between buildings
- Environments requiring fault tolerance

## 3.8 Wireless LAN: IEEE 802.11x and Bluetooth Standards

### IEEE 802.11 (Wi-Fi)

IEEE 802.11 is a set of standards for wireless local area networks.

#### 802.11 Architecture

Basic components of 802.11 networks:
- **Stations (STA)**: Computing devices with wireless network interfaces
- **Access Points (AP)**: Base stations for wireless network
- **Basic Service Set (BSS)**: Group of stations using same AP
- **Distribution System (DS)**: System connecting multiple APs
- **Extended Service Set (ESS)**: Multiple BSSs connected by DS

![802.11 Architecture](https://i.imgur.com/vIFcZKm.png)

#### MAC Sublayer

IEEE 802.11 defines two MAC sublayers:

1. **Distributed Coordination Function (DCF)**:
   - Uses CSMA/CA as access method
   - Basic access method required in all implementations
   - Provides asynchronous data service

2. **Point Coordination Function (PCF)**:
   - Optional extension built on top of DCF
   - Uses centralized, contention-free polling
   - Provides both asynchronous and time-bounded service
   - Rarely implemented in commercial products

![802.11 MAC Sublayers](https://i.imgur.com/LYbhBCg.png)

#### CSMA/CA in 802.11

The CSMA/CA process in 802.11:

1. **Physical Carrier Sensing**:
   - Station checks if medium is idle

2. **Virtual Carrier Sensing**:
   - Network Allocation Vector (NAV)
   - Time medium will be busy based on Duration field

3. **InterFrame Spaces**:
   - SIFS (Short IFS): Highest priority, for ACKs
   - PIFS (PCF IFS): Medium priority, for PCF
   - DIFS (DCF IFS): Lowest priority, for DCF

4. **Exponential Backoff**:
   - Random delay before transmission attempt
   - Contention window doubles after each collision

![CSMA/CA Process](https://i.imgur.com/GpGEDKU.png)

**Hidden Terminal Problem**:
- Stations unable to detect each other's transmissions
- Solved using RTS/CTS mechanism

![Hidden Terminal Problem](https://i.imgur.com/q1H1JT0.png)

#### 802.11 Frame Format

![802.11 Frame Format](https://i.imgur.com/xnpBXMR.png)

- **Frame Control (2 bytes)**: Type, subtype, flags
- **Duration/ID (2 bytes)**: Sets NAV value
- **Addresses**: Four address fields for:
  - Source address
  - Destination address
  - Transmitting station address
  - Receiving station address
- **Sequence Control (2 bytes)**: For fragmenting and reassembling
- **Data (0-2312 bytes)**: Actual payload
- **FCS (4 bytes)**: CRC for error detection

#### IEEE 802.11 Standards

Major 802.11 standards include:

| Standard | Frequency | Max Data Rate | Range | Key Features |
|----------|-----------|---------------|-------|--------------|
| 802.11a  | 5 GHz     | 54 Mbps       | ~35m  | OFDM modulation |
| 802.11b  | 2.4 GHz   | 11 Mbps       | ~40m  | DSSS modulation |
| 802.11g  | 2.4 GHz   | 54 Mbps       | ~40m  | Backward compatible with 802.11b |
| 802.11n  | 2.4/5 GHz | 600 Mbps      | ~70m  | MIMO technology, channel bonding |
| 802.11ac | 5 GHz     | 6.9 Gbps      | ~35m  | MU-MIMO, wider channels |
| 802.11ax | 2.4/5/6 GHz | 9.6 Gbps    | ~40m  | OFDMA, higher efficiency |

### Bluetooth (IEEE 802.15)

Bluetooth is a wireless technology standard for exchanging data over short distances.

**Characteristics**:
- Operates in 2.4 GHz ISM band
- Short-range communication (1-100 meters)
- Frequency-hopping spread spectrum (1600 hops/second)
- Low power consumption
- Ad-hoc network formation

![Bluetooth Concept](https://i.imgur.com/9i7c0Iv.png)

#### Bluetooth Device Classes

Bluetooth devices are classified by power and range:

| Class | Max Power  | Range       | Typical Devices |
|-------|------------|-------------|-----------------|
| 1     | 100 mW     | ~100 meters | Access points, USB dongles |
| 2     | 2.5 mW     | ~10 meters  | Mobile phones, headsets |
| 3     | 1 mW       | ~1 meter    | Keyboards, mice |

#### Bluetooth Protocol Architecture

Bluetooth uses a layered protocol architecture:

![Bluetooth Protocol Stack](https://i.imgur.com/U3P3EPO.png)

**Core Protocols**:
- **Radio**: Physical layer specification
- **Baseband**: MAC procedures, channel control
- **Link Manager Protocol (LMP)**: Link setup and control
- **Logical Link Control and Adaptation Protocol (L2CAP)**: Multiplexing, segmentation
- **Service Discovery Protocol (SDP)**: Device capability discovery

**Cable Replacement Protocol**:
- **RFCOMM**: Serial port emulation

**Adopted Protocols**:
- **TCP/IP**: Internet connectivity
- **OBEX**: Object exchange
- **WAE/WAP**: Wireless application environment

#### Bluetooth Network Topologies

1. **Piconet**:
   - Basic Bluetooth network
   - One master device and up to 7 active slave devices
   - Up to 255 parked devices (inactive)
   - All devices share master's clock and hopping sequence

![Bluetooth Piconet](https://i.imgur.com/CGGqT0U.png)

2. **Scatternet**:
   - Multiple interconnected piconets
   - Device can be master in one piconet, slave in another
   - Allows extending network range and capacity

![Bluetooth Scatternet](https://i.imgur.com/IM0dvj5.png)

#### Bluetooth Security

Bluetooth implements several security features:

1. **Authentication**:
   - Challenge-response mechanism
   - Verifies identity of devices

2. **Encryption**:
   - Based on shared secret key
   - Protects data confidentiality

3. **Authorization**:
   - Controls access to services and resources

4. **Pairing**:
   - Process of creating shared keys
   - Simple Secure Pairing (SSP) in newer versions

## 3.9 Token Bus, Token Ring and Virtual LAN

### Token Bus (IEEE 802.4)

Token Bus combines logical ring control with physical bus topology.

**Characteristics**:
- Physical bus topology
- Logical ring for token passing
- Station can transmit only when it has the token
- Token passed to next logical station (not physically adjacent)
- Used primarily in industrial automation

![Token Bus Architecture](https://i.imgur.com/E85mPcR.png)

**Operation**:
1. Stations organized in logical ring
2. Station with token can transmit for limited time
3. When finished, passes token to next logical station
4. If station fails, token management protocols recover

**Token Management**:
- **Initialization**: Creating initial token
- **Adding Stations**: Integrating new stations into logical ring
- **Removing Stations**: Handling departing or failed stations
- **Lost Token Recovery**: Detecting and replacing lost tokens

### Token Ring (IEEE 802.5)

Token Ring uses token passing on a physical ring topology.

**Characteristics**:
- Physical ring topology (often implemented as logical ring on star)
- Token circulates around ring
- Station can transmit only when it has the token
- Data flows in one direction around ring

![Token Ring Architecture](https://i.imgur.com/1nrZCcH.png)

**Frame Transmission Process**:
1. Station waits for token
2. Converts token to start-of-frame sequence
3. Transmits data frame
4. Frame circulates completely around ring
5. Originating station removes its frame
6. Inserts new token for next station

**Frame Format**:
- **Starting Delimiter**: Marks beginning of frame
- **Access Control**: Contains token and priority bits
- **Frame Control**: Identifies frame type
- **Destination Address**: MAC address of recipient
- **Source Address**: MAC address of sender
- **Data**: Actual payload
- **FCS**: CRC for error detection
- **Ending Delimiter**: Marks end of frame
- **Frame Status**: Contains acknowledgment bits

![Token Ring Frame Format](https://i.imgur.com/NVcv4Y7.png)

**Token Ring Features**:
- **Priority System**: Higher priority traffic can preempt lower
- **Early Token Release**: Releases token immediately after transmission
- **Monitor Station**: Handles ring maintenance
- **Fault Management**: Automatic reconfiguration after failure

### Virtual LAN (VLAN)

VLAN is a logical grouping of network devices regardless of physical location.

**Definition**:
- Logical segmentation of network independent of physical topology
- Creates separate broadcast domains within a switch
- Devices in same VLAN communicate as if on same LAN

![VLAN Concept](https://i.imgur.com/cxEqGHM.png)

#### Types of VLANs

1. **Port-Based (Static) VLAN**:
   - Each switch port assigned to specific VLAN
   - Device connected to port becomes part of that VLAN
   - Simple configuration but less flexible
   - Most common implementation

![Port-Based VLAN](https://i.imgur.com/Q8g9rCu.png)

2. **MAC Address-Based (Dynamic) VLAN**:
   - VLAN membership based on device's MAC address
   - Device gets same VLAN regardless of connection point
   - More flexible but more complex
   - Requires VLAN Membership Policy Server (VMPS)

3. **Protocol-Based VLAN**:
   - VLAN membership based on protocol type (e.g., IP, IPX)
   - Useful for segregating traffic by protocol

4. **Application-Based VLAN**:
   - VLAN membership based on application type
   - Used for QoS implementation

#### VLAN Trunking

VLAN trunking allows multiple VLANs to travel over a single link:

- **IEEE 802.1Q**: Adds 4-byte tag to frame header
- **VLAN Tagging**: Identifies VLAN membership in frame
- **Trunk Links**: Carry traffic for multiple VLANs between switches

![VLAN Trunking](https://i.imgur.com/xK1IzpY.png)

#### Benefits of VLANs

1. **Improved Security**: Segregation of sensitive systems
2. **Reduced Broadcast Traffic**: Smaller broadcast domains
3. **Flexible Network Design**: Logical grouping regardless of location
4. **Simplified Administration**: Easier moves, adds, and changes
5. **Load Balancing**: Better distribution of network traffic
6. **Traffic Management**: QoS implementation by VLAN

#### VLAN Considerations

- **Inter-VLAN Routing**: Requires Layer 3 device (router or Layer 3 switch)
- **Management Complexity**: Requires careful planning and documentation
- **Implementation Costs**: Requires VLAN-capable switches
- **Scalability**: Limited by switch capabilities

![Inter-VLAN Routing](https://i.imgur.com/MZJP4sK.png)

## Summary of Key Concepts

The Data Link layer provides several critical functions in network communication:

1. **Framing**: Organizes bits into manageable units
2. **Error Control**: Detects and potentially corrects transmission errors
3. **Flow Control**: Prevents sender from overwhelming receiver
4. **Access Control**: Manages access to shared media

Key protocols and technologies include:

- **Error Detection**: Parity, Checksum, CRC
- **Error Correction**: Hamming Code
- **Flow Control**: Stop-and-Wait, Go-Back-N, Selective Repeat
- **Medium Access**: CSMA/CD, CSMA/CA, Token Passing
- **Standards**: Ethernet (802.3), Wi-Fi (802.11), Bluetooth (802.15)
- **Logical Organization**: VLANs

Understanding these concepts provides the foundation for reliable data communication over various network types and media.

## Practice Questions

1. Explain the difference between byte stuffing and bit stuffing with examples.

2. A sender needs to send a frame of 1000 bits to a receiver using Stop-and-Wait protocol. Propagation delay is 5 ms and transmission rate is 100 kbps. Calculate:
   
   a. Transmission time
   
   b. Total time to send the frame and receive acknowledgment

   c. Channel utilization

3. Using CRC with generator polynomial x³ + x + 1, compute the CRC for data 10110.

4. For a 7-bit data 1010101, calculate the Hamming code with even parity.

5. Compare Go-Back-N ARQ and Selective Repeat ARQ in terms of efficiency, complexity, and buffer requirements.

6. Explain the hidden terminal problem in wireless networks and how it is addressed.

7. Calculate the maximum efficiency of Pure ALOHA and Slotted ALOHA.

8. Describe the advantages and disadvantages of static and dynamic channel allocation.

9. Compare the frame formats of Ethernet and 802.11.

10. Explain how VLANs improve network security and performance.
