# Windows 11 Hardening Lab

## Goal
Harden the Windows 11 system against common network-based attacks and verify the results through attack simulation

## Environment
- Windows 11 (target)
- Windows Defender
- Windows Firewall
- Kali Linux (attacker)

## Initial State
Default Windows configuration with standard services enabled

## Actions

- [x] Review open ports and services  
  - Identified listening services including SMB (445) and RPC (135)
  - Verified associated processes using `netstat` and `tasklist`

- [x] Harden firewall rules  
  - Restricted File and Printer Sharing rules to LocalSubnet
  - Removed unnecessary Any → Any exposure

- [x] Disable unnecessary services (e.g. SMB where applicable)  
  - SMBv1 confirmed disabled
  - SMBv2 kept enabled (required by system)

- [ ] Enable Defender protections  
  - To be reviewed and hardened next
     
## Next Steps

- Review Windows Defender Attack Surface Reduction (ASR) rules
- Enable additional logging for blocked connections
- Re-run network scan to validate hardening results
- Document before/after differences

## Verification
- Network scanning before and after hardening
- Observation of blocked connections
- Log review

## Status
This lab is ongoing and updated as I progress.

## What I Aim to Learn
- How exposed a default Windows system is
- How attackers discover targets
- How defensive changes affect attack surface

- ---

> Note: The focus of this lab is on understanding exposure and mitigation trade-offs rather than achieving a fully locked-down system.


## Hands-on: Port & Service Hardening (SMB & RPC)

### Initial Port Scan (netstat)

Initial inspection of listening ports revealed the following notable services:

- TCP 445 (SMB) – LISTENING on all interfaces
- TCP 135 (RPC Endpoint Mapper) – LISTENING on all interfaces

These ports were exposed on `0.0.0.0` and `::`, meaning they accepted connections from any network interface.

---

### SMB (TCP 445) Analysis

Port 445 is used by SMB (Server Message Block), which enables file and printer sharing in Windows environments.

Findings:
- SMBv1 was disabled
- SMBv2 was enabled

Although SMBv2 is more secure, exposing it broadly increases attack surface.  
Firewall rules under **File and Printer Sharing** were reviewed and restricted to `LocalSubnet`.

Actions taken:
- Verified SMBv1 disabled
- Limited SMB access via Windows Defender Firewall

---

### RPC (TCP 135) Analysis

Port 135 is required for Windows RPC services and cannot be fully disabled without breaking system functionality.

Findings:
- Service was running under `svchost.exe`
- Port was listening on all interfaces

Actions taken:
- Firewall rules were reviewed
- RPC access was restricted to `LocalSubnet` only

---

### Result

After hardening:
- Ports 135 and 445 remain technically listening
- External exposure is significantly reduced
- System functionality remains intact

This approach balances security and usability while reducing unnecessary attack vectors.

