# Experiment - 12

# Experiment 12: Examining WAN Connections

## Aim

Examine WAN connectivity on a Frame Relay network using Cisco IOS show commands on Branch1, Branch2, and Main routers.

## Concepts

- **Show commands**: Display static device state for troubleshooting and monitoring.
- **Frame Relay**: Legacy WAN technology using virtual circuits over a packet-switched network.
- **LMI**: Local Management Interface, signaling protocol between router and Frame Relay switch.
- **PVC**: Permanent Virtual Circuit, a predefined logical path between two endpoints.
- **DLCI**: Data Link Connection Identifier, locally significant number identifying a PVC.

## Topology & Addressing

Branch1, Branch2, and Main routers connect through a Frame Relay cloud; Branch1A, Branch2A, and MainA are attached PCs.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Branch1 | Serial0/0/0 | verify in lab | verify in lab | verify in lab |
| Branch1 | FastEthernet0/0 | verify in lab | verify in lab | verify in lab |
| Branch2 | Serial0/0/0 | verify in lab | verify in lab | verify in lab |
| Branch2 | FastEthernet0/0 | verify in lab | verify in lab | verify in lab |
| Main | Serial0/0/0 | verify in lab | verify in lab | verify in lab |
| Main | FastEthernet0/0 | verify in lab | verify in lab | verify in lab |
| Branch1A | NIC | verify in lab | verify in lab | verify in lab |
| Branch2A | NIC | verify in lab | verify in lab | verify in lab |
| MainA | NIC | verify in lab | verify in lab | verify in lab |

## Procedure

1. Open the required file `Examining WAN Connections.pka` in Cisco Packet Tracer.
Why: The file contains the preconfigured Frame Relay topology.
2. Click Branch1 and run the following show commands.

```
show running-config        ! View router configuration
show ip interface brief    ! View interface status
show frame-relay map       ! View DLCI-to-IP mappings
show frame-relay pvc       ! View PVC status
show frame-relay lmi       ! View LMI statistics
```

Why: These commands reveal connectivity and Frame Relay circuit status.

⚠️ Note: Frame Relay is a legacy WAN technology not covered in current CCNA. If the .pka file or commands are unsupported in your Packet Tracer version, use a point-to-point serial link with PPP encapsulation to practice the same show commands (`show ip interface brief`, `show running-config`, `show interfaces`). The reflection questions still apply.

1. Click Branch2 and run the same show commands.

```
show running-config
show ip interface brief
show frame-relay map
show frame-relay pvc
show frame-relay lmi
```

Why: Confirms Branch2 configuration and circuit status.

1. Click Main and run the same show commands.

```
show running-config
show ip interface brief
show frame-relay map
show frame-relay pvc
show frame-relay lmi
```

Why: Confirms Main router configuration and circuit status.

1. Answer the reflection questions.
Why: Reinforces understanding of show command benefits.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `show running-config` on Branch1, Branch2, Main | Displays interfaces, IPs, and Frame Relay encapsulation |
| `show ip interface brief` | Lists interfaces with IP addresses and up/up status |
| `show frame-relay map` | Shows DLCI-to-IP mappings for each PVC |
| `show frame-relay pvc` | Shows PVC status (ACTIVE/INACTIVE) and statistics |
| `show frame-relay lmi` | Shows LMI type, status, and counters |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| `show frame-relay` commands not recognized | Packet Tracer version lacks Frame Relay support | Use PPP serial link; run `show interfaces` instead |
| Interface status down | Serial link not connected or clock rate missing | Check cabling and `clock rate` on DCE side |
| No DLCI mappings | Frame Relay switch not configured | Verify cloud configuration in .pka file |
| Ping between branches fails | PVC inactive or wrong IP | Check `show frame-relay pvc` and IP addressing |
| `show running-config` empty | Not in privileged EXEC mode | Enter `enable` first |

## Viva Q&A

**Q1: In what situations would it be beneficial to use the various show commands?**
A1: For troubleshooting, monitoring, and documenting network state at a specific time.

**Q2: What beneficial information can be obtained from the various show commands?**
A2: Interface status, IP addresses, routing tables, ARP mappings, and WAN circuit health.

## Result

The WAN connections were examined using show commands on Branch1, Branch2, and Main routers.

## Submission Checklist

- [ ]  Opened `Examining WAN Connections.pka`
- [ ]  Examined Branch1 with `show running-config` and `show ip interface brief`
- [ ]  Examined Branch1 with `show frame-relay map`, `show frame-relay pvc`, `show frame-relay lmi`
- [ ]  Examined Branch2 with all show commands
- [ ]  Examined Main with all show commands
- [ ]  Answered reflection questions a and b