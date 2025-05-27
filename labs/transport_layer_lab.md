# Transport Layer (Layer 4) Demonstration Lab

## Lab Topology
```
    PC0 ---- Switch ---- Router ---- Switch ---- Server0
                                              |
                                           Server1
```

## Required Devices
- 1 PC
- 2 Servers
- 2 Switches (2960)
- 1 Router (2911)

## Lab Steps

### Part 1: TCP Three-Way Handshake

1. **Build the topology and configure**
   
   **PC0:**
   - IP: 192.168.1.10/24
   - Gateway: 192.168.1.1
   
   **Server0 (Web Server):**
   - IP: 192.168.2.10/24
   - Gateway: 192.168.2.1
   - Enable HTTP service
   
   **Server1 (DNS Server):**
   - IP: 192.168.2.20/24
   - Gateway: 192.168.2.1
   - Enable DNS service
   
   **Router:**
   ```
   Router(config)# interface GigabitEthernet 0/0
   Router(config-if)# ip address 192.168.1.1 255.255.255.0
   Router(config-if)# no shutdown
   
   Router(config)# interface GigabitEthernet 0/1
   Router(config-if)# ip address 192.168.2.1 255.255.255.0
   Router(config-if)# no shutdown
   ```

2. **Demonstrate TCP handshake**
   - Open Simulation Mode
   - Filter to show only TCP packets
   - From PC0, open Web Browser
   - Enter Server0's IP: 192.168.2.10
   - Step through simulation to observe:
     - SYN packet (PC to Server)
     - SYN-ACK packet (Server to PC)
     - ACK packet (PC to Server)
     - HTTP data transfer

3. **Examine TCP headers**
   - Click on packets in simulation
   - Observe:
     - Source and destination ports
     - Sequence numbers
     - Acknowledgment numbers
     - TCP flags

### Part 2: TCP vs UDP Comparison

1. **Configure DNS on Server1**
   - Add DNS record: example.com -> 192.168.2.10
   - Configure PC0 to use DNS server: 192.168.2.20

2. **Demonstrate UDP with DNS**
   - Clear simulation
   - Filter for DNS packets
   - From PC0, ping example.com
   - Observe:
     - Single DNS query (UDP)
     - Single DNS response (UDP)
     - No handshake process

3. **Compare protocols**
   - Create a comparison showing:
     - TCP: Connection-oriented, reliable
     - UDP: Connectionless, best-effort

### Part 3: Port Numbers and Services

1. **Set up multiple services on Server0**
   - Enable HTTP (port 80)
   - Enable HTTPS (port 443)
   - Enable FTP (port 21)

2. **Demonstrate different port usage**
   - Use Simulation Mode
   - Access different services:
     - HTTP: http://192.168.2.10
     - HTTPS: https://192.168.2.10
     - FTP: ftp://192.168.2.10
   - Observe different destination ports

3. **Show transport layer multiplexing**
   - Open multiple browser tabs
   - Access same server simultaneously
   - Note different source ports for each connection

### Part 4: Access Control Lists (ACLs) - Layer 4 Filtering

1. **Create ACL to block specific ports**
   ```
   Router(config)# access-list 101 deny tcp any any eq 80
   Router(config)# access-list 101 permit ip any any
   
   Router(config)# interface GigabitEthernet 0/1
   Router(config-if)# ip access-group 101 out
   ```

2. **Test ACL**
   - Try to access HTTP (should fail)
   - Try to access HTTPS (should work)
   - Try to ping (should work)

3. **Modify ACL for specific hosts**
   ```
   Router(config)# no access-list 101
   Router(config)# access-list 101 deny tcp host 192.168.1.10 any eq 21
   Router(config)# access-list 101 permit ip any any
   ```

### Part 5: Connection States and Sessions

1. **Demonstrate connection states**
   - Establish multiple TCP connections
   - On router, show connections:
   ```
   Router# show ip nat translations (if NAT is configured)
   ```

2. **Simulate connection termination**
   - Use Simulation Mode
   - Close web browser
   - Observe TCP FIN packets
   - See four-way termination process

### Part 6: Quality of Service (QoS)

1. **Configure basic QoS**
   ```
   Router(config)# class-map match-any HTTP-TRAFFIC
   Router(config-cmap)# match protocol http
   
   Router(config)# policy-map QOS-POLICY
   Router(config-pmap)# class HTTP-TRAFFIC
   Router(config-pmap-c)# bandwidth 512
   ```

2. **Apply QoS policy**
   ```
   Router(config)# interface GigabitEthernet 0/1
   Router(config-if)# service-policy output QOS-POLICY
   ```

## Key Observations
- TCP provides reliable, connection-oriented communication
- UDP provides fast, connectionless communication
- Port numbers identify specific services/applications
- TCP uses three-way handshake for connection establishment
- Transport layer provides end-to-end communication
- ACLs can filter based on Layer 4 information
- QoS can prioritize traffic based on transport layer protocols
