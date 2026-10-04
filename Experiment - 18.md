# Experiment - 18

Examining Network Address Translation (NAT)

## Aim

To examine how a NAT border router changes IP addresses as web traffic crosses it, in both directions. Tool: Cisco Packet Tracer Simulation mode.

## Concepts

- **NAT:** Network Address Translation swaps private addresses for public ones at a border router.
- **Inside local address:** The private address of an inside host, as seen inside the network.
- **Inside global address:** The public address that represents the inside host to the outside.
- **ARP and DNS:** ARP (Address Resolution Protocol) finds a MAC address for an IP address. DNS (Domain Name System) finds an IP address for a name.
- **Simulation mode:** Shows each packet step by step. **Capture/Forward** advances one event.

## Topology & Addressing

Customer PC and Customer Server ([www.customerserver.com](http://www.customerserver.com/), HTTP and DNS) connect via a 2960 Customer Switch to the Customer Router (1841, the NAT border router). It links over serial to the ISP Router (1841), which connects to an ISP Switch with the ISP Workstation and ISP Server ([www.ispserver.com](http://www.ispserver.com/), HTTP and DNS).

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Customer PC | NIC | Not given | Not given | Not given |
| Customer Server | NIC | Not given | Not given | Not given |
| Customer Router | Inside / Outside | Not given | Not given | N/A |
| ISP Router | Inside / Outside | Not given | Not given | N/A |
| ISP Workstation | NIC | Not given | Not given | Not given |
| ISP Server | NIC | Not given | Not given | Not given |

⚠️ Note: The raw material gives no addresses, and this experiment reads them from packets. Use the provided activity file with NAT preconfigured; building it manually also needs NAT and routing, which is out of scope. Verify in lab.

## Procedure

1. Check that all link lights are green. If amber, switch between Realtime and Simulation mode until green, then stay in **Simulation mode**.
Why: Traffic fails while links are still starting.
2. Click **Customer PC > Desktop > Web Browser**, enter `www.ispserver.com`, and click **Go**.
Why: This starts an inside-to-outside request.
3. Open the first ARP packet in the Event List and note the IP address Customer PC is trying to resolve.
Why: ARP precedes any frame sent on the LAN.
4. Click **Capture/Forward** twice. Note which device answers the ARP request and the MAC address in the reply.
Why: The reply gives the MAC needed for the DNS request.
5. Click **Capture/Forward** twice. Note the protocol of the new packet (see Outbound PDU Details).
Why: This is the name lookup for [www.ispserver.com](http://www.ispserver.com/).
6. Click **Capture/Forward** twice. Open the packet at the [www.customerserver.com](http://www.customerserver.com/) server, select **Outbound PDU Details**, scroll to the Application Layer, and note the ISP server IP address.
Why: This shows DNS answered the lookup.
7. Click **Capture/Forward** twice. Note why Customer PC sends another ARP request.
Why: The ISP server is on a different network, so the next hop is needed.
8. Click **Capture/Forward** 10 times, until Customer PC creates an HTTP request.
Why: The PC now has all addressing needed.
⚠️ Note: Capture/Forward counts may differ in current Packet Tracer; stop when the HTTP request packet appears. Verify in lab.
9. Click **Capture/Forward** three times. Open the packet at **Customer Router** and note the inside local and inside global addresses of Customer PC.
Why: The router has translated the source address.
10. Click **Capture/Forward** seven times until the HTTP reply reaches Customer Router. Open the packet and compare the addresses.
Why: The reply is translated back before reaching Customer PC.
11. Click **ISP Workstation > Desktop > Web Browser**, enter `www.customerserver.com`, and click **Go** in Simulation mode.
Why: This starts an outside-to-inside request.
12. Step through with **Auto Capture/Play** or **Capture/Forward**. ARP and DNS repeat before the HTTP request is created.
Why: The workstation needs the same addressing information.
13. When the HTTP request arrives at **Customer Router**, open the packet and note the inside local and inside global addresses.
Why: This shows translation for the inside server.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| First ARP target (step 3) | IP of the DNS server, [www.customerserver.com](http://www.customerserver.com/); verify in lab |
| ARP reply sender and MAC (step 4) | Customer Server and its MAC address; verify in lab |
| New packet protocol (step 5) | DNS |
| ISP server IP (step 6) | Address in the DNS reply; record from lab |
| Second ARP reason (step 7) | MAC of the default gateway (Customer Router) is needed |
| Customer PC addresses (step 9) | Private inside local address; different public inside global address; record from lab |
| HTTP reply (step 10) | Addresses are translated back as the packet reaches Customer PC |
| ISP Workstation request (step 13) | Inside global (public) address of Customer Server maps to its inside local (private) address; record from lab |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| No packets in Event List | Not in Simulation mode | Switch to Simulation mode before clicking **Go**. |
| Web page does not load | Links still amber | Toggle Realtime and Simulation until green. |
| Name does not resolve | DNS service off on server | Check Server > Services > DNS; verify in lab. |
| Packet seems lost during stepping | Wrong number of Capture/Forward clicks | Keep clicking until the stated packet appears. |
| Addresses look untranslated | Viewing the wrong side of the router | Check the packet's inbound and outbound PDU tabs. |

## Viva Q&A

1. **Q:** What is NAT? **A:** It translates private IP addresses to public ones at a border router.
2. **Q:** What is an inside local address? **A:** The private address of an inside host.
3. **Q:** What is an inside global address? **A:** The public address representing an inside host.
4. **Q:** Which protocol finds an IP address from a name? **A:** DNS.
5. **Q:** Why does the PC send a second ARP request? **A:** To find the gateway MAC for the remote network.
6. **Q:** Which device is the NAT border router here? **A:** The Customer Router.

## Result

NAT translation was observed in Simulation mode for requests from an inside host and from an outside host.

## Submission Checklist

- [ ]  Topology screenshot (all links green)
- [ ]  ARP request and reply screenshots with answers
- [ ]  DNS packet showing the ISP server IP
- [ ]  Customer Router packet: inside local and global addresses
- [ ]  HTTP reply packet at Customer Router
- [ ]  ISP Workstation request at Customer Router with addresses
- [ ]  Viva answers written