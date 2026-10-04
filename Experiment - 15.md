# Experiment - 15

# Experiment 15: Placing ACLs

## Aim

Apply the preconfigured ACLs on the correct interfaces and verify that permitted traffic passes while denied traffic is blocked.

## Concepts

- **ACL**: Access Control List, an ordered set of permit/deny rules for traffic.
- **Standard ACL**: Filters by source IP only; placed closest to the destination.
- **Extended ACL**: Filters by source, destination, protocol, and port; placed closest to the source.
- **Implicit deny**: The final invisible rule that drops all traffic not matched.
- **access-group**: Command that binds an ACL to an interface and direction.

## Topology & Addressing

Internet Host reaches the LAN through Cluster0, Gateway, and Core; Distribution1 and Distribution2 serve the access switches and servers.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Internet Host | NIC | verify in lab | verify in lab | verify in lab |
| Web server | NIC | 192.168.0.3 | verify in lab | verify in lab |
| Sales server | NIC | 192.168.10.2 | verify in lab | verify in lab |
| HR server | NIC | 192.168.40.2 | verify in lab | verify in lab |
| Gateway | verify in lab | verify in lab | verify in lab | N/A |
| Distribution1 | verify in lab | verify in lab | verify in lab | verify in lab |
| Distribution2 | verify in lab | verify in lab | verify in lab | verify in lab |

⚠️ Note: The raw material lists the HR server as 192.168.40.2 in Step 1b and 192.168.40.1 in Step 4. Confirm the real address with `ipconfig` on the HR server.

## Procedure

1. Open the required file `Placing ACLs.pka`.
Why: The file contains the topology with ACLs already configured but not applied.
2. From the PCs, ping each other and the servers; from Internet Host, open the browser and reach the Web server, Sales server, and HR server.
Why: Establishes the baseline connectivity before ACLs take effect.

⚠️ Note: In Packet Tracer, use the PC **Command Prompt** for ping and the **Web Browser** with `http://<server-ip>` for browser tests.

1. Access Distribution1 and view its ACL.

```
enable                  ! Enter privileged EXEC mode
show running-config     ! Display active configuration
show access-lists 1     ! Display ACL number 1
```

Why: Reveals which traffic the standard ACL permits or denies.

1. Access Gateway and view its ACL.

```
enable                  ! Enter privileged EXEC mode
show running-config     ! Display active configuration
show access-lists 100   ! Display ACL number 100
```

Why: Reveals which traffic the extended ACL permits or denies.

⚠️ Note: The background states the ACLs were configured on Gateway and Distribution2, while Step 2a says Distribution1. Check both routers with `show access-lists` and use whichever holds the ACLs.

1. Decide the interface for each ACL using these rules: extended closest to source, standard closest to destination, one ACL per port per protocol per direction.
Why: Wrong placement lets denied traffic reach the destination or blocks permitted traffic.
2. Apply the standard ACL 1 on the interface closest to the destination.

```
enable                          ! Enter privileged EXEC mode
configure terminal              ! Enter global configuration mode
interface <destination-facing subinterface>   ! Verify exact name in lab
ip access-group 1 out           ! Bind ACL 1 outbound
exit
```

Why: Standard ACLs filter by source, so they belong near the destination.

⚠️ Note: Confirm the exact interface name and direction (in or out) with `show ip interface brief` before applying; Packet Tracer rejects a second ACL on the same port, protocol, and direction.

1. Apply the extended ACL 100 on the interface closest to the source.

```
configure terminal              ! Enter global configuration mode
interface <source-facing interface>           ! Verify exact name in lab
ip access-group 100 in          ! Bind ACL 100 inbound
exit
```

Why: Extended ACLs filter precisely, so they belong near the source.

1. Verify the effects from Internet Host, HR2, and RandD2 as listed in the Verification table.
Why: Confirms the ACLs permit and deny the intended traffic.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Internet Host pings any device except HR1 and HR server | Ping succeeds to all others; HR1 and HR server unreachable |
| Internet Host browses to 192.168.0.3 | Web server page loads |
| Internet Host browses to 192.168.40.1 and 192.168.10.2 | Both fail |
| HR2 pings and browses HR server 192.168.40.1 | Both succeed |
| RandD2 pings and browses HR server 192.168.40.1 | Both fail |
| `show access-lists` on each router | Displays ACL entries and match counters |
| `show running-config` on the applied interface | Shows the `ip access-group` line |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| ACL has no effect | Not applied to any interface | Apply with `ip access-group <number> <direction>` |
| Permitted traffic blocked | Wrong direction or interface | Recheck in/out and move the ACL |
| Second ACL rejected on a port | One ACL per port, protocol, direction | Remove the existing binding first |
| HR2 still blocked | Standard ACL placed too far from destination | Move ACL 1 closer to the HR server |
| All traffic blocked | Implicit deny reached | Check rule order; add the missing permit before the deny |

## Viva Q&A

**Q1: How can ACLs be used to control the flow of network traffic?**
A1: They permit or deny packets by matching source, destination, protocol, or port.

**Q2: By default, what is always the last statement in an ACL?**
A2: An implicit deny that drops all traffic not matched by earlier rules.

**Q3: Where should a standard ACL be placed?**
A3: Closest to the destination.

**Q4: Where should an extended ACL be placed?**
A4: Closest to the source.

**Q5: How many ACLs can be applied per port, protocol, and direction?**
A5: Only one.

**Q6: What command binds an ACL to an interface?**
A6: `ip access-group <number> <in|out>`.

## Result

The preconfigured ACLs were applied to the correct interfaces, and permitted and denied traffic matched the expected results.

## Submission Checklist

- [ ]  Opened `Placing ACLs.pka`
- [ ]  Verified baseline connectivity between PCs and servers
- [ ]  Verified Internet Host browser access to Web, Sales, and HR servers before ACLs
- [ ]  Viewed Distribution1 ACL with `show running-config` and `show access-lists 1`
- [ ]  Viewed Gateway ACL with `show running-config` and `show access-lists 100`
- [ ]  Determined the correct interface for each ACL
- [ ]  Applied the standard ACL closest to the destination
- [ ]  Applied the extended ACL closest to the source
- [ ]  Confirmed Internet Host can reach Web server only
- [ ]  Confirmed HR2 can reach HR server
- [ ]  Confirmed RandD2 cannot reach HR server
- [ ]  Answered both reflection questions