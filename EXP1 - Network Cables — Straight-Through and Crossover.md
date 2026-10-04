# Experiment - 1

# Network Cables — Straight-Through and Crossover

## Aim

Study different types of network cables and practically implement straight-through and crossover cables using a crimping tool.

## Concepts

- **Twisted-pair cable**: four wire pairs twisted to reduce electromagnetic interference.
- **RJ-45 connector**: 8-pin plug terminated onto the cable end.
- **Straight-through cable**: both ends use the same wiring standard; connects unlike devices.
- **Crossover cable**: one end uses T568A, the other T568B; connects like devices.
- **T568A/T568B**: two colour-code standards for arranging wires in the RJ-45 pins.

## Topology & Addressing

Physical cable-building exercise; no IP addressing applies. Packet Tracer simulation uses the connections below.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| PC1 | FastEthernet0 | 192.168.1.1 | 255.255.255.0 | — |
| PC2 | FastEthernet0 | 192.168.1.2 | 255.255.255.0 | — |
| Switch | FastEthernet0/1 | — | — | — |

## Procedure

1. Strip about 2 inches of the outer plastic jacket from the cable end.
Why: Exposes the four wire pairs without nicking the copper conductors.
2. Untwist the pairs just enough to arrange them in the required colour order.
Why: Category 5 allows a maximum of 0.5 inch (1.27 cm) of untwisted wire at the termination; more causes signal degradation.
3. Arrange wires for the required standard using the tables below.

**T568B (used on both ends for straight-through)**

```
Pin 1  White/Orange
Pin 2  Orange
Pin 3  White/Green
Pin 4  Blue
Pin 5  White/Blue
Pin 6  Green
Pin 7  White/Brown
Pin 8  Brown
```

**T568A (used on one end for crossover)**

```
Pin 1  White/Green
Pin 2  Green
Pin 3  White/Orange
Pin 4  Blue
Pin 5  White/Blue
Pin 6  Orange
Pin 7  White/Brown
Pin 8  Brown
```

⚠️ Note: In Cisco Packet Tracer, physical crimping cannot be simulated. Use **Connections > Copper Straight-Through** for PC-to-Switch or Router-to-Switch links, and **Copper Cross-Over** for PC-to-PC or Switch-to-Switch links. The **Automatically Choose Connection Type** option selects the correct cable automatically.

1. Trim the wires straight across, leaving about 0.5 inch exposed from the jacket.
Why: Ensures all wires reach the RJ-45 contact pins evenly.
2. Slide the wires fully into the RJ-45 connector.
Why: Each wire must seat against its numbered pin for continuity.
3. Insert the connector into the crimping tool and squeeze firmly.
Why: Permanently presses the metal contacts through the wire insulation.
4. Repeat steps 1–6 for the second end using the same standard (straight-through) or the opposite standard (crossover).
5. Test the cable with a cable tester or in Packet Tracer by connecting devices and checking link status.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Cable tester (straight-through) | Pins 1–8 light in sequence on both ends |
| Cable tester (crossover) | Pins 1↔3, 2↔6 crossed; others straight |
| Packet Tracer: hover over link | Tooltip shows correct cable type |
| PC1 ping PC2 | Successful reply (after IP configuration) |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Cable tester shows open circuit | Wire not fully inserted or poor crimp | Re-terminate the connector |
| Link light does not turn on | Wrong cable type for device pair | Use crossover for PC-to-PC; straight-through for PC-to-Switch |
| Intermittent connection | Nicked wire or loose RJ-45 | Cut off the end and re-crimp |
| Packet Tracer link shows red | Incorrect cable type selected | Delete link; re-add with correct cable |
| Untwisted length exceeds 0.5 inch | Wires untwisted too far back | Re-strip and keep twists tight up to the connector |

## Viva Q&A

**Q1: What is the maximum untwisted wire length for Category 5?**
A: 0.5 inch (1.27 cm).

**Q2: Which pins are swapped in a crossover cable?**
A: Pins 1↔3 and 2↔6.

**Q3: When do you use a straight-through cable?**
A: To connect devices operating at different OSI layers (e.g., PC to switch).

**Q4: What is the difference between T568A and T568B?**
A: The orange and green wire pairs are swapped (pins 1, 2, 3, 6).

**Q5: Why must the twists be maintained close to the connector?**
A: Twists cancel interference; excess untwisting degrades signal quality.

**Q6: Can a crossover cable connect a PC to a switch?**
A: Not normally; modern switches support auto-MDI/MDI-X, so verify in lab.

## Result

Straight-through and crossover cables were successfully constructed and verified for correct pin continuity.

## Submission Checklist

- [ ]  Straight-through cable built with T568B on both ends
- [ ]  Crossover cable built with T568A on one end, T568B on the other
- [ ]  Both cables tested with a cable tester or Packet Tracer
- [ ]  Pin-to-colour tables recorded in lab notebook
- [ ]  Viva questions answered