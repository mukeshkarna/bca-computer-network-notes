# Quick Reference: Routing Protocols Configuration

## RIP Configuration
```bash
router rip
version 2
no auto-summary
network [network-address]
```

## OSPF Configuration
```bash
router ospf [process-id]
router-id [x.x.x.x]
network [network] [wildcard-mask] area [area-id]
```

## EIGRP Configuration
```bash
router eigrp [AS-number]
no auto-summary
network [network] [wildcard-mask]
eigrp router-id [x.x.x.x]
```

## BGP Configuration
```bash
router bgp [AS-number]
bgp router-id [x.x.x.x]
neighbor [ip-address] remote-as [AS-number]
network [network] mask [subnet-mask]
```

## Verification Commands

### All Protocols
```bash
show ip route
show ip protocols
show running-config
```

### RIP Specific
```bash
show ip rip database
debug ip rip
```

### OSPF Specific
```bash
show ip ospf neighbor
show ip ospf database
show ip ospf interface brief
debug ip ospf adj
```

### EIGRP Specific
```bash
show ip eigrp neighbors
show ip eigrp topology
show ip eigrp interfaces
debug eigrp packets
```

### BGP Specific
```bash
show ip bgp summary
show ip bgp
show ip bgp neighbors
debug ip bgp updates
```

## Protocol Comparison Table

| Feature | RIP | OSPF | EIGRP | BGP |
|---------|-----|------|-------|-----|
| Type | Distance Vector | Link State | Advanced DV | Path Vector |
| Metric | Hop Count | Cost | Composite | AS Path |
| Max Hops | 15 | Unlimited | 224 | Unlimited |
| Update Time | 30 sec | Triggered | Triggered | Triggered |
| Algorithm | Bellman-Ford | Dijkstra | DUAL | Path Selection |
| Convergence | Slow | Fast | Very Fast | Slow |
| Use Case | Small Networks | Enterprise | Enterprise | Internet |

## Administrative Distances

| Protocol | AD |
|----------|-----|
| Connected | 0 |
| Static | 1 |
| EIGRP | 90 |
| OSPF | 110 |
| RIP | 120 |
| External EIGRP | 170 |
| iBGP | 200 |
| eBGP | 20 |

## Network Design Tips

1. **Small Networks (< 15 routers)**
   - RIP is simple and sufficient
   - Easy to configure and troubleshoot

2. **Medium Networks (15-50 routers)**
   - OSPF provides better scalability
   - Use areas for hierarchy

3. **Large Enterprise Networks**
   - OSPF with multiple areas
   - EIGRP for Cisco-only environments
   - Consider redistribution between protocols

4. **Internet/ISP Networks**
   - BGP for external routing
   - OSPF/IS-IS for internal routing
   - Careful policy configuration

## Teaching Progression

1. **Week 1: Static Routing**
   - Understand routing tables
   - Manual route configuration
   - Default routes

2. **Week 2: RIP**
   - Distance vector basics
   - Simple configuration
   - Limitations

3. **Week 3: OSPF**
   - Link state concepts
   - Single area OSPF
   - Multi-area OSPF

4. **Week 4: EIGRP**
   - Hybrid approach
   - Advanced features
   - Cisco proprietary

5. **Week 5: BGP**
   - Inter-AS routing
   - Policy routing
   - Internet routing

6. **Week 6: Redistribution**
   - Multi-protocol networks
   - Metric conversion
   - Route filtering

## Lab Assessment Checklist

### Student Can:
- [ ] Configure IP addresses correctly
- [ ] Enable routing protocols
- [ ] Verify neighbor relationships
- [ ] Check routing tables
- [ ] Test end-to-end connectivity
- [ ] Troubleshoot common issues
- [ ] Document configurations
- [ ] Explain protocol differences
- [ ] Choose appropriate protocol for scenario
- [ ] Implement redistribution

## Common Mistakes to Avoid

1. **Wrong Network Statements**
   - RIP uses classful networks
   - OSPF uses wildcard masks
   - BGP uses subnet masks

2. **Interface Issues**
   - Forgetting "no shutdown"
   - Wrong IP addresses
   - Mismatched subnets

3. **Protocol Mismatches**
   - Different AS numbers
   - Wrong area IDs
   - Incompatible versions

4. **Redistribution Errors**
   - Missing metrics
   - Route loops
   - Suboptimal paths

## Practice Scenarios

1. **Basic Connectivity**
   - 3 routers in a line
   - Single protocol
   - Test connectivity

2. **Redundant Paths**
   - Add backup links
   - Test failover
   - Compare convergence

3. **Multi-Protocol**
   - Different domains
   - Redistribution
   - End-to-end testing

4. **Internet Simulation**
   - Multiple AS networks
   - BGP peering
   - Policy routing
