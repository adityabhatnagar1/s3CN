# Experiment - 2

# Study and Implementation of Network Devices

## Aim

Study and implement repeater, hub, switch, bridge, router, and gateway in Cisco Packet Tracer on Windows 11.

## Concepts

- **Repeater & Hub (Layer 1):** Regenerate signals; hub is a multiport repeater that floods to all ports.
- **Switch & Bridge (Layer 2):** Forward frames based on MAC addresses; bridge is a two-port switch.
- **Router (Layer 3):** Forwards packets between different IP networks using a routing table.
- **Gateway:** Translates between different protocols or network architectures.
- **Collision domain:** Repeater and hub share one domain; switch and bridge create separate domains per port.

## Topology & Addressing

Each device is tested with two or more PCs in a single network; the router scenario uses two networks.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| PC1 | FastEthernet0 | 192.168.1.1 | 255.255.255.0 | 192.168.1.254 |
| PC2 | FastEthernet0 | 192.168.1.2 | 255.255.255.0 | 192.168.1.254 |
| PC3 | FastEthernet0 | 192.168.1.3 | 255.255.255.0 | 192.168.1.254 |
| Router | Gig0/0 | 192.168.1.254 | 255.255.255.0 | - |
| Router | Gig0/1 | 192.168.2.254 | 255.255.255.0 | - |

## Procedure

1. **Repeater:** Place two PCs and one Repeater-PT. Connect PC1 to port 0 and PC2 to port 1 using Copper Straight-Through cable.
Why: A repeater has two ports and regenerates the signal between them.
2. Assign IP addresses 192.168.1.1 and 192.168.1.2 to PC1 and PC2 via Desktop > IP Configuration.
Why: Both PCs must be on the same subnet to communicate.
3. Open Command Prompt on PC1 and run `ping 192.168.1.2`.
Why: Verifies signal regeneration through the repeater.
4. **Hub:** Place three PCs and one Hub-PT. Connect all PCs to the hub using Copper Straight-Through cable.
Why: A hub is a multiport repeater that connects multiple devices.
5. Assign IP addresses 192.168.1.1, 192.168.1.2, and 192.168.1.3 to PC1, PC2, and PC3.
Why: All devices share the same network segment.
6. Enter Simulation mode, ping from PC1 to PC2, and observe PC3 also receives the packet.
Why: A hub floods frames to all ports except the incoming port.
7. **Switch:** Place three PCs and one Switch-PT. Connect all PCs to the switch.
Why: A switch forwards frames only to the destination MAC address.
8. Assign the same IP addresses as in step 5. Ping from PC1 to PC2 in Simulation mode.
Why: PC3 should not receive the packet, unlike with a hub.
9. **Bridge:** Place two PCs and one Bridge-PT. Connect PC1 to port 1 and PC2 to port 2.
Why: A bridge connects two network segments at Layer 2.
10. Assign IP addresses 192.168.1.1 and 192.168.1.2. Ping from PC1 to PC2.
Why: Verifies Layer 2 forwarding across the bridge.
11. **Router:** Place two PCs and one Router-PT. Connect PC1 to Gig0/0 and PC2 to Gig0/1 using Copper Straight-Through cable.
Why: A router connects different IP networks.
12. Configure the router interfaces:

```
Router> enable                      ! Enter privileged mode
Router# configure terminal          ! Enter global configuration
Router(config)# interface gig0/0    ! Select first interface
Router(config-if)# ip address 192.168.1.254 255.255.255.0
Router(config-if)# no shutdown      ! Enable interface
Router(config-if)# exit
Router(config)# interface gig0/1    ! Select second interface
Router(config-if)# ip address 192.168.2.254 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# end
Router# write memory                ! Save configuration
```

```
Why: Assigns IP addresses and enables both router interfaces.
```

1. Set PC1 IP to 192.168.1.1 with gateway 192.168.1.254. Set PC2 IP to 192.168.2.1 with gateway 192.168.2.254.
Why: PCs need the router as gateway to reach other networks.
2. Ping from PC1 to PC2.
Why: Verifies inter-network routing.
3. **Gateway:** Place one Home Gateway (Wireless Devices > Home Gateway) and one PC. Connect PC to the gateway's Ethernet port.
Why: A gateway translates between different protocols.
4. Assign PC an IP in the same subnet as the gateway and ping the gateway.
Why: Verifies gateway connectivity.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Ping PC1 to PC2 through repeater | Successful |
| Ping PC1 to PC2 through hub (Simulation mode) | Successful; PC3 receives the packet |
| Ping PC1 to PC2 through switch (Simulation mode) | Successful; PC3 does not receive the packet |
| Ping PC1 to PC2 through bridge | Successful |
| Ping PC1 to PC2 through router | Successful with correct gateways |
| Ping PC to Home Gateway | Successful |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Repeater or hub not found | Located under Hubs category | Look in Network Devices > Hubs |
| Cable link light red | Wrong cable type | Use Copper Straight-Through for PC-to-device |
| Ping fails on router | Interface not enabled | Enter `no shutdown` on each interface |
| PC cannot reach other network | Missing default gateway | Set gateway on PC to router interface IP |
| Packet not flooding in hub | Not in Simulation mode | Switch to Simulation mode and capture events |

## Viva Q&A

1. **Q:** At which OSI layer does a repeater operate? **A:** Physical layer (Layer 1).
2. **Q:** What is the main difference between a hub and a switch? **A:** A hub floods frames to all ports; a switch forwards only to the destination port.
3. **Q:** How does a bridge differ from a switch? **A:** A bridge typically has two ports; a switch has many ports.
4. **Q:** What is the function of a router? **A:** It forwards packets between different networks using a routing table.
5. **Q:** What is a gateway? **A:** A device that translates between different protocols or network architectures.
6. **Q:** How many ports does a repeater have? **A:** Two ports.

## Result

All six network devices were studied and implemented in Cisco Packet Tracer, and their forwarding behavior was verified.

## Submission Checklist

- [ ]  Repeater topology with two PCs and successful ping
- [ ]  Hub topology with three PCs and observation of flooding in Simulation mode
- [ ]  Switch topology with three PCs and observation of MAC-based forwarding
- [ ]  Bridge topology with two PCs and successful ping
- [ ]  Router topology with two networks and successful inter-network ping
- [ ]  Gateway topology with Home Gateway and successful ping
- [ ]  Screenshots of each topology
- [ ]  Written answers to Viva Q&A

⚠️ Note: In current Packet Tracer, Repeater-PT and Hub-PT are under **Network Devices > Hubs**. Bridge-PT is under **Network Devices > Switches** (Generic). There is no generic Gateway-PT; use **Router-PT** or **Home Gateway** (Wireless Devices) to simulate gateway functions.