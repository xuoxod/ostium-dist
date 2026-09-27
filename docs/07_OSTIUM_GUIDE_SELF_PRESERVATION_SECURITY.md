# 🛡️ 07_OSTIUM_GUIDE_SELF_PRESERVATION_SECURITY

> **Target Audience:** Systems Architects & Security Engineers  
> **Topic:** Host Self-Awareness Architecture and the Self-Preservation Invariant  

---

## 🏛️ What Is Host Self-Awareness?

When running an active perimeter security platform equipped with the authority to disassociate stations, drop connections, and modify router hardware access control lists, **accidental self-lockout** represents a significant operational risk.

`ostium` enforces an autonomous **Host Identity & Self-Preservation Engine**:
1. At initialization, it interrogates local kernel interfaces to discover the host machine's identity, all physical and virtual network adapters, and every assigned IPv4 and IPv6 address.
2. In visual presentations and terminal dashboards, the host machine is highlighted with a distinct badge: `★ THIS HOST (<hostname>)`.
3. It enforces an unbreakable **Self-Preservation Invariant**: any manual command, automated heuristic, or rogue script attempting to disassociate or blacklist the host's own IP or MAC address is blocked at the application boundary before network frames are dispatched.

---

## 🔍 Inspecting Host Identity

To view the machine's discovered network identity matrix:

```bash
$ ostium whoami

================================================================================
  OSTIUM HOST IDENTITY & SELF-AWARENESS MATRIX
================================================================================
  Hostname:          sentinel-node-01
  Primary Interface: wlan0
  Primary MAC:       58:CE:2A:XX:XX:XX
  Primary LAN IP:    192.168.1.57
  Self Badge:        ★ THIS HOST (sentinel-node-01)
--------------------------------------------------------------------------------
  ACTIVE INTERFACES:
    • lo           MAC: 00:00:00:00:00:00  Up: false Loopback: true
    • eth0         MAC: 52:54:00:XX:XX:XX  Up: true  Loopback: false
    • wlan0        MAC: 58:CE:2A:XX:XX:XX  Up: true  Loopback: false
================================================================================
  Self-Preservation Invariant: ACTIVE (All host MACs and IPs protected from ACL drops)
================================================================================
```

---

## 🚫 Self-Preservation in Action

### 1. Command-Line Interface Guard
If an operator inadvertently executes a disassociation command against the host:
```bash
$ ostium eject 58:CE:2A:XX:XX:XX

🛡️ SELF-PRESERVATION GUARD: Operation prevented by invariant.
MAC 58:CE:2A:XX:XX:XX belongs to this host (sentinel-node-01); self-targeting prohibited.
```
The command terminates immediately without affecting local or network connectivity.

### 2. Live Terminal Radar Guard (`ostium watch`)
* The host machine is pinned to row 1 of the station list and highlighted in **Cyan**.
* If an operator presses **`[K]`** (Kick) or **`[B]`** (Blacklist) while the cursor is positioned on `★ THIS HOST`, the interface triggers an alert:
  > `🛡️ Invariant: MAC belongs to this host; self-targeting prohibited.`
