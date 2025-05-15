# Unit 5: Transport Layer

## Introduction to Transport Layer

The Transport Layer in the Open System Interconnection (OSI) model is responsible for end-to-end delivery over a network. Whereas the network layer is concerned with the end-to-end delivery of individual packets and it does not recognize any relationship between those packets.

• This layer treats each packet independently because each packet belongs to a different message.
• The transport layer ensures that each message should reach its destination completely and in order so that it maintains error and flow control to the source to destination to ensure proper data transmission.  
• The transport layer establishes a connection between two end ports. A connection is a single logical path from source to destination which is associated with all the packets in a message.
• Transport Layer uses some standard protocols to enhance its functionalities are TCP (Transmission Control Protocol), UDP (User Datagram Protocol), DCCP (Datagram Congestion Control Protocol), etc.

![Transport Layer Relationship](relationship_diagram.png)
*Figure shows the relationship of the transport layer to the network and session layer.*

## 5.1 Functions of Transport Layer

Specific functions of the transport layer are as follows:

### 1. Service-point addressing

• Computers often run many programs at the same time. Due to this, source-to-destination delivery means delivery from a specific job (currently running program) on one computer to a specific job (currently running program) on the other system not only one computer to the next.
• For this reason, the transport layer added a specific type of address to its header, it is referred to as a service point address or port address.
• By this address each packet reaches the correct computer and also the transport layer gets the complete message to the correct process on that computer.

### 2. Segmentation and Reassembly

• In segmentation, a message is divided into transmittable segments; each segment containing a sequence number. This number enables this layer to reassemble the message.
• Upon arriving at its destination system message is reassembled correctly, identify and replaces packets that were lost in transmission.

### 3. Connection Control

It can be either of two types:
- **Connectionless Transport Layer**
- **Connection Oriented Transport Layer**

#### i) Connectionless Transport Layer
• This Transport Layer treats each packet as an individual and delivers it to the destination machine.
• In this type of transmission, the receiver does not send an acknowledgment to the sender about the receipt of a packet. This is a faster communication technique.

#### ii) Connection Oriented Transport Layer
• This Transport Layer creates a connection with the Transport Layer at the destination machine before transmitting the packets to the destination.
• To Create a connection following three steps are possible:
  - Connection establishment
  - Data transfer
  - Connection termination

When all the data are transmitted connection is terminated. Connectionless Service is less reliable than connection Oriented Service.

### 4. Multiplexing and Demultiplexing

• Multiple packets from diverse applications are transmitted across a network needs very dedicated control mechanisms, which are found in the transport layer.
• The transport layer accepts packets from different processes. These packets are differentiated by their port numbers and pass them to the network layer after adding proper headers.
• In Demultiplexing, at the receiver's side to obtain the data coming from various processes. It receives the segments of data from the network layer and delivers it to the appropriate process running on the receiver's machine.

### 5. Flow control

• The transport layer also responsible for the flow control mechanism between the adjacent layers of the TCP/IP model.
• It does not perform across a single link even it performs an end-to-end node.
• By imposing flow control techniques data loss can be prevented from the cause of the sender and slow receiver.
• For instance, it uses the method of sliding window protocol in this method receiver sends a window back to the sender to inform the size of the data is received.

### 6. Error Control

• Error Control is also performed end to end like the data link layer.
• In this layer to ensure that the entire message arrives at the receiving transport layer without any error(damage, loss or duplication). Error Correction is achieved through retransmission of the packet.
• The data has arrived or not and checks for the integrity of data, it uses the ACK and NACK services to inform the sender.

## 5.2 Elements of Transport Protocols

### Transport Protocol Environment

![Transport Protocol Environment](environment_diagram.png)
*(a) Environment of the data link layer.*
*(b) Environment of the transport layer.*

### End-to-end delivery

The transport layer transmits the entire message to the destination. Therefore, it ensures the end-to-end delivery of an entire message from a source to the destination.

### Reliable delivery

The transport layer provides reliability services by retransmitting the lost and damaged packets.

The reliable delivery has four aspects:
- Error control
- Sequence control
- Loss control
- Duplication control

#### Error Control
• The primary role of reliability is Error Control. In reality, no transmission will be 100 percent error-free delivery. Therefore, transport layer protocols are designed to provide error-free transmission.
• The data link layer also provides the error handling mechanism, but it ensures only node-to-node error-free delivery. However, node-to-node reliability does not ensure the end-to-end reliability.
• The data link layer checks for the error between each network. If an error is introduced inside one of the routers, then this error will not be caught by the data link layer. It only detects those errors that have been introduced between the beginning and end of the link. Therefore, the transport layer performs the checking for the errors end-to-end to ensure that the packet has arrived correctly.

#### Sequence Control
• The second aspect of the reliability is sequence control which is implemented at the transport layer.
• On the sending end, the transport layer is responsible for ensuring that the packets received from the upper layers can be used by the lower layers. On the receiving end, it ensures that the various segments of a transmission can be correctly reassembled.

#### Loss Control
Loss Control is a third aspect of reliability. The transport layer ensures that all the fragments of a transmission arrive at the destination, not some of them. On the sending end, all the fragments of transmission are given sequence numbers by a transport layer. These sequence numbers allow the receiver's transport layer to identify the missing segment.

#### Duplication Control
Duplication Control is the fourth aspect of reliability. The transport layer guarantees that no duplicate data arrive at the destination. Sequence numbers are used to identify the lost packets; similarly, it allows the receiver to identify and discard duplicate segments.

### Flow Control
Flow control is used to prevent the sender from overwhelming the receiver. If the receiver is overloaded with too much data, then the receiver discards the packets and asking for the retransmission of packets. This increases network congestion and thus, reducing the system performance. The transport layer is responsible for flow control. It uses the sliding window protocol that makes the data transmission more efficient as well as it controls the flow of data so that the receiver does not become overwhelmed. Sliding window protocol is byte oriented rather than frame oriented.

### Addressing

![Addressing](addressing_diagram.png)
*TSAPs, NSAPs and transport connections.*

• According to the layered model, the transport layer interacts with the functions of the session layer. Many protocols combine session, presentation, and application layer protocols into a single layer known as the application layer. In these cases, delivery to the session layer means the delivery to the application layer. Data generated by an application on one machine must be transmitted to the correct application on another machine. In this case, addressing is provided by the transport layer.
• The transport layer provides the user address which is specified as a station or port. The port variable represents a particular TS user of a specified station known as a Transport Service access point (TSAP). Each station has only one transport entity.
• The transport layer protocols need to know which upper-layer protocols are communicating.

### Connection Establishment

![Connection Establishment](connection_establishment.png)

Three protocol scenarios for establishing a connection using a three-way handshake. CR denotes CONNECTION REQUEST.
(a) Normal operation
(b) Old CONNECTION REQUEST appearing out of nowhere
(c) Duplicate CONNECTION REQUEST and duplicate ACK

### Connection Release

CONNECTION RELEASE Connection at transport can be released in two ways:
1. **Asymmetric**: if one of host terminates connection, then in both the direction, data communication will be terminated.
2. **Symmetric**: if one of the host disconnects connection, then it can not send the data but it can receive it.

#### Asymmetric Release
Abrupt disconnection with loss of data.

#### Four protocol scenarios for releasing a connection
(a) Normal case of a three-way handshake
(b) Final ACK lost
(c) Response lost
(d) Response lost and subsequent DRs lost

TCP Connection Release uses symmetric approach. It is called Four Way handshaking for connection termination.

### Flow Control and Buffering

Transport layer manages end to end flow. If the receiver is not able to cope with the flow of data, then data flow should be controlled from sender side, that part is done on Transport layer.

Data link layer is also doing flow control, but it controls flow of data between adjacent nodes in path from source to destination.

Reasons of packet loss at receiver is slow processing speed or insufficient buffer to store the data.

Buffers are allocated at sender and receiver side. If the network service is reliable, so every send TPDU sent will be delivered to receiver and will be buffered and processes at receiver, so no need to keep buffer at sender.

But if network service is unreliable and receiver may not be able to handle every incoming TPDU then sender should also keep a buffer, where copy of TPDU resides until its ACK comes.

Buffers can be allocated in fixed size when connection sets up or buffer can be varied dynamically according to free memory. First case is called static buffer allocation.

![Buffer Types](buffer_types.png)
*(a) Chained fixed-size buffers.*
*(b) Chained variable-sized buffers.*
*(c) One large circular buffer per connection.*

**Dynamic Buffer Allocation**: as connections are opened and closed, memory available changes, so sender and receiver dynamically adjust buffer allocations.

In dynamic buffer allocation, initially sender will request certain number of buffers based on perceived need. Receiver will grant as many buffers as it can.

```
Sender              Receiver
<Req. 8 buffers>
                    <Buff Alloc. 4>
<Seq.:0, data=m0>
<Seq.:1, data=m1>
<Seq.:2, data=m2>
                    <Ack:2, Buf:2>
<Seq.:3, data=m3>
<Seq.:4, data=m4>
```

### Multiplexing

![Multiplexing Types](multiplexing_types.png)
*(a) Upward multiplexing. (b) Downward multiplexing.*

#### Multiplexing & De-multiplexing
• The addressing mechanism allows multiplexing and De-multiplexing by the transport layer

##### Multiplexing
• At the sender site, there may be several processes that need to send packets. However, there is only one transport layer protocol at any time. This is a many-to-one relationship and requires multiplexing.
• The protocol accepts messages from different processes, differentiated by their assigned port numbers. After adding the header, the transport layer passes the packet to the network layer

##### De-multiplexing
• At the receiver site, the relationship is one-to-many and requires Demultiplexing. The transport layer receives datagrams from the network layer.
• After error checking and dropping of the header, the transport layer delivers each message to the appropriate process based on the port number

### CRASH RECOVERY

• Hosts and routers are subject to crash.
• Router crash is easier to handle since transport entities are alive at the host, routers are only intermediate nodes which forwards packet, they do not have transport layer entity.

**How to recover from host crashes?**
One client(host) is sending a file to server(receiver host). Transport layer at server simply passes TPDU to transport layer. While transmission was ongoing, server crashes.

Server crashes and comes up → table initiated, does not know where it was?

Server sends a broadcast TPDU to all host, announcing that it had just crashed and requesting that its clients inform it about status of all open connection.

Each client can be in one of two states:
- S0: no outstanding TPDU
- S1: one TPDU outstanding

Now it seems that if TPDU is outstanding, client should transmit it, but there can be different hidden situations:

1. If server has first sent ACK and before it can send TPDU to next layer, server crashes. In this case, client will get ACK so it will not retransmit, and TPDU is lost by server.
2. If server first sends packet to next layer, then it crashes before it can send ACK. In this case though server has already received TPDU, client thinks TPDU is lost and it will retransmit.

Server(Receiving host) can be programmed in two ways: 1. ACK first 2. write first

Three events are possible at server, sending ACK(A), sending packet to next layer(W), crashing (C).
Three events can occur in six different cases: AC(W) AWC C(AW), C(WA) WAC WC(A)

Client(sending host) can be programmed in four ways:
1. always retransmit last TPDU
2. never retransmit last TPDU
3. retransmit only in S0
4. retransmit only in S1

## 5.3 User Datagram Protocol (UDP)

User Datagram Protocol (UDP) is a Transport Layer protocol. UDP is a part of Internet Protocol suite, referred as UDP/IP suite. Unlike TCP, it is unreliable and connectionless protocol. So, there is no need to establish connection prior to data transfer.

Though Transmission Control Protocol (TCP) is the dominant transport layer protocol used with most of Internet services; provides assured delivery, reliability and much more but all these services cost us with additional overhead and latency. Here, UDP comes into picture. For the realtime services like computer gaming, voice or video communication, live conferences; we need UDP. Since high performance is needed, UDP permits packets to be dropped instead of processing delayed packets. There is no error checking in UDP, so it also save bandwidth.

User Datagram Protocol (UDP) is more efficient in terms of both latency and bandwidth.

### Uses of UDP/Features
• UDP is used when acknowledgement of data does not hold any significance.
• UDP is good protocol for data flowing in one direction.
• UDP is simple and suitable for query based communications.
• UDP is not connection oriented.
• UDP does not provide congestion control mechanism.
• UDP does not guarantee ordered delivery of data.
• UDP is stateless.
• UDP is suitable protocol for streaming applications such as VoIP, multimedia streaming.

### UDP Header

![UDP Header Format](udp_header.png)

**Note**: Unlike TCP, Checksum calculation is not mandatory in UDP. No Error control or flow control is provided by UDP. Hence UDP depends on IP and ICMP for error reporting.

### UDP Application
Here are few applications where UDP is used to transmit data:
• Domain Name Services
• Simple Network Management Protocol
• Trivial File Transfer Protocol
• Routing Information Protocol
• Kerberos

### UDP Operation

UDP is designed to do as little as possible, and little is exactly what it does.

#### What UDP Does
UDP's only real task is to take data from higher-layer protocols and place it in UDP messages, which are then passed down to the Internet Protocol for transmission. The basic steps for transmission using UDP are:

1. **Higher-Layer Data Transfer**: An application sends a message to the UDP software.
2. **UDP Message Encapsulation**: The higher-layer message is encapsulated into the Data field of a UDP message. The headers of the UDP message are filled in, including the Source Port of the application that sent the data to UDP, and the Destination Port of the intended recipient. The checksum value may also be calculated.
3. **Transfer Message To IP**: The UDP message is passed to IP for transmission.

And that's about it. Of course, on reception at the destination device this short procedure is reversed.

#### What UDP Does Not
In fact, UDP is so simple, that its operation is very often described in terms of what it does not do, instead of what it does. As a transport protocol, some of the most important things UDP does not do include the following:
- UDP does not establish connections before sending data. It just packages it and… off it goes.
- UDP does not provide acknowledgments to show that data was received.
- UDP does not provide any guarantees that its messages will arrive.
- UDP does not detect lost messages and retransmit them.
- UDP does not ensure that data is received in the same order that they were sent.
- UDP does not provide any mechanism to manage the flow of data between devices, or handle congestion.

### Remote Procedure Call (RPC)

A very different communications model, usually but not always implemented over UDP, is that of Remote Procedure Call, or RPC. The name comes from the idea that a procedure call is being made over the network; host A packages up a request, with parameters, and sends it to host B, which returns a reply. The term request/reply protocol is also used for this. The side making the request is known as the client, and the other side the server. Common example is that of DNS, Other examples include password verification, system information retrieval, database queries and file I/O (below).

#### Network File Sharing
In terms of total packet volume, the application making the greatest use of early RPC was Sun's Network File Sharing, or NFS; this allowed for a filesystem on the server to be made available to clients.
- For read() operations
- For write() operations

#### Sun RPC
The original simple model above is quite serviceable. However, in the RPC implementation developed by Sun Microsystems and documented in RFC 1831 (and officially known as Open Network Computing, or ONC, RPC), the final acknowledgment was omitted.

#### Serialization
In some RPC systems, even those with explicit ACKs, requests are executed serially by the server.

#### Refinements
One basic network-level improvement to RPC concerns the avoidance of IP-level fragmentation. While fragmentation is not a major performance problem on a single LAN, it may have difficulties over longer distances. One possible refinement is an RPC-level large-message protocol, that fragments at the RPC layer and which supports a mechanism for retransmission, if necessary, only of those fragments that are actually lost.

## 5.4 Principles of Reliable Data Transfer

### Building a Reliable Data Transfer Protocol

The sending side of the data transfer protocol will be invoked from above by a call to `rdt_send()`. On the receiving side, `rdt_rcv()` will be called when a packet arrives from the receiving side of the channel. When the rdt protocol wants to deliver data to the upper layer, it will do so by calling `deliver_data()`. Both the send and receive sides of rdt send packets to the other side by a call to `udt_send()`.

#### Reliable Data Transfer over a Perfectly Reliable Channel: rdt1.0
We first consider the simplest case in which the underlying channel is completely reliable. The protocol itself, which we will call rdt1.0, is trivial.

![rdt1.0 FSM](rdt1_0.png)
*Figure: rdt1.0 - a protocol for a completely reliable channel*

The sending side of rdt simply accepts data from the upper-layer via the `rdt_send(data)` event, puts the data into a packet (via the action `make_pkt(packet,data)`) and sends the packet into the channel. On the receiving side, rdt receives a packet from the underlying channel via the `rdt_rcv(packet)` event, removes the data from the packet (via the action `extract(packet,data)`) and passes the data up to the upper-layer.

#### Reliable Data Transfer over a Channel with Bit Errors: rdt2.0
A more realistic model of the underlying channel is one in which bits in a packet may be corrupted. Such bit errors typically occur in the physical components of a network as a packet is transmitted, propagates, or is buffered. We'll continue to assume for the moment that all transmitted packets are received (although their bits may be corrupted) in the order in which they were sent.

Fundamentally, two additional protocol capabilities are required in ARQ protocols to handle the presence of bit errors:
• Error detection
• Receiver feedback

![rdt2.0 FSM](rdt2_0.png)
*Figure: rdt2.0 - a protocol for a channel with bit-errors*

Consider three possibilities for handling corrupted ACKs or NAKs:
• A first possibility: The speaker would probably ask "What did you say?" (thus introducing a new type of sender-to-receiver packet to our protocol).
• A second alternative: Add enough checksum bits to allow the sender to not only detect, but recover from, bit errors.
• A third approach: For the sender to simply resend the current data packet when it receives a garbled ACK or NAK packet.

#### rdt2.1: sender, handles garbled ACK/NAKs

![rdt2.1 sender](rdt2_1_sender.png)
*Figure: rdt2.1 sender*

![rdt2.1 receiver](rdt2_1_receiver.png)
*Figure: rdt2.1 receiver*

#### rdt2.2: NAK-free protocol

![rdt2.2 sender](rdt2_2_sender.png)
*Figure: rdt2.2 sender*

![rdt2.2 receiver](rdt2_2_receiver.png)
*Figure: rdt2.2 receiver*

#### Reliable Data Transfer over a Lossy Channel with Bit Errors: rdt3.0
Suppose now that in addition to corrupting bits, the underlying channel can lose packets as well, a not uncommon event in today's computer networks.

Two additional concerns must now be addressed by the protocol:
- How to detect packet loss
- What to do when this occurs

The use of checksumming, sequence numbers, ACK packets, and retransmissions - the techniques already developed in rdt 2.2 - will allow us to answer the latter concern. Handling the first concern will require adding a new protocol mechanism.

![rdt3.0 sender](rdt3_0_sender.png)
*Figure: rdt3.0 sender FSM*

![rdt3.0 operation](rdt3_0_operation.png)
*Figure: Operation of rdt3.0, the alternating bit protocol*

Because packet sequence numbers alternate between 0 and 1, protocol rdt3.0 is sometimes known as the alternating bit protocol.

### Pipelined Reliable Data Transfer Protocols

Protocol pipelining is a technique in which multiple requests are written out to a single socket without waiting for the corresponding responses (acknowledged). Pipelining can be used in various application layer network protocols, like HTTP/1.1, SMTP and FTP.

• Range of sequence numbers must be increased.
• Data or Packet should be buffered at sender and/or receiver.

![Stop-and-wait vs Pipelined](pipelined.png)
*Figure: Stop-and-wait versus pipelined protocols*

The solution to this particular performance problem is a simple one: rather than operate in a stop-and-wait manner, the sender is allowed to send multiple packets without waiting for acknowledgements. Since the many in-transit sender-to-receiver packets can be visualized as filling a pipeline, this technique is known as pipelining.

Two generic forms of pipelined protocols are:
1. Go-Back-N
2. Selective repeat

### Comparison: Go-Back-N vs Selective Repeat

| Sr. No. | Key | Go-Back-N | Selective Repeat |
|---------|-----|-----------|------------------|
| 1 | Definition | In Go-Back-N if a sent frame is found suspected or damaged then all the frames are retransmitted till the last packet. | In Selective Repeat, only the suspected or damaged frames are retransmitted. |
| 2 | Sender Window Size | Sender Window is of size N. | Sender Window size is same as N. |
| 3 | Receiver Window Size | Receiver Window Size is 1. | Receiver Window Size is N. |
| 4 | Complexity | Go-Back-N is easier to implement. | In Selective Repeat, receiver window needs to sort the frames. |
| 5 | Efficiency | Efficiency of Go-Back-N = N / (1 + 2a). | Efficiency of Selective Repeat = N / (1 + 2a). |
| 6 | Acknowledgement | Acknowledgement type is cumulative. | Acknowledgement type is individual. |

### Go-Back-N (GBN)
• Retransmits all the frames that sent after the frame which suspects to be damaged or lost.
• If error rate is high, it wastes a lot of bandwidth.
• Less complicated.
• Window size N-1
• Sorting is neither required at sender side nor at receiver side.
• Receiver does not store the frames received after the damaged frame until the damaged frame is retransmitted.
• No searching of frame is required neither on sender side nor on receiver
• NAK number refer to the next expected frame number.
• It more often used.

![GBN sender view](gbn_sender_view.png)
*Figure: Sender's view of sequence numbers in Go-Back-N*

![GBN sender FSM](gbn_sender_fsm.png)
*Figure: Extended FSM description of GBN sender*

![GBN receiver FSM](gbn_receiver_fsm.png)
*Figure: Extended FSM description of GBN receiver*

![GBN operation](gbn_operation.png)
*Figure: Go-Back-N in operation*

### Selective Repeat (SR)
• Retransmits only those frames that are suspected to lost or damaged.
• Comparatively less bandwidth is wasted in retransmitting.
• More complex as it requires to apply extra logic and sorting and storage, at sender and receiver.
• Window Size is <= (N+1)/2
• Receiver must be able to sort as it has to maintain the sequence of the frames.
• Receiver stores the frames received after the damaged frame in the buffer until the damaged frame is replaced.
• The sender must be able to search and select only the requested frame.
• NAK number refer to the frame lost.
• It is less in practice because of its complexity.

![SR views](sr_views.png)
*Figure: SR sender and receiver views of sequence number space*

![SR operation](sr_operation.png)
*Figure: SR Operation*

![SR dilemma](sr_dilemma.png)
*Figure: SR receiver dilemma with too large windows: a new packet or a retransmission*

## 5.5 Transmission Control Protocol (TCP)

The Transmission Control Protocol (TCP) is one of the most important protocols of Internet Protocols suite. It is most widely used protocol for data transmission in communication network such as internet.

It works together with IP and provides a reliable transport service between processes using the network layer service provided by the IP protocol.

### TCP Services to the application layer

1. **Process-to-Process Communication** – TCP provides process to process communication, i.e, the transfer of data takes place between individual processes executing on end systems. This is done using port numbers or port addresses. Port numbers are 16 bit long that help identify which process is sending or receiving data on a host.

2. **Stream oriented** – This means that the data is sent and received as a stream of bytes (unlike UDP or IP that divides the bits into datagrams or packets).

3. **Full duplex service** – This means that the communication can take place in both directions at the same time.

4. **Connection oriented service** – Unlike UDP, TCP provides connection oriented service. It defines 3 different phases:
   • Connection establishment
   • Data transfer
   • Connection termination

5. **Reliability** – TCP is reliable as it uses checksum for error detection, attempts to recover lost or corrupted packets by re-transmission, acknowledgement policy and timers. It uses features like byte number and sequence number and acknowledgement number so as to ensure reliability. Also, it uses congestion control mechanisms.

6. **Multiplexing** – TCP does multiplexing and de-multiplexing at the sender and receiver ends respectively as a number of logical connections can be established between port numbers over a physical connection.

### TCP Features
• TCP is reliable protocol. That is, the receiver always sends either positive or negative acknowledgement about the data packet to the sender, so that the sender always has bright clue about whether the data packet is reached the destination or it needs to resend it.
• TCP ensures that the data reaches intended destination in the same order it was sent.
• TCP is connection oriented. TCP requires that connection between two remote points be established before sending actual data.
• TCP provides error-checking and recovery mechanism.
• TCP provides end-to-end communication.
• TCP provides flow control and quality of service.
• TCP operates in Client/Server point-to-point mode.
• TCP provides full duplex server, i.e. it can perform roles of both receiver and sender.

### TCP Segment Header

![TCP Header Format](tcp_header.png)

The length of TCP header is minimum 20 bytes long and maximum 60 bytes.

• **Source Port (16-bits)** - It identifies source port of the application process on the sending device.
• **Destination Port (16-bits)** - It identifies destination port of the application process on the receiving device.
• **Sequence Number (32-bits)** - Sequence number of data bytes of a segment in a session.
• **Acknowledgement Number (32-bits)** - When ACK flag is set, this number contains the next sequence number of the data byte expected and works as acknowledgement of the previous data received.
• **Data Offset (4-bits)** - This field implies both, the size of TCP header (32-bit words) and the offset of data in current packet in the whole TCP segment.
• **Reserved (3-bits)** - Reserved for future use and all are set zero by default.
• **Flags (1-bit each)**
  - **NS** - Nonce Sum bit is used by Explicit Congestion Notification signaling process.
  - **CWR** - When a host receives packet with ECE bit set, it sets Congestion Windows Reduced to acknowledge that ECE received.
  - **ECE** - It has two meanings:
    - If SYN bit is clear to 0, then ECE means that the IP packet has its CE (congestion experience) bit set.
    - If SYN bit is set to 1, ECE means that the device is ECT capable.
  - **URG** - It indicates that Urgent Pointer field has significant data and should be processed.
  - **ACK** - It indicates that Acknowledgement field has significance. If ACK is cleared to 0, it indicates that packet does not contain any acknowledgement.
  - **PSH** - When set, it is a request to the receiving station to PUSH data (as soon as it comes) to the receiving application without buffering it.
  - **RST** - Reset flag has the following features:
    - It is used to refuse an incoming connection.
    - It is used to reject a segment.
    - It is used to restart a connection.
  - **SYN** - This flag is used to set up a connection between hosts.
  - **FIN** - This flag is used to release a connection and no more data is exchanged thereafter. Because packets with SYN and FIN flags have sequence numbers, they are processed in correct order.
• **Windows Size** - This field is used for flow control between two stations and indicates the amount of buffer (in bytes) the receiver has allocated for a segment, i.e. how much data is the receiver expecting.
• **Checksum** - This field contains the checksum of Header, Data and Pseudo Headers.
• **Urgent Pointer** - It points to the urgent data byte if URG flag is set to 1.
• **Options** - It facilitates additional options which are not covered by the regular header. Option field is always described in 32-bit words. If this field contains data less than 32-bit, padding is used to cover the remaining bits to reach 32-bit boundary.

## 5.6 Principles of Congestion Control

In this section, we consider the problem of congestion control in a general context, seeking to understand why congestion is a "bad thing," how network congestion is manifested in the performance received by upper-layer applications, and various approaches that can be taken to avoid, or react to, network congestion.

### 5.6.1 The Causes and the "Costs" of Congestion

In each case, we'll look at why congestion occurs in the first place, and the "cost" of congestion.

#### Scenario 1: Two senders, a router with infinite buffers
Considering perhaps the simplest congestion scenario possible: Two hosts (A and B) each have a connection that share a single hop between source and destination.

![Congestion Scenario 1](congestion_scenario1.png)
*Figure 5.6-1: Congestion scenario 1: two connections sharing a single hop with infinite buffers*

#### Scenario 2: Two senders, a router with finite buffers
Let us now slightly modify scenario 1 in the following two ways:
• First, the amount of router buffering is assumed to be finite.
• Second, we assume that each connection is reliable.

If a packet containing a transport-level segment is dropped at the router, it will eventually be retransmitted by the sender.

![Congestion Scenario 2](congestion_scenario2.png)
*Figure 5.6-3: Scenario 2: two hosts (with retransmissions) and a router with finite buffers*

#### Scenario 3: Four senders, routers with finite buffers, and multihop paths
In our final congestion scenario, four hosts transmit packets, each over overlapping two-hop paths. We again assume that each host uses a timeout/retransmission mechanism to implement a reliable data transfer service, that all hosts have the same value of λin, and that all router links have capacity C bytes/sec.

![Congestion Scenario 3](congestion_scenario3.png)
*Figure 5.6-5: Four senders, routers with finite buffers, and multihop paths*

Let us consider the connection from Host A to Host C, passing through Routers R1 and R2. The A-C connection shares router R1 with the D-B connection and shares router R2 with the B-D connection.

### 5.6.2 Approaches Toward Congestion Control

At the broadest level, approaches toward congestion control can be distinguished based on whether or not the network layer provides any explicit assistance to the transport layer for congestion control purposes:

• **End-end congestion control**: In an end-end approach towards congestion control, the network layer provides no explicit support to the transport layer for congestion control purposes. Even the presence of congestion in the network must be inferred by the end systems based only on observed network behavior (e.g., packet loss and delay).

• **Network-assisted congestion control**: With network-assisted congestion control, network-layer components (i.e., routers) provide explicit feedback to the sender regarding the congestion state in the network. This feedback may be as simple as a single bit indicating congestion at a link.

For network-assisted congestion control, congestion information is typically fed back from the network to the sender in one of two ways:
- Direct feedback may be sent from a network router to the sender. This form of notification typically takes the form of a choke packet (essentially saying, "I'm congested!").

![Feedback Pathways](feedback_pathways.png)
*Figure 5.6-7: Two feedback pathways for network-indicated congestion information*

#### ATM ABR Congestion Control
• Adopt ATM terminology (e.g., using the term "switch" rather than "router," and the term "call" rather than "packet").
• With ATM ABR service, data cells are transmitted from a source to a destination through a series of intermediate switches.
• Interspersed with the data cells are so-called RM (Resource Management) cells;
• RM cells can be used to convey congestion-related information among the hosts and switches.
• RM cells can thus be used to provide both direct network feedback and network-feedback-via-the-receiver.

![ATM ABR Framework](atm_abr_framework.png)
*Figure 5.6-8: Congestion control framework for ATM ABR service*

• ATM ABR congestion control is a rate-based approach. That is, the sender explicitly computes a maximum rate at which it can send and regulates itself accordingly.
• ABR provides three mechanisms for signaling congestion-related information from the switches to the receiver:
  - EFCI (Explicit Forward Congestion Indication) bit.
  - CI and NI (No Increase) bits.
  - Explicit Rate (ER) setting.

An ATM ABR source adjusts the rate at which it can send cells as a function of the CI, NI and ER values in a returned RM cell.

### Congestion Control

**What is congestion?**
A state occurring in network layer when the message traffic is so heavy that it slows down network response time.

**Effects of Congestion**
• As delay increases, performance decreases.
• If delay increases, retransmission occurs, making situation worse.

### Congestion control algorithms

#### Leaky Bucket Algorithm
• Suppose we have a bucket in which we are pouring water in a random order but we have to get water in a fixed rate, for this we will make a hole at the bottom of the bucket. It will ensure that water coming out is in a some fixed rate, and also if bucket will full we will stop pouring in it.
• The input rate can vary, but the output rate remains constant. Similarly, in networking, a technique called leaky bucket can smooth out bursty traffic. Bursty chunks are stored in the bucket and sent out at an average rate.

In the figure, we assume that the network has committed a bandwidth of 3 Mbps for a host. The use of the leaky bucket shapes the input traffic to make it conform to this commitment. In Figure the host sends a burst of data at a rate of 12 Mbps for 2 s, for a total of 24 Mbits of data. The host is silent for 5 s and then sends data at a rate of 2 Mbps for 3 s, for a total of 6 Mbits of data. In all, the host has sent 30 Mbits of data in 10 s. The leaky bucket smooths the traffic by sending out data at a rate of 3 Mbps during the same 10 s.

**Algorithm for variable-length packets:**
1. Initialize a counter to n at the tick of the clock.
2. If n is greater than the size of the packet, send the packet and decrement the counter by the packet size. Repeat this step until n is smaller than the packet size.
3. Reset the counter and go to step 1.

**Example**: Let n=1000
```
Packet Queue:
Since n > front of Queue i.e. n > 200
Therefore, n = 1000 - 200 = 800
Packet size of 200 is sent to the network.

Now Again n > front of the queue i.e. n > 400
Therefore, n = 800 - 400 = 400
Packet size of 400 is sent to the network.

Since n < front of queue
Therefore, the procedure is stop.
Initialize n = 1000 on another tick of clock.
```

This procedure is repeated until all the packets are sent to the network.

#### Token Bucket Algorithm
The leaky bucket algorithm enforces output pattern at the average rate, no matter how bursty the traffic is. So in order to deal with the bursty traffic we need a flexible algorithm so that the data is not lost. One such algorithm is token bucket algorithm.

**Steps of this algorithm:**
1. In regular intervals tokens are thrown into the bucket.
2. The bucket has a maximum capacity.
3. If there is a ready packet, a token is removed from the bucket, and the packet is sent.
4. If there is no token in the bucket, the packet cannot be sent.

![Token Bucket Algorithm](token_bucket.png)

In figure (A) we see a bucket holding three tokens, with five packets waiting to be transmitted. For a packet to be transmitted, it must capture and destroy one token. In figure (B) We see that three of the five packets have gotten through, but the other two are stuck waiting for more tokens to be generated.

**Example:**
For a host machine that uses the token bucket algorithm for congestion control, the token bucket has a capacity of 1 megabyte and the maximum output rate is 20 megabytes per second. Tokens arrive at a rate to sustain output at a rate of 10 megabytes per second. The token bucket is currently full and the machine needs to send 12 megabytes of data. The minimum time required to transmit the data is _________________ seconds.

```
According to the token bucket algorithm, the minimum time required
to send 1 MB of data or the maximum rate of data transmission is
given by:
S = C / (M - P)
Where,
M = Maximum burst rate,
P = Rate of arrival of a token,
C = capacity of the bucket
M = 20 MB
P = 10 MB
C = 1 MB
S = 1 / (20 - 10) = 0.1 sec
```

Since, the bucket is initially full, it already has 1 MB to transmit so it will be transmitted instantly. So, we are left with only (12 – 1), i.e. 11 MB of data to be transmitted.

Time required to send the 11 MB will be 11 * 0.1 = 1.1 sec

#### Comparison: Leaky Bucket vs Token Bucket

| LEAKY BUCKET | TOKEN BUCKET |
|--------------|--------------|
| When the host has to send a packet, packet is thrown in bucket. | In this leaky bucket holds tokens generated at regular intervals of time. |
| Bucket leaks at constant rate | Bucket has maximum capacity. |
| Bursty traffic is converted into uniform traffic by leaky bucket. | If there is a ready packet, a token is removed from bucket and packet is sent. |
| In practice bucket is a finite queue outputs at finite rate | If there is no token in bucket, packet cannot be sent. |

**Some advantage of token Bucket over leaky bucket:**
• If bucket is full in token Bucket, tokens are discarded not packets. While in leaky bucket, packets are discarded.
• Token Bucket can send Large bursts at a faster rate while leaky bucket always sends packets at constant rate.

### Subnetting Example Solutions

#### Example 1: 192.168.10.121/27

**Solution:**
- On bit = 27
- Off bit (n) = 32-27 = 5
a. Subnet mask = 255.255.255.11100000 = 255.255.255.224
b. No of IP = 2^n = 2^5 = 32
c. Valid IP address = 2^n - 2 = 2^5 - 2 = 32-2 = 30

The IP starts with:
```
192.168.10.0  - 192.168.10.31
192.168.10.32 - 192.168.10.63
192.168.10.64 - 192.168.10.95
192.168.10.96 - 192.168.10.127
```

d. Network IP = 192.168.10.96
e. Broadcast IP = 192.168.10.127
f. First IP = 192.168.10.97
g. Last IP = 192.168.10.126
h. Position of given host = 25 [count from 96-121]

#### Example 2: 192.168.5.83/28

**Solution:**
- On bit = 28
- Off bit (n) = 32-28 = 4
a. Subnet mask = 255.255.255.11110000 = 255.255.255.240
b. No of IP = 2^n = 2^4 = 16
c. Valid IP address = 2^n - 2 = 2^4 - 2 = 16-2 = 14

The IP starts with:
```
192.168.5.0   - 192.168.5.15
192.168.5.16  - 192.168.5.31
192.168.5.32  - 192.168.5.47
192.168.5.48  - 192.168.5.63
192.168.5.64  - 192.168.5.79
192.168.5.80  - 192.168.5.95
```

d. Network IP = 192.168.5.80
e. Broadcast IP = 192.168.5.95
f. First IP = 192.168.5.81
g. Last IP = 192.168.5.94
h. Position of given host = 4 [count from 80-83]

## Summary

The Transport Layer (Layer 4) is responsible for:
- End-to-end delivery of messages
- Process-to-process communication using port numbers
- Segmentation and reassembly of messages
- Flow control and error control
- Congestion control mechanisms

Key protocols at this layer include:
- **TCP**: Reliable, connection-oriented protocol with congestion control
- **UDP**: Unreliable, connectionless protocol for fast delivery

Important concepts covered:
1. Functions of Transport Layer
2. Transport Protocol Elements (addressing, connection management)
3. UDP characteristics and applications
4. Reliable Data Transfer protocols (rdt1.0 through rdt3.0)
5. Pipelined protocols (Go-Back-N and Selective Repeat)
6. TCP features and header structure
7. Congestion control principles and algorithms
8. Flow control and buffering mechanisms

This chapter provides the foundation for understanding how applications communicate reliably across networks, managing issues like packet loss, ordering, flow control, and congestion.