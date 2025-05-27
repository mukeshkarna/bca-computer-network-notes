# Routing Protocols Demonstration Labs

## Overview
This lab series demonstrates the configuration and operation of major routing protocols:
- RIP (Routing Information Protocol)
- OSPF (Open Shortest Path First)
- EIGRP (Enhanced Interior Gateway Routing Protocol)
- BGP (Border Gateway Protocol) - if available in your PT version
- IS-IS (Intermediate System to Intermediate System) - limited in PT

## LAB 1: RIP Configuration and Operation

### Topology
```
    [R1]--------[R2]--------[R3]
     |           |           |
   LAN1        LAN2        LAN3
192.168.1.0  192.168.2.0  192.168.3.0
```

### RIP Configuration Steps

1. **Basic Setup**
   ```
   ! On R1
   Router(config)# hostname R1
   R1(config)# interface g0/0
   R1(config-if)# ip address 10.1.1.1 255.255.255.0
   R1(config-if)# no shutdown
   R1(config)# interface g0/1
   R1(config-if)# ip address 192.168.1.1 255.255.255.0
   R1(config-if)# no shutdown
   
   ! On R2
   Router(config)# hostname R2
   R2(config)# interface g0/0
   R2(config-if)# ip address 10.1.1.2 255.255.255.0
   R2(config-if)# no shutdown
   R2(config)# interface g0/1
   R2(config-if)# ip address 10.2.2.1 255.255.255.0
   R2(config-if)# no shutdown
   R2(config)# interface g0/2
   R2(config-if)# ip address 192.168.2.1 255.255.255.0
   R2(config-if)# no shutdown
   
   ! On R3
   Router(config)# hostname R3
   R3(config)# interface g0/0
   R3(config-if)# ip address 10.2.2.2 255.255.255.0
   R3(config-if)# no shutdown
   R3(config)# interface g0/1
   R3(config-if)# ip address 192.168.3.1 255.255.255.0
   R3(config-if)# no shutdown
   ```

2. **Configure RIP v2**
   ```
   ! On all routers
   Router(config)# router rip
   Router(config-router)# version 2
   Router(config-router)# no auto-summary
   
   ! On R1
   R1(config-router)# network 10.0.0.0
   R1(config-router)# network 192.168.1.0
   
   ! On R2
   R2(config-router)# network 10.0.0.0
   R2(config-router)# network 192.168.2.0
   
   ! On R3
   R3(config-router)# network 10.0.0.0
   R3(config-router)# network 192.168.3.0
   ```

3. **Verify RIP Operation**
   ```
   R1# show ip route
   R1# show ip protocols
   R1# show ip rip database
   R1# debug ip rip
   ```

4. **Demonstrate RIP Characteristics**
   - Maximum hop count: 15
   - Updates every 30 seconds
   - Distance vector protocol
   - Uses hop count as metric

5. **Show RIP Limitations**
   - Add more routers to reach 16 hops
   - Show route becomes unreachable

## LAB 2: OSPF Configuration and Operation

### Topology
```
         Area 0 (Backbone)
    [R1]--------[R2]--------[R3]
     |           |           |
   Area 1     Area 0      Area 2
    |           |           |
   LAN1        LAN2        LAN3
```

### OSPF Configuration Steps

1. **Configure OSPF**
   ```
   ! On R1
   R1(config)# router ospf 1
   R1(config-router)# network 10.1.1.0 0.0.0.255 area 0
   R1(config-router)# network 192.168.1.0 0.0.0.255 area 1
   
   ! On R2 (ABR - Area Border Router)
   R2(config)# router ospf 1
   R2(config-router)# network 10.1.1.0 0.0.0.255 area 0
   R2(config-router)# network 10.2.2.0 0.0.0.255 area 0
   R2(config-router)# network 192.168.2.0 0.0.0.255 area 0
   
   ! On R3
   R3(config)# router ospf 1
   R3(config-router)# network 10.2.2.0 0.0.0.255 area 0
   R3(config-router)# network 192.168.3.0 0.0.0.255 area 2
   ```

2. **Configure OSPF Parameters**
   ```
   ! Set router ID
   R1(config)# router ospf 1
   R1(config-router)# router-id 1.1.1.1
   
   ! Configure interface costs
   R1(config)# interface g0/0
   R1(config-if)# ip ospf cost 10
   
   ! Set interface priorities for DR election
   R1(config-if)# ip ospf priority 100
   ```

3. **Verify OSPF Operation**
   ```
   R1# show ip ospf neighbor
   R1# show ip ospf database
   R1# show ip ospf interface
   R1# show ip route ospf
   ```

4. **Demonstrate OSPF Features**
   - Designated Router (DR) election
   - Different area types
   - Link-state database
   - Fast convergence

### Advanced OSPF Lab

1. **Create Multi-Area OSPF**
   ```
   Area 0 (Backbone)
        |
   [R1]--[R2]--[R3]
    |     |     |
  Area1 Area0 Area2
   |     |     |
  [R4]  [R5]  [R6]
   |     |     |
  LAN   LAN   LAN
   ```

2. **Configure Stub Areas**
   ```
   R1(config)# router ospf 1
   R1(config-router)# area 1 stub
   
   R4(config)# router ospf 1
   R4(config-router)# area 1 stub
   ```

3. **Configure Virtual Links**
   - Use when an area doesn't connect directly to Area 0
   ```
   R2(config)# router ospf 1
   R2(config-router)# area 1 virtual-link 4.4.4.4
   ```

## LAB 3: EIGRP Configuration

### Topology
```
    [R1]--------[R2]--------[R3]
     |     \     |     /     |
   LAN1     \   LAN2  /    LAN3
             [R4]----
               |
             LAN4
```

### EIGRP Configuration Steps

1. **Basic EIGRP Setup**
   ```
   ! On R1
   R1(config)# router eigrp 100
   R1(config-router)# network 10.0.0.0
   R1(config-router)# network 192.168.1.0
   R1(config-router)# no auto-summary
   
   ! Repeat for other routers with their networks
   ```

2. **Configure EIGRP Metrics**
   ```
   ! Modify bandwidth and delay
   R1(config)# interface g0/0
   R1(config-if)# bandwidth 1000000
   R1(config-if)# delay 10
   ```

3. **Verify EIGRP**
   ```
   R1# show ip eigrp neighbors
   R1# show ip eigrp topology
   R1# show ip route eigrp
   ```

4. **Demonstrate EIGRP Features**
   - Feasible successors
   - Unequal cost load balancing
   - Rapid convergence
   - Composite metric

## LAB 4: Protocol Comparison Lab

### Multi-Protocol Topology
```
    RIP Domain          OSPF Domain
    [R1]---[R2]---[R3]---[R4]---[R5]
     |             |             |
   LAN1          LAN3          LAN5
```

### Configuration Steps

1. **Configure Different Protocols**
   - R1-R2: RIP
   - R2-R3-R4: OSPF
   - R4-R5: EIGRP
   - R3: Redistribution point

2. **Configure Redistribution**
   ```
   ! On R3 (Redistribution Router)
   R3(config)# router ospf 1
   R3(config-router)# redistribute rip metric 100 subnets
   R3(config-router)# redistribute eigrp 100 metric 100 subnets
   
   R3(config)# router rip
   R3(config-router)# redistribute ospf 1 metric 5
   
   R3(config)# router eigrp 100
   R3(config-router)# redistribute ospf 1 metric 1000 100 255 1 1500
   ```

3. **Compare Protocol Behaviors**
   - Convergence times
   - Update mechanisms
   - Metric calculations
   - Administrative distances

## LAB 5: BGP Configuration (Basic)

### Topology
```
    AS 100          AS 200
    [R1]------------[R2]
     |               |
   LAN1            LAN2
```

### BGP Configuration Steps

1. **Configure eBGP**
   ```
   ! On R1 (AS 100)
   R1(config)# router bgp 100
   R1(config-router)# neighbor 10.1.1.2 remote-as 200
   R1(config-router)# network 192.168.1.0 mask 255.255.255.0
   
   ! On R2 (AS 200)
   R2(config)# router bgp 200
   R2(config-router)# neighbor 10.1.1.1 remote-as 100
   R2(config-router)# network 192.168.2.0 mask 255.255.255.0
   ```

2. **Verify BGP**
   ```
   R1# show ip bgp summary
   R1# show ip bgp
   R1# show ip bgp neighbors
   ```

## Demonstration Techniques

### 1. Convergence Testing
1. **Setup baseline connectivity**
2. **Shutdown an interface**
3. **Measure convergence time**
   - RIP: 180 seconds (worst case)
   - OSPF: <10 seconds
   - EIGRP: <5 seconds

### 2. Metric Manipulation
1. **Change interface costs/metrics**
2. **Show path selection changes**
3. **Demonstrate load balancing**

### 3. Protocol Behavior Analysis
1. **Use debug commands**
   ```
   debug ip rip
   debug ip ospf adj
   debug eigrp packets
   ```

2. **Capture with Simulation Mode**
   - Filter for routing updates
   - Show packet contents
   - Explain protocol messages

### 4. Failure Scenarios
1. **Link failures**
   - Primary path down
   - Backup path activation

2. **Router failures**
   - Neighbor detection
   - Route withdrawal

3. **Flapping links**
   - Route instability
   - Dampening mechanisms

## Assessment Activities

### 1. Protocol Identification
- Given routing table output, identify protocols
- Match characteristics to protocols

### 2. Configuration Challenges
- Configure multi-protocol network
- Implement redistribution
- Optimize routing paths

### 3. Troubleshooting Scenarios
- Fix routing loops
- Resolve adjacency issues
- Correct redistribution problems

### 4. Design Projects
- Design redundant network
- Implement hierarchical routing
- Plan protocol migration

## Key Learning Points

### RIP
- Distance vector
- Hop count metric
- Simple but limited
- 15 hop maximum

### OSPF
- Link state
- Cost metric
- Hierarchical design
- Fast convergence

### EIGRP
- Advanced distance vector
- Composite metric
- Cisco proprietary
- Very fast convergence

### BGP
- Path vector
- AS path metric
- Internet routing
- Policy-based routing

## Tips for Teaching

1. **Start Simple**
   - Single protocol first
   - Add complexity gradually
   - Build on success

2. **Compare and Contrast**
   - Side-by-side comparisons
   - Same topology, different protocols
   - Highlight differences

3. **Real-World Context**
   - When to use each protocol
   - Industry best practices
   - Scalability considerations

4. **Hands-On Practice**
   - Let students experiment
   - Break things and fix them
   - Learn from mistakes

5. **Documentation**
   - Require topology diagrams
   - Configuration documentation
   - Troubleshooting logs
