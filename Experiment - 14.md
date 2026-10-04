# Experiment - 14

# Experiment 14: Demonstrating Distribution Layer Functions

## Aim

Demonstrate distribution layer functions by observing EIGRP updates, testing VLAN connectivity, and tracing packets in Packet Tracer Simulation mode.

## Concepts

- **Distribution layer**: Aggregates access switches and applies routing and policy.
- **VLAN**: Logical network segment; different VLANs need Layer 3 routing to communicate.
- **Router subinterface**: Virtual interface that serves one VLAN on a physical router port.
- **EIGRP**: Routing protocol that exchanges route information between routers.
- **Simulation mode**: Packet Tracer mode that steps packets and lists protocol events.

## Topology & Addressing

Topology is defined in the required file; PC0–PC4 connect through switches to a router with subinterfaces.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| PC0 | NIC | verify in lab | verify in lab | verify in lab |
| PC1 | NIC | verify in lab | verify in lab | verify in lab |
| PC2 | NIC | verify in lab | verify in lab | verify in lab |
| PC3 | NIC | verify in lab | verify in lab | verify in lab |
| PC4 | NIC | verify in lab | verify in lab | verify in lab |
| Router | FastEthernet subinterfaces | verify in lab | verify in lab | N/A |

## Procedure

1. Open the required file `Demonstrating Distribution Layer Functions.pka`.
Why: The file contains the prebuilt topology and configurations.
2. Enter Simulation mode in Packet Tracer.
Why: Simulation mode allows event-by-event inspection.
3. Click **Edit Filters**, select **EIGRP**, then click **Reset Simulation**.
Why: Filters the event list to show only EIGRP updates.

⚠️ Note: In current Packet Tracer, EIGRP is under the IPv4 tab in Edit Filters. If it is not visible, click **Show All/None** first.

1. Click **Auto Capture/Play** and observe EIGRP updates.
Why: Runs the simulation and displays routing protocol traffic.

⚠️ Note: The button may be labeled **Play** or **Auto Capture / Play** in newer Packet Tracer versions.

1. Switch to Realtime mode.
Why: Realtime mode tests normal packet forwarding.
2. From PC0, ping PC1, PC2, PC3, and PC4.

```
ping PC1   ! Replace with PC1 IP shown in topology
ping PC2   ! Replace with PC2 IP shown in topology
ping PC3   ! Replace with PC3 IP shown in topology
ping PC4   ! Replace with PC4 IP shown in topology
```

Why: Tests connectivity from PC0 to the other devices.

⚠️ Note: In the Packet Tracer PC Command Prompt, use destination IP addresses, not hostnames, unless DNS is configured. Run `ipconfig` on each PC to find its IP.

1. From PC1, ping PC0, PC2, PC4, and PC3.

```
ping PC0   ! Replace with PC0 IP shown in topology
ping PC2   ! Replace with PC2 IP shown in topology
ping PC4   ! Replace with PC4 IP shown in topology
ping PC3   ! Replace with PC3 IP shown in topology
```

Why: Tests reverse and other connectivity paths.

1. Switch from Realtime mode to Simulation mode.
Why: Prepares Packet Tracer to trace individual PDUs.
2. Create a simple PDU from PC0 to PC1. Click **Capture/Forward** until the PDU completes the trip to PC1 and back.
Why: Traces the full request and reply path.

⚠️ Note: Click the closed envelope icon, then click PC0 and PC1. Repeat **Capture/Forward** until the event list shows the return to PC0.

1. In the event list, view the PDU events.
Why: Shows filtering, routing, and forwarding decisions.
2. Create another simple PDU from PC0 to PC2.
Why: Compares the path to a different destination VLAN.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| EIGRP updates after Auto Capture/Play | EIGRP packets appear in the event list |
| PC0 ping PC1 | Record success or failure for reflection |
| PC1 ping PC0 default gateway | Succeeds, per reflection premise |
| Simulation PDU PC0 to PC1 | Completes round trip or shows where it is dropped |
| Event list for PC0 to PC2 | Displays PDU events for the second path |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| EIGRP filter missing | Packet Tracer filter list hidden | Click Show All/None, then select EIGRP |
| PDU does not move | Not in Simulation mode or no PDU created | Switch to Simulation, create PDU, click Capture/Forward |
| Ping fails | Wrong IP or hostname not resolved | Run `ipconfig` and ping the IP address |
| No EIGRP updates | Routing not configured or filter too narrow | Reset Simulation and check filters |
| Subinterfaces missing | File configuration changed | Reload the original `.pka` file |

## Viva Q&A

**Q1: Why can’t PC0 communicate with PC1 but PC1 can communicate with PC0’s default gateway?**
A1: PC0 likely lacks a route to PC1’s VLAN or is filtered; PC1 reaches the gateway through its own subnet. Verify in lab.

**Q2: What effect on connectivity would removing the subinterfaces have?**
A2: Inter-VLAN routing would stop, so devices on different VLANs could not communicate.

**Q3: Why must a router be in the topology to have communication between the VLANs?**
A3: A router performs Layer 3 routing between separate VLAN subnets.

**Q4: What distribution layer functions are demonstrated?**
A4: Packet filtering and route summarization between VLANs.

**Q5: What is the purpose of Simulation mode?**
A5: It steps packets and lists protocol events for inspection.

**Q6: What does the EIGRP filter show?**
A6: It displays only EIGRP routing updates in the event list.

## Result

EIGRP updates, VLAN connectivity, and packet paths were examined to demonstrate distribution layer functions.

## Submission Checklist

- [ ]  Opened `Demonstrating Distribution Layer Functions.pka`
- [ ]  Entered Simulation mode
- [ ]  Selected EIGRP in Edit Filters
- [ ]  Clicked Reset Simulation
- [ ]  Clicked Auto Capture/Play and observed EIGRP updates
- [ ]  Tested PC0 pings to PC1, PC2, PC3, PC4 in Realtime mode
- [ ]  Tested PC1 pings to PC0, PC2, PC4, PC3 in Realtime mode
- [ ]  Switched to Simulation mode
- [ ]  Created PC0-to-PC1 PDU and clicked Capture/Forward to completion
- [ ]  Viewed PDU events in event list
- [ ]  Created PC0-to-PC2 PDU
- [ ]  Answered reflection questions c, d, and e