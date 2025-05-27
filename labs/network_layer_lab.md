# Network Layer (Layer 3) Demonstration Lab

## Lab Topology
```
    Network 192.168.1.0/24          Network 192.168.2.0/24
    PC0 ---- Switch0 ---- Router0 ---- Switch1 ---- PC1
                            |
                          Router1
                            |
                         Switch2 ---- PC2
                         Network 192.168.3.0/24
```

## Required Devices
- 3 PCs
- 3 Switches (2960)
- 2 Routers (2911 or similar)

## Lab Steps

### Part 1: Basic IP Configuration and Routing

1. **Build the topology**
   - Connect devices as shown above
   - Use straight-through cables for PC-to-Switch and Router-to-Switch
   - Use crossover cable for Router-to-Router

2. **Configure IP addresses**
   
   **PC0:**
   - IP: 192.168.1.10/24
   - Default Gateway: 192.168.1.1
   
   **PC1:**
   - IP: 192.168.2.10/24
   - Default Gateway: 192.168.2.1
   
   **PC2:**
   - IP: 192.168.3.10/24
   - Default Gateway: 192.168.3.1
   
   **Router0:**
   ```
   Router> enable
   Router# configure terminal
   Router(config)# interface GigabitEthernet 0/0
   Router(config-if)# ip address 192.168.1.1 255.255.255.0
   Router(config-if)# no shutdown
   Router(config-if)# exit
   
   Router(config)# interface GigabitEthernet 0/1
   Router(config-if)# ip address 192.168.2.1 255.255.255.0
   Router(config-if)# no shutdown
   Router(config-if)# exit
   
   Router(config)# interface GigabitEthernet 0/2
   Router(config-if)# ip address 10.0.0.1 255.255.255.252
   Router(config-if)# no shutdown
   Router(config-if)# exit
   ```
   
   **Router1:**
   ```
   Router(config)# interface GigabitEthernet 0/0
   Router(config-if)# ip address 10.0.0.2 255.255.255.252
   Router(config-if)# no shutdown
   Router(config-if)# exit
   
   Router(config)# interface GigabitEthernet 0/1
   Router(config-if)# ip address 192.168.3.1 255.255.255.0
   Router(config-if)# no shutdown
   Router(config-if)# exit
   ```

3. **Test local connectivity**
   - Ping from PC0 to its gateway (192.168.1.1)
   - Ping from PC1 to its gateway (192.168.2.1)
   - Ping from PC2 to its gateway (192.168.3.1)

### Part 2: Static Routing

1. **Check routing tables before configuration**
   ```
   Router0# show ip route
   Router1# show ip route
   ```
   - Note only directly connected networks

2. **Configure static routes**
   
   **On Router0:**
   ```
   Router0(config)# ip route 192.168.3.0 255.255.255.0 10.0.0.2
   ```
   
   **On Router1:**
   ```
   Router1(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1
   Router1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.1
   ```

3. **Verify routing tables**
   ```
   Router0# show ip route
   Router1# show ip route
   ```
   - Note the 'S' entries for static routes

4. **Test end-to-end connectivity**
   - Ping from PC0 to PC2
   - Ping from PC1 to PC2
   - Use tracert to see the path

### Part 3: Dynamic Routing (RIP)

1. **Remove static routes**
   ```
   Router0(config)# no ip route 192.168.3.0 255.255.255.0 10.0.0.2
   Router1(config)# no ip route 192.168.1.0 255.255.255.0 10.0.0.1
   Router1(config)# no ip route 192.168.2.0 255.255.255.0 10.0.0.1
   ```

2. **Configure RIP on both routers**
   
   **Router0:**
   ```
   Router0(config)# router rip
   Router0(config-router)# version 2
   Router0(config-router)# network 192.168.1.0
   Router0(config-router)# network 192.168.2.0
   Router0(config-router)# network 10.0.0.0
   Router0(config-router)# no auto-summary
   ```
   
   **Router1:**
   ```
   Router1(config)# router rip
   Router1(config-router)# version 2
   Router1(config-router)# network 192.168.3.0
   Router1(config-router)# network 10.0.0.0
   Router1(config-router)# no auto-summary
   ```

3. **Verify RIP operation**
   ```
   Router0# show ip protocols
   Router0# show ip route
   ```
   - Note 'R' entries for RIP-learned routes

4. **Test dynamic routing**
   - Ping between all PCs
   - Shut down an interface and observe route convergence

### Part 4: Subnet Demonstration

1. **Create a new subnet scenario**
   - Use network 172.16.0.0/24
   - Divide into 4 subnets

2. **Calculate subnets**
   - Subnet 1: 172.16.0.0/26 (0-63)
   - Subnet 2: 172.16.0.64/26 (64-127)
   - Subnet 3: 172.16.0.128/26 (128-191)
   - Subnet 4: 172.16.0.192/26 (192-255)

3. **Implement subnetting**
   - Reconfigure one network with new subnet
   - Demonstrate that devices in different subnets need routing

### Part 5: Packet Analysis

1. **Use Simulation Mode**
   - Set up a ping from PC0 to PC2
   - Switch to Simulation Mode
   - Click on packets to examine:
     - Source and destination IP addresses
     - TTL decrementation
     - Router decision process

2. **Observe routing decisions**
   - Watch packet at each router
   - See routing table lookup
   - Observe next-hop decisions

## Key Observations
- Layer 3 uses IP addresses for logical addressing
- Routers make forwarding decisions based on routing tables
- Static routes are manually configured
- Dynamic routing protocols automatically share route information
- Subnetting divides networks into smaller segments
- TTL prevents routing loops
