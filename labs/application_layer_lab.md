# Application Layer (Layers 5-7) Demonstration Lab

## Lab Topology
```
                            [Router]
                           /        \
                 [Switch0]            [Switch1]
                /    |    \              |
             PC0   PC1   DHCP-Server   Web-Server
                                        |
                                    DNS-Server
                                        |
                                    Email-Server
```

## Required Devices
- 2 PCs
- 3-4 Servers
- 2 Switches (2960)
- 1 Router (2911)

## Lab Steps

### Part 1: DHCP Configuration

1. **Initial Setup**
   - Build topology as shown
   - Initially configure static IPs to set up DHCP server
   
   **DHCP Server:**
   - IP: 192.168.1.5/24
   - Gateway: 192.168.1.1

2. **Configure DHCP Server**
   - Click on DHCP Server
   - Go to Services > DHCP
   - Configure DHCP pool:
     - Default Gateway: 192.168.1.1
     - DNS Server: 192.168.2.10
     - Start IP: 192.168.1.100
     - Subnet Mask: 255.255.255.0
     - Maximum Users: 50
   - Turn DHCP service ON

3. **Configure Router for DHCP Relay (if needed)**
   ```
   Router(config)# interface GigabitEthernet 0/0
   Router(config-if)# ip address 192.168.1.1 255.255.255.0
   Router(config-if)# ip helper-address 192.168.1.5
   Router(config-if)# no shutdown
   ```

4. **Test DHCP**
   - On PC0 and PC1:
     - Go to IP Configuration
     - Select DHCP
     - Click "DHCP Request"
   - Verify PCs receive IP addresses automatically

5. **Monitor DHCP Process**
   - Use Simulation Mode
   - Filter for DHCP packets
   - Observe:
     - DHCP Discover (broadcast)
     - DHCP Offer
     - DHCP Request
     - DHCP Acknowledge

### Part 2: DNS Configuration

1. **Configure DNS Server**
   - Server IP: 192.168.2.10/24
   - Gateway: 192.168.2.1
   - Go to Services > DNS
   - Add DNS records:
     - www.example.com -> 192.168.2.20
     - mail.example.com -> 192.168.2.30
   - Turn DNS service ON

2. **Configure Web Server**
   - IP: 192.168.2.20/24
   - Enable HTTP service
   - Modify index.html with custom content

3. **Test DNS Resolution**
   - On PC0, set DNS server to 192.168.2.10
   - Open Web Browser
   - Navigate to www.example.com
   - Use Simulation Mode to see:
     - DNS query
     - DNS response
     - HTTP request to resolved IP

### Part 3: Web Services (HTTP/HTTPS)

1. **Configure HTTP**
   - On Web Server, ensure HTTP is enabled
   - Create custom web page

2. **Configure HTTPS**
   - Enable HTTPS service
   - Note the difference in port numbers (443 vs 80)

3. **Test Web Services**
   - From PC0:
     - Access http://www.example.com
     - Access https://www.example.com
   - Observe in Simulation Mode:
     - Different protocols
     - Different port numbers
     - Encryption indicators for HTTPS

### Part 4: Email Services

1. **Configure Email Server**
   - IP: 192.168.2.30/24
   - Go to Services > EMAIL
   - Configure domains and users:
     - Domain: example.com
     - Users: user1, user2
   - Enable SMTP and POP3

2. **Configure Email Clients**
   - On PC0:
     - Go to Desktop > Email
     - Configure:
       - Your Name: User1
       - Email Address: user1@example.com
       - SMTP Server: mail.example.com
       - POP3 Server: mail.example.com
   - Repeat for PC1 with user2

3. **Test Email Communication**
   - Send email from user1 to user2
   - Use Simulation Mode to observe:
     - DNS lookup for mail server
     - SMTP communication (port 25)
     - POP3 retrieval (port 110)

### Part 5: FTP Services

1. **Configure FTP Server**
   - Enable FTP service on one server
   - Create users and set permissions

2. **Test FTP**
   - From PC Command Prompt:
   ```
   ftp 192.168.2.20
   ```
   - Login with credentials
   - Test file transfer commands

### Part 6: Complete Application Flow

1. **Demonstrate Full Stack Communication**
   - Start with PC having no configuration
   - Use DHCP to get IP
   - Use DNS to resolve names
   - Access web services
   - Send email
   - Show how all layers work together

2. **Trace Complete Communication**
   - Use Simulation Mode
   - Follow a web request from start to finish:
     - DHCP for IP address
     - ARP for local gateway
     - DNS for name resolution
     - TCP handshake
     - HTTP request/response
   - Click on packets to see encapsulation at each layer

### Part 7: Application Layer Protocols Comparison

1. **Create Protocol Comparison Table**
   
   | Protocol | Port | Transport | Purpose |
   |----------|------|-----------|---------|
   | HTTP | 80 | TCP | Web browsing |
   | HTTPS | 443 | TCP | Secure web |
   | DNS | 53 | UDP | Name resolution |
   | DHCP | 67/68 | UDP | IP configuration |
   | SMTP | 25 | TCP | Email sending |
   | POP3 | 110 | TCP | Email retrieval |
   | FTP | 21/20 | TCP | File transfer |

2. **Demonstrate Each Protocol**
   - Use filters in Simulation Mode
   - Show packet structure for each

## Key Observations
- Application layer protocols provide user services
- Each protocol has specific port numbers
- Some use TCP (reliable), others use UDP (fast)
- DNS is critical for name resolution
- DHCP automates network configuration
- Application protocols often work together
- Encapsulation adds headers at each layer
