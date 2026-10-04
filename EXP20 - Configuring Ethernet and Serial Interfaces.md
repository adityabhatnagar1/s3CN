# Experiment - 20

# Experiment 20: Configuring Ethernet and Serial Interfaces

## Aim

To configure a LAN Fast Ethernet interface and a WAN serial interface on the Customer 1841 router, then verify and save the configuration. Tool: Cisco Packet Tracer.

## Concepts

- **Interface:** A router port. Each interface needs its own IP address and mask and must be enabled with `no shutdown`.
- **LAN vs WAN interface:** A LAN (local network) interface is Fast Ethernet. A WAN (wide area, links to the ISP) interface is serial.
- **Interface description:** A text label on a port. It does not change behaviour.
- **IOS modes:** `>` is user EXEC, `#` is privileged EXEC, `(config)#` is global configuration, and `(config-if)#` is interface configuration. Interface settings need interface configuration mode.
- **Running vs startup configuration:** Running-config is the active configuration (RAM), lost on reload. Startup-config is saved in NVRAM and loads at boot.

## Topology & Addressing

Customer PC and CustomerServer connect to the CustomerSwitch (2960), which connects to the CustomerRouter (1841). A serial link joins it to the ISPRouter (1841), which connects to the ISPSwitch with ISPWorkstation and ISPServer. The Customer PC also has a console cable to CustomerRouter (cyan line).

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| CustomerRouter | Fa0/0 | 192.168.1.1 | 255.255.255.0 | N/A |
| CustomerRouter | Serial0/1/0 | 209.165.200.225 | 255.255.255.224 | N/A |
| ISPRouter | Serial (WAN) | 209.165.200.226 | 255.255.255.224 | N/A |
| ISPRouter | LAN | Not given | Not given | N/A |
| Customer PC, CustomerServer, ISPWorkstation, ISPServer | NIC | Not given | Not given | Not given |

⚠️ Note: The ISPRouter WAN address 209.165.200.226 is taken from the ping step. The PC and server addresses are not given and are not needed. Use the provided activity file, which has the ISP side preconfigured; verify in lab. If the serial port name differs, confirm it with `show ip interface brief`.

## Procedure

1. On Customer PC open **Desktop > Terminal**, click OK, and enter the console password `cisco`.
Why: The console is a direct management link to CustomerRouter.
2. Enter privileged EXEC mode with the password `cisco123`, then global configuration mode.

```
   Router>enable                  ! prompts for cisco123
   CustomerRouter#configure terminal   ! prompt becomes CustomerRouter(config)#
```

Why: Interface changes are made from global configuration mode.
3. Configure the LAN Fast Ethernet interface.

```
   CustomerRouter(config)#interface FastEthernet 0/0                 ! select the LAN interface
   CustomerRouter(config-if)#description Connected to CustomerSwitch ! label the port
   CustomerRouter(config-if)#ip address 192.168.1.1 255.255.255.0    ! set IP and mask
   CustomerRouter(config-if)#no shutdown                             ! enable the interface
   CustomerRouter(config-if)#end                                     ! return to privileged EXEC
```

Why: Router interfaces are shut down by default.
4. Verify the LAN interface.

```
   CustomerRouter#show ip route    ! expect: C 192.168.1.0/24 is directly connected, FastEthernet0/0
```

Why: A `C` entry confirms the interface is up with an address.
5. Configure the WAN serial interface.

```
   CustomerRouter#configure terminal                                     ! global configuration mode
   CustomerRouter(config)#interface Serial0/1/0                          ! select the serial interface
   CustomerRouter(config-if)#description Connected to ISP                ! label the port
   CustomerRouter(config-if)#ip address 209.165.200.225 255.255.255.224  ! set IP and mask
   CustomerRouter(config-if)#no shutdown                                 ! enable the interface
   CustomerRouter(config-if)#end                                         ! return to privileged EXEC
```

Why: The serial link to the ISP needs its own address in the 209.165.200.224/27 network.
⚠️ Note: The serial port name may differ by module (for example Serial0/0/0); use `?` and `show ip interface brief` to confirm. Verify in lab.
6. Verify the configuration.

```
   CustomerRouter#show run    ! check descriptions, IPs, and that Fa0/1 shows shutdown
```

Why: Shows what is currently configured.
7. Test connectivity to the ISP router WAN interface.

```
   CustomerRouter#ping 209.165.200.226    ! expect !!!!! (100 percent)
```

Why: Confirms the serial link works.
8. Test the LAN interface address.

```
   CustomerRouter#ping 192.168.1.1    ! expect !!!!! (100 percent)
```

Why: Confirms the router's own LAN address responds.
⚠️ Note: The raw material says this ping verifies the customer switch, but 192.168.1.1 is the router's own address. The switch address is not given; verify in lab.
9. Save the configuration.

```
   CustomerRouter#copy run start    ! save running-config to startup-config
```

Why: Unsaved changes are lost on reload.
10. Click **Check Results** at the bottom of the instruction window.
Why: This grades the configuration.
⚠️ Note: Check Results exists only in the original activity file. If you built the topology manually, skip it and use the Verification table.
11. Answer the Reflection questions (see Viva Q&A).
Why: Completes the assessment.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| `show ip route` | `C 192.168.1.0/24 is directly connected, FastEthernet0/0` |
| `show run` | Fa0/0 and Serial0/1/0 show descriptions and IP addresses; Fa0/1 shows `shutdown` |
| `ping 209.165.200.226` | `!!!!!`, success rate 100 percent (5/5) |
| `ping 192.168.1.1` | `!!!!!`, success rate 100 percent (5/5) |
| `show startup-config` | Interface settings present after saving |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Interface shows administratively down | `no shutdown` not entered | Enter `no shutdown` under the interface. |
| Ping to 209.165.200.226 fails | Wrong IP or mask, or ISP side down | Check `show ip interface brief`; verify in lab. |
| `% Invalid input detected` | Wrong mode or wrong port name | Check the prompt; use `?`. |
| No `C` route for 192.168.1.0/24 | Interface down or no address | Recheck the IP, mask, and cabling. |
| Settings lost after reload | Configuration not saved | Run `copy run start`. |

## Viva Q&A

1. **Q:** What happens when you ping the LAN IP address of the ISP router, and why? **A:** It fails because the router has no route to the ISP LAN; verify in lab.
2. **Q:** Which mode is needed to set an interface description? **A:** `CustomerRouter(config-if)#`.
3. **Q:** Why was Fa0/0 shut down after the reboot? **A:** The configuration was not saved to startup-config.
4. **Q:** What does `no shutdown` do? **A:** It enables the interface.
5. **Q:** How do you save the configuration? **A:** `copy run start`.
6. **Q:** Which command shows directly connected networks? **A:** `show ip route`.

## Result

The Fast Ethernet and serial interfaces on the Customer router were configured, verified, and saved.

## Submission Checklist

- [ ]  Topology screenshot
- [ ]  LAN interface configuration commands
- [ ]  `show ip route` output
- [ ]  Serial interface configuration commands
- [ ]  `show run` output
- [ ]  Ping outputs to 209.165.200.226 and 192.168.1.1
- [ ]  `copy run start` output
- [ ]  Check Results screenshot (or manual verification)
- [ ]  Reflection answers written