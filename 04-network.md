# Network Security

Controls covered in this phase:

1. Wi-Fi
2. Ethernet
3. Firewall
4. DNS
5. Remote Access
6. VPN

## 1. Wi-Fi

**Status:** See [Control 1 — Wi-Fi](#control-1--wi-fi) below.

## 2. Ethernet

**Status:** See [Control 2 — Ethernet](#control-2--ethernet) below.

## 3. Firewall

**Status:** See [Control 3 — Firewall](#control-3--firewall) below.

## 4. DNS

**Status:** See [Control 4 — DNS](#control-4--dns) below.

## 5. Remote Access

**Status:** See [Control 5 — Remote Access](#control-5--remote-access) below.

## 6. VPN

**Status:** See [Control 6 — VPN](#control-6--vpn) below.

---

## Control 1 — Wi-Fi

**Phase:** 4 — Network

**Control:** 1 — Wi-Fi

**Objective:** Confirm whether Wi-Fi is in use for development work, and if so, that it is WPA2/WPA3-protected.

**Current Status:** Not Applicable

### 1. Check
```powershell
Get-NetAdapter
netsh wlan show interfaces
```

### 2. Understand
- No wireless network adapter is present on this machine — only Ethernet and virtual adapters (VirtualBox, Hyper-V) plus a disconnected Bluetooth PAN adapter
- `netsh wlan show interfaces` failed because the Wireless AutoConfig Service (wlansvc) is not running, consistent with no Wi-Fi hardware being in use
- All development network traffic on this machine goes over the wired Ethernet connection (see Control 2)

### 3. Fix
- No fix required — not applicable

### 4. Verify
- Confirmed via `Get-NetAdapter` — no adapter with `MediaType` indicating wireless is listed
- Confirmed via `netsh wlan show interfaces` — Wireless AutoConfig Service not running

### 5. Record
| Item | Value |
|---|---|
| Wireless adapter present | No |
| Wi-Fi in use | No |
| Verification Date | August 19, 2026 |
| Status | Not applicable — wired Ethernet only |

---

## Control 2 — Ethernet

**Phase:** 4 — Network

**Control:** 2 — Ethernet

**Objective:** Confirm the wired network adapter in use and its Windows network-category classification.

**Current Status:** Verified

### 1. Check
```powershell
Get-NetAdapter
Get-NetConnectionProfile
```

### 2. Understand
- The active physical adapter is a Realtek Gaming GbE Family Controller
- Windows classifies the active connection's `NetworkCategory` as Public, which applies the stricter default firewall behavior compared to Private/Domain (see Control 3 — Firewall)

### 3. Fix
- No fix required

### 4. Verify
- Re-ran `Get-NetConnectionProfile`; confirmed `NetworkCategory: Public`, `IPv4Connectivity: Internet`

### 5. Record
| Item | Value |
|---|---|
| Adapter | Realtek Gaming GbE Family Controller |
| Adapter Status | Up |
| Network Category | Public |
| Verification Date | August 19, 2026 |
| Status | Verified |

---

## Control 3 — Firewall

**Phase:** 4 — Network

**Control:** 3 — Firewall

**Objective:** Confirm Windows Firewall is enabled and protecting the active network profile.

**Current Status:** Verified

### 1. Check
- This control was already fully audited in [01-computer-security.md, Control 3 — Windows Firewall](01-computer-security.md#control-3--windows-firewall): Domain, Private, and Public profiles all confirmed enabled; firewall service running with Automatic startup; 249 enabled inbound Allow rules reviewed
- Reused that verification here rather than repeating the same commands

### 2. Understand
- The active network profile for this machine is Public (Control 2), so the Public firewall profile is the one that matters day-to-day; it was confirmed enabled in the Control 3 check referenced above

### 3. Fix
- No fix required — see source control for details

### 4. Verify
- See [01-computer-security.md, Control 3](01-computer-security.md#control-3--windows-firewall) for the full check/verify record

### 5. Record
| Item | Value |
|---|---|
| Verification source | 01-computer-security.md, Control 3 — Windows Firewall |
| Public profile firewall | Enabled |
| Verification Date | August 19, 2026 |
| Status | Verified |

---

## Control 4 — DNS

**Phase:** 4 — Network

**Control:** 4 — DNS

**Objective:** Confirm which DNS resolvers are configured for the active network interface.

**Current Status:** Verified

### 1. Check
```powershell
Get-DnsClientServerAddress -AddressFamily IPv4
```

### 2. Understand
- DNS servers determine where domain-name lookups are sent; using the ISP's default resolver can expose browsing patterns to the ISP, while third-party resolvers are a common alternative
- Neither choice is inherently a security requirement for local development, but the configuration should be a deliberate one rather than an unknown default

### 3. Fix
- No fix required — DNS is already configured to well-known public resolvers rather than left at ISP default

### 4. Verify
- Confirmed via `Get-DnsClientServerAddress` for the Ethernet interface

### 5. Record
| Item | Value |
|---|---|
| Interface | Ethernet |
| DNS Servers | 8.8.8.8, 1.1.1.1 |
| Verification Date | August 19, 2026 |
| Status | Verified |

---

## Control 5 — Remote Access

**Phase:** 4 — Network

**Control:** 5 — Remote Access

**Objective:** Ensure remote-access services (Remote Desktop, WinRM, SSH server) are disabled unless explicitly required, since each would allow remote control of or command execution on this machine if reachable.

**Current Status:** Verified

### 1. Check
```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name fDenyTSConnections
Get-Service WinRM
Get-Service | Where-Object { $_.Name -match "sshd|ssh-agent" }
```

### 2. Understand
- `fDenyTSConnections = 1` means Remote Desktop connections are denied (RDP disabled)
- WinRM (used for PowerShell Remoting) is stopped with Manual startup, so it does not start automatically and is not currently listening
- No `sshd` (SSH server) service is installed — only the local `ssh-agent` exists, and it is Stopped/Disabled, which is a credential-caching helper, not a remote-access server

### 3. Fix
- No fix required — all three remote-access surfaces are already disabled or absent

### 4. Verify
- Re-ran the same three checks; results match section 1 above

### 5. Record
| Item | Value |
|---|---|
| Remote Desktop (RDP) | Disabled (`fDenyTSConnections = 1`) |
| WinRM | Stopped, Manual startup |
| SSH server (`sshd`) | Not installed |
| `ssh-agent` | Stopped, Disabled |
| Verification Date | August 19, 2026 |
| Status | Verified — no remote-access services exposed |

---

## Control 6 — VPN

**Phase:** 4 — Network

**Control:** 6 — VPN

**Objective:** Use a VPN that meets Netlit's requirements, once those requirements are confirmed.

**Current Status:** Pending Netlit confirmation

### 1. Check
- No VPN is currently configured
- Netlit has not yet specified: whether a company VPN is provided, whether a commercial VPN is acceptable, required protocol, always-on requirement, kill switch requirement, split tunneling policy, required DNS configuration, required authentication mechanism

### 2. Understand
- Selecting a VPN before these requirements are confirmed risks picking one that doesn't meet Netlit's actual policy
- Tommy asked for a VPN recommendation for use from Bangladesh; the candidates below are for that discussion, not a final selection

### 3. Fix
- Not applicable yet — no fix to apply until requirements are confirmed

### 4. Verify
- Not applicable yet

### 5. Record

| Provider | Notes relevant to Bangladesh use |
|---|---|
| Mullvad | WireGuard-first, independently audited no-log policy, holds up well under ISP throttling common in Bangladesh; no personal-info account signup |
| Proton VPN | Audited no-log policy, WireGuard support, Stealth protocol option to reduce throttling/DPI interference; Swedish/EU jurisdiction may suit a Sweden-based company |
| NordVPN | NordLynx (WireGuard-based), large server network, business/Teams plan available if Netlit wants centrally managed accounts |

All three support WireGuard, which generally performs better than OpenVPN on Bangladeshi ISPs. Final choice follows whatever protocol/kill-switch/split-tunnel/DNS requirements Netlit specifies.

| Item | Value |
|---|---|
| VPN configured | No |
| Requirements confirmed with Netlit | No |
| Verification Date | August 19, 2026 |
| Status | Pending Netlit confirmation of requirements |
