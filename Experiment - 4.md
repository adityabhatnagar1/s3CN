# Experiment - 4

# Experiment 4: Connecting Computers in a Local Area Network

## Aim

Share an Internet connection from one host computer to client computers on a LAN using Internet Connection Sharing (ICS), and implement the same setup in Cisco Packet Tracer.

## Concepts

- **ICS**: A Windows feature that lets one computer share its Internet connection with others on the same LAN.
- **DHCP**: A protocol that automatically assigns IP addresses to devices on a network.
- **NAT**: A method that translates private IP addresses to a public address for Internet access.
- **Default gateway**: The device that routes traffic from the LAN to external networks.
- **Private IP range**: Addresses like 192.168.x.x used inside a LAN, not routable on the Internet.

## Topology & Addressing

Two client PCs connect to a switch, which connects to the host PC (ICS server) and, through a simulated Internet router, to an external network.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Host PC (ICS Server) | LAN adapter | 192.168.0.1 | 255.255.255.0 | — |
| Client PC 1 | LAN adapter | DHCP (192.168.0.2–254) | 255.255.255.0 | 192.168.0.1 |
| Client PC 2 | LAN adapter | DHCP (192.168.0.2–254) | 255.255.255.0 | 192.168.0.1 |
| Internet Router (simulated) | WAN | Verify in lab | Verify in lab | — |

⚠️ Note: Packet Tracer does not emulate Windows ICS. Use a Home Router or a Cisco router with DHCP and NAT to achieve the same result.

## Procedure

1. Connect the host PC's LAN adapter to a switch port, and connect both client PCs to the same switch.
Why: All devices must be on the same LAN segment.
2. On the host PC, open **Control Panel → Network and Internet → Network Connections**.
Why: This is where adapter properties are configured.
3. Right-click the adapter connected to the Internet, select **Properties**, then open the **Sharing** tab.
Why: The Sharing tab holds the ICS setting.

⚠️ Note: In Windows 11, open Network Connections by pressing **Win + R**, typing `ncpa.cpl`, and pressing Enter. If the Sharing tab is missing, run `services.msc`, locate **Internet Connection Sharing (SharedAccess)**, set it to Automatic, and start the service.

1. Check **Allow other network users to connect through this computer's Internet connection**.
Why: This enables ICS on the selected adapter.
2. If sharing a dial-up connection, check **Establish a dial-up connection whenever a computer on my network attempts to access the Internet**.
Why: This allows the host to dial automatically when a client needs Internet access.
3. Click **OK**, then **Yes** on the confirmation dialog.
Why: Windows will set the LAN adapter to 192.168.0.1 / 255.255.255.0.
4. On each client PC, open **Network Connections** and right-click **Local Area Connection**.
Why: Client adapter settings must match the ICS server's network.
5. Select **Internet Protocol (TCP/IP)**, click **Properties**, and choose **Obtain an IP address automatically**.
Why: The ICS server acts as a DHCP server and assigns addresses automatically.

⚠️ Note: The original material lists 192.168.31.202 / 255.255.255.0 / 192.168.31.1 as client values. That subnet (192.168.31.x) does not match the ICS server subnet (192.168.0.x). Use DHCP or a static address in 192.168.0.2–254 with gateway 192.168.0.1.

1. Click **OK** to apply, then close the properties window.
2. In Packet Tracer, build the topology and configure the router's LAN interface with IP 192.168.0.1 / 255.255.255.0.
Why: This simulates the ICS host's LAN adapter address.

```
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.0.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```

1. Configure the DHCP pool on the router.
    
    Why: This simulates automatic address assignment by ICS.
    

cisco

```
Router(config)# ip dhcp excluded-address 192.168.0.1
Router(config)# ip dhcp pool LAN_POOL
Router(dhcp-config)# network 192.168.0.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.0.1
Router(dhcp-config)# exit
```

1. Configure NAT overload on the router's outside interface.
    
    Why: This simulates the address translation ICS performs.
    

cisco

```
Router(config)# access-list 1 permit 192.168.0.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip nat inside
Router(config-if)# exit
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip nat outside
Router(config-if)# exit
```

1. On each PC in Packet Tracer, open **Desktop → IP Configuration** and select **DHCP**.
    
    Why: This confirms the client obtains an address automatically.
    

## **Verification**

| **Command/Test** | **Expected Result** |
| --- | --- |
| `ipconfig` on client PC (Windows) | IP in 192.168.0.x range, gateway 192.168.0.1 |
| `ipconfig` on PC (Packet Tracer) | IP in 192.168.0.x range, gateway 192.168.0.1 |
| `ping 192.168.0.1` from client | Reply from host/server |
| `ping` to external address | Reply confirms NAT/ICS translation works |
| `show ip dhcp binding` on router | Lists leased addresses for connected PCs |
| `show ip nat translations` on router | Shows active NAT translations |

## **Troubleshooting**

| **Problem** | **Cause** | **Fix** |
| --- | --- | --- |
| ICS Sharing tab missing | ICS service not running | Run `services.msc`, start **Internet Connection Sharing (SharedAccess)** |
| Client cannot get IP | DHCP blocked or not configured | Verify router DHCP pool is active; run `ipconfig /renew` |
| Ping to 192.168.0.1 fails | Client IP or gateway mismatch | Recheck client IP is in 192.168.0.x and gateway is 192.168.0.1 |
| No Internet access on client | NAT not configured | Verify `ip nat inside`/`ip nat outside` on correct interfaces |
| First ping times out | ARP resolution delay | Retry the ping; first packet loss is normal |

## **Viva Q&A**

**Q1: What is Internet Connection Sharing?**

A Windows feature that lets one computer share its Internet connection with other LAN devices.

**Q2: What IP address does ICS assign to the host LAN adapter?**

192.168.0.1 with subnet mask 255.255.255.0.

**Q3: What range do client computers use for static IP addresses under ICS?**

192.168.0.2 to 192.168.0.254, with gateway 192.168.0.1.

**Q4: What is the role of NAT in this experiment?**

NAT translates private LAN addresses to the public address used for Internet access.

**Q5: How does a client obtain its IP address from the ICS host?**

The ICS host runs a DHCP server, and the client uses automatic IP assignment.

**Q6: What command shows active NAT translations on a Cisco router?**

`show ip nat translations`.

## **Result**

The host computer successfully shared its Internet connection with client computers on the LAN, and the same topology was verified in Packet Tracer using DHCP and NAT.

## **Submission Checklist**

- □
    
    ICS enabled on the host computer's Internet-facing adapter
    
- □
    
    Host LAN adapter confirmed at 192.168.0.1 / 255.255.255.0
    
- □
    
    Client PCs configured for automatic IP assignment
    
- □
    
    Client PCs received IP addresses in the 192.168.0.x range
    
- □
    
    Ping test from client to 192.168.0.1 succeeded
    
- □
    
    Packet Tracer topology built with router, switch, and two PCs
    
- □
    
    Router DHCP pool configured and verified with `show ip dhcp binding`
    
- □
    
    NAT overload configured and verified with `show ip nat translations`
    
- □
    
    All Viva questions answered