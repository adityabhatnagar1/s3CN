# Experiment - 9

# Experiment 9: Connecting a Switch

## Aim

Connect the customer switch to the LAN and verify its existing configuration and IP connectivity to the customer router.

## Concepts

- **Straight-through cable**: Connects unlike devices, such as a switch to a router or PC.
- **Console access**: Local terminal connection for out-of-band switch management.
- **Running configuration**: Active settings in RAM, displayed by `show running-config`.
- **VLAN1 management IP**: Logical switch interface used for in-band management.
- **Enable secret**: Encrypted privileged EXEC password that overrides enable password.

## Topology & Addressing

Customer PC, Local Server, and Customer Router connect to Customer Switch; ISP PC, ISP Router, ISP Switch, and ISP Server form the ISP side.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Customer Router | FastEthernet0/0 | 192.168.1.1 | 255.255.255.0 | N/A |
| Customer Switch | VLAN1 | 192.168.1.5 | 255.255.255.0 | verify in lab |
| Customer PC | NIC | verify in lab | verify in lab | verify in lab |
| Local Server | NIC | verify in lab | verify in lab | verify in lab |
| ISP PC | NIC | verify in lab | verify in lab | verify in lab |
| ISP Router | verify in lab | verify in lab | verify in lab | verify in lab |
| ISP Switch | verify in lab | verify in lab | verify in lab | verify in lab |
| ISP Server | NIC | verify in lab | verify in lab | verify in lab |

## Procedure

1. Connect Customer Router FastEthernet0/0 to Customer Switch FastEthernet0/1 using a straight-through cable.
⚠️ Note: In Packet Tracer, use Copper Straight-Through cable. Modern switches auto-detect, but straight-through matches the original wiring.
Why: This links the router LAN interface to the switch.
2. Connect Customer PC to Customer Switch FastEthernet0/2 using a straight-through cable.
Why: This connects the workstation to the LAN.
3. Connect Local Server to Customer Switch FastEthernet0/3 using a straight-through cable.
Why: This connects the server to the LAN.
4. From Customer PC, open terminal emulation to the switch console. Use cisco as the console password.
⚠️ Note: In Packet Tracer, open Terminal from PC Desktop. On Windows 11, use PuTTY or Tera Term with serial 9600-8-N-1.
Why: Console access lets you verify the switch without network connectivity.
5. Enter privileged EXEC mode with password cisco123.

```
enable          ! Enter privileged EXEC mode
! Password: cisco123
```

Why: Privileged EXEC mode allows show running-config.

1. Verify the running configuration for required settings.

```
show running-config   ! Display active configuration
```

Why: Confirms VLAN1 IP, subnet mask, console/vty passwords, enable password, and enable secret.

1. Verify IP connectivity to the customer router.

```
ping 192.168.1.1   ! Test reachability to router LAN interface
```

Why: Confirms the switch can reach the router on the local subnet.

1. Click Check Results in the Packet Tracer instruction window.
Why: Confirms graded items are complete.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `show running-config` | VLAN1 IP 192.168.1.5, mask 255.255.255.0 |
| `show running-config` | Console password required, vty password required |
| `show running-config` | Enable password and enable secret configured |
| `ping 192.168.1.1` from switch CLI | ICMP replies from 192.168.1.1 |
| **Check Results** button | All graded items complete |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| No console prompt | Wrong cable or port | Use light blue console cable to Console port |
| Cannot enter privileged mode | Wrong password | Use cisco123 |
| Ping fails | VLAN1 down or wrong IP | Check `show ip interface brief` and verify 192.168.1.5 |
| `show running-config` missing passwords | Configuration absent | Reapply passwords from Experiment 6 |
| Check Results fails | Cabling or settings incomplete | Verify Fa0/1, Fa0/2, Fa0/3 and all passwords |

## Viva Q&A

**Q1: What is the significance of the enable secret command compared to the enable password?**
A1: Enable secret is encrypted and takes precedence; enable password is plain text.

**Q2: If you want to remove the requirement to enter a password to access the console, what commands do you issue from privileged EXEC mode?**
A2: `configure terminal`, `line console 0`, `no password`, `no login`, `end`.

## Result

The switch is connected to the customer LAN, and its configuration and connectivity to 192.168.1.1 are verified.

## Submission Checklist

- [ ]  Customer Router Fa0/0 connected to Customer Switch Fa0/1
- [ ]  Customer PC connected to Customer Switch Fa0/2
- [ ]  Local Server connected to Customer Switch Fa0/3
- [ ]  Console session opened with password cisco
- [ ]  Privileged EXEC entered with password cisco123
- [ ]  `show running-config` confirms VLAN1 IP 192.168.1.5/24
- [ ]  `show running-config` confirms console, vty, enable password, and enable secret
- [ ]  Ping to 192.168.1.1 succeeds
- [ ]  Check Results button clicked