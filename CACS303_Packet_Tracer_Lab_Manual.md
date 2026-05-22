# CACS 303 – Computer Networks
## Cisco Packet Tracer Lab Manual
### Tribhuvan University | BCA Program

---

> **Software Required:** Cisco Packet Tracer 8.x (or later)  
> **Prerequisite:** Basic understanding of OSI model and IP addressing

---

## Table of Contents

1. [Lab 1 – Introduction to Packet Tracer Simulation Tools](#lab-1)
2. [Lab 2 – Creating a Peer-to-Peer Network](#lab-2)
3. [Lab 3 – Creating a Local Area Network (LAN)](#lab-3)
4. [Lab 4 – Interconnecting Two Different LANs](#lab-4)
5. [Lab 5 – Router Configuration using CLI](#lab-5)
6. [Lab 6 – Static Routing](#lab-6)
7. [Lab 7 – Dynamic Routing Protocol: RIP](#lab-7)
8. [Lab 8 – Dynamic Routing Protocol: OSPF](#lab-8)
9. [Lab 9 – Dynamic Routing Protocol: BGP](#lab-9)
10. [Lab 10 – Configuring DHCP Server](#lab-10)
11. [Lab 11 – Configuring DNS Server](#lab-11)
12. [Lab 12 – Configuring FTP Server](#lab-12)
13. [Lab 13 – Configuring Web Server](#lab-13)
14. [Lab 14 – Packet Capture and Header Analysis using Wireshark](#lab-14)

---

<a name="lab-1"></a>
## Lab 1: Introduction to Packet Tracer Simulation Tools

### Objective
Get familiar with the Cisco Packet Tracer interface, tools, and simulation environment.

### Background
Cisco Packet Tracer is a network simulation tool that allows you to create network topologies, configure devices, and simulate network traffic without needing physical hardware.

### Key Interface Components

| Area | Description |
|------|-------------|
| Menu Bar | File, Edit, Options, View, Tools, Help |
| Toolbar | Common shortcuts (New, Open, Save, Print, etc.) |
| Device Panel | All available network devices (bottom left) |
| Workspace | Area where you build your network topology |
| Simulation Panel | Appears when switching to Simulation Mode |
| Status Bar | Shows current mode and other info |

### Steps

**Step 1: Launch Cisco Packet Tracer**
- Open Cisco Packet Tracer from the Start Menu or Desktop.
- Login with your Cisco NetAcad credentials (or skip if offline mode is available).

**Step 2: Explore the Device Panel**
- Look at the bottom-left panel. You will see categories:
  - **Routers** – Used to connect different networks
  - **Switches** – Used to connect devices within a LAN
  - **End Devices** – PCs, Laptops, Servers, Phones
  - **Connections** – Cable types (Copper Straight-through, Crossover, Fiber, etc.)
  - **Wireless Devices** – Access Points, Wireless Routers

**Step 3: Add a Device to the Workspace**
- Click on **End Devices** in the device panel.
- Drag a **PC** to the workspace.
- Click on the PC to open its configuration window.
- Explore the tabs: **Physical**, **Config**, **Desktop**, **Custom Interface**.

**Step 4: Explore the Config Tab**
- Click the **Config** tab on the PC.
- You can see interface settings, gateway settings, and DNS settings.

**Step 5: Explore the Desktop Tab**
- Click the **Desktop** tab on the PC.
- You will find tools like:
  - **Command Prompt** – For ping, ipconfig, tracert commands
  - **Web Browser** – For accessing web servers
  - **IP Configuration** – For setting static IP

**Step 6: Switch to Simulation Mode**
- Click the **Simulation** button at the bottom right (looks like a clock icon).
- Notice the **Event List** and **Play/Pause** controls appear.
- Switch back to **Realtime** mode when done.

**Step 7: Explore Cable Types**
- Click **Connections** (lightning bolt icon) in the device panel.
- Hover over each cable type to read its description:
  - **Copper Straight-Through** – PC to Switch, Switch to Router
  - **Copper Cross-Over** – PC to PC, Switch to Switch
  - **Fiber** – Long-distance, high-speed connections
  - **Serial DCE/DTE** – Router to Router WAN links

### Expected Outcome
Students should be comfortable navigating the Packet Tracer interface and understand the purpose of each tool and device type.

---

<a name="lab-2"></a>
## Lab 2: Creating and Configuring a Simple Peer-to-Peer Network (Two PCs)

### Objective
Create a peer-to-peer network between two PCs using a crossover cable and test connectivity using the ping command.

### Background
A peer-to-peer network connects two computers directly without a central device like a switch or hub. A **crossover cable** is used for direct PC-to-PC connections.

### Network Diagram

```
PC0 [192.168.1.1] ----crossover cable---- PC1 [192.168.1.2]
```

### Steps

**Step 1: Add Two PCs**
- Open Packet Tracer.
- Go to **End Devices** and drag **two PCs** onto the workspace.
- Label them `PC0` and `PC1`.

**Step 2: Connect the PCs**
- Click **Connections** (lightning bolt icon).
- Select **Copper Cross-Over** cable.
- Click on `PC0`, select **FastEthernet0**.
- Click on `PC1`, select **FastEthernet0**.
- You should see the link indicator turn **green** (may take a few seconds).

**Step 3: Configure IP Address on PC0**
- Click on `PC0` → **Desktop** tab → **IP Configuration**.
- Set the following:
  ```
  IP Address:     192.168.1.1
  Subnet Mask:    255.255.255.0
  Default Gateway: (leave blank)
  ```
- Close the window.

**Step 4: Configure IP Address on PC1**
- Click on `PC1` → **Desktop** tab → **IP Configuration**.
- Set the following:
  ```
  IP Address:     192.168.1.2
  Subnet Mask:    255.255.255.0
  Default Gateway: (leave blank)
  ```
- Close the window.

**Step 5: Test Connectivity**
- Click on `PC0` → **Desktop** tab → **Command Prompt**.
- Type the following command:
  ```
  ping 192.168.1.2
  ```
- Press Enter. You should see **4 replies** from `192.168.1.2`.

**Step 6: Verify from PC1**
- Click on `PC1` → **Desktop** → **Command Prompt**.
- Type:
  ```
  ping 192.168.1.1
  ```
- You should again receive **4 successful replies**.

**Step 7: Use ipconfig**
- On either PC's Command Prompt, type:
  ```
  ipconfig
  ```
- This shows the IP, Subnet Mask, and Gateway assigned to the PC.

### Expected Output
```
Pinging 192.168.1.2 with 32 bytes of data:
Reply from 192.168.1.2: bytes=32 time<1ms TTL=128
Reply from 192.168.1.2: bytes=32 time<1ms TTL=128
Reply from 192.168.1.2: bytes=32 time<1ms TTL=128
Reply from 192.168.1.2: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

### Troubleshooting
- If ping fails, check that the cable is **crossover** (not straight-through).
- Ensure both PCs are on the **same subnet** (first 3 octets must match).
- Check that link lights on both sides are **green**.

---

<a name="lab-3"></a>
## Lab 3: Creating a Local Area Network (LAN) and Testing Connectivity

### Objective
Build a LAN by connecting multiple PCs through a switch and verify network communication between all devices.

### Background
A Local Area Network (LAN) uses a **switch** as the central device. All PCs connect to the switch using **straight-through cables**. All devices must be in the same IP subnet.

### Network Diagram

```
       PC0 (192.168.1.1)
        |
       PC1 (192.168.1.2)
        |
   [Switch0]
        |
       PC2 (192.168.1.3)
        |
       PC3 (192.168.1.4)
```

### Steps

**Step 1: Add Devices**
- Add **1 Switch** (from Network Devices → Switches → 2960).
- Add **4 PCs** (from End Devices).
- Arrange them neatly around the switch.

**Step 2: Connect All PCs to the Switch**
- Select **Copper Straight-Through** cable.
- Connect each PC's **FastEthernet0** port to any available port on the switch.
- Repeat for all 4 PCs.
- Wait for all link indicators to turn **green**.

**Step 3: Assign IP Addresses**

| Device | IP Address | Subnet Mask |
|--------|------------|-------------|
| PC0 | 192.168.1.1 | 255.255.255.0 |
| PC1 | 192.168.1.2 | 255.255.255.0 |
| PC2 | 192.168.1.3 | 255.255.255.0 |
| PC3 | 192.168.1.4 | 255.255.255.0 |

- For each PC: Click PC → **Desktop** → **IP Configuration** → Enter IP and Subnet Mask.

**Step 4: Test Connectivity**
- Open Command Prompt on `PC0`.
- Ping all other PCs one by one:
  ```
  ping 192.168.1.2
  ping 192.168.1.3
  ping 192.168.1.4
  ```

**Step 5: Observe Switch Behavior in Simulation Mode**
- Switch to **Simulation Mode**.
- Send a ping from `PC0` to `PC2`.
- Click **Play** and observe how the packet travels from PC0 → Switch → PC2.
- Notice the switch forwards the packet only to the target PC (not all PCs — this is unlike a hub).

### Expected Outcome
All PCs should be able to ping each other successfully (0% packet loss).

---

<a name="lab-4"></a>
## Lab 4: Interconnecting Two Different LANs and Testing Connectivity

### Objective
Connect two separate LANs using a router and test inter-network communication.

### Background
Two LANs on different subnets cannot communicate without a **router**. The router connects both networks and forwards traffic between them. Each PC must have its **Default Gateway** set to the router's interface IP on its subnet.

### Network Diagram

```
LAN 1 (192.168.1.0/24)          LAN 2 (192.168.2.0/24)
PC0 [192.168.1.1]               PC2 [192.168.2.1]
PC1 [192.168.1.2]               PC3 [192.168.2.2]
     |                               |
 [Switch0]                       [Switch1]
     |                               |
     +-------[Router0]---------------+
         Fa0/0: 192.168.1.254   Fa0/1: 192.168.2.254
```

### Steps

**Step 1: Build the Topology**
- Add 2 Switches, 1 Router (e.g., 2811), and 4 PCs.
- Connect PC0 and PC1 to Switch0.
- Connect PC2 and PC3 to Switch1.
- Connect Switch0 to Router's **Fa0/0** port.
- Connect Switch1 to Router's **Fa0/1** port.
- Use **Copper Straight-Through** cables for all connections.

**Step 2: Assign IP Addresses to PCs**

| Device | IP Address | Subnet Mask | Default Gateway |
|--------|------------|-------------|-----------------|
| PC0 | 192.168.1.1 | 255.255.255.0 | 192.168.1.254 |
| PC1 | 192.168.1.2 | 255.255.255.0 | 192.168.1.254 |
| PC2 | 192.168.2.1 | 255.255.255.0 | 192.168.2.254 |
| PC3 | 192.168.2.2 | 255.255.255.0 | 192.168.2.254 |

**Step 3: Configure the Router**
- Click on the Router → **CLI** tab.
- Press **Enter** to start, then type:
  ```
  enable
  configure terminal
  
  interface FastEthernet0/0
  ip address 192.168.1.254 255.255.255.0
  no shutdown
  exit
  
  interface FastEthernet0/1
  ip address 192.168.2.254 255.255.255.0
  no shutdown
  exit
  
  end
  write memory
  ```

**Step 4: Test Connectivity**
- From `PC0`, ping `PC1` (same LAN):
  ```
  ping 192.168.1.2
  ```
- From `PC0`, ping `PC2` (different LAN):
  ```
  ping 192.168.2.1
  ```
- All pings should succeed.

### Expected Outcome
PCs within the same LAN and across different LANs can all ping each other successfully.

---

<a name="lab-5"></a>
## Lab 5: Router Configuration Using Command Line Interface (CLI)

### Objective
Learn essential router CLI commands to configure hostname, passwords, interfaces, and view device information.

### Background
Cisco routers are configured using the **IOS (Internetwork Operating System)** through the CLI. There are several configuration modes:
- **User EXEC Mode** – Basic monitoring (`>`)
- **Privileged EXEC Mode** – Advanced commands (`#`)
- **Global Configuration Mode** – Device-wide settings (`(config)#`)
- **Interface Configuration Mode** – Per-interface settings (`(config-if)#`)

### Steps

**Step 1: Add a Router**
- Add a **Router 2811** to the workspace.
- Click on it → **CLI** tab.

**Step 2: Enter Privileged EXEC Mode**
```
Router> enable
Router#
```

**Step 3: Enter Global Configuration Mode**
```
Router# configure terminal
Router(config)#
```

**Step 4: Set Hostname**
```
Router(config)# hostname MyRouter
MyRouter(config)#
```

**Step 5: Set Console Password**
```
MyRouter(config)# line console 0
MyRouter(config-line)# password cisco123
MyRouter(config-line)# login
MyRouter(config-line)# exit
```

**Step 6: Set Enable (Privileged) Password**
```
MyRouter(config)# enable secret strongpassword
```

**Step 7: Configure an Interface**
```
MyRouter(config)# interface FastEthernet0/0
MyRouter(config-if)# ip address 192.168.1.1 255.255.255.0
MyRouter(config-if)# no shutdown
MyRouter(config-if)# description LAN Interface
MyRouter(config-if)# exit
```

**Step 8: View Running Configuration**
```
MyRouter# show running-config
```

**Step 9: View Interface Status**
```
MyRouter# show ip interface brief
```
Expected output:
```
Interface         IP-Address      OK?  Method  Status    Protocol
FastEthernet0/0   192.168.1.1     YES  manual  up        up
```

**Step 10: Save Configuration**
```
MyRouter# write memory
```
or
```
MyRouter# copy running-config startup-config
```

### Key CLI Commands Reference

| Command | Purpose |
|---------|---------|
| `enable` | Enter privileged mode |
| `configure terminal` | Enter global config mode |
| `show running-config` | View current configuration |
| `show ip interface brief` | View interface summary |
| `show version` | View IOS version info |
| `no shutdown` | Enable an interface |
| `write memory` | Save configuration |

---

<a name="lab-6"></a>
## Lab 6: Static Routing

### Objective
Configure static routes on routers to enable communication between three different networks.

### Background
**Static Routing** means network administrators manually define the path packets take to reach remote networks. The command syntax is:
```
ip route [destination-network] [subnet-mask] [next-hop-ip]
```

### Network Diagram

```
Network A              Network B              Network C
192.168.1.0/24         192.168.2.0/24         192.168.3.0/24

PC0[.1]               PC1[.1]               PC2[.1]
  |                       |                       |
[Switch0]             [Switch1]             [Switch2]
  |                       |                       |
[Router0]----------[Router1]----------[Router2]
 Fa0/0:.254  S0/0:.1  S0/0:.2  S0/1:.1  S0/1:.2  Fa0/0:.254

Serial link 1: 10.0.0.0/24     Serial link 2: 10.0.1.0/24
```

### Steps

**Step 1: Build the Topology**
- Add 3 Routers, 3 Switches, 3 PCs.
- Connect PC0 → Switch0 → Router0 (Fa0/0).
- Connect PC1 → Switch1 → Router1 (Fa0/0).
- Connect PC2 → Switch2 → Router2 (Fa0/0).
- Connect Router0 ↔ Router1 using **Serial DCE** cable (S0/0 on both).
- Connect Router1 ↔ Router2 using **Serial DCE** cable (S0/1 on both).

**Step 2: Assign IP Addresses to PCs**

| Device | IP | Subnet Mask | Gateway |
|--------|-----|-------------|---------|
| PC0 | 192.168.1.1 | 255.255.255.0 | 192.168.1.254 |
| PC1 | 192.168.2.1 | 255.255.255.0 | 192.168.2.254 |
| PC2 | 192.168.3.1 | 255.255.255.0 | 192.168.3.254 |

**Step 3: Configure Router0**
```
Router0(config)# interface FastEthernet0/0
Router0(config-if)# ip address 192.168.1.254 255.255.255.0
Router0(config-if)# no shutdown
Router0(config-if)# exit

Router0(config)# interface Serial0/0
Router0(config-if)# ip address 10.0.0.1 255.255.255.0
Router0(config-if)# clock rate 64000
Router0(config-if)# no shutdown
Router0(config-if)# exit

! Static routes to reach Network B and C
Router0(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
Router0(config)# ip route 192.168.3.0 255.255.255.0 10.0.0.2
```

**Step 4: Configure Router1**
```
Router1(config)# interface FastEthernet0/0
Router1(config-if)# ip address 192.168.2.254 255.255.255.0
Router1(config-if)# no shutdown
Router1(config-if)# exit

Router1(config)# interface Serial0/0
Router1(config-if)# ip address 10.0.0.2 255.255.255.0
Router1(config-if)# no shutdown
Router1(config-if)# exit

Router1(config)# interface Serial0/1
Router1(config-if)# ip address 10.0.1.1 255.255.255.0
Router1(config-if)# clock rate 64000
Router1(config-if)# no shutdown
Router1(config-if)# exit

! Static routes
Router1(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1
Router1(config)# ip route 192.168.3.0 255.255.255.0 10.0.1.2
```

**Step 5: Configure Router2**
```
Router2(config)# interface FastEthernet0/0
Router2(config-if)# ip address 192.168.3.254 255.255.255.0
Router2(config-if)# no shutdown
Router2(config-if)# exit

Router2(config)# interface Serial0/1
Router2(config-if)# ip address 10.0.1.2 255.255.255.0
Router2(config-if)# no shutdown
Router2(config-if)# exit

! Static routes
Router2(config)# ip route 192.168.1.0 255.255.255.0 10.0.1.1
Router2(config)# ip route 192.168.2.0 255.255.255.0 10.0.1.1
```

**Step 6: Verify Routing Table**
```
Router0# show ip route
```

**Step 7: Test Connectivity**
```
PC0> ping 192.168.2.1
PC0> ping 192.168.3.1
```

---

<a name="lab-7"></a>
## Lab 7: Implementing Dynamic Routing Protocol – RIP

### Objective
Configure RIP (Routing Information Protocol) on routers so they automatically learn routes to all networks.

### Background
**RIP (Routing Information Protocol)** is a distance-vector routing protocol. It automatically shares routing information between routers. RIP uses **hop count** as its metric (max 15 hops). We use **RIP version 2** which supports subnet masks.

### Topology
Use the same 3-router topology from Lab 6.

### Steps

**Step 1: Complete Basic Interface Configuration**
- Configure all router interfaces with IP addresses as done in Lab 6 (Steps 1–3).
- **Do NOT add static routes this time.**

**Step 2: Enable RIP on Router0**
```
Router0(config)# router rip
Router0(config-router)# version 2
Router0(config-router)# network 192.168.1.0
Router0(config-router)# network 10.0.0.0
Router0(config-router)# no auto-summary
Router0(config-router)# exit
```

**Step 3: Enable RIP on Router1**
```
Router1(config)# router rip
Router1(config-router)# version 2
Router1(config-router)# network 192.168.2.0
Router1(config-router)# network 10.0.0.0
Router1(config-router)# network 10.0.1.0
Router1(config-router)# no auto-summary
Router1(config-router)# exit
```

**Step 4: Enable RIP on Router2**
```
Router2(config)# router rip
Router2(config-router)# version 2
Router2(config-router)# network 192.168.3.0
Router2(config-router)# network 10.0.1.0
Router2(config-router)# no auto-summary
Router2(config-router)# exit
```

**Step 5: Verify RIP is Working**
- Wait 30–60 seconds for RIP to converge.
```
Router0# show ip route
Router0# show ip protocols
```
- You should see routes marked with **R** (RIP) in the routing table.

**Step 6: Test Connectivity**
```
PC0> ping 192.168.2.1
PC0> ping 192.168.3.1
```

### RIP vs Static Routing Comparison

| Feature | Static Routing | RIP |
|---------|---------------|-----|
| Configuration | Manual | Automatic |
| Scalability | Poor for large networks | Moderate |
| Convergence | Instant | Slow (30s updates) |
| Metric | N/A | Hop count |

---

<a name="lab-8"></a>
## Lab 8: Implementing Dynamic Routing Protocol – OSPF

### Objective
Configure OSPF (Open Shortest Path First) on multiple routers and verify automatic route learning.

### Background
**OSPF** is a link-state routing protocol that uses the **Dijkstra algorithm** to calculate the shortest path. Unlike RIP, OSPF uses **cost** (based on bandwidth) as its metric. OSPF is faster to converge and more scalable than RIP.

Key Concepts:
- **Process ID** – Local identifier for OSPF process (1–65535), doesn't need to match between routers.
- **Area 0 (Backbone Area)** – All OSPF routers must connect to Area 0.
- **Wildcard Mask** – Inverse of subnet mask (e.g., 255.255.255.0 → 0.0.0.255).

### Topology
Use the same 3-router topology from Lab 6.

### Steps

**Step 1: Configure Interfaces**
- Configure all router interfaces as in Lab 6.

**Step 2: Configure OSPF on Router0**
```
Router0(config)# router ospf 1
Router0(config-router)# network 192.168.1.0 0.0.0.255 area 0
Router0(config-router)# network 10.0.0.0 0.0.0.255 area 0
Router0(config-router)# exit
```

**Step 3: Configure OSPF on Router1**
```
Router1(config)# router ospf 1
Router1(config-router)# network 192.168.2.0 0.0.0.255 area 0
Router1(config-router)# network 10.0.0.0 0.0.0.255 area 0
Router1(config-router)# network 10.0.1.0 0.0.0.255 area 0
Router1(config-router)# exit
```

**Step 4: Configure OSPF on Router2**
```
Router2(config)# router ospf 1
Router2(config-router)# network 192.168.3.0 0.0.0.255 area 0
Router2(config-router)# network 10.0.1.0 0.0.0.255 area 0
Router2(config-router)# exit
```

**Step 5: Verify OSPF**
```
Router0# show ip ospf neighbor
Router0# show ip route
Router0# show ip ospf
```
- Routes learned via OSPF are marked with **O** in the routing table.

**Step 6: Test Connectivity**
```
PC0> ping 192.168.3.1
```

### OSPF vs RIP Comparison

| Feature | RIP | OSPF |
|---------|-----|------|
| Type | Distance-Vector | Link-State |
| Metric | Hop Count | Cost (Bandwidth) |
| Max Hops | 15 | No limit |
| Convergence | Slow | Fast |
| Scalability | Small networks | Large networks |

---

<a name="lab-9"></a>
## Lab 9: Implementing Dynamic Routing Protocol – BGP

### Objective
Configure BGP (Border Gateway Protocol) between two routers representing different Autonomous Systems (AS).

### Background
**BGP** is the routing protocol of the Internet. It is used to exchange routing information between different **Autonomous Systems (AS)** – typically different organizations or ISPs. BGP is called an **Exterior Gateway Protocol (EGP)**.

Key Concepts:
- **AS Number** – Unique identifier for each autonomous system (1–65535 for public, 64512–65535 for private).
- **Neighbor (Peer)** – A router that BGP exchanges information with.
- **eBGP** – BGP between different AS numbers.

### Network Diagram

```
AS 100                          AS 200
PC0 [192.168.1.1]          PC1 [192.168.2.1]
     |                              |
[Router0]----Serial----[Router1]
 Fa0/0:192.168.1.254    Fa0/0:192.168.2.254
 S0/0: 10.0.0.1         S0/0: 10.0.0.2
     (AS 100)                (AS 200)
```

### Steps

**Step 1: Build the Topology**
- Add 2 Routers, 2 Switches, 2 PCs.
- Connect as shown above.

**Step 2: Configure Interfaces and PCs**
- Configure PC IP addresses with correct gateways.
- Configure router interfaces with IPs (as in previous labs).

**Step 3: Configure BGP on Router0 (AS 100)**
```
Router0(config)# router bgp 100
Router0(config-router)# neighbor 10.0.0.2 remote-as 200
Router0(config-router)# network 192.168.1.0 mask 255.255.255.0
Router0(config-router)# exit
```

**Step 4: Configure BGP on Router1 (AS 200)**
```
Router1(config)# router bgp 200
Router1(config-router)# neighbor 10.0.0.1 remote-as 100
Router1(config-router)# network 192.168.2.0 mask 255.255.255.0
Router1(config-router)# exit
```

**Step 5: Verify BGP**
```
Router0# show ip bgp
Router0# show ip bgp summary
Router0# show ip route
```
- BGP routes appear with **B** in the routing table.

**Step 6: Test Connectivity**
```
PC0> ping 192.168.2.1
```

---

<a name="lab-10"></a>
## Lab 10: Configuring DHCP Server to Assign IP Addresses Dynamically

### Objective
Configure a DHCP server so that client PCs automatically receive IP addresses, subnet mask, and default gateway.

### Background
**DHCP (Dynamic Host Configuration Protocol)** automatically assigns IP addresses to clients. Instead of manually configuring each PC, a DHCP server handles IP assignment dynamically. This is used in most real-world networks.

### Network Diagram

```
PC0 (DHCP Client)
PC1 (DHCP Client)
PC2 (DHCP Client)
        |
    [Switch0]
        |
    [Server0] (DHCP Server: 192.168.1.100)
        |
    [Router0] (Gateway: 192.168.1.254)
```

### Steps

**Step 1: Build the Topology**
- Add 1 Server, 3 PCs, 1 Switch, 1 Router.
- Connect all to the switch using straight-through cables.

**Step 2: Configure the Server (Static IP)**
- Click on `Server0` → **Desktop** → **IP Configuration**.
  ```
  IP Address:     192.168.1.100
  Subnet Mask:    255.255.255.0
  Default Gateway: 192.168.1.254
  ```

**Step 3: Enable DHCP Service on Server**
- Click on `Server0` → **Services** tab → **DHCP**.
- Turn the DHCP service **ON**.
- Configure the pool:
  ```
  Pool Name:       serverPool
  Default Gateway: 192.168.1.254
  DNS Server:      192.168.1.100
  Start IP Address: 192.168.1.1
  Subnet Mask:     255.255.255.0
  Maximum Users:   50
  ```
- Click **Save**.

**Step 4: Configure Router Interface**
```
Router0(config)# interface FastEthernet0/0
Router0(config-if)# ip address 192.168.1.254 255.255.255.0
Router0(config-if)# no shutdown
Router0(config-if)# exit
```

**Step 5: Set PCs to Use DHCP**
- Click on `PC0` → **Desktop** → **IP Configuration**.
- Select **DHCP** radio button.
- Wait a few seconds — the PC will automatically receive an IP address.
- Repeat for `PC1` and `PC2`.

**Step 6: Verify IP Assignment**
- On each PC, go to **Desktop** → **Command Prompt**:
  ```
  ipconfig
  ```
- Each PC should show a unique IP in the range 192.168.1.1–192.168.1.50.

**Step 7: Test Connectivity**
```
PC0> ping 192.168.1.100
PC0> ping 192.168.1.254
```

---

<a name="lab-11"></a>
## Lab 11: Configuring DNS Server for Domain Name Mapping

### Objective
Configure a DNS server to resolve domain names to IP addresses, and test name resolution from client PCs.

### Background
**DNS (Domain Name System)** translates human-readable domain names (like `www.example.com`) into IP addresses. Without DNS, users would have to remember IP addresses for every website.

### Network Diagram

```
PC0 (DNS Client)
PC1 (DNS Client)
        |
    [Switch0]
        |
    [Server0] 192.168.1.100 (DNS + Web Server)
```

### Steps

**Step 1: Build the Topology**
- Add 1 Server, 2 PCs, 1 Switch.
- Connect all to the switch.

**Step 2: Configure Server IP**
- `Server0` → **Desktop** → **IP Configuration**:
  ```
  IP Address:  192.168.1.100
  Subnet Mask: 255.255.255.0
  ```

**Step 3: Enable DNS Service on Server**
- Click `Server0` → **Services** → **DNS**.
- Turn DNS service **ON**.
- Add a DNS record:
  ```
  Name:    www.mysite.com
  Type:    A Record
  Address: 192.168.1.100
  ```
- Click **Add**.

**Step 4: Enable HTTP Service on Server**
- Click `Server0` → **Services** → **HTTP**.
- Turn HTTP service **ON** (it is usually ON by default).

**Step 5: Configure Client PCs**
- `PC0` → **Desktop** → **IP Configuration**:
  ```
  IP Address:  192.168.1.1
  Subnet Mask: 255.255.255.0
  DNS Server:  192.168.1.100
  ```
- `PC1`:
  ```
  IP Address:  192.168.1.2
  Subnet Mask: 255.255.255.0
  DNS Server:  192.168.1.100
  ```

**Step 6: Test DNS via Web Browser**
- Click on `PC0` → **Desktop** → **Web Browser**.
- In the URL bar, type:
  ```
  www.mysite.com
  ```
- Press Enter. The browser should load the default Cisco Packet Tracer web page from the server.

**Step 7: Test DNS via Command Prompt**
```
PC0> nslookup www.mysite.com
```
Expected output:
```
Server: 192.168.1.100
Address: 192.168.1.100

Name: www.mysite.com
Address: 192.168.1.100
```

---

<a name="lab-12"></a>
## Lab 12: Configuring FTP Server

### Objective
Configure an FTP server and use a client PC to upload and download files using FTP.

### Background
**FTP (File Transfer Protocol)** is used to transfer files between computers over a network. It uses **port 21** for control and **port 20** for data transfer. In Packet Tracer, the server provides an FTP service and clients access it using the command line.

### Network Diagram

```
PC0 (FTP Client: 192.168.1.1)
PC1 (FTP Client: 192.168.1.2)
        |
    [Switch0]
        |
    [Server0] (FTP Server: 192.168.1.100)
```

### Steps

**Step 1: Build and Configure the Topology**
- Add Server, 2 PCs, Switch.
- Assign IPs:
  - Server0: 192.168.1.100 / 255.255.255.0
  - PC0: 192.168.1.1 / 255.255.255.0
  - PC1: 192.168.1.2 / 255.255.255.0

**Step 2: Enable FTP on the Server**
- Click `Server0` → **Services** → **FTP**.
- Turn FTP service **ON**.
- The default user is already configured. Add a user:
  ```
  Username: student
  Password: cisco
  ```
- Check permissions: **Read, Write, Delete, Rename, List**
- Click **Add**.

**Step 3: Note Existing Files on Server**
- You will see sample files already listed (e.g., `c2600-i-mz.121-5.bin`).

**Step 4: Connect from PC0 using FTP**
- Click `PC0` → **Desktop** → **Command Prompt**.
- Type:
  ```
  ftp 192.168.1.100
  ```
- When prompted:
  ```
  Username: student
  Password: cisco
  ```

**Step 5: FTP Commands**

Once connected, use these FTP commands:

| Command | Purpose |
|---------|---------|
| `dir` | List files on server |
| `get filename` | Download a file |
| `put filename` | Upload a file |
| `delete filename` | Delete a file |
| `quit` | Exit FTP session |

**Step 6: Try Downloading a File**
```
ftp> dir
ftp> get iosv2-0.bin
ftp> quit
```

**Step 7: Verify the Downloaded File**
- After quitting FTP, you can verify the file exists on the PC using:
  ```
  dir
  ```

---

<a name="lab-13"></a>
## Lab 13: Configuring Web Server

### Objective
Configure an HTTP web server and access it from client PCs using a web browser.

### Background
A **web server** hosts websites and serves web pages to clients using the **HTTP protocol** on **port 80** (or HTTPS on port 443). Clients use a **web browser** to access these pages.

### Network Diagram

```
PC0 (Web Client: 192.168.1.1)
PC1 (Web Client: 192.168.1.2)
        |
    [Switch0]
        |
    [Server0] (Web Server: 192.168.1.100)
```

### Steps

**Step 1: Build the Topology**
- Add Server, 2 PCs, Switch.
- Connect all to switch with straight-through cables.
- Assign IPs:
  - Server0: 192.168.1.100 / 255.255.255.0
  - PC0: 192.168.1.1 / 255.255.255.0
  - PC1: 192.168.1.2 / 255.255.255.0

**Step 2: Enable HTTP on the Server**
- Click `Server0` → **Services** → **HTTP**.
- Ensure the service is **ON**.
- You can see and edit the default `index.html` page.

**Step 3: Edit the Web Page (Optional)**
- Click on `index.html` in the HTTP service.
- Modify the HTML content:
  ```html
  <html>
  <head><title>My Network Lab</title></head>
  <body>
  <h1>Welcome to CACS 303 Lab</h1>
  <p>This is a web server configured in Cisco Packet Tracer.</p>
  </body>
  </html>
  ```
- Click **Save**.

**Step 4: Access Web Server from PC0**
- Click `PC0` → **Desktop** → **Web Browser**.
- In the address bar, type:
  ```
  http://192.168.1.100
  ```
- Press Enter. The custom webpage should load.

**Step 5: Access Using Domain Name (Optional – combines with Lab 11)**
- Configure DNS on the server (as in Lab 11) with `www.mylab.com` pointing to `192.168.1.100`.
- Set PC's DNS server to `192.168.1.100`.
- In the browser, type `www.mylab.com` — it should resolve and load the page.

**Step 6: Observe HTTP Traffic in Simulation Mode**
- Switch to **Simulation Mode**.
- Set filter to show only **HTTP** packets.
- Open the browser on PC0 and make a request.
- Click **Play** and observe:
  - TCP 3-way handshake (SYN → SYN-ACK → ACK)
  - HTTP GET Request from client
  - HTTP 200 OK Response from server

### Expected Outcome
The web browser on PC0 and PC1 successfully loads the web page hosted on Server0.

---

<a name="lab-14"></a>
## Lab 14: Capturing a Packet and Performing Header Analysis Using Wireshark

### Objective
Capture network packets using Wireshark and analyze the headers at different OSI layers (Ethernet, IP, TCP, HTTP).

### Background
**Wireshark** is a network protocol analyzer that captures live network traffic and displays packet headers in detail. It is an essential tool for network troubleshooting and understanding how protocols work at a deep level.

> **Note:** In Cisco Packet Tracer, you can use the **Simulation Mode** with **PDU (Protocol Data Unit) Inspector** to achieve similar results. For actual Wireshark use, this lab also describes steps for running Wireshark on your real PC.

### Part A: Packet Analysis in Cisco Packet Tracer (Simulation Mode)

**Step 1: Build a Simple Network**
- Create a network with PC0, Switch, and Server0 (as in Lab 13).
- Configure IPs and ensure connectivity.

**Step 2: Switch to Simulation Mode**
- Click the **Simulation** button (bottom right).
- In the **Event List Filters**, check only:
  - HTTP
  - TCP
  - ICMP

**Step 3: Generate Traffic**
- Click on `PC0` → **Desktop** → **Web Browser**.
- Type the server's IP address and press Enter.

**Step 4: Step Through the Packets**
- Click **Play** or use **Capture/Forward** (single step button).
- Click on any packet in the **Event List** to inspect it.

**Step 5: Inspect PDU Headers**
- Click on a packet → **PDU Information** window opens.
- Click **Inbound PDU Details** or **Outbound PDU Details**.
- Observe headers layer by layer:

**Ethernet Frame (Layer 2 – Data Link):**
```
Preamble:          10101010...
Destination MAC:   00:60:70:XX:XX:XX
Source MAC:        00:01:97:XX:XX:XX
Type:              0x0800 (IPv4)
```

**IP Packet (Layer 3 – Network):**
```
Version:           4
Header Length:     20 bytes
TTL:               128
Protocol:          6 (TCP)
Source IP:         192.168.1.1
Destination IP:    192.168.1.100
```

**TCP Segment (Layer 4 – Transport):**
```
Source Port:       1025 (random client port)
Destination Port:  80 (HTTP)
Sequence Number:   0
Flags:             SYN
```

**HTTP Data (Layer 7 – Application):**
```
GET / HTTP/1.1
Host: 192.168.1.100
```

### Part B: Wireshark on Real PC (Physical Lab)

**Step 1: Install Wireshark**
- Download from [https://www.wireshark.org](https://www.wireshark.org) and install.

**Step 2: Start Capture**
- Open Wireshark.
- Select your **active network interface** (e.g., Ethernet or Wi-Fi).
- Click the **shark fin** button (Start Capture).

**Step 3: Generate Traffic**
- Open a web browser and visit `http://example.com`.
- Or open Command Prompt and run:
  ```
  ping 8.8.8.8
  ```

**Step 4: Stop Capture**
- Click the **red square** button to stop capturing.

**Step 5: Filter Packets**
- In the filter bar, type:
  ```
  http
  ```
  or
  ```
  icmp
  ```
  or
  ```
  tcp
  ```
- Press Enter to filter only those packets.

**Step 6: Analyze a Packet**
- Click on any captured packet.
- Expand the layers in the **Packet Details** pane:
  - **Frame** – Physical layer info (length, timestamp)
  - **Ethernet II** – Source and Destination MAC addresses
  - **Internet Protocol** – Source and Destination IP, TTL, Protocol
  - **Transmission Control Protocol** – Ports, Sequence/Ack numbers, Flags
  - **Hypertext Transfer Protocol** – HTTP Method, URL, Status Code

**Step 7: Analyze ICMP Ping**
- Filter: `icmp`
- Select an ICMP Echo Request packet.
- Observe:
  - **Type: 8** = Echo Request (ping)
  - **Type: 0** = Echo Reply (pong)

### Header Analysis Summary Table

| OSI Layer | Protocol | Key Fields to Identify |
|-----------|----------|----------------------|
| Layer 7 (Application) | HTTP | Method (GET/POST), URL, Status Code |
| Layer 4 (Transport) | TCP | Source Port, Dest Port, Flags (SYN/ACK), Seq/Ack No. |
| Layer 3 (Network) | IP | Source IP, Dest IP, TTL, Protocol |
| Layer 2 (Data Link) | Ethernet | Source MAC, Dest MAC, EtherType |
| Layer 1 (Physical) | Frame | Length, Timestamp |

### Expected Outcome
Students can identify and explain each field in packet headers across multiple OSI layers using Wireshark or Packet Tracer's simulation mode.

---

## Quick Reference: Useful CLI Commands

```
! View Commands
show running-config          - View current configuration
show startup-config          - View saved configuration
show ip interface brief      - Quick interface status summary
show ip route                - View routing table
show ip protocols            - View routing protocols running
show ip ospf neighbor        - View OSPF neighbors
show ip bgp summary          - View BGP peer summary

! Interface Commands
interface FastEthernet0/0    - Enter interface config mode
ip address [IP] [MASK]       - Assign IP address
no shutdown                  - Enable the interface
shutdown                     - Disable the interface

! Routing
ip route [net] [mask] [next-hop]    - Add static route
router rip                          - Enable RIP
router ospf [process-id]            - Enable OSPF
router bgp [as-number]              - Enable BGP
network [network-id]                - Advertise a network

! Save and Reset
write memory                 - Save config to NVRAM
copy running-config startup-config  - Same as above
erase startup-config         - Erase saved config
reload                       - Restart the router
```

---

## IP Addressing Cheat Sheet

| Class | Range | Default Subnet Mask | Typical Use |
|-------|-------|---------------------|-------------|
| A | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 | Large organizations |
| B | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 | Medium organizations |
| C | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 | Small networks (labs) |

**Private IP Ranges (use in labs):**
- `10.0.0.0 – 10.255.255.255`
- `172.16.0.0 – 172.31.255.255`
- `192.168.0.0 – 192.168.255.255`

---

*End of CACS 303 Cisco Packet Tracer Lab Manual*  
*Tribhuvan University | BCA Program | Computer Networks*
