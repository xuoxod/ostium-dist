# 🛡️ 07_OSTIUM_GUIDE_SELF_PRESERVATION_SECURITY

> **Product:** Ostium Sovereign Sentinel  
> **Audience:** Operators & Security Practitioners  
> **Topic:** Host Self-Awareness & Self-Preservation Invariant  

---

## 🏛️ What Is Sentinel Self-Awareness?

When running an active perimeter defense platform that has the authority to kick stations, disassociate wireless clients, and update router hardware ACL blacklists, **accidentally locking yourself out** is one of the highest operational risks.

Ostium introduces a zero-dependency **Host Identity & Self-Preservation Engine**:
1. It automatically discovers your machine's hostname, all physical/virtual network cards, and every assigned IPv4 and IPv6 address.
2. It visually highlights your machine with a prominent badge: `★ THIS HOST (<hostname>)`.
3. It enforces an unbreakable **Self-Preservation Guard**: even if you or an automated script try to eject or blacklist your machine's IP or MAC, the operation is blocked before any command is sent to the router.

---

## 🔍 Inspecting Your Sentinel Identity

To see your machine's network identity snapshot:

```bash
$ ostium whoami

================================================================================
  OSTIUM HOST IDENTITY & SELF-AWARENESS MATRIX
================================================================================
  Hostname:          xuoux
  Primary Interface: wlp0s20f3
  Primary MAC:       58:CE:2A:3E:42:7F
  Primary LAN IP:    192.168.1.57
  Self Badge:        ★ THIS HOST (xuoux)
--------------------------------------------------------------------------------
  ACTIVE INTERFACES (5 TOTAL):
    • lo           MAC: 00:00:00:00:00:00  Up: false Loopback: true
    • virbr0       MAC: 52:54:00:A4:41:F5  Up: true  Loopback: false
    • wlp0s20f3    MAC: 58:CE:2A:3E:42:7F  Up: true  Loopback: false
    ...
================================================================================
  Self-Preservation Invariant: ACTIVE (All host MACs and IPs protected from ACL drops)
================================================================================
```

---

## 🚫 Self-Preservation in Action

### 1. In the CLI
If an operator accidentally runs an eject command targeting the host's own MAC or IP:
```bash
$ ostium eject 58:CE:2A:3E:42:7F

🛡️ SELF-PRESERVATION GUARD: Self-targeting prevented (self-preservation invariant): 
MAC 58:CE:2A:3E:42:7F belongs to this host (xuoux); self-preservation invariant enforced
```
The command terminates instantly with zero impact on network connectivity.

### 2. In the Interactive TUI Dashboard (`ostium watch`)
* Use `[↑]` (Up) and `[↓]` (Down) arrow keys to navigate the active device list.
* `★ THIS HOST` is pinned to row 1 and styled in bright **Cyan**.
* If you press **`[K]`** (Kick) or **`[B]`** (Blacklist) while `★ THIS HOST` is selected, the bottom status line displays:
  > `🛡️ Invariant: MAC 58:CE:2A:3E:42:7F belongs to this host (xuoux); self-preservation invariant enforced`
