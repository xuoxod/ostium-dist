# 🖥️ 04_OSTIUM_GUIDE_TUI_REFERENCE

> **Interactive Terminal User Interface Controls & Live Radar Reference**  

---

## ⌨️ Hotkey Action Map

When running `ostium watch`, the following keyboard controls are active:

| Key | Action | Description |
| :---: | :--- | :--- |
| **`K`** | **Kick / Disassociate** | Dispatches a hardware disassociation frame to eject the selected rogue station. |
| **`B`** | **Blacklist MAC** | Appends the selected station's MAC address to the router's permanent hardware ACL deny list. |
| **`P`** | **Audit PMF** | Queries and displays the live 802.11w Protected Management Frames policy status. |
| **`C`** | **Test Chime** | Executes the celestial alert chime to verify system audio notifications. |
| **`Q`** | **Quit** | Restores terminal raw mode, leaves alternate screen, and exits cleanly. |

---

## 📊 Status Indicators

* **TRUSTED (Green)**: Station MAC is pre-approved in `~/.config/ostium/whitelist.json`.
* **ROGUE (Red)**: Unrecognized device connected to your network.
* **SUSPECT_WARDRIVER (Magenta)**: Unrecognized device with weak signal ($\le -75\text{dBm}$) or known offensive RF chipset (Alfa Network / Realtek).
* **BLOCKED (Gray)**: Station is currently restricted by router hardware ACL.
