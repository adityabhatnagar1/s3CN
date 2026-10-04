# Experiment - 7

# Experiment 7: Performing an Initial Router Configuration

## Aim

To apply an initial configuration to a Cisco router using the IOS CLI: host name, passwords, a MOTD banner, and basic settings. The configuration is then verified and saved.

## Concepts

- **CLI and console:** The CLI (Command Line Interface) is text-based router control. The console port is the direct management connection from a PC.
- **enable password vs enable secret:** Both protect privileged EXEC mode. `secret` is stored hashed (scrambled one-way); if both are set, `secret` is used.
- **Line passwords:** `line console 0` protects the console. `line vty 0 4` protects the five vty (virtual terminal) lines used for Telnet (remote text login).
- **service password-encryption:** Scrambles all clear-text line and enable passwords in the configuration. This is weak, reversible protection.
- **MOTD banner and domain lookup:** The MOTD (message of the day) banner shows before login. `no ip domain-lookup` stops the router treating mistyped commands as host names.

## Topology & Addressing

Customer side: Customer PC, Customer Switch (2960), Customer Router (1841), Customer Server. ISP side: ISP Router (1841), ISP Switch (2960), ISP Workstation, ISP Server. The two routers connect the networks.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| Customer PC | FastEthernet0 | Not given | Not given | Not given |
| Customer Router (1841) | Not given | Not given | Not given | N/A |
| ISP Router (1841) | Not given | Not given | Not given | N/A |
| Customer Switch / ISP Switch (2960) | N/A | N/A | N/A | N/A |
| Customer Server / ISP Server / ISP Workstation | FastEthernet0 | Not given | Not given | Not given |

⚠️ Note: The raw material gives no addresses, and this experiment needs none. Use the provided activity file if available; otherwise verify in lab. The 1841 is an older model; if it is missing from the device list, use the closest router and verify in lab.

## Procedure

1. On Customer PC, open Desktop > Terminal and click OK with default settings to reach the Customer Router console.
Why: The console is a direct management link.
2. Set the host name.

```
   Router>enable                         ! enter privileged EXEC mode
   Router#configure terminal             ! enter global configuration mode
   Router(config)#hostname CustomerRouter ! set the host name
```

Why: A unique name identifies the device in the prompt.
3. Set the privileged mode passwords.

```
   CustomerRouter(config)#enable password cisco     ! clear-text privileged password
   CustomerRouter(config)#enable secret cisco123    ! hashed privileged password
```

Why: `secret` takes priority, so only `cisco123` works for enable.
4. Set the console password.

```
   CustomerRouter(config)#line console 0   ! select the console line
   CustomerRouter(config-line)#password cisco123  ! set the line password
   CustomerRouter(config-line)#login       ! require the password at login
   CustomerRouter(config-line)#exit        ! back to global config
```

Why: Without `login`, the password is never requested.
5. Set the vty password for Telnet.

```
   CustomerRouter(config)#line vty 0 4     ! select all five vty lines
   CustomerRouter(config-line)#password cisco123  ! set the line password
   CustomerRouter(config-line)#login       ! require the password at login
   CustomerRouter(config-line)#exit        ! back to global config
```

Why: Telnet access is refused until a vty password exists.
6. Check that passwords are in clear text.

```
   CustomerRouter(config)#do show running-config   ! view active configuration
```

Why: This shows the risk before encryption.
7. Encrypt all clear-text passwords, then check again.

```
   CustomerRouter(config)#service password-encryption  ! scramble clear-text passwords
   CustomerRouter(config)#do show running-config       ! confirm passwords are scrambled
```

Why: This stops passwords being read over someone's shoulder.
8. Configure the MOTD banner.

```
   CustomerRouter(config)#banner motd $Authorized Access Only!$  ! $ marks start and end of text
```

Why: It warns anyone attempting to log in.
9. Test the banner and passwords: type `exit` twice, read the banner, then log back in with the passwords.
Why: This proves the banner and line password work.
10. Turn off domain lookup.

```
    CustomerRouter(config)#no ip domain-lookup   ! stop DNS lookups of mistyped commands
```

```
Why: A mistyped command such as `emable` otherwise pauses while the router tries to resolve it.
```

1. Save the configuration.

```
    CustomerRouter(config)#end         ! return to privileged EXEC
    CustomerRouter#copy run start      ! save running-config to startup-config
```

```
Why: Unsaved changes are lost on reload.
```

1. Verify: log out of the terminal session, log in with the console password, enter privileged EXEC with the enable password, then click **Check Results**.
Why: This confirms the saved settings work from a fresh login.
⚠️ Note: Check Results exists only in the original activity file. If you built the topology manually, skip it and use the Verification table. Some steps are not graded by Packet Tracer.

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Prompt after step 2 | `CustomerRouter>` or `CustomerRouter#` |
| `show running-config` (before step 7) | `enable password cisco` and line passwords in clear text |
| `show running-config` (after step 7) | Passwords shown as `7` followed by scrambled text; secret shown as a hash |
| `exit` twice | Banner "Authorized Access Only!" appears, then password prompt |
| `enable` | Asks for `cisco123` (the secret), not `cisco` |
| Mistyped command (e.g. `emable`) | `% Invalid input` or unknown command error with no DNS delay; verify in lab |
| `show startup-config` | Contains hostname, banner, and passwords |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| No password prompt at console | `login` not entered | Enter `login` under `line console 0`. |
| `cisco` rejected for enable | `enable secret` overrides `enable password` | Use `cisco123`. |
| Banner not shown | Delimiter missing or mismatched | Re-enter the `banner motd` line with matching `$`. |
| Settings gone after reload | Not saved | Run `copy run start`. |
| `% Invalid input detected` | Wrong mode or typo | Check the prompt; use `?`. |

## Viva Q&A

1. **Q:** Which Cisco IOS CLI commands did you use most? **A:** `enable`, `configure terminal`, `show running-config`, and `copy run start`.
2. **Q:** How can you make the customer router passwords more secure? **A:** Use `enable secret`, longer and complex passwords, and avoid clear-text `enable password`; verify in lab for further options.
3. **Q:** Why is `enable secret` better than `enable password`? **A:** It is stored hashed, not readable.
4. **Q:** What does `login` do under a line? **A:** It forces the password to be entered.
5. **Q:** What is the purpose of `no ip domain-lookup`? **A:** It stops delays when a command is mistyped.
6. **Q:** What does `service password-encryption` do? **A:** It scrambles all clear-text passwords in the configuration.

## Result

The router host name, passwords, banner, and basic settings were configured, saved, and verified successfully.

## Submission Checklist

- [ ]  Topology screenshot
- [ ]  Host name and password configuration commands
- [ ]  `show running-config` before and after encryption
- [ ]  Banner and login test screenshot
- [ ]  `no ip domain-lookup` and `copy run start` output
- [ ]  Check Results screenshot (or manual verification)
- [ ]  Reflection answers written