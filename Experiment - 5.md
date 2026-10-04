# Experiment - 5

# Experiment 5: Basic Network Commands and Router Configuration Commands

## Aim

To study basic network commands (ping, tracert, nslookup, pathping) and router configuration commands (mode switching, interface setup, saving configuration). Tools: Windows 11 Command Prompt and Cisco Packet Tracer.

## Concepts

- **ICMP and ping:** ICMP (Internet Control Message Protocol) carries test and error messages. `ping` sends an Echo Request; a reply shows the host is alive.
- **Hop, tracert, pathping:** A hop is one router crossed. `tracert` lists each hop and its delay. `pathping` adds packet-loss statistics per hop.
- **DNS and nslookup:** DNS (Domain Name System) maps names to IP addresses. `nslookup` queries a DNS server.
- **IOS modes:** User EXEC (`>`) allows basic checks. Privileged EXEC (`#`) allows full show and copy commands. Global config (`(config)#`) changes settings.
- **Running vs startup config:** Running-config is the active configuration in RAM and is lost on reload. Startup-config is stored permanently and loads at boot.

## Topology & Addressing

PC1 connects to Router Gi0/0 and PC2 connects to Router Gi0/1, forming two networks: 192.168.2.0/24 and 192.168.1.0/24.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| PC1 | FastEthernet0 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| Router | Gi0/0 | 192.168.2.1 | 255.255.255.0 | N/A |
| Router | Gi0/1 | 192.168.1.1 | 255.255.255.0 | N/A |
| PC2 | FastEthernet0 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |

⚠️ Note: The raw material gives only 192.168.1.2 and the first tracert hop 192.168.2.1. The PC1 address, router 192.168.1.1, and masks are assumed; verify in lab. This table uses a 2911 router; an ISR4331 uses Gi0/0/0 and Gi0/0/1.

## Procedure

1. Place PC1, PC2, and one 2911 router. Connect PC1 to Gi0/0 and PC2 to Gi0/1 using the Automatic Connection cable.
Why: Automatic selects the correct cable type.
2. Set each PC's IP, mask, and gateway from the table (Desktop > IP Configuration).
Why: PCs need a gateway to reach the other network.
3. Open the router CLI and configure both interfaces.

```
   enable                              ! user EXEC to privileged EXEC
   configure terminal                  ! enter global config mode
   interface gigabitEthernet 0/0       ! select first interface
   ip address 192.168.2.1 255.255.255.0 ! set IP and mask
   no shutdown                         ! turn the port on
   exit                                ! back to global config
   interface gigabitEthernet 0/1
   ip address 192.168.1.1 255.255.255.0
   no shutdown
   end                                 ! return to privileged EXEC
```

Why: Router ports are shut down by default.
4. Practise help and abbreviations.

```
   Router>?                ! list commands in current mode
   Router#co?              ! list commands starting with "co" (no space before ?)
   Router#configure ?      ! list arguments (space before ?)
   Router#sh version       ! "sh" abbreviates "show"
```

Why: IOS accepts any unique prefix of a command.
⚠️ Note: `configure ?` may list more options than memory, network, and terminal in current IOS; verify in lab.
5. Switch between modes.

```
   Router# configure terminal   ! privileged EXEC to global config
   Router(config)# exit         ! back to privileged EXEC
   Router# disable              ! back to user EXEC
   Router> enable               ! return to privileged EXEC
```

Why: The prompt shows the current mode and which commands are allowed.
6. View, save, and confirm the configuration.

```
   show running-config                    ! active configuration (RAM)
   copy running-config startup-config     ! save to permanent memory
   show startup-config                    ! confirm saved configuration
```

Why: Unsaved changes are lost on reload or power failure.
⚠️ Note: The raw material says "flash memory", but startup-config is saved in NVRAM; flash holds the IOS image. `write memory` is an equivalent save command.
7. Check router processes and statistics.

```
   show version             ! IOS version, uptime, memory
   show processes cpu       ! CPU load per process
   show interfaces          ! status and packet counters per interface
```

Why: These show router health and traffic.
⚠️ Note: Packet Tracer simulates only part of IOS output; if a command is rejected, verify in lab.
8. Run the IP commands.

```
   show ip interface brief  ! IP and status per interface
   show ip route            ! routing table
```

Why: Confirms addressing and that both networks are known to the router.
9. On PC1 open Desktop > Command Prompt and run ping.

```
   ping 192.168.1.2
```

Why: Replies prove the path through the router works.
10. Run tracert on PC1.

```
    tracert 192.168.1.2
```

```
Why: Lists each router crossed and its delay.
```

1. Run nslookup.

```
    nslookup
```

```
Why: Shows the default DNS server first.
⚠️ Note: Packet Tracer's PC needs a DNS server configured for nslookup; otherwise run it in the Windows 11 Command Prompt. Verify in lab.
```

1. In the Windows 11 Command Prompt, run pathping.

```
    pathping 192.168.1.12
```

```
Why: Runs a trace, then about 25 seconds of loss statistics.
⚠️ Note: Packet Tracer PCs have no pathping. Run it on the Windows host. The raw sample target 192.168.1.12 was unreachable (100% loss); use a live host to see normal results.
```

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `ping 192.168.1.2` (PC1) | Replies with TTL=127 (128 minus one router). The first request may time out (ARP), giving 3 of 4 received. |
| `tracert 192.168.1.2` | Hop 1 is 192.168.2.1, hop 2 is 192.168.1.2, then "Trace complete." |
| `nslookup` | Default DNS server shown, then a `>` prompt. |
| `pathping 192.168.1.12` | Per-hop Lost/Sent table; an unreachable host shows 100/100 = 100%. |
| `show ip interface brief` | Gi0/0 and Gi0/1 show their IPs and "up up". |
| `show ip route` | `C` entries for 192.168.2.0/24 and 192.168.1.0/24. |
| `show startup-config` | Interface settings appear after saving. |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| All pings time out | Interface shut down or wrong gateway | Run `no shutdown`; recheck the PC gateway. |
| Only first ping times out | ARP (IP-to-MAC lookup) delay | Normal; repeat the ping. |
| Destination host unreachable | Wrong IP or mask, or no route | Check `show ip interface brief` and `show ip route`. |
| Config missing after reload | Not saved | Run `copy running-config startup-config`. |
| % Invalid input detected | Wrong mode or typo | Check the prompt; use `?`. |

## Viva Q&A

1. **Q:** Which protocol does ping use? **A:** ICMP Echo Request and Echo Reply.
2. **Q:** How does pathping differ from tracert? **A:** It also reports packet loss and delay per hop.
3. **Q:** What does TTL=127 indicate? **A:** The packet started at 128 and crossed one router.
4. **Q:** Which command moves from user to privileged mode? **A:** `enable`.
5. **Q:** Why save the running-config? **A:** RAM contents are lost on reload or power failure.
6. **Q:** What does `co?` display? **A:** All commands beginning with "co".

## Result

The basic network commands and router configuration commands were executed and verified successfully.

## Submission Checklist

- [ ]  Topology screenshot with addressing
- [ ]  Router configuration commands
- [ ]  Help, abbreviation, and mode-switching output
- [ ]  Saved configuration (`show startup-config`)
- [ ]  Router processes and IP command outputs
- [ ]  ping, tracert, nslookup, and pathping screenshots
- [ ]  Viva answers written