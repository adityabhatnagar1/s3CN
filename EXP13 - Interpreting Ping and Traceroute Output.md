# Experiment - 13

# Experiment 13: Interpreting Ping and Traceroute Output

## Aim

To test end-to-end connectivity with ping and traceroute from hosts and a router. To distinguish successful from unsuccessful ping and traceroute results. Tool: Cisco Packet Tracer.

## Concepts

- **Ping:** Sends ICMP (Internet Control Message Protocol) echo requests to test reachability. A PC shows `Reply` or `Request timed out`; a router shows `!` (success) or `.` (failure).
- **Traceroute:** Lists each router (hop, one router crossed) on the path to a destination. `tracert` is the PC command; `traceroute` is the router command. A failed hop shows `* *`.
- **Convergence:** The state where all devices have learned the routes and links are stable. Pings may fail before this.
- **ARP:** Address Resolution Protocol maps an IP address to a MAC address. It usually causes the first ping to fail.
- **DNS:** Domain Name System maps names such as `www.cisco.com` to IP addresses.

## Topology & Addressing

Four branch LANs (N, W, S, E), each with a host, a 2960 switch, and a 1841 router, connect through a central ISP Network cloud. The ISP cloud also links the servers [www.public.com](http://www.public.com/), [www.cisco.com](http://www.cisco.com/), and [www.domain.com](http://www.domain.com/).

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| N-Host | NIC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| W-Host | NIC | 192.168.2.10 | 255.255.255.0 | Not given |
| S-Host | NIC | 192.168.3.10 | 255.255.255.0 | Not given |
| E-Host | NIC | 192.168.4.10 | 255.255.255.0 | Not given |
| [www.cisco.com](http://www.cisco.com/) | NIC | 64.100.1.185 | Not given | Not given |
| DNS server | NIC | 64.100.1.242 | Not given | Not given |

⚠️ Note: Masks come from the /24 labels in the diagram. The N-Host gateway comes from tracert hop 1, and 64.100.1.242 is the DNS server shown in the router output. Other gateways and branch router addresses are not given; use the provided activity file or verify in lab.

## Procedure

1. Make the network converge: switch between Simulation mode and Realtime mode until all link lights turn green.
Why: Tests fail while routes are still being learned.
⚠️ Note: If the topology was built manually, wait about a minute for the links to turn green. Check the lights again; verify in lab.
2. On **N-Host > Desktop > Command Prompt**, ping the Cisco server by name.

```
   ping www.cisco.com    ! name resolved by DNS to 64.100.1.185
```

Why: A first-ping timeout followed by replies is normal because of ARP. Repeat the ping to see all replies succeed.
3. On N-Host, ping E-Host. Press **Ctrl+C** to stop early.

```
   ping 192.168.4.10     ! expected to fail
```

Why: Shows an unsuccessful ping (100% loss).
4. Click **N-Branch > CLI**, press Enter, and ping the Cisco server.

```
   N-Branch>ping www.cisco.com    ! router ping, name resolved by DNS
```

Why: Router output uses `.` for a failure and `!` for a success.
5. On N-Branch, ping E-Host. Press **Ctrl+C** to stop early.

```
   N-Branch>ping 192.168.4.10     ! expected to fail
```

Why: Shows a failed router ping (0 percent).
6. On N-Host, trace the route to the Cisco server.

```
   tracert www.cisco.com    ! lists every hop to 64.100.1.185
```

Why: Each hop is a router replying three times.
7. On N-Host, trace the route to E-Host. Press **Ctrl+C** after the timeouts.

```
   tracert 192.168.4.10     ! fails after hop 6
```

Why: The last responding hop (64.100.1.154) is where troubleshooting starts. Ping alone only says reachable or unreachable.
8. On N-Branch, trace the route to the Cisco server.

```
   N-Branch>traceroute www.cisco.com
```

Why: Output is like the PC's, except a PC prints the IP after the three times.
9. On N-Branch, trace the route to E-Host. Press **Ctrl+C** to stop.

```
   N-Branch>traceroute 192.168.4.10    ! fails at the same address as the PC trace
```

Why: Confirms the fault is in the same place from both sources.
10. Practise: ping and trace from W-Host and S-Host to any other destination, and from N-Branch to other locations.
Why: Builds skill in reading results.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `ping www.cisco.com` (N-Host) | Resolves to 64.100.1.185; 3 of 4 replies, TTL=123 (first may time out) |
| `ping 192.168.4.10` (N-Host) | `Request timed out`; 100% loss |
| `ping www.cisco.com` (N-Branch) | `.!!!!`; success rate 80 percent (4/5) |
| `ping 192.168.4.10` (N-Branch) | Dots only; 0 percent success |
| `tracert www.cisco.com` | 6 hops ending at 64.100.1.185, then "Trace complete." |
| `tracert 192.168.4.10` | Hops 1-6 reply (last 64.100.1.154), then `* * *` |
| `traceroute www.cisco.com` (N-Branch) | 5 hops ending at 64.100.1.185 |
| `traceroute 192.168.4.10` (N-Branch) | Hops 1-5 reply (last 64.100.1.154), then `* * *` |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| First ping always times out | ARP not yet resolved | Normal; repeat the ping. |
| Name does not resolve | DNS server unreachable or not set | Ping the IP directly; check the DNS setting. |
| All pings time out at the start | Network not converged | Toggle Simulation/Realtime until the links are green. |
| Trace ends with `* * *` | Fault at or after the last replying hop | Find which device owns that last IP and check its configuration. |
| Command runs too long | Timeouts continue to 30 hops | Press **Ctrl+C**. |

## Viva Q&A

1. **Q:** Why does the first ping often fail? **A:** ARP has not yet resolved the destination.
2. **Q:** What do `.` and `!` mean in router ping? **A:** `.` is a timeout; `!` is a reply.
3. **Q:** How does traceroute help more than ping? **A:** It shows the last responding hop, a starting point for troubleshooting.
4. **Q:** What does `* *` mean in a trace? **A:** The hop did not reply.
5. **Q:** Which command stops a running ping or trace? **A:** **Ctrl+C**.
6. **Q:** How did N-Host get 64.100.1.185 for [www.cisco.com](http://www.cisco.com/)? **A:** Through DNS.

## Result

Ping and traceroute were run from hosts and a router, and successful and unsuccessful outputs were identified.

## Submission Checklist

- [ ]  Topology screenshot
- [ ]  N-Host ping output ([www.cisco.com](http://www.cisco.com/) and 192.168.4.10)
- [ ]  N-Branch ping output ([www.cisco.com](http://www.cisco.com/) and 192.168.4.10)
- [ ]  N-Host tracert output (both destinations)
- [ ]  N-Branch traceroute output (both destinations)
- [ ]  Practice ping/trace from W-Host, S-Host, and N-Branch
- [ ]  Viva answers written