# RIP Protocol - Complete Step-by-Step Guide

## Lab Setup: Basic RIP Configuration

### Required Devices
- 3 Routers (2811 or 2901)
- 3 Switches (2960)
- 3 PCs
- Cables (Copper Straight-through and Copper Cross-over)

### Step 1: Create the Topology

1. **Open Cisco Packet Tracer**

2. **Add Routers to the Workspace**
   - Click on "Routers" in the device panel (bottom left)
   - Select "2811" or "2901" router
   - Click and place 3 routers on the workspace
   - Label them R1, R2, and R3 (click on router, then click on label below)

3. **Add Switches to the Workspace**
   - Click on "Switches" in the device panel
   - Select "2960" switch
   - Place 3 switches below each router
   - Label them SW1, SW2, and SW3

4. **Add PCs to the Workspace**
   - Click on "End Devices" in the device panel
   - Select "PC-PT"
   - Place 1 PC below each switch
   - Label them PC1, PC2, and PC3

5. **Connect the Devices**
   - Click on "Connections" (lightning bolt icon)
   - Select "Copper Cross-over" (orange cable)
   - Connect:
     - R1 GigabitEthernet0/0 → R2 GigabitEthernet0/0
     - R2 GigabitEthernet0/1 → R3 GigabitEthernet0/0
   
   - Select "Copper Straight-through" (solid black line)
   - Connect:
     - R1 GigabitEthernet0/1 → SW1 FastEthernet0/1
     - R2 GigabitEthernet0/2 → SW2 FastEthernet0/1
     - R3 GigabitEthernet0/1 → SW3 FastEthernet0/1
     - SW1 FastEthernet0/2 → PC1 FastEthernet0
     - SW2 FastEthernet0/2 → PC2 FastEthernet0
     - SW3 FastEthernet0/2 → PC3 FastEthernet0

### Step 2: Configure IP Addresses on Routers

#### Configure Router R1:

1. **Click on R1**
2. **Click on "CLI" tab**
3. **Press Enter** to get started
4. **Type these commands:**

```
enable
configure terminal
hostname R1

! Configure interface to R2
interface GigabitEthernet0/0
ip address 10.1.1.1 255.255.255.0
no shutdown
exit

! Configure interface to SW1/PC1
interface GigabitEthernet0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

! Save configuration
exit
write memory
```

#### Configure Router R2:

1. **Click on R2**
2. **Click on "CLI" tab**
3. **Type these commands:**

```
enable
configure terminal
hostname R2

! Configure interface to R1
interface GigabitEthernet0/0
ip address 10.1.1.2 255.255.255.0
no shutdown
exit

! Configure interface to R3
interface GigabitEthernet0/1
ip address 10.2.2.1 255.255.255.0
no shutdown
exit

! Configure interface to SW2/PC2
interface GigabitEthernet0/2
ip address 192.168.2.1 255.255.255.0
no shutdown
exit

! Save configuration
exit
write memory
```

#### Configure Router R3:

1. **Click on R3**
2. **Click on "CLI" tab**
3. **Type these commands:**

```
enable
configure terminal
hostname R3

! Configure interface to R2
interface GigabitEthernet0/0
ip address 10.2.2.2 255.255.255.0
no shutdown
exit

! Configure interface to SW3/PC3
interface GigabitEthernet0/1
ip address 192.168.3.1 255.255.255.0
no shutdown
exit

! Save configuration
exit
write memory
```

### Step 3: Configure IP Addresses on PCs

#### Configure PC1:
1. **Click on PC1**
2. **Click on "Desktop" tab**
3. **Click on "IP Configuration"**
4. **Configure:**
   - IP Address: 192.168.1.10
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 192.168.1.1

#### Configure PC2:
1. **Click on PC2**
2. **Click on "Desktop" tab**
3. **Click on "IP Configuration"**
4. **Configure:**
   - IP Address: 192.168.2.10
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 192.168.2.1

#### Configure PC3:
1. **Click on PC3**
2. **Click on "Desktop" tab**
3. **Click on "IP Configuration"**
4. **Configure:**
   - IP Address: 192.168.3.10
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 192.168.3.1

### Step 4: Configure RIP on All Routers

#### Configure RIP on R1:
1. **Click on R1**
2. **Go to CLI tab**
3. **Type these commands:**

```
enable
configure terminal

! Enable RIP version 2
router rip
version 2
no auto-summary

! Advertise directly connected networks
network 10.0.0.0
network 192.168.1.0

exit
write memory
```

#### Configure RIP on R2:
1. **Click on R2**
2. **Go to CLI tab**
3. **Type these commands:**

```
enable
configure terminal

! Enable RIP version 2
router rip
version 2
no auto-summary

! Advertise directly connected networks
network 10.0.0.0
network 192.168.2.0

exit
write memory
```

#### Configure RIP on R3:
1. **Click on R3**
2. **Go to CLI tab**
3. **Type these commands:**

```
enable
configure terminal

! Enable RIP version 2
router rip
version 2
no auto-summary

! Advertise directly connected networks
network 10.0.0.0
network 192.168.3.0

exit
write memory
```

### Step 5: Verify RIP Configuration

1. **On each router, check the routing table:**
```
show ip route
```

2. **Check RIP-specific information:**
```
show ip rip database
show ip protocols
```

3. **Test connectivity between PCs:**
   - Click on PC1
   - Go to Desktop → Command Prompt
   - Type: `ping 192.168.2.10` (PC2)
   - Type: `ping 192.168.3.10` (PC3)

### Step 6: Use Simulation Mode to See RIP Updates

1. **Click on "Simulation" mode (bottom right)**
2. **Click "Edit Filters"**
3. **Uncheck all except "RIP"**
4. **Click "Auto Capture / Play"**
5. **Watch RIP updates being exchanged every 30 seconds**

---

# OSPF Protocol - Complete Step-by-Step Guide

## Lab Setup: Basic OSPF Configuration

### Use the Same Topology as RIP Lab

### Step 1: Remove RIP Configuration (if needed)

On each router (R1, R2, R3):
```
enable
configure terminal
no router rip
exit
write memory
```

### Step 2: Configure OSPF on All Routers

#### Configure OSPF on R1:
1. **Click on R1**
2. **Go to CLI tab**
3. **Type these commands:**

```
enable
configure terminal

! Enable OSPF process
router ospf 1

! Set router ID (optional but recommended)
router-id 1.1.1.1

! Advertise networks - note the wildcard mask
network 10.1.1.0 0.0.0.255 area 0
network 192.168.1.0 0.0.0.255 area 0

exit
write memory
```

#### Configure OSPF on R2:
1. **Click on R2**
2. **Go to CLI tab**
3. **Type these commands:**

```
enable
configure terminal

! Enable OSPF process
router ospf 1

! Set router ID
router-id 2.2.2.2

! Advertise networks
network 10.1.1.0 0.0.0.255 area 0
network 10.2.2.0 0.0.0.255 area 0
network 192.168.2.0 0.0.0.255 area 0

exit
write memory
```

#### Configure OSPF on R3:
1. **Click on R3**
2. **Go to CLI tab**
3. **Type these commands:**

```
enable
configure terminal

! Enable OSPF process
router ospf 1

! Set router ID
router-id 3.3.3.3

! Advertise networks
network 10.2.2.0 0.0.0.255 area 0
network 192.168.3.0 0.0.0.255 area 0

exit
write memory
```

### Step 3: Verify OSPF Configuration

1. **Check OSPF neighbors on each router:**
```
show ip ospf neighbor
```

2. **Check OSPF database:**
```
show ip ospf database
```

3. **Check routing table:**
```
show ip route
```

4. **Check OSPF interfaces:**
```
show ip ospf interface brief
```

### Step 4: Test Connectivity

1. **From PC1, ping other PCs:**
   - Click on PC1
   - Desktop → Command Prompt
   - Type: `ping 192.168.2.10`
   - Type: `ping 192.168.3.10`

### Step 5: Advanced OSPF - Multi-Area Configuration

#### Modify the topology to use multiple areas:

1. **Reconfigure OSPF on R1:**
```
enable
configure terminal
router ospf 1

! Remove old network statements
no network 10.1.1.0 0.0.0.255 area 0
no network 192.168.1.0 0.0.0.255 area 0

! Add networks to different areas
network 10.1.1.0 0.0.0.255 area 0
network 192.168.1.0 0.0.0.255 area 1

exit
write memory
```

2. **Keep R2 in Area 0 (backbone):**
```
! R2 configuration remains the same - all in Area 0
```

3. **Reconfigure OSPF on R3:**
```
enable
configure terminal
router ospf 1

! Remove old network statements
no network 10.2.2.0 0.0.0.255 area 0
no network 192.168.3.0 0.0.0.255 area 0

! Add networks to different areas
network 10.2.2.0 0.0.0.255 area 0
network 192.168.3.0 0.0.0.255 area 2

exit
write memory
```

### Step 6: Demonstrate Protocol Differences

#### Create a Comparison Lab:

1. **Save this as "OSPF-Lab"**
2. **Create a new file and rebuild the same topology**
3. **Configure half with RIP and half with OSPF**
4. **Show differences in:**
   - Update frequency
   - Convergence time
   - Metric calculation
   - Routing decisions

### Common Troubleshooting Commands

For both RIP and OSPF:
```
! General routing
show ip route
show ip protocols
show running-config

! RIP specific
show ip rip database
debug ip rip

! OSPF specific  
show ip ospf neighbor
show ip ospf database
show ip ospf interface
debug ip ospf adj
```

## Tips for Students

1. **Always verify interfaces are up:**
   - Check for green triangles on connections
   - Use `show ip interface brief`

2. **Check IP connectivity first:**
   - Ping between directly connected routers
   - Then test routing protocols

3. **Use Simulation mode:**
   - Filter for specific protocols
   - Step through packet by packet
   - Observe protocol behavior

4. **Common Mistakes to Avoid:**
   - Wrong network statements in RIP/OSPF
   - Forgetting "no shutdown" on interfaces
   - Incorrect wildcard masks in OSPF
   - Not saving configurations

5. **Best Practices:**
   - Always use `write memory` to save
   - Document your configurations
   - Test step by step
   - Use meaningful hostnames
