# EIGRP Protocol - Complete Step-by-Step Guide

## Lab Setup: Basic EIGRP Configuration

### Step 1: Create the Topology
Same as RIP/OSPF lab topology

### Step 2: Clear Previous Routing Protocols

On each router:
```
enable
configure terminal
no router rip
no router ospf 1
exit
write memory
```

### Step 3: Configure EIGRP on All Routers

#### Configure EIGRP on R1:
1. **Click on R1**
2. **Go to CLI tab**
3. **Type these commands:**

```
enable
configure terminal

! Enable EIGRP with AS number 100
router eigrp 100

! Disable auto-summarization
no auto-summary

! Advertise networks
network 10.1.1.0 0.0.0.255
network 192.168.1.0 0.0.0.255

! Optional: Set router ID
eigrp router-id 1.1.1.1

exit
write memory
```

#### Configure EIGRP on R2:
```
enable
configure terminal

router eigrp 100
no auto-summary
network 10.1.1.0 0.0.0.255
network 10.2.2.0 0.0.0.255
network 192.168.2.0 0.0.0.255
eigrp router-id 2.2.2.2

exit
write memory
```

#### Configure EIGRP on R3:
```
enable
configure terminal

router eigrp 100
no auto-summary
network 10.2.2.0 0.0.0.255
network 192.168.3.0 0.0.0.255
eigrp router-id 3.3.3.3

exit
write memory
```

### Step 4: Verify EIGRP Configuration

```
! Check EIGRP neighbors
show ip eigrp neighbors

! Check EIGRP topology
show ip eigrp topology

! Check routing table
show ip route

! Check EIGRP interfaces
show ip eigrp interfaces
```

### Step 5: EIGRP Advanced Features

#### Configure Unequal Cost Load Balancing:

1. **Add another path between R1 and R3**
2. **Configure different metrics**
3. **Enable variance:**

```
router eigrp 100
variance 2
```

---

# BGP Protocol - Complete Step-by-Step Guide

## Lab Setup: Basic eBGP Configuration

### Create a New Topology:

```
AS 65001          AS 65002
[ISP1-R1]--------[ISP2-R1]
    |                |
[LAN-SW1]        [LAN-SW2]
    |                |
  [PC1]            [PC2]
```

### Step 1: Build the Topology

1. **Add Devices:**
   - 2 Routers (2911)
   - 2 Switches (2960)
   - 2 PCs
   - Connect with appropriate cables

### Step 2: Configure IP Addresses

#### ISP1-R1:
```
enable
configure terminal
hostname ISP1-R1

interface GigabitEthernet0/0
ip address 200.1.1.1 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
ip address 10.1.1.1 255.255.255.0
no shutdown
exit

exit
write memory
```

#### ISP2-R1:
```
enable
configure terminal
hostname ISP2-R1

interface GigabitEthernet0/0
ip address 200.1.1.2 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
ip address 20.1.1.1 255.255.255.0
no shutdown
exit

exit
write memory
```

### Step 3: Configure BGP

#### Configure BGP on ISP1-R1:
```
enable
configure terminal

! Enable BGP for AS 65001
router bgp 65001

! Define BGP neighbor
neighbor 200.1.1.2 remote-as 65002

! Advertise network
network 10.1.1.0 mask 255.255.255.0

! Optional: Set router ID
bgp router-id 1.1.1.1

exit
write memory
```

#### Configure BGP on ISP2-R1:
```
enable
configure terminal

! Enable BGP for AS 65002
router bgp 65002

! Define BGP neighbor
neighbor 200.1.1.1 remote-as 65001

! Advertise network
network 20.1.1.0 mask 255.255.255.0

! Optional: Set router ID
bgp router-id 2.2.2.2

exit
write memory
```

### Step 4: Verify BGP Configuration

```
! Check BGP summary
show ip bgp summary

! Check BGP routes
show ip bgp

! Check BGP neighbors
show ip bgp neighbors

! Check routing table
show ip route
```

### Step 5: Configure PCs

**PC1:**
- IP: 10.1.1.10
- Mask: 255.255.255.0
- Gateway: 10.1.1.1

**PC2:**
- IP: 20.1.1.10
- Mask: 255.255.255.0
- Gateway: 20.1.1.1

---

# Complete Multi-Protocol Lab

## Advanced Topology for Protocol Comparison

```
     RIP Domain                OSPF Domain              EIGRP Domain
[R1]----[R2]----[R3]----[ABR-R4]----[R5]----[R6]----[ASBR-R7]----[R8]
 |              |                    |              |              |
PC1            PC2                  PC3            PC4            PC5
```

### Step 1: Configure Different Protocols in Different Domains

1. **R1-R2-R3: RIP Domain**
2. **R3-R4-R5-R6: OSPF Domain** 
3. **R6-R7-R8: EIGRP Domain**

### Step 2: Configure Redistribution

#### On R3 (RIP to OSPF):
```
router ospf 1
redistribute rip metric 100 subnets

router rip
redistribute ospf 1 metric 5
```

#### On R6 (OSPF to EIGRP):
```
router eigrp 100
redistribute ospf 1 metric 1000 100 255 1 1500

router ospf 1
redistribute eigrp 100 metric 100 subnets
```

### Step 3: Verify End-to-End Connectivity

```
! From PC1, ping all other PCs
ping 192.168.2.10
ping 192.168.3.10
ping 192.168.4.10
ping 192.168.5.10
```

### Step 4: Analyze Protocol Behavior

1. **Use Simulation Mode**
2. **Filter for different protocols**
3. **Observe:**
   - Update frequencies
   - Packet types
   - Convergence behavior

## Troubleshooting Guide

### Common Issues and Solutions:

1. **No Neighbors Forming:**
   - Check interface status (`show ip interface brief`)
   - Verify IP connectivity (`ping`)
   - Check protocol configuration

2. **Routes Not Appearing:**
   - Verify network statements
   - Check for correct masks/wildcards
   - Ensure protocols are enabled

3. **Redistribution Not Working:**
   - Check metric values
   - Verify redistribution commands
   - Look for route filters

### Debug Commands:

```
! RIP debugging
debug ip rip

! OSPF debugging
debug ip ospf adj
debug ip ospf packet

! EIGRP debugging
debug eigrp packet
debug eigrp neighbor

! BGP debugging
debug ip bgp
debug ip bgp updates
```

## Best Practices for Teaching

1. **Start Simple**
   - Single protocol first
   - Add complexity gradually
   - Build confidence

2. **Use Visual Aids**
   - Topology diagrams
   - Simulation mode
   - Packet captures

3. **Encourage Experimentation**
   - Let students break things
   - Try different scenarios
   - Learn from failures

4. **Document Everything**
   - Configuration commands
   - Topology diagrams
   - Test results

5. **Real-World Context**
   - When to use each protocol
   - Enterprise scenarios
   - ISP applications
