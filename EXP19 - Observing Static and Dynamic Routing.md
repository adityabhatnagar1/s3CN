# Experiment - 19

# Experiment 19: Observing Static and Dynamic Routing

## Aim

To observe the network behaviour under static and default routing, then compare it with dynamic routing (EIGRP) after a link failure. Tool: Cisco Packet Tracer.

## Concepts

- **Static route:** A route typed in by hand. It does not adapt when a link fails.
- **Default route:** A static route used when no other route matches the destination.
- **Dynamic routing:** Routers exchange routes automatically and adapt to failures. **EIGRP** (Enhanced Interior Gateway Routing Protocol) is a Cisco dynamic protocol; **AS 10** is its process number, which must match on all routers.
- **Administrative distance (AD):** A trust rating for route sources; the lowest value wins. Static routes default to 1 and EIGRP to 90; here the static routes use 130.
- **Auto-summary:** Automatic summarising of networks at class boundaries. `no auto-summary` turns it off.

## Topology & Addressing

The Ticket Sales Office (PC0, PC1, PC2, Switch2 2950, BR2 1841, ISP2 2620XM) connects to the Main Stadium Network (Edge1, Edge2, Switch0, Switch1) through a Frame Relay cloud (BR2 to Edge2), and through the Internet Service Provider site (ISP2 to Edge1).

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| PC0 / PC1 / PC2 | NIC | Not given | Not given | Not given |
| BR2 | Serial (Frame Relay) / Fa0/0 | Not given | Not given | N/A |
| ISP2 | Serial / Fa0/0 | Not given | Not given | N/A |
| Edge1 | Fa0/0 | Not given | Not given | N/A |
| Edge2 | Serial / Fa0/0 | Not given | Not given | N/A |

⚠️ Note: The raw material gives no addresses. Open the required file `Observing Static and Dynamic Routing.pka` and read them from `show ip interface brief`; verify in lab. If you do not have the file, the topology cannot be rebuilt from this material.

## Procedure

1. Open `Observing Static and Dynamic Routing.pka`. Check that link lights are green.
Why: The Ticket Sales Office is preconfigured with static and default routes.
⚠️ Note: If lights stay amber, switch between Simulation and Realtime mode; verify in lab.
2. On **PC0 > Desktop > Command Prompt**, trace to the Edge1 Fa0/0 address.

```
   tracert <Edge1 Fa0/0 IP>    ! expected to succeed; note the path
```

Why: Gives a baseline path under static routing.
3. On BR2, shut down the Frame Relay link.

```
   BR2>enable                      ! privileged EXEC mode
   BR2#configure terminal          ! global configuration mode
   BR2(config)#interface serial 0/0/0   ! Frame Relay port (verify in lab)
   BR2(config-if)#shutdown         ! turn the link off
   BR2(config-if)#end
```

Why: Simulates a link failure.
⚠️ Note: The serial port name is not given; confirm it with `show ip interface brief`.
4. Repeat the trace from PC0 and record what happens.

```
   tracert <Edge1 Fa0/0 IP>    ! expected to fail; static routes do not adapt
```

Why: Shows static routing cannot reroute around a failed link.
5. Configure EIGRP AS 10 on BR2, listing all directly connected networks.

```
   BR2(config)#router eigrp 10                    ! start EIGRP, AS 10
   BR2(config-router)#network <network> <wildcard> ! repeat for each connected network
   BR2(config-router)#no auto-summary             ! disable auto-summary
   BR2(config-router)#end
```

Why: Allows BR2 to learn routes automatically.
⚠️ Note: Use wildcard masks (for example 0.0.0.255 for /24) or classful `network` statements; verify in lab.
6. Configure EIGRP AS 10 on ISP2 in the same way.

```
   ISP2(config)#router eigrp 10                    ! start EIGRP, AS 10
   ISP2(config-router)#network <network> <wildcard> ! repeat for each connected network
   ISP2(config-router)#no auto-summary
   ISP2(config-router)#end
```

Why: Both routers must run the same AS to share routes.
7. Wait for EIGRP to converge, then trace again from PC0 to the Edge1 Fa0/0 address.

```
   tracert <Edge1 Fa0/0 IP>    ! expected to succeed again
```

Why: Confirms dynamic routing restored connectivity.
8. Compare the third trace with the first. Record whether the path changed and how.
Why: The path is expected to use the ISP link instead of Frame Relay; verify in lab.
9. Answer the Reflection questions (see Viva Q&A).
Why: Completes the assessment.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `tracert` to Edge1 Fa0/0 (step 2) | Succeeds, with the baseline path recorded |
| `tracert` to Edge1 Fa0/0 (step 4) | Fails, with timeouts or "unreachable" |
| `show ip protocols` (BR2, ISP2) | EIGRP AS 10 listed; auto-summary disabled; verify in lab |
| `show ip route` (BR2) | `D` entries from EIGRP |
| `tracert` to Edge1 Fa0/0 (step 7) | Succeeds, likely via a different path |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Trace still fails after EIGRP | Missing `network` statement | Add every directly connected network on both routers. |
| No EIGRP neighbours | Different AS numbers | Use `router eigrp 10` on both. |
| EIGRP routes not used | Static route AD is lower | Check AD with `show ip route`; static is set to 130 here. |
| Wrong interface shut down | Port name guessed | Confirm with `show ip interface brief`. |
| Trace stays unsuccessful | Frame Relay link still down | Check `show ip interface brief` for the ISP-side path. |

## Viva Q&A

1. **Q:** What are the advantages of dynamic routing? **A:** It adapts automatically to failures and needs less manual setup.
2. **Q:** What are the advantages of static and default routing? **A:** They use little CPU and bandwidth, and are simple and predictable.
3. **Q:** What if the static routes had an AD of 30? **A:** They would beat EIGRP (90), so EIGRP routes would not be used.
4. **Q:** What if the static routes had an AD of 230? **A:** EIGRP (90) would be preferred; statics would be backups only.
5. **Q:** Why was the trace unsuccessful after the shutdown? **A:** Static routes do not change when a link fails.
6. **Q:** What does `no auto-summary` do? **A:** It stops summarising networks at class boundaries.

## Result

Static routing failed after the link shutdown, and EIGRP dynamic routing restored connectivity.

## Submission Checklist

- [ ]  Topology screenshot
- [ ]  First tracert (successful)
- [ ]  BR2 shutdown commands
- [ ]  Second tracert (failed)
- [ ]  EIGRP configuration on BR2 and ISP2
- [ ]  Third tracert (successful) and path comparison
- [ ]  Reflection answers written