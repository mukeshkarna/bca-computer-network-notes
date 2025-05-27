# Chapter 7: Network Security - Comprehensive Notes

## 7.1 A Model for Network Security

Network security is the practice of securing a computer network from intruders, whether targeted attackers or opportunistic malware. It involves policies, procedures, and technologies designed to protect the confidentiality, integrity, and availability of network resources.

### Components of Network Security Model

1. **Assets**: Resources that need protection
   - Data (databases, files, intellectual property)
   - Hardware (servers, routers, switches)
   - Software (applications, operating systems)
   - Network services (email, web services)

2. **Threats**: Potential dangers to assets
   - **Passive Threats**: Eavesdropping, traffic analysis
   - **Active Threats**: Masquerading, replay attacks, modification, denial of service

3. **Vulnerabilities**: Weaknesses that can be exploited
   - Software bugs
   - Misconfiguration
   - Weak passwords
   - Unpatched systems

4. **Attacks**: Exploitation of vulnerabilities
   - **Interruption**: Availability attack (DoS)
   - **Interception**: Confidentiality attack (eavesdropping)
   - **Modification**: Integrity attack (data tampering)
   - **Fabrication**: Authenticity attack (impersonation)

### Security Services (CIA Triad + More)

1. **Confidentiality**: Ensuring information is accessible only to authorized users
   - *Real-world example*: Bank account details should only be visible to account holder and authorized bank staff

2. **Integrity**: Maintaining accuracy and completeness of data
   - *Real-world example*: Medical records must not be altered without proper authorization

3. **Availability**: Ensuring resources are accessible when needed
   - *Real-world example*: ATM networks must be available 24/7 for customer transactions

4. **Authentication**: Verifying identity of users/systems
   - *Real-world example*: Two-factor authentication for online banking

5. **Authorization**: Controlling access to resources
   - *Real-world example*: Role-based access in corporate systems

6. **Non-repudiation**: Preventing denial of actions
   - *Real-world example*: Digital signatures on contracts

### Network Security Architecture

```
[Sender] → [Security Transform] → [Network] → [Inverse Transform] → [Receiver]
                ↑                               ↑
         [Secret Information]           [Secret Information]
```

The model includes:
- **Trusted Third Party**: Certificate authorities, key distribution centers
- **Security Algorithms**: Encryption, digital signatures, hash functions
- **Secret Information**: Keys, passwords, authentication data

### Common Network Attacks

1. **Denial of Service (DoS)**
   - *Example*: 2016 Dyn DNS attack that affected Twitter, Netflix, Reddit
   - Overwhelms target with traffic making it unavailable

2. **Man-in-the-Middle (MITM)**
   - *Example*: Fake Wi-Fi hotspots in airports intercepting user data
   - Attacker intercepts communication between two parties

3. **Phishing**
   - *Example*: Fake PayPal emails requesting login credentials
   - Social engineering to steal sensitive information

4. **SQL Injection**
   - *Example*: 2017 Equifax breach affecting 147 million people
   - Exploiting web application vulnerabilities to access databases

## 7.2 Principles of Cryptography: Symmetric Key and Public Key

Cryptography is the science of secret writing, providing security through mathematical algorithms and keys.

### Symmetric Key Cryptography

In symmetric cryptography, the same key is used for both encryption and decryption.

#### Characteristics:
- **Speed**: Fast encryption/decryption
- **Key Management**: Challenging with multiple parties
- **Key Distribution**: Secure key sharing is difficult

#### Types of Symmetric Algorithms:

1. **Stream Ciphers**
   - Encrypt data bit by bit or byte by byte
   - *Example*: RC4 (used in WEP, now deprecated)
   - Use case: Real-time communication

2. **Block Ciphers**
   - Encrypt fixed-size blocks of data
   - *Example*: AES (Advanced Encryption Standard)

#### Advanced Encryption Standard (AES)
- **Key Sizes**: 128, 192, or 256 bits
- **Block Size**: 128 bits
- **Real-world Usage**: 
  - Wi-Fi WPA2/WPA3 encryption
  - HTTPS connections
  - File encryption (BitLocker, FileVault)
  - VPN tunnels

#### AES Encryption Process:
1. **SubBytes**: Byte substitution using S-box
2. **ShiftRows**: Permutation of bytes
3. **MixColumns**: Linear transformation
4. **AddRoundKey**: XOR with round key

*Real-world Example*: When you connect to a secure Wi-Fi network, AES encrypts all data between your device and the router.

#### Data Encryption Standard (DES) - Legacy
- **Key Size**: 56 bits (now considered insecure)
- **Block Size**: 64 bits
- **Status**: Deprecated due to small key size
- **Triple DES (3DES)**: Uses DES three times with different keys

### Public Key Cryptography (Asymmetric)

Uses a pair of keys: public key (shareable) and private key (secret).

#### Characteristics:
- **Key Pairs**: Mathematically related but computationally infeasible to derive one from the other
- **Speed**: Slower than symmetric encryption
- **Key Distribution**: Easier - public keys can be shared openly
- **Digital Signatures**: Enables non-repudiation

#### Applications:
1. **Digital Signatures**: Authentication and non-repudiation
2. **Key Exchange**: Secure distribution of symmetric keys
3. **Encryption**: Secure communication without prior key sharing

*Real-world Example*: When you visit an HTTPS website, your browser uses the server's public key to encrypt a symmetric key, which is then used for the actual data encryption (SSL/TLS handshake).

### Hybrid Cryptosystems

Most practical systems combine both approaches:
1. **Public Key Cryptography**: For key exchange and digital signatures
2. **Symmetric Cryptography**: For bulk data encryption

*Example*: SSL/TLS protocol used in HTTPS
- RSA/ECDH for key exchange
- AES for data encryption
- SHA for message authentication

## 7.3 Public Key Algorithm - RSA

RSA (Rivest-Shamir-Adleman) is the most widely used public-key cryptosystem, based on the mathematical difficulty of factoring large prime numbers.

### RSA Algorithm Overview

#### Key Generation:
1. **Select two large prime numbers**: p and q
2. **Compute n = p × q**: n is the modulus
3. **Compute φ(n) = (p-1)(q-1)**: Euler's totient function
4. **Choose e**: 1 < e < φ(n), gcd(e, φ(n)) = 1 (commonly e = 65537)
5. **Compute d**: d ≡ e⁻¹ (mod φ(n))
6. **Public Key**: (n, e)
7. **Private Key**: (n, d)

#### Encryption Process:
- **Ciphertext**: C = M^e mod n (where M is the message)

#### Decryption Process:
- **Message**: M = C^d mod n

### RSA Example (Simplified)

Let's use small numbers for illustration:

1. **Choose primes**: p = 7, q = 11
2. **Compute n**: n = 7 × 11 = 77
3. **Compute φ(n)**: φ(77) = 6 × 10 = 60
4. **Choose e**: e = 13 (gcd(13, 60) = 1)
5. **Compute d**: d = 37 (since 13 × 37 ≡ 1 mod 60)
6. **Keys**: Public (77, 13), Private (77, 37)

**Encryption**: Message M = 2
- C = 2^13 mod 77 = 30

**Decryption**: Ciphertext C = 30
- M = 30^37 mod 77 = 2

### RSA Security

#### Key Size Recommendations:
- **1024 bits**: Deprecated (factored in 2009)
- **2048 bits**: Current minimum standard
- **3072 bits**: Recommended for high security
- **4096 bits**: Maximum practical size

#### Real-world RSA Applications:

1. **SSL/TLS Certificates**
   - *Example*: When you see the lock icon in your browser, RSA may be securing the connection
   - Used for key exchange in HTTPS connections

2. **Email Encryption (PGP/GPG)**
   - *Example*: Journalists use PGP with RSA keys to securely communicate with sources
   - Provides end-to-end encryption for email

3. **Code Signing**
   - *Example*: Microsoft uses RSA to sign Windows updates
   - Ensures software authenticity and integrity

4. **SSH Authentication**
   - *Example*: Developers use RSA key pairs to securely access remote servers
   - Replaces password-based authentication

### RSA Vulnerabilities and Mitigations

1. **Factorization Attacks**
   - *Mitigation*: Use sufficiently large keys (2048+ bits)

2. **Timing Attacks**
   - *Mitigation*: Implement constant-time algorithms

3. **Chosen Ciphertext Attacks**
   - *Mitigation*: Use padding schemes (OAEP)

## 7.4 Digital Signature Algorithm

Digital signatures provide authentication, integrity, and non-repudiation for digital documents and messages.

### Digital Signature Process

#### Signing Process:
1. **Hash the message**: Create a fixed-size digest
2. **Encrypt hash with private key**: This creates the digital signature
3. **Attach signature to message**: Send message + signature

#### Verification Process:
1. **Hash the received message**: Using the same hash function
2. **Decrypt signature with public key**: Retrieve the original hash
3. **Compare hashes**: If they match, signature is valid

### Hash Functions in Digital Signatures

#### Properties of Cryptographic Hash Functions:
1. **Deterministic**: Same input always produces same output
2. **Fixed Output Size**: Regardless of input size
3. **Avalanche Effect**: Small input change causes large output change
4. **One-way**: Computationally infeasible to reverse
5. **Collision Resistant**: Hard to find two inputs with same output

#### Common Hash Functions:

1. **SHA-256 (Secure Hash Algorithm)**
   - **Output Size**: 256 bits
   - **Usage**: Bitcoin blockchain, SSL certificates
   - *Example*: Each Bitcoin block header is hashed with SHA-256

2. **SHA-3**
   - **Output Sizes**: 224, 256, 384, 512 bits
   - **Status**: Latest NIST standard
   - **Usage**: New applications requiring high security

3. **MD5 (Message Digest 5)** - Deprecated
   - **Output Size**: 128 bits
   - **Status**: Cryptographically broken
   - **Historical Use**: File integrity checks

### RSA Digital Signatures

#### RSA Signature Generation:
1. **Hash message**: H = hash(M)
2. **Sign hash**: S = H^d mod n (using private key)
3. **Send**: Message M and signature S

#### RSA Signature Verification:
1. **Hash received message**: H' = hash(M)
2. **Decrypt signature**: H = S^e mod n (using public key)
3. **Verify**: H' = H?

### Digital Signature Algorithm (DSA)

DSA is a U.S. Federal Government standard for digital signatures, based on discrete logarithm problem.

#### DSA Parameters:
- **p**: Large prime (1024-3072 bits)
- **q**: Prime divisor of (p-1), 160-256 bits
- **g**: Generator of order q in Z*p
- **x**: Private key (random integer < q)
- **y**: Public key (y = g^x mod p)

#### DSA Signature Process:
1. **Choose random k**: 0 < k < q
2. **Compute r**: r = (g^k mod p) mod q
3. **Compute s**: s = k⁻¹(H(m) + xr) mod q
4. **Signature**: (r, s)

### Real-world Digital Signature Applications

1. **PDF Document Signing**
   - *Example*: Adobe Acrobat's digital signatures for contracts
   - Provides legal non-repudiation

2. **Software Distribution**
   - *Example*: Apple signs all iOS apps with digital certificates
   - Prevents installation of malicious software

3. **Financial Transactions**
   - *Example*: Banks use digital signatures for electronic fund transfers
   - Ensures transaction authenticity and prevents fraud

4. **Email Security (S/MIME)**
   - *Example*: Corporate email systems use S/MIME for signed emails
   - Provides email authentication and integrity

5. **Blockchain Technology**
   - *Example*: Bitcoin transactions are signed with ECDSA
   - Proves ownership of cryptocurrency

## 7.5 Communication Security: IPSec, VPN, Firewalls, Wireless Security

### IPSec (Internet Protocol Security)

IPSec is a protocol suite for securing IP communications by authenticating and encrypting each IP packet in a communication session.

#### IPSec Components:

1. **Authentication Header (AH)**
   - Provides authentication and integrity
   - No encryption (data visible but tamper-proof)
   - *Use case*: When confidentiality is not required but authenticity is crucial

2. **Encapsulating Security Payload (ESP)**
   - Provides confidentiality, authentication, and integrity
   - Encrypts payload data
   - *Use case*: Full protection for sensitive data

#### IPSec Modes:

1. **Transport Mode**
   - Protects payload of IP packet
   - Original IP header remains
   - *Usage*: End-to-end communication between hosts
   - *Example*: Secure communication between two computers

2. **Tunnel Mode**
   - Protects entire IP packet
   - New IP header added
   - *Usage*: VPN gateways, site-to-site connections
   - *Example*: Branch office connecting to headquarters

#### IPSec Security Associations (SA)

- **Definition**: Relationship between communicating parties that defines security services
- **Parameters**: Security protocols, algorithms, keys, key lifetimes
- **Database**: Security Association Database (SAD) stores active SAs

#### Real-world IPSec Applications:

1. **Corporate VPNs**
   - *Example*: Cisco ASA firewalls use IPSec for remote access VPNs
   - Employees securely access company networks from home

2. **Site-to-Site VPNs**
   - *Example*: Connecting multiple office locations
   - Branch offices connect securely over internet

3. **Cloud Security**
   - *Example*: AWS VPC VPN connections use IPSec
   - Secure connectivity between on-premises and cloud resources

### Virtual Private Networks (VPNs)

VPNs create secure, encrypted tunnels over public networks, providing privacy and security for data transmission.

#### Types of VPNs:

1. **Remote Access VPN**
   - Individual users connect to corporate network
   - *Example*: Employee working from home accesses company servers
   - **Protocols**: L2TP/IPSec, OpenVPN, IKEv2

2. **Site-to-Site VPN**
   - Connects entire networks at different locations
   - *Example*: Headquarters connected to branch offices
   - **Types**: Intranet VPN, Extranet VPN

3. **Client-to-Site VPN**
   - Software-based VPN clients
   - *Example*: NordVPN, ExpressVPN for personal use

#### VPN Protocols:

1. **OpenVPN**
   - **Security**: Uses SSL/TLS protocols
   - **Advantages**: Open source, highly secure, cross-platform
   - **Usage**: Popular for both personal and enterprise use

2. **L2TP/IPSec**
   - **L2TP**: Layer 2 Tunneling Protocol
   - **IPSec**: Provides encryption
   - **Usage**: Built into most operating systems

3. **PPTP (Point-to-Point Tunneling Protocol)**
   - **Status**: Deprecated due to security vulnerabilities
   - **Historical**: Early VPN protocol, now avoided

4. **WireGuard**
   - **Modern**: Newest VPN protocol
   - **Advantages**: Simple, fast, secure
   - **Status**: Gaining adoption in commercial VPN services

#### VPN Security Benefits:

1. **Encryption**: Protects data in transit
2. **Authentication**: Verifies user identity
3. **Access Control**: Limits network access
4. **Traffic Filtering**: Can block malicious content

#### Real-world VPN Use Cases:

1. **Remote Work**
   - *Example*: COVID-19 pandemic drove massive VPN adoption
   - Employees access office resources securely from home

2. **Privacy Protection**
   - *Example*: Journalists in restrictive countries use VPNs
   - Bypass censorship and protect identity

3. **Geo-blocking Circumvention**
   - *Example*: Accessing streaming content from different regions
   - Business travelers accessing home country services

### Firewalls

Firewalls are network security devices that monitor and control incoming and outgoing network traffic based on predetermined security rules.

#### Types of Firewalls:

1. **Packet Filtering Firewalls (Stateless)**
   - **Function**: Examine individual packets
   - **Rules**: Based on IP addresses, ports, protocols
   - **Limitations**: No connection state awareness
   - *Example*: Basic router ACLs (Access Control Lists)

2. **Stateful Inspection Firewalls**
   - **Function**: Track connection states
   - **Advantage**: Understand traffic context
   - **Security**: Better than packet filtering
   - *Example*: Cisco ASA, pfSense

3. **Application Layer Firewalls (Proxy Firewalls)**
   - **Function**: Inspect application-specific data
   - **Deep Inspection**: Understand application protocols
   - **Performance**: Slower but more secure
   - *Example*: Web Application Firewalls (WAF)

4. **Next-Generation Firewalls (NGFW)**
   - **Features**: 
     - Traditional firewall capabilities
     - Intrusion Prevention System (IPS)
     - Application awareness and control
     - SSL/TLS inspection
   - *Example*: Palo Alto Networks, FortiGate

#### Firewall Deployment Models:

1. **Network-based Firewalls**
   - **Location**: Between networks (perimeter)
   - **Protection**: Entire network segments
   - *Example*: Corporate firewall at internet gateway

2. **Host-based Firewalls**
   - **Location**: Individual computers
   - **Protection**: Single host
   - *Example*: Windows Defender Firewall, iptables on Linux

#### Firewall Rules and Policies:

1. **Default Deny**: Block all traffic unless explicitly allowed
2. **Rule Order**: Process rules from top to bottom
3. **Logging**: Record allowed/denied traffic for analysis

#### DMZ (Demilitarized Zone)

- **Purpose**: Separate public services from internal network
- **Components**: Web servers, email servers, DNS servers
- **Security**: Additional layer of protection
- *Example*: Company website hosted in DMZ, accessible from internet but isolated from internal network

#### Real-world Firewall Applications:

1. **Enterprise Perimeter Security**
   - *Example*: Bank's firewall protecting customer data
   - Filters malicious traffic from internet

2. **Web Application Protection**
   - *Example*: Cloudflare WAF protecting e-commerce sites
   - Blocks SQL injection, XSS attacks

3. **Industrial Control Systems**
   - *Example*: Power plant SCADA systems protected by specialized firewalls
   - Critical infrastructure protection

### Wireless Security

Wireless networks face unique security challenges due to the broadcast nature of radio communications.

#### Wireless Security Threats:

1. **Eavesdropping**
   - **Risk**: Radio signals can be intercepted
   - **Impact**: Unauthorized access to data
   - *Example*: Packet sniffing on open Wi-Fi networks

2. **Rogue Access Points**
   - **Risk**: Unauthorized APs in corporate networks
   - **Impact**: Bypass security controls
   - *Example*: Evil twin attacks in coffee shops

3. **Denial of Service**
   - **Risk**: Radio frequency jamming
   - **Impact**: Network unavailability
   - *Example*: Disrupting Wi-Fi at public events

#### Wi-Fi Security Protocols:

1. **WEP (Wired Equivalent Privacy)** - Deprecated
   - **Status**: Broken, easily cracked
   - **Key Size**: 40 or 104 bits
   - **Vulnerability**: Weak key scheduling algorithm
   - *Never use WEP in production environments*

2. **WPA (Wi-Fi Protected Access)**
   - **Improvement**: Over WEP
   - **Key Management**: TKIP (Temporal Key Integrity Protocol)
   - **Status**: Deprecated in favor of WPA2
   - **Vulnerability**: Susceptible to certain attacks

3. **WPA2 (Wi-Fi Protected Access 2)**
   - **Standard**: IEEE 802.11i
   - **Encryption**: AES-CCMP (Counter Mode with CBC-MAC Protocol)
   - **Key Size**: 128-bit AES
   - **Authentication**: Pre-Shared Key (PSK) or Enterprise (802.1X)
   - *Current standard for most networks*

4. **WPA3 (Wi-Fi Protected Access 3)**
   - **Features**:
     - SAE (Simultaneous Authentication of Equals)
     - Forward Secrecy
     - Protection against offline dictionary attacks
     - Enhanced security for open networks
   - **Status**: Latest standard, gradually being adopted
   - *Example*: New smartphones and routers support WPA3

#### Enterprise Wireless Security:

1. **802.1X Authentication**
   - **Components**: Supplicant, Authenticator, Authentication Server
   - **Protocols**: EAP (Extensible Authentication Protocol)
   - **Benefits**: Centralized authentication, per-user keys
   - *Example*: Corporate Wi-Fi requiring domain credentials

2. **RADIUS (Remote Authentication Dial-In User Service)**
   - **Function**: Centralized authentication server
   - **Integration**: With Active Directory, LDAP
   - **Features**: Authentication, Authorization, Accounting (AAA)

#### EAP Types:

1. **EAP-TLS**
   - **Security**: Digital certificates for authentication
   - **Strength**: Mutual authentication
   - **Usage**: High-security environments

2. **PEAP (Protected EAP)**
   - **Function**: Tunnels other EAP methods
   - **Common**: PEAP-MSCHAPv2
   - **Usage**: Corporate networks with username/password

3. **EAP-TTLS**
   - **Function**: Similar to PEAP
   - **Flexibility**: Supports multiple inner authentication methods

#### Wireless Security Best Practices:

1. **Strong Authentication**
   - Use WPA3 or WPA2 with strong passwords
   - Implement 802.1X for enterprise networks

2. **Network Segmentation**
   - Separate guest and corporate networks
   - Use VLANs for isolation

3. **Monitoring and Detection**
   - Wireless Intrusion Detection Systems (WIDS)
   - Regular security audits
   - Rogue AP detection

4. **Physical Security**
   - Secure access point placement
   - Antenna positioning to minimize signal leakage

#### Real-world Wireless Security Examples:

1. **Airport Wi-Fi Security**
   - *Challenge*: Open networks with thousands of users
   - *Solution*: Captive portals, traffic isolation, monitoring
   - *Risk*: Man-in-the-middle attacks on unsecured connections

2. **Healthcare Networks**
   - *Example*: Hospital Wi-Fi for medical devices
   - *Requirements*: HIPAA compliance, device authentication
   - *Solution*: 802.1X with device certificates

3. **Retail Environments**
   - *Example*: Store Wi-Fi for customers and POS systems
   - *Security*: Separate networks, PCI compliance for payment systems
   - *Challenge*: Balancing security with customer convenience

4. **Smart Home Security**
   - *Devices*: IoT devices, smart thermostats, security cameras
   - *Challenges*: Default passwords, firmware updates
   - *Solutions*: Network segmentation, regular updates, strong passwords

### Integrated Security Architecture

Modern networks implement layered security (Defense in Depth):

1. **Perimeter Security**: Firewalls, IPS
2. **Network Security**: VPNs, network segmentation
3. **Endpoint Security**: Antivirus, host-based firewalls
4. **Application Security**: WAF, secure coding practices
5. **Data Security**: Encryption, access controls
6. **Identity Management**: Authentication, authorization

*Real-world Example*: A bank's security architecture includes:
- Firewalls at network perimeter
- VPNs for remote access
- 802.1X for wireless authentication
- Application firewalls for web services
- Database encryption for customer data
- Multi-factor authentication for employees
- Intrusion detection systems for monitoring
- Security incident response procedures

This comprehensive approach ensures multiple layers of protection, so if one layer fails, others continue to provide security.