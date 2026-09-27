# 🖥️ 04_OSTIUM_GUIDE_TUI_REFERENCE

> **Target Audience:** Security Operators & Systems Engineers  
> **Topic:** Terminal User Interface Navigation & Interactive Threat Stream Controls  

---

## ⌨️ Hotkey Navigation Matrix

When running the interactive dashboard (`ostium watch`), the terminal interface provides single-keypress command execution:

| Key | Action | Technical Description |
| :---: | :--- | :--- |
| **`[↑] / [↓]`** | **Station Cursor** | Navigate through the active station table. Host identity is pinned to row 1. |
| **`[K]`** | **Kick / Disassociate** | Dispatches an immediate hardware disassociation request to sever station connection. |
| **`[B]`** | **Blacklist MAC** | Appends the selected station MAC to the gateway's permanent hardware ACL deny list. |
| **`[P]`** | **Audit PMF** | Queries and displays active 802.11w Protected Management Frames policy status. |
| **`[C]`** | **Test Audio Alert** | Triggers an audio notification pulse to verify local sound hardware alerting. |
| **`[Q]`** | **Exit** | Restores terminal state, exits alternate screen buffer, and shuts down cleanly. |

---

## 📊 Station Status Classification

* **TRUSTED (Cyan / Green)**: Station MAC is registered in the operator's verified whitelist (`~/.config/ostium/whitelist.json`) or matches host identity.
* **ROGUE (Red)**: Unrecognized device actively communicating across the network.
* **SUSPECT_WARDRIVER (Magenta)**: Unrecognized station exhibiting weak RSSI ($\le -75\text{dBm}$) or matching offensive wireless chipset hardware signatures.
* **BLOCKED (Gray)**: Station currently restricted by gateway hardware ACL policy.

---

## 🛡️ Host Invariant Guard

The local host running the sentinel is automatically discovered, highlighted with `★ THIS HOST`, and pinned to row 1. Attempting to disassociate (`[K]`) or blacklist (`[B]`) the host machine triggers an instant invariant block, preventing self-lockout.
