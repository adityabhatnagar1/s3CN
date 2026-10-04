# Experiment - 10

# Experiment 10: Configuring WEP on a Wireless Router

## Aim

To configure WEP security on a Linksys wireless router and a wireless workstation, then verify connectivity to the customer network. Tool: Cisco Packet Tracer.

## Concepts

- **SSID:** Service Set Identifier, the name of a wireless network. The router and workstation must use the same SSID.
- **WEP:** Wired Equivalent Privacy, an old method that encrypts wireless data using a shared key.
- **Pre-shared key:** A secret entered on both the router and the workstation. Here it is `1a2b3c4d5e`, 10 hexadecimal digits (0-9, a-f).
- **WEP weakness:** WEP keys can be cracked quickly, so WEP is considered insecure.

## Topology & Addressing

The Customer Wireless Workstation connects wirelessly to the Customer Wireless Router (Linksys-WRT300N), which links to the customer network (Customer Switch, Customer Router, Customer Server).

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Customer Wireless Router | LAN | 192.168.2.1 | Not given | N/A |
| Customer Wireless Workstation | Wireless | Not given | Not given | Not given |
| Customer Server | NIC | 192.168.1.10 | Not given | Not given |

⚠️ Note: The raw material gives no masks, no workstation address, and no cable layout. The workstation most likely receives its address from the router by DHCP (automatic addressing); verify in lab. The ISP devices need no configuration.

## Procedure

1. Click **Customer Wireless Router**, then open the **GUI** tab.
Why: The GUI tab is the router's web management page.
2. Click **Wireless** and change Network Name (SSID) from `Default` to `CustomerWireless`. Leave other settings at default.
Why: The workstation must later use this exact name.
3. Click **Save Settings** at the bottom of Basic Wireless Settings.
Why: Unsaved changes are discarded.
4. Open **Wireless > Wireless Security**.
Why: This page holds the current security parameters.
5. In the Security Mode drop-down, select **WEP**.
Why: This makes the router require a key.
⚠️ Note: WEP is obsolete and insecure. In current Packet Tracer the same page also offers WPA2 Personal; use it for real networks, and use WEP here only because this experiment requires it.
6. In the **Key1** box, type `1a2b3c4d5e`.
Why: This is the pre-shared key for the wireless network.
⚠️ Note: Leave the encryption length at its default (expected 40/64-bit for a 10-digit key); verify in lab.
7. Click **Save Settings** at the bottom of Wireless Security.
Why: The security settings apply only after saving.
8. Click **Customer Wireless Workstation**, open the **Config** tab, and click **Wireless**.
Why: This shows the workstation's wireless settings.
⚠️ Note: If no Wireless option appears, the PC has a wired adapter. Power it off, replace the wired module with the Linksys WMP300N wireless module, and power it on; verify in lab.
9. Change the SSID to `CustomerWireless`.
Why: The SSID must match the router.
10. Set Security Mode to **WEP**, enter `1a2b3c4d5e` in the Key box, then close the window.
Why: The key must match the router for the link to form.
⚠️ Note: Some Packet Tracer versions label this field "Authentication"; verify in lab.
11. Confirm that a wireless connection line appears between the workstation and the router.
Why: This shows the SSID and key are accepted.
12. Click **Customer Wireless Workstation > Desktop > Command Prompt** and run the commands below.

```
    ipconfig /all          ! show full network settings of the workstation
    ping 192.168.2.1       ! test the router's LAN interface
```

```
Why: Confirms the workstation has an address and can reach the router.
```

1. Close the Command Prompt, open **Web Browser**, enter `http://192.168.1.10`, and press Enter.
Why: The Intranet page proves the workstation reaches the rest of the customer network.
2. Click **Check Results** at the bottom of the instruction window.
Why: This grades the configuration.
⚠️ Note: Check Results exists only in the original activity file. If you built the topology manually, skip it and use the Verification table.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Wireless link on topology | Connection line between workstation and router |
| `ipconfig /all` | Workstation shows an IP address and gateway on the router's network; verify in lab |
| `ping 192.168.2.1` | Replies received from the router |
| Browser: `http://192.168.1.10` | Customer Intranet page loads |
| Check Results | 100% completion; verify in lab |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| No wireless connection | SSID or key mismatch (both case-sensitive) | Re-enter `CustomerWireless` and `1a2b3c4d5e` on both devices. |
| Router settings revert | Save Settings not clicked | Click **Save Settings** on each page. |
| No Wireless option on workstation | Wired adapter installed | Fit the WMP300N module (step 8 note). |
| Ping to 192.168.2.1 fails | Workstation has no valid IP | Check `ipconfig /all`; set the IP to automatic (DHCP). |
| Web page does not load | URL typo or server unreachable | Check `http://192.168.1.10`; verify Customer Server is on. |

## Viva Q&A

1. **Q:** What is the purpose of using WEP on a wireless network? **A:** It encrypts wireless data so only devices with the key can join and read it.
2. **Q:** What is the significance of the key used to secure WEP? **A:** It is the shared secret both devices must match to connect and decrypt data.
3. **Q:** Is WEP the best choice for wireless security? **A:** No; WEP is weak, and WPA2 or WPA3 should be used.
4. **Q:** What is an SSID? **A:** The name of the wireless network.
5. **Q:** Why must the router and workstation use the same key? **A:** A mismatched key prevents the connection from forming.
6. **Q:** Which command shows the workstation's network settings? **A:** `ipconfig /all`.

## Result

WEP security was configured on the Linksys wireless router and the workstation, and connectivity to the customer network was verified.

## Submission Checklist

- [ ]  Topology screenshot
- [ ]  Router Wireless settings screenshot (SSID `CustomerWireless`)
- [ ]  Router Wireless Security screenshot (WEP, Key1)
- [ ]  Workstation wireless configuration screenshot
- [ ]  `ipconfig /all` and `ping 192.168.2.1` output
- [ ]  Intranet web page screenshot
- [ ]  Check Results screenshot (or manual verification)
- [ ]  Reflection answers written