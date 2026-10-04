# Experiment - 16

# Experiment 16: Exploring Different LAN Switch Options

## Aim

To choose the correct cable types and switch modules for connecting a router, a Linksys wireless router, and two PCs to a new standalone switch. Tool: Cisco Packet Tracer.

## Concepts

- **Module:** A plug-in card that adds ports to a switch or router. The empty switch has no ports until modules are fitted.
- **Straight-through cable:** A copper cable joining unlike devices, such as a PC to a switch. A **crossover cable** joins like devices, such as a switch to a switch.
- **Fast Ethernet (FE):** A 100 Mbps standard. Copper FE ports use RJ-45 connectors and twisted-pair cable.
- **Link lights:** Amber means the link is still starting. Green means the cable and ports are working at both ends.

## Topology & Addressing

PC0 and PC1 (PC-PT), Linksys-WRT300N Wireless Router0, and Router0 (Router-PT) connect to Switch0 (Switch-PT-Empty). Router0 also has a serial link to Router1 (1841).

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| PC0 | FastEthernet0 | Not given | Not given | Not given |
| PC1 | FastEthernet0 | Not given | Not given | Not given |
| Linksys-WRT300N | Ethernet (LAN) | Not given | Not given | N/A |
| Router0 (Router-PT) | Ethernet port (verify in lab) | Not given | Not given | N/A |
| Switch0 | 4 added ports | N/A | N/A | N/A |

⚠️ Note: The raw material gives no addresses, and this experiment tests only physical connectivity. Use the provided activity file if available; otherwise verify in lab. Router-PT and Switch-PT-Empty are older devices; if either is missing from the device list, use the closest equivalent and verify in lab.

## Procedure

1. Click **Router0**, open the **Physical** tab, and hover over each port to read its type. Click the port type to read its description.
Why: This identifies which port can connect to the switch.
2. Click the **Linksys-WRT300N** and view the **Physical** tab picture to find its Ethernet port.
Why: The port type decides which cable is needed.
3. Record the answers to the four questions in Viva Q&A (interface and cable for Router0; interface and cable for the Linksys).
Why: The cable choice depends on the devices joined.
⚠️ Note: Packet Tracer may accept either cable on some links. Use the theory rule: straight-through for unlike devices, crossover for like devices; verify in lab.
4. Click **Switch0**, open the **Physical** tab, and explore each module under **Modules**. Choose copper Fast Ethernet modules.
Why: The customer wants copper cabling.
5. Power off Switch0 using the power button in the Physical Device View.
Why: Modules can only be added while the device is off.
6. Drag four copper Fast Ethernet modules into four free switch slots.
Why: One port each is needed for Router0, the Linksys, PC0, and PC1.
⚠️ Note: The module is expected to be PT-SWITCH-NM-1CFE (1 copper Fast Ethernet port); verify in lab.
7. Power on Switch0, open the **Config** tab, select each new interface, and confirm **On** is checked.
Why: A port that is off will not bring up a link.
8. Connect Router0 to the first available Switch0 port using the cable from step 3.
Why: This creates the router-to-switch link.
9. Click **Router0 > Config**, select the connected interface, and confirm **On** is checked.
Why: Router ports are off by default.
10. Verify a green light at each end of the Router0 link.
Why: Green on both ends means the cabling is correct.
⚠️ Note: Links may take about a minute to turn green. Switch between Simulation and Realtime mode to speed this up.
11. Connect the Linksys-WRT300N to the second available Switch0 port using the cable from step 3, and verify green lights.
Why: This adds the wireless router to the LAN.
12. Connect PC0 and PC1 to the remaining Switch0 ports using straight-through cables, and verify green lights on both links.
Why: A PC and a switch are unlike devices.
13. Click **Check Results** at the bottom of the instruction window.
Why: This grades the activity.
⚠️ Note: Check Results exists only in the original activity file. If you built the topology manually, skip it and use the Verification table.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Switch0 Physical tab | Four copper Fast Ethernet modules installed |
| Switch0 Config tab | All four new interfaces have **On** checked |
| Router0 link | Green light at both ends |
| Linksys link | Green light at both ends |
| PC0 and PC1 links | Green light at both ends of each |
| Check Results | 100% completion; verify in lab |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Cannot add module | Switch is powered on | Power off, add module, power on. |
| Link light stays amber | Link still starting | Toggle Simulation and Realtime mode; wait about a minute. |
| Link light stays red or off | Wrong cable or port turned off | Check the cable type; tick **On** in Config. |
| No suitable port on Router0 | Router has no free copper Ethernet port | Add a module while powered off; verify in lab. |
| Cable will not connect | Wrong connector type | Choose a copper cable, not fibre or serial. |

## Viva Q&A

1. **Q:** Which interface is available on Router0 to connect to the new switch? **A:** A copper Ethernet port (Fast Ethernet expected); verify in lab.
2. **Q:** What cable connects Router0 to the switch? **A:** Straight-through copper (router and switch are unlike devices).
3. **Q:** Which interface on the Linksys 300N connects to the switch? **A:** Its Ethernet LAN port; verify in lab.
4. **Q:** What cable connects the Linksys to the switch? **A:** Crossover by theory (its LAN ports act like a switch); verify in lab.
5. **Q:** How many switch interfaces are needed and why? **A:** Four: Router0, Linksys, PC0, and PC1.
6. **Q:** Why power off the switch before adding modules? **A:** Modules cannot be inserted while the device is on.

## Result

Four modules were added to Switch0, and Router0, the Linksys 300N, and both PCs were connected with the correct cables and verified.

## Submission Checklist

- [ ]  Topology screenshot
- [ ]  Router0 Physical tab showing the port type
- [ ]  Switch0 Physical tab showing the four added modules
- [ ]  Switch0 Config tab with all interfaces On
- [ ]  Final topology with all links green
- [ ]  Check Results screenshot (or manual verification)
- [ ]  Viva answers written