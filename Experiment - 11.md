# Experiment - 11

# Experiment 11: Using the Cisco IOS Show Commands

## Aim

Use Cisco IOS show commands on an ISP router to inspect ARP, flash, routes, interfaces, protocols, users, and version. This activity is not graded.

## Concepts

- **User EXEC**: Limited mode with `>` prompt for basic monitoring.
- **Privileged EXEC**: Full mode with `#` prompt, required for detailed show commands.
- **Show commands**: Display device state, configuration, and statistics.
- **ARP**: Maps IP addresses to MAC addresses.
- **Flash**: Non-volatile storage that holds the Cisco IOS image.

## Topology & Addressing

ISP PC connects to Switch0, which connects to ISP Router and ISP Server.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| ISP Router | verify in lab | verify in lab | verify in lab | verify in lab |
| ISP PC | NIC | verify in lab | verify in lab | verify in lab |
| Switch0 | verify in lab | verify in lab | verify in lab | verify in lab |
| ISP Server | NIC | verify in lab | verify in lab | verify in lab |

## Procedure

1. From ISP PC, open terminal emulation to the ISP Router.
⚠️ Note: In Packet Tracer, open Terminal from the ISP PC Desktop. On Windows 11, use PuTTY or Tera Term with serial 9600-8-N-1.
Why: Console access provides local management without network connectivity.
2. Enter privileged EXEC mode.

```
enable   ! Enter privileged EXEC mode
```

Why: Privileged EXEC mode allows all show commands used in this activity.

1. Run each show command and read the output.

```
show arp          ! Display IP-to-MAC address mappings
show flash        ! Display flash memory size and contents
show ip route     ! Display the routing table
show interfaces   ! Display interface status and statistics
show protocols    ! Display routed protocol status per interface
show users        ! Display active terminal sessions
show version      ! Display IOS version, uptime, and hardware
```

Why: Each command reveals a different part of router operation.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `show arp` | Lists IP-to-MAC mappings |
| `show flash` | Reports flash memory size and files |
| `show ip route` | Lists known networks and next hops |
| `show interfaces` | Shows up/down status and counters |
| `show protocols` | Shows routed protocols and interface states |
| `show users` | Lists active lines and users |
| `show version` | Shows IOS version, uptime, and hardware |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| No terminal prompt | Wrong cable or port | Connect console cable to Console port and ISP PC |
| `enable` rejected | No password set or wrong password | Press Enter; if prompted, use lab password |
| Show command not recognized | Typo or wrong IOS mode | Re-enter command in privileged EXEC mode |
| Output scrolls past screen | Long output | Press Space to page, or use `show ... |
| Cannot see interface status | Interface shut down | Check `show interfaces` and `no shutdown` if needed |

## Viva Q&A

**Q1: Why do you need to be in privileged EXEC mode to explore the Cisco IOS show commands used in this activity?**
A1: Privileged EXEC mode permits detailed operational and configuration output.

**Q2: How much flash memory is reported?**
A2: Verify in lab from the `show flash` output.

**Q3: Which of the following is subnetted? 209.165.201.0, 209.165.201.1, 209.165.201.10**
A3: 209.165.201.0 is the network; verify subnet mask in lab from `show ip route`.

**Q4: Which interface is up and running? Serial0/1/0, FastEthernet0/1, FastEthernet0/0, VLAN1**
A4: Verify in lab from `show interfaces` or `show ip interface brief`.

## Result

The ISP router's show command output was inspected for ARP, flash, routes, interfaces, protocols, users, and version.

## Submission Checklist

- [ ]  Connected to ISP Router from ISP PC
- [ ]  Entered privileged EXEC mode
- [ ]  Ran `show arp`
- [ ]  Ran `show flash`
- [ ]  Ran `show ip route`
- [ ]  Ran `show interfaces`
- [ ]  Ran `show protocols`
- [ ]  Ran `show users`
- [ ]  Ran `show version`
- [ ]  Answered all reflection questions