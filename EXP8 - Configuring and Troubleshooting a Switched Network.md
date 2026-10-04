# Experiment - 8

# Experiment 8: Configuring and Troubleshooting a Switched Network

## Aim

To configure basic switch and router management: console access, host name, VLAN 1, clock, passwords, banners, duplex/speed, port security, unused ports, and configuration backup. Tool: Cisco Packet Tracer.

## Concepts

- **Console session:** A direct text connection to a device's CLI (Command Line Interface). Here PC1 reaches S1 through a console cable and terminal window.
- **VLAN 1 interface:** A VLAN (Virtual LAN) is a logical network group. The VLAN 1 interface gives the switch its management IP; a default gateway lets it reach other networks.
- **Duplex and speed mismatch:** Duplex is send/receive in both directions at once. If the two ends of a link differ in duplex or speed, the link works badly or not at all.
- **Port security:** Limits which MAC addresses (hardware addresses) may use a port. A "sticky" MAC is learned automatically and saved in the running configuration.
- **err-disabled:** A port automatically shut down after a security violation. It stays down until re-enabled manually.

## Topology & Addressing

PC1 (Fa0/18), Server, and R1 (1841) connect to S1 (2960); PC2 replaces PC1 on Fa0/18 during the port security test.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| R1 | Fa0/0 | 172.17.99.1 | 255.255.255.0 | N/A |
| S1 | VLAN 1 | 172.17.99.11 | 255.255.255.0 | 172.17.99.1 |
| PC1 | NIC | 172.17.99.21 | 255.255.255.0 | 172.17.99.1 |
| PC2 | NIC | 172.17.99.22 | 255.255.255.0 | 172.17.99.1 |
| Server | NIC | 172.17.99.31 | 255.255.255.0 | 172.17.99.1 |

⚠️ Note: The raw table lists S1's address under Fa0/1, but it is set on the VLAN 1 interface. Gateways for hosts are not given; 172.17.99.1 is assumed. The diagram does not show clearly whether Fa0/1 or Fa0/24 goes to R1 or Server; verify in lab. If R1 Fa0/0 has no address, set it and run `no shutdown`; verify in lab.

## Procedure

1. Connect a console cable from PC1 (RS 232 port) to S1 (Console port). On PC1 open Desktop > Terminal and click OK with default settings.
Why: S1's Config and CLI tabs are disabled in this activity.
2. Configure the host name, Fa0/1, VLAN 1, and default gateway on S1.

```
   Switch>enable                                   ! privileged EXEC mode
   Switch#configure terminal                       ! global configuration mode
   Switch(config)#hostname S1                      ! set host name
   S1(config)#interface fastethernet 0/1           ! select port Fa0/1
   S1(config-if)#switchport mode access            ! set port to access mode
   S1(config-if)#exit
   S1(config)#interface vlan 1                     ! select management interface
   S1(config-if)#ip address 172.17.99.11 255.255.255.0 ! set IP and mask
   S1(config-if)#no shutdown                       ! turn interface on
   S1(config-if)#exit
   S1(config)#ip default-gateway 172.17.99.1       ! set gateway to R1
   S1(config)#end
```

Why: A switch needs an IP and gateway to be managed and to reach other networks.
3. Test connectivity from S1 to R1.

```
   S1#ping 172.17.99.1    ! S1 should reach R1
```

Why: Confirms VLAN 1 is active and the gateway is reachable.
4. Use Help to set the clock, then verify it.

```
   S1#clock ?                              ! list clock options
   S1#clock set ?                          ! shows the time format: hh:mm:ss
   S1#clock set 10:30:00 4 October 2026    ! set time, day, month, year
   S1#show clock                           ! verify the time
```

Why: `?` shows each required argument step by step.
⚠️ Note: Packet Tracer may not simulate the entered time correctly and does not grade this step.
5. Configure passwords on S1.

```
   S1#configure terminal
   S1(config)#enable secret class            ! encrypted privileged password
   S1(config)#line console 0                 ! select console line
   S1(config-line)#password cisco            ! console password
   S1(config-line)#login                     ! require password at login
   S1(config-line)#exit
   S1(config)#line vty 0 15                  ! select all Telnet lines
   S1(config-line)#password cisco            ! Telnet password
   S1(config-line)#login                     ! require password at login
   S1(config-line)#exit
```

Why: `secret` is stored hashed; `login` makes the line ask for the password.
6. View the configuration, then encrypt the line passwords.

```
   S1(config)#do show running-config         ! line passwords appear in clear text
   S1(config)#service password-encryption    ! scramble clear-text passwords
```

Why: Clear-text passwords can be read by anyone viewing the configuration.
7. Configure the banner on S1.

```
   S1(config)#banner motd $Authorized Access Only$   ! no period, no extra spaces
   S1(config)#end
```

Why: Packet Tracer grades the text exactly, and it is case-sensitive.
8. Click R1 to open its CLI and configure it with the same basic commands.

```
   Router>enable
   Router#configure terminal
   Router(config)#hostname R1                        ! set host name
   R1(config)#enable secret class                    ! encrypted privileged password
   R1(config)#line console 0
   R1(config-line)#password cisco                    ! console password
   R1(config-line)#login
   R1(config-line)#exit
   R1(config)#line vty 0 4                           ! Telnet lines on a router
   R1(config-line)#password cisco                    ! Telnet password
   R1(config-line)#login
   R1(config-line)#exit
   R1(config)#service password-encryption            ! encrypt line passwords
   R1(config)#banner motd $Authorized Access Only$   ! login banner
   R1(config)#end
```

Why: Routers and switches share these commands.
9. Fix the duplex and speed mismatch on the PC1 and Server ports of S1.

```
   S1#configure terminal
   S1(config)#interface range fastethernet 0/1 , fastethernet 0/18  ! Server and PC1 ports (verify in lab)
   S1(config-if-range)#speed auto       ! negotiate speed
   S1(config-if-range)#duplex auto      ! negotiate duplex
   S1(config-if-range)#end
```

Why: Both ends must agree on speed and duplex.
⚠️ Note: The raw material does not state the required values. If auto does not clear the problem, set a fixed `speed 100` and `duplex full`; verify in lab.
10. Verify that PC1 and Server can ping S1, R1, and each other.
Why: Confirms the mismatch is solved.
11. Enable port security on the PC1 port.

```
    S1#configure terminal
    S1(config)#interface fastethernet 0/18
    S1(config-if)#switchport mode access                   ! port security needs an access port
    S1(config-if)#switchport port-security                 ! enable port security
    S1(config-if)#switchport port-security maximum 1       ! allow one MAC address
    S1(config-if)#switchport port-security mac-address sticky ! keep first learned MAC
    S1(config-if)#end
```

```
Why: Only the learned device may use this port.
```

1. Verify port security on Fa0/18.

```
    S1#show port-security interface fastethernet 0/18   ! shows status, mode, MAC count
```

```
Why: Before any traffic, Total MAC Addresses is 0.
```

1. Send a ping from PC1 to S1, then check the running configuration on S1.

```
    S1#show running-config   ! look under FastEthernet0/18 for the sticky MAC line
```

```
Why: The ping makes S1 learn PC1's MAC address.
```

1. Remove the Fa0/18 cable from PC1, connect PC2 to Fa0/18, wait for green link lights, and ping S1 from PC2 if needed.
Why: A different MAC address triggers a violation.
2. Check the violation results.

```
    S1#show port-security interface fastethernet 0/18   ! status Secure-shutdown
    S1#show interface fa0/18                            ! down, line protocol down (err-disabled)
```

```
Why: Both outputs confirm the port was shut down.
```

1. Disconnect PC2, reconnect PC1 to Fa0/18, then re-enable the port.

```
    S1#configure terminal
    S1(config)#interface fastethernet 0/18
    S1(config-if)#shutdown       ! reset the err-disabled state
    S1(config-if)#no shutdown    ! bring the port back up
    S1(config-if)#end
```

```
Why: An err-disabled port does not recover by itself.
⚠️ Note: The raw material gives only `no shutdown`; in real IOS, `shutdown` first is often required. Verify in lab.
```

1. Disable all unused ports on S1.

```
    S1#configure terminal
    S1(config)#interface range fastethernet 0/2 - 17    ! unused Fast Ethernet ports
    S1(config-if-range)#shutdown
    S1(config-if-range)#exit
    S1(config)#interface range fastethernet 0/19 - 23   ! more unused ports
    S1(config-if-range)#shutdown
    S1(config-if-range)#exit
    S1(config)#interface range gigabitethernet 0/1 - 2  ! gigabit uplink ports
    S1(config-if-range)#shutdown
    S1(config-if-range)#end
```

```
Why: Open unused ports let unauthorised devices connect.
⚠️ Note: The raw material names Gig 1/1 and Gig 1/2. The Packet Tracer 2960-24TT labels them Gig0/1 and Gig0/2; check names with `show ip interface brief`. Do not shut down Fa0/24 if it connects to R1 or Server; verify in lab.
```

1. Save the configuration on S1 and R1 to NVRAM.

```
    S1#copy running-config startup-config   ! repeat the same command on R1
```

```
Why: Unsaved changes are lost on reload.
```

1. Back up each startup configuration to Server (TFTP, a file transfer protocol), then check the Server files.

```
    S1#copy startup-config tftp          ! repeat on R1
    Address or name of remote host []? 172.17.99.31   ! Server IP
    Destination filename [S1-confg]?     ! press Enter; R1 uses R1-confg
```

```
Why: A backup allows recovery after a failure.
Then open Server > Services > TFTP and confirm `S1-confg` and `R1-confg` are listed.
```

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Check Results after steps 1, 3, 6, 7, 8, 10 | 8%, 31%, 42%, 46%, 65%, 73% |
| `ping 172.17.99.1` (S1) | Replies from R1 |
| `show clock` | Clock shows the time set (Packet Tracer may not keep it correctly) |
| `show running-config` (after step 6) | Line passwords encrypted; enable secret shown as a hash |
| `show port-security interface fa0/18` (step 12) | Enabled, Secure-up, Shutdown mode, Maximum 1, Total MAC 0 |
| Same command after step 15 | Secure-shutdown, Total MAC 1, Violation Count 1; Last Source Address may differ |
| `show interface fa0/18` (step 15) | down, line protocol down (err-disabled) |
| Check Results after steps 16, 17, 19 | 77%, 96%, 100% |
| Server TFTP file list | `S1-confg` and `R1-confg` present |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| S1 cannot ping R1 | VLAN 1 down, wrong IP, or no gateway | Run `no shutdown` on VLAN 1; recheck IP and gateway. |
| Banner not graded | Text differs in case, spaces, or period | Re-enter exactly: `Authorized Access Only`. |
| PC1 or Server cannot ping S1 | Speed/duplex mismatch | Set `speed auto` and `duplex auto` on the port. |
| Fa0/18 stays down after PC1 returns | Port is err-disabled | Remove PC2; run `shutdown`, then `no shutdown`. |
| TFTP backup fails | Wrong Server IP or TFTP service off | Use 172.17.99.31; check Server > Services > TFTP. |

## Viva Q&A

1. **Q:** Which command generated the Fa0/18 port security output? **A:** `show port-security interface fastethernet 0/18`.
2. **Q:** What does the sticky option do? **A:** It saves the first learned MAC address in the running configuration.
3. **Q:** What does err-disabled mean? **A:** The port was shut down after a security violation.
4. **Q:** Why does S1 need a default gateway? **A:** To reach devices on other networks.
5. **Q:** What causes a duplex mismatch? **A:** The two link ends use different duplex or speed settings.
6. **Q:** Why use `enable secret` rather than `enable password`? **A:** It is stored hashed and cannot be read directly.

## Result

S1 and R1 were configured, secured, backed up to the Server, and verified, and Check Results reached 100%.

## Submission Checklist

- [ ]  Console connection and topology screenshot
- [ ]  S1 host name, VLAN 1, and gateway with ping to R1
- [ ]  `show clock` output
- [ ]  Passwords, encryption, and banner on S1 and R1
- [ ]  Duplex/speed fix and connectivity pings
- [ ]  Port security outputs: before, sticky MAC, violation, err-disabled
- [ ]  Port re-enabled and unused ports shut down
- [ ]  Server TFTP file list showing `S1-confg` and `R1-confg`
- [ ]  Check Results screenshot at 100%
- [ ]  Viva answers written