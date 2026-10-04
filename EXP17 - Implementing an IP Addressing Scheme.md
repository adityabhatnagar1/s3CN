# Experiment - 17

# Experiment 17: Implementing an IP Addressing Scheme

## Aim

Subnet 192.168.1.0/24 for LAN-A (50 hosts) and LAN-B (40 hosts), assign valid host addresses, configure devices, and verify connectivity.

## Concepts

- Subnetting divides one IP network into smaller networks.
- Host bits are bits left for device addresses; subnet bits are borrowed from the host portion.
- Usable hosts per subnet = 2^host bits - 2.
- Default gateway is the router interface on the same subnet as the host.
- Packet Tracer Config tab generates equivalent IOS commands.

## Topology & Addressing

Customer Router connects LAN-A and LAN-B to an ISP router; ISP side is preconfigured. Configured addressing:

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| HostA | NIC | 192.168.1.62 | 255.255.255.192 | 192.168.1.1 |
| HostB | NIC | 192.168.1.126 | 255.255.255.192 | 192.168.1.65 |
| Customer Router | FastEthernet0/0 | 192.168.1.1 | 255.255.255.192 | — |
| Customer Router | FastEthernet0/1 | 192.168.1.65 | 255.255.255.192 | — |

## Procedure

1. Subnet 192.168.1.0/24. Answer these questions before configuring:
- How many bits must be left for host addresses?
- How many bits can now be taken from the host portion to make a subnet?
- How many hosts does each subnet support?
- How many subnets are created?
- What is the new subnet mask?
Why: determines the mask and subnet ranges.
1. Assign host addresses. Answer these questions:
- What is the subnet address for subnet 0?
- What is the subnet address for subnet 1?
- Assign subnet 0 to LAN-A and subnet 1 to LAN-B.
- What is the first address in subnet 0? Assign it to Customer Router FastEthernet0/0.
- What is the first address in subnet 1? Assign it to Customer Router FastEthernet0/1.
- What is the last address in subnet 0? Assign it to HostA.
- What is the last address in subnet 1? Assign it to HostB.
- What is the default gateway for HostA?
- What is the default gateway for HostB?
Why: maps valid addresses to devices.
1. Configure HostA. Click HostA > Desktop > IP Configuration. Enter HostA IP address, subnet mask, and default gateway from Steps 1–2.
Why: sets host addressing.
2. Configure HostB. Click HostB > Desktop > IP Configuration. Enter HostB IP address, subnet mask, and default gateway from Steps 1–2.
Why: sets host addressing.
3. Check results. On the Assessment Items tab, HostA and HostB should show green checkmarks. If not, read the feedback. If feedback is hidden, drag the right side of the Activity Results window to resize it.
⚠️ Note: If the Assessment Items tab is absent in your Packet Tracer file, verify manually with `ipconfig` and `ping`.
4. Configure Customer Router. Click Customer Router > Config tab. Under Interface, click FastEthernet0/0. Enter the IP address and subnet mask, then set Port Status to On.
Why: activates the LAN-A gateway.
Repeat for FastEthernet0/1.
Why: activates the LAN-B gateway.
5. Observe the Equivalent IOS Commands window. Scroll or resize it to view the generated commands.
Why: shows the CLI equivalent.
Optional CLI:

```
enable
configure terminal
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.192  ! LAN-A gateway
 no shutdown
interface FastEthernet0/1
 ip address 192.168.1.65 255.255.255.192  ! LAN-B gateway
 no shutdown
end
write memory
```

⚠️ Note: In current Packet Tracer, use CLI if the Config tab is unavailable; verify interface names with `show ip interface brief`.

1. Check results. On the Assessment Items tab, Customer Router should show green checkmarks. If not, read the feedback.
Why: confirms router LAN interfaces.
2. Verify connectivity. Use Add Simple PDU to create pings between HostA, HostB, ISP Workstation, and ISP Server. Or click HostA/HostB > Desktop > Command Prompt, then use `ping` to test other devices. To obtain another device IP, hover the mouse pointer over it. Check the Connectivity Tests tab; each test should be successful.
⚠️ Note: If ISP-side devices do not reply, their IPs/routes may be preconfigured; verify in lab.
3. Reflection. Answer:
- How many subnets are still available for future expansion?
- What would be the two subnet addresses if the host requirement was 80 hosts per LAN?
- Challenge: Create your own Packet Tracer network using the same topology, but implement an addressing scheme based on 80 hosts per LAN. Have another student or instructor check your work.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| HostA `ipconfig` | 192.168.1.62 /26, gateway 192.168.1.1 |
| HostB `ipconfig` | 192.168.1.126 /26, gateway 192.168.1.65 |
| Customer Router `show ip interface brief` | Fa0/0 192.168.1.1 up/up; Fa0/1 192.168.1.65 up/up |
| Ping HostA to HostB | Reply |
| Ping HostA to ISP Workstation/Server | Reply if ISP preconfigured; verify in lab |
| Assessment Items/Connectivity Tests | Green checkmarks/successful |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Host cannot ping gateway | Wrong mask/gateway or interface down | Correct host IP/mask/gateway; set router interface On |
| Router interface down | Port Status Off | Set Port Status to On or use `no shutdown` |
| Assessment tab missing | Standalone PT file | Verify manually with `ping` and `ipconfig` |
| Ping to ISP fails | WAN/default route not configured | Verify in lab; do not change ISP without instructions |
| Hosts cannot communicate | Wrong subnet assignment | Use 192.168.1.0/26 for LAN-A and 192.168.1.64/26 for LAN-B |

## Viva Q&A

Q: How many host bits are needed for 50 hosts? A: 6 bits, giving 62 usable hosts.
Q: What is the new subnet mask? A: 255.255.255.192 or /26.
Q: How many subnets are created? A: 4.
Q: What is subnet 1 address? A: 192.168.1.64/26.
Q: What is the default gateway for HostA? A: 192.168.1.1.
Q: How many subnets remain for expansion? A: 2.

## Result

The 192.168.1.0/24 address space is subnetted, hosts and router LAN interfaces are configured, and connectivity is verified.

## Submission Checklist

- [ ]  Subnetting questions answered
- [ ]  HostA and HostB configured
- [ ]  Customer Router Fa0/0 and Fa0/1 configured
- [ ]  Connectivity tested
- [ ]  Reflection and challenge completed
- [ ]  Viva questions answered