# Experiment - 6

# Experiment 6: Performing an Initial Switch Configuration

## Aim

Configure a Cisco Catalyst 2960 switch with hostname, passwords, management IP, and default gateway, then verify reachability to a remote server.

## Concepts

- **Hostname**: A unique device name used for identification on the network.
- **Enable password vs. enable secret**: `enable secret` stores an encrypted password and takes precedence over `enable password`.
- **Console and vty lines**: Console is local physical access; vty lines are for remote Telnet/SSH access.
- **VLAN1 interface**: A logical management interface on the default VLAN, not a physical Fast Ethernet port.
- **Default gateway**: The next-hop IP for traffic destined outside the local subnet.

## Topology & Addressing

The Customer PC connects to the Customer Switch console port. The switch connects to the Customer Router, which links to the ISP Router. The ISP Router connects to the ISP Switch, which serves the Local Server and ISP Server. The VSP Workstation connects to the Customer Router.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Customer Switch | VLAN1 | 192.168.1.5 | 255.255.255.0 | 192.168.1.1 |
| ISP Server | NIC | 209.165.201.10 | verify in lab | verify in lab |

⚠️ Note: The 2960-24TT has 24 Fast Ethernet ports and 2 Gigabit Ethernet uplink ports. Verify the exact model in your Packet Tracer instance.

## Procedure

1. Connect the Customer PC to the Customer Switch console port using a console cable. Open the Terminal application on the PC.

⚠️ Note: In Packet Tracer, select the light blue console cable, connect to the PC's RS-232 port and the switch's Console port. Open Terminal from the PC's Desktop tab. Accept default settings (9600 baud, 8 data bits, no parity, 1 stop bit).

Why: Console access provides out-of-band management when no IP is configured.

1. Enter privileged EXEC mode and global configuration mode.

```
enable                      ! Enter privileged EXEC mode
configure terminal          ! Enter global configuration mode
```

Why: Privileged mode allows all show and configuration commands.

1. Set the hostname to CustomerSwitch.

```
hostname CustomerSwitch
```

Why: A unique hostname identifies the device in prompts and network maps.

1. Configure the privileged EXEC password and secret.

```
enable password cisco       ! Sets unencrypted privileged password
enable secret cisco123      ! Sets encrypted privileged password (takes precedence)
```

Why: The enable secret encrypts the password and overrides enable password.

1. Configure the console line password.

```
line console 0              ! Enter console line configuration
password cisco              ! Set console password
login                       ! Require password at console login
exit
```

Why: Prevents unauthorized local access via the console port.

1. Configure the vty line password for remote access.

```
line vty 0 15               ! Enter vty lines 0 through 15
password cisco              ! Set vty password
login                       ! Require password at vty login
exit
```

Why: vty lines control Telnet and SSH sessions to the switch.

1. Assign an IP address to interface VLAN1.

```
interface vlan 1            ! Enter VLAN1 interface configuration
ip address 192.168.1.5 255.255.255.0
no shutdown                 ! Activate the interface
exit
```

Why: The VLAN1 interface is the switch's management IP for remote access.

1. Configure the default gateway.

```
ip default-gateway 192.168.1.1
```

Why: The switch uses this gateway to reach subnets other than 192.168.1.0/24.

1. Return to privileged EXEC mode and verify connectivity.

```
end
ping 209.165.201.10
```

Why: The ping tests whether the switch can reach a remote IP through the gateway.

⚠️ Note: The first one or two pings may fail while ARP converges. A 60% success rate (3/5) is acceptable.

1. Click the **Check Results** button in the Packet Tracer instruction window.

⚠️ Note: Not all commands are graded by Packet Tracer. Verify configuration manually with `show running-config`.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `ping 209.165.201.10` from CustomerSwitch | 3/5 or 5/5 ICMP replies (first pings may time out) |
| `show running-config` | Displays hostname, enable secret, line passwords, VLAN1 IP, and default gateway |
| `show ip interface brief` | VLAN1 shows 192.168.1.5 with status up/up |
| **Check Results** button in Packet Tracer | All graded items show green checkmarks |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Terminal shows no output | Console cable not connected or wrong port | Connect light blue console cable to RS-232 and Console ports |
| `ping` fails completely | VLAN1 shutdown or wrong IP | Run `no shutdown` under interface vlan 1 and verify IP with `show ip interface brief` |
| Enable secret not accepted | Password shorter than minimum length | Use at least 8 characters for `enable secret` |
| Remote access denied | `login` missing on vty lines | Enter `login` under `line vty 0 15` |
| Switch cannot ping remote server | Default gateway missing or incorrect | Verify `ip default-gateway 192.168.1.1` in running-config |

## Viva Q&A

**Q1: What is the significance of assigning the IP address to the VLAN1 interface instead of any Fast Ethernet interface?**

A1: VLAN1 is a logical management interface; physical switch ports are Layer 2 and cannot hold an IP for management.

**Q2: What command is necessary to enforce password authentication on the console and vty lines?**

A2: The `login` command under line configuration mode.

**Q3: How many gigabit ports are available on the Cisco Catalyst 2960 switch used in this activity?**

A3: The 2960-24TT model has 2 Gigabit Ethernet uplink ports.

**Q4: What is the difference between `enable password` and `enable secret`?**

A4: `enable password` is stored in plain text; `enable secret` is encrypted and takes precedence when both are configured.

**Q5: Why is `no shutdown` required on interface VLAN1?**

A5: By default, VLAN1 may be administratively down; `no shutdown` activates it for management traffic.

**Q6: What is the purpose of the default gateway on a Layer 2 switch?**

A6: It allows the switch's management IP to communicate with devices on other subnets.

## Result

The Customer Switch is configured with hostname CustomerSwitch, console/vty/enable passwords, management IP 192.168.1.5/24 on VLAN1, and default gateway 192.168.1.1, and it successfully pings the ISP Server at 209.165.201.10.

## Submission Checklist

- [ ]  Console connection established and terminal session active
- [ ]  Hostname set to CustomerSwitch
- [ ]  Enable password (cisco) and enable secret (cisco123) configured
- [ ]  Console password (cisco) and login enabled
- [ ]  vty password (cisco) and login enabled
- [ ]  VLAN1 IP address 192.168.1.5/24 configured and no shutdown applied
- [ ]  Default gateway 192.168.1.1 configured
- [ ]  Ping to 209.165.201.10 successful (3/5 or better)
- [ ]  Check Results button clicked and verified