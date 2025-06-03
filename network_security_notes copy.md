# Unit 7: Network Security - Study Notes

## Basic Concepts of Cryptography

**Cryptography** is a method of using advanced mathematical principles in storing and transmitting data in a particular form so that only those whom it is intended can read and process it.

### Cryptography Terms

- **Encryption**: Process of locking up information using cryptography. Information that has been locked this way is encrypted.
- **Decryption**: Process of unlocking the encrypted information using cryptographic techniques.
- **Key**: A secret like a password used to encrypt and decrypt information. There are different types of keys used in cryptography.
- **Steganography**: Science of hiding information from people who would snoop. The difference between steganography and encryption is that snooper may not be able to tell there's any hidden information in the first place.

---

## 7.1 A Model for Network Security

### General Model Components

1. **Security-related transformation** on the information to be sent
   - Examples: encryption of the message, addition of authentication codes
   
2. **Secret information** shared by the two principals and unknown to the opponent
   - Example: encryption key used with transformation to scramble/unscramble messages

3. **Trusted third party** may be needed to achieve secure transmission
   - May distribute secret information to principals
   - May arbitrate disputes concerning message authenticity

### Four Basic Tasks in Designing Security Service

1. **Design an algorithm** for performing the security-related transformation
2. **Generate the secret information** to be used with the algorithm
3. **Develop methods** for distribution and sharing of secret information
4. **Specify a protocol** to be used by principals using the security algorithm and secret information

### Security Threats

**Information Access Threats**: Intercept or modify data on behalf of users who should not have access to that data.

**Service Threats**: Exploit service flaws in computers to inhibit use by legitimate users.

### Security Mechanisms

1. **Gatekeeper function**: 
   - Password-based login procedures
   - Screening logic to detect and reject worms, viruses, and attacks

2. **Internal controls**: 
   - Monitor activity and analyze stored information
   - Detect presence of unwanted intruders

---

## 7.2 Principles of Cryptography: Symmetric Key and Public Key

### Symmetric Encryption

- **Definition**: Simplest kind of encryption involving only one secret key to cipher and decipher information
- **Characteristics**:
  - Old and best-known technique
  - Uses a secret key (number, word, or string of random letters)
  - Blended with plain text to change content in particular way
  - Both sender and recipient must know the secret key

**Examples**: AES, DES, RC5, RC6
**Most widely used**: AES-128, AES-192, AES-256

**Main Disadvantage**: All parties must exchange the key before they can decrypt data

*Real-world example*: Banking systems use AES-256 for encrypting customer data during transactions.

### Asymmetric Encryption (Public Key Cryptography)

- **Definition**: Relatively new method compared to symmetric encryption
- **Characteristics**:
  - Uses two keys to encrypt plain text
  - Secret keys exchanged over Internet or large network
  - Ensures malicious persons do not misuse keys
  - Uses two related keys to boost security

**Key Types**:
- **Public Key**: Made freely available to anyone who might want to send you a message
- **Private Key**: Kept secret so only you know it

**Encryption Rules**:
- Message encrypted using public key can only be decrypted using private key
- Message encrypted using private key can be decrypted using public key

**Examples**: RSA, DSA

*Real-world example*: HTTPS websites use public key cryptography - your browser uses the website's public key to encrypt data, which only the website's private key can decrypt.

### Digital Certificates in Asymmetric Encryption

**Certificate**: Package of information that identifies a user and server
- Contains organization's name, issuing organization, user's email, country, user's public key
- Used to discover public keys in client-server communication
- Can uniquely identify the holder

---

## 7.3 Public Key Algorithm - RSA

**RSA Algorithm**: Public key encryption technique considered most secure way of encryption. Invented by Rivest, Shamir and Adleman in 1978.

### Features
- Popular exponentiation in finite field over integers including prime numbers
- Uses sufficiently large integers making it difficult to solve
- Two sets of keys: private key and public key

### RSA Algorithm Steps

**Step 1: Generate RSA Modulus**
- Select two prime numbers p and q
- Calculate their product N = p × q

**Step 2: Derived Number (e)**
- Choose number e such that 1 < e < (p-1) and (q-1)
- e should have no common factor with (p-1) and (q-1) except 1

**Step 3: Public Key**
- Pair of numbers n and e forms RSA public key
- Made public

**Step 4: Private Key**
- Private key d calculated from p, q, and e
- Formula: ed = 1 mod (p-1)(q-1)

### Encryption and Decryption Formulas

**Encryption**: C = P^e mod n
**Decryption**: Plaintext = C^d mod n

### Example (Class Problem)
- **Prime numbers**: P = 53, Q = 59
- **Public key**: n = 3127, e = 3
- **Private key**: d = 2011
- **Encryption of "HI"**: H=8, I=9 → 89^3 mod 3127 = 1394
- **Decryption**: 1394^2011 mod 3127 = 89 → "HI"

*Real-world example*: Online banking uses RSA for secure key exchange when you log into your account.

---

## 7.4 Digital Signature Algorithm

**Digital Signature**: Public-key primitives of message authentication that bind a person/entity to digital data. This binding can be independently verified by receiver and any third party.

### Model of Digital Signature
Digital signature is a cryptographic value calculated from data and a secret key known only by the signer.

### Importance of Digital Signature

1. **Message Authentication**: When verifier validates digital signature using sender's public key, assured that signature created only by sender with corresponding private key

2. **Data Integrity**: If attacker modifies data, digital signature verification fails. Hash of modified data won't match verification algorithm output

3. **Non-repudiation**: Only signer has knowledge of signature key, can only create unique signature on given data

### Four Essential Elements of Security
Privacy, Authentication, Integrity, and Non-repudiation achieved by combining public-key encryption with digital signature.

### Encryption with Digital Signature

**Two Possibilities**:
1. **Sign-then-encrypt**: Can be exploited by receiver to spoof sender identity (not preferred)
2. **Encrypt-then-sign**: More reliable and widely adopted

**Process**: Receiver verifies signature using sender's public key, then retrieves data through decryption using private key.

### Advantages of DSA
- Strong strength levels with smaller signature length
- Less signature computation speed
- Requires less storage compared to other standards
- Patent free

### Disadvantages of DSA
- Requires lot of time for authentication due to complicated remainder operators
- Data not encrypted, only authenticated
- Dependent on SHA1 hash security
- Time-consuming computation

*Real-world example*: PDF documents are digitally signed to ensure they haven't been tampered with and to verify the author's identity.

---

## 7.5 Communication Security: IPSec, VPN, Firewalls, Wireless Security

## IP Security (IPSec)

**Definition**: Set of protocols providing security for Internet Protocol using cryptography. Used for setting up secure Virtual Private Networks (VPNs).

### IPSec Security Services

1. **Authentication Header (AH)**:
   - Authenticates sender
   - Discovers changes in data during transmission
   - Provides data integrity, authentication, anti-replay
   - Does not provide encryption

2. **Encapsulating Security Payload (ESP)**:
   - Performs authentication for sender
   - Encrypts data being sent
   - Provides data integrity, encryption, authentication, anti-replay

### IPSec Modes

1. **Tunnel Mode**: Takes whole IP packet to form secure communication between two gateways
2. **Transport Mode**: Only encapsulates IP payload (not entire packet) for secure channel

### Internet Key Exchange (IKE)
- Network security protocol for dynamic key exchange
- Finds way over Security Association (SA) between 2 devices
- Provides message content protection
- Implements standard algorithms like SHA and MD5

### IPSec Applications
- Electronic mail security
- Network management
- Web access security

### Benefits of IPSec
- Strong security for all traffic crossing perimeter when implemented in firewall/router
- Transparent to applications (below transport layer)
- No need to change software on user/server systems
- Can be transparent to end users

*Real-world example*: Corporate employees use IPSec VPNs to securely access company resources while working remotely.

---

## Virtual Private Network (VPN)

**Definition**: Allows user to connect to private network over Internet securely and privately. Creates encrypted connection called VPN tunnel.

### Types of VPN

1. **Remote Access VPN**:
   - Permits user to connect to private network remotely
   - Access all services and resources
   - Useful for home users and business users

2. **Site to Site VPN** (Router-to-Router VPN):
   - Used in large companies
   - Connects network of one office to another office network
   - **Intranet based VPN**: Several offices of same company connected
   - **Extranet based VPN**: Companies connect to office of another company

### VPN Protocol Types

1. **Internet Protocol Security (IPSec)**:
   - Secures Internet communication across IP network
   - Verifies session and encrypts each data packet
   - **Transport mode**: Encrypts message in data packet
   - **Tunneling mode**: Encrypts whole data packet

2. **Layer 2 Tunneling Protocol (L2TP)**:
   - Often combined with IPSec for highly secure VPN
   - Generates tunnel between two L2TP connection points

3. **Point-to-Point Tunneling Protocol (PPTP)**:
   - Generates tunnel and confines data packet
   - Uses PPP to encrypt data between connection
   - Widely used since early Windows release

4. **SSL and TLS**:
   - Web browser acts as client
   - User access restricted to specific applications instead of entire network
   - Used by online shopping websites

5. **OpenVPN**:
   - Open source VPN for Point-to-Point and Site-to-Site connections
   - Uses traditional security protocol based on SSL and TLS

6. **Secure Shell (SSH)**:
   - Generates encrypted VPN tunnel
   - Data transferred from local port to remote server through encrypted tunnel

*Real-world example*: Netflix employees working from different countries use VPNs to securely access internal company systems and databases.

---

## Firewall

**Definition**: Network security device (hardware or software-based) that monitors incoming and outgoing traffic and accepts, rejects, or drops specific traffic based on defined security rules.

### Firewall Actions
- **Accept**: Allow the traffic
- **Reject**: Block traffic but reply with "unreachable error"
- **Drop**: Block traffic with no reply

### Purpose
Establishes barrier between secured internal networks and outside untrusted networks (like Internet).

### How Firewall Works
- Analyzes incoming traffic based on pre-established rules
- Filters traffic from unsecured or suspicious sources
- Guards traffic at computer's entry points called ports
- Controls access based on source addresses, destination addresses, and port numbers

### Types of Firewall

1. **Host-based Firewalls**:
   - Installed on each network node
   - Controls each incoming and outgoing packet
   - Software application part of operating system
   - Protects each host from attacks and unauthorized access

2. **Network-based Firewalls**:
   - Function on network level
   - Filter all incoming and outgoing traffic across network
   - Usually dedicated system with proprietary software
   - Might have two or more network interface cards (NICs)

### Generation of Firewalls

1. **First Generation - Packet Filtering Firewall**:
   - Controls network access by monitoring packets
   - Allows/stops based on source/destination IP, protocols, ports
   - Analyzes traffic at transport protocol layer

2. **Second Generation - Stateful Inspection Firewall**:
   - Determines connection state of packet
   - More efficient than packet filtering firewall

3. **Third Generation - Application Layer Firewall**:
   - Inspects and filters packets on any OSI layer up to application layer
   - Can block specific content
   - Recognizes when applications and protocols are being misused

4. **Next Generation Firewalls (NGFW)**:
   - Deployed to stop modern security breaches
   - Handles advanced malware attacks and application-layer attacks

*Real-world example*: Corporate networks use next-generation firewalls to block employees from accessing social media during work hours while allowing access to business-related websites.

---

## Wireless Security

**Definition**: Securing wireless network from malicious attempts and unauthorized access.

### Wireless Security Delivery Methods

1. **Hardware-based**: Routers and switches with built-in encryption measures
2. **Wireless IDS and IPS**: Detects, alerts, and prevents wireless network breaches
3. **Wireless security algorithms**: WEP, WPA, WPA2, WPA3

### Automated Wireless Hacking Tools
- AirCrack
- AirSnort
- Cain & Able
- Wireshark
- NetStumbler

### Wireless Security Standards

1. **Wired Equivalent Privacy (WEP) - 1999**:
   - Oldest security algorithm
   - Uses initialization vector (IV) method
   - Originally limited to 64-bit encryption due to U.S. export restrictions
   - Later developed 128-bit and 256-bit versions
   - **Weakness**: Easily crackable, now deprecated

2. **Wi-Fi Protected Access (WPA) - 2003**:
   - Replaced WEP vulnerabilities
   - Most common configuration: WPA-PSK (Pre-Shared Key)
   - Uses 256-bit encryption
   - Considerable enhancement over 64-bit and 128-bit keys

3. **Wi-Fi Protected Access II (WPA2) - 2006**:
   - Became official after WPA became outdated
   - Uses AES algorithms as necessary encryption component
   - Uses CCMP (Counter Cipher Mode - Block Chaining Message Authentication Protocol)
   - Replaced TKIP

4. **Wi-Fi Protected Access 3 (WPA3) - Latest**:
   - Third iteration developed under Wi-Fi Alliance
   - Personal and enterprise security-support features
   - Uses 384-bit Hashed Message Authentication Mode
   - Uses 256-bit Galois/Counter Mode Protocol (GCMP-256)
   - Uses 256-bit Broadcast/Multicast Integrity Protocol
   - Provides perfect forward secrecy mechanism support

*Real-world example*: Modern smartphones automatically prefer WPA3 networks when available, falling back to WPA2 for older routers, and warning users about unsecured or WEP networks in public places like airports or coffee shops.
