# Data Link Layer (Layer 2) Demonstration Lab

## Lab Topology
```
        [Switch0]
       /    |    \
    PC0    PC1    PC2
    
    [Switch1]----[Switch2]
        |           |
       PC3         PC4
```

## Required Devices
- 5 PCs
- 3 Switches (2960)

## Lab Steps

### Part 1: MAC Address Learning

1. **Build the topology**
   - Connect PC0, PC1, PC2 to Switch0
   - Configure IPs:
     - PC0: 192.168.1.1/24
     - PC1: 192.168.1.2/24
     - PC2: 192.168.1.3/24

2. **Check initial MAC address table**
   ```
   Switch> enable
   Switch# show mac-address-table
   ```
   - Note: Table should be empty initially

3. **Generate traffic to learn MAC addresses**
   - From PC0, ping PC1
   - Check MAC table again:
   ```
   Switch# show mac-address-table
   ```
   - Observe MAC addresses learned on specific ports

4. **Verify MAC addresses on PCs**
   - On PC0, go to Desktop > Command Prompt
   ```
   ipconfig /all
   ```
   - Compare with switch MAC table entries

### Part 2: VLAN Configuration

1. **Create VLANs on Switch0**
   ```
   Switch> enable
   Switch# configure terminal
   Switch(config)# vlan 10
   Switch(config-vlan)# name Sales
   Switch(config-vlan)# exit
   Switch(config)# vlan 20
   Switch(config-vlan)# name Engineering
   Switch(config-vlan)# exit
   ```

2. **Assign ports to VLANs**
   ```
   Switch(config)# interface FastEthernet 0/1
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 10
   Switch(config-if)# exit
   
   Switch(config)# interface FastEthernet 0/2
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 10
   Switch(config-if)# exit
   
   Switch(config)# interface FastEthernet 0/3
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 20
   Switch(config-if)# exit
   ```

3. **Test VLAN isolation**
   - Ping from PC0 to PC1 (should work - same VLAN)
   - Ping from PC0 to PC2 (should fail - different VLAN)

4. **Verify VLAN configuration**
   ```
   Switch# show vlan brief
   ```

### Part 3: Trunk Configuration

1. **Connect Switch1 to Switch2**
   - Use crossover cable between switches

2. **Configure trunk ports**
   ```
   Switch1(config)# interface FastEthernet 0/24
   Switch1(config-if)# switchport mode trunk
   
   Switch2(config)# interface FastEthernet 0/24
   Switch2(config-if)# switchport mode trunk
   ```

3. **Create same VLANs on both switches**
   - Repeat VLAN creation on Switch1 and Switch2

4. **Test trunk functionality**
   - Assign PC3 to VLAN 10 on Switch1
   - Assign PC4 to VLAN 10 on Switch2
   - Configure IPs in same subnet
   - Test connectivity across trunk

### Part 4: ARP Demonstration

1. **Clear ARP tables**
   - On PC0: `arp -d`

2. **Monitor ARP process**
   - Open Simulation Mode
   - Filter for ARP packets only
   - Ping from PC0 to PC1
   - Step through simulation to see:
     - ARP request (broadcast)
     - ARP reply (unicast)
     - ICMP ping following ARP resolution

3. **Check ARP cache**
   - On PC0: `arp -a`
   - Verify MAC address of PC1 is cached

## Key Observations
- Switches learn MAC addresses dynamically
- VLANs provide Layer 2 segmentation
- Trunk ports carry multiple VLAN traffic
- ARP resolves IP to MAC addresses
- Data Link layer uses MAC addresses for local delivery
