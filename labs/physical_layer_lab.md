# Physical Layer (Layer 1) Demonstration Lab

## Lab Topology
```
PC0 -------- Switch0 -------- PC1
              |
             PC2
```

## Required Devices
- 3 PCs (PC0, PC1, PC2)
- 1 Switch (2960)
- Appropriate cables

## Lab Steps

### Part 1: Understanding Cable Types

1. **Add devices to workspace**
   - Drag 3 PCs to the workspace
   - Drag 1 Switch 2960 to the workspace

2. **Test different cable types**
   - Select "Copper Straight-Through" cable
   - Connect PC0 to Switch0 (FastEthernet0/1)
   - Connect PC1 to Switch0 (FastEthernet0/2)
   - Observe the green triangles indicating successful connections

3. **Demonstrate wrong cable type**
   - Try connecting PC2 directly to PC1 with straight-through cable
   - Notice the red 'X' indicating wrong cable type
   - Replace with crossover cable for PC-to-PC connection

4. **Configure IP addresses**
   - PC0: 192.168.1.1/24
   - PC1: 192.168.1.2/24
   - PC2: 192.168.1.3/24

### Part 2: Physical Layer Status

1. **Check interface status on switch**
   ```
   Switch> enable
   Switch# show interfaces status
   ```
   - Note the port status, speed, and duplex settings

2. **Simulate cable failure**
   - Delete the cable between PC0 and Switch0
   - Check interface status again
   - Note the status change to "notconnect"

3. **Check physical layer details**
   ```
   Switch# show interfaces FastEthernet 0/1
   ```
   - Observe hardware details, line protocol status

### Part 3: Media Types Demonstration

1. **Replace with fiber connection (if available in your PT version)**
   - Remove PC0
   - Add a router with fiber interface
   - Connect using fiber cable
   - Show the difference in interface properties

2. **Test connectivity**
   - Use Simple PDU to test between all devices
   - Use Simulation Mode to show bit transmission

### Part 4: Hub vs Switch Behavior

1. **Replace Switch with Hub**
   - Delete Switch0
   - Add Hub-PT
   - Reconnect all PCs to the hub

2. **Demonstrate collision domain**
   - Configure all PCs with IPs in same subnet
   - Open Simulation Mode
   - Send simultaneous pings from PC0 and PC1 to PC2
   - Observe collisions on the hub

3. **Compare with switch behavior**
   - Replace hub with switch
   - Repeat the simultaneous ping test
   - Note no collisions occur

## Key Observations
- Cable types matter for physical connections
- Physical layer provides bit transmission
- Hubs create single collision domain
- Switches separate collision domains
- Physical layer status affects all upper layers
