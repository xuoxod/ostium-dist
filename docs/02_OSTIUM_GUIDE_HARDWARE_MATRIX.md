# 🗺️ 02_OSTIUM_GUIDE_HARDWARE_MATRIX

> **Tested Hardware, Gateway Compatibility Registry & RF Interfaces**  
> **Guiding Principle:** Decoupled Zero-Privilege Layer Architecture  

---

## 🏷️ 1. Gateway Compatibility Registry (Tier 1 vs Tier 2 vs Tier 3)

| Manufacturer / Carrier | Router Model | Hardware Platform | Layer-2 Discovery | WIDS Detection | Hardware Eject (ACL) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Verizon Fios** | **CR1000B** | Qualcomm IPQ8072A (ARM64) | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax/6E) | ✅ Full (QSDK JSON-RPC) |
| **Verizon Fios** | **CR1000A** | Arcadyan / Qualcomm IPQ8072A | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax/6E) | ✅ Full (QSDK JSON-RPC) |
| **Verizon Fios** | **G3100** | Arcadyan / Qualcomm IPQ8074 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | ✅ Full (QSDK JSON-RPC) |
| **Verizon Fios** | **E3200 / CE1000A**| Arcadyan Wi-Fi 6 Extender | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | ✅ Full (QSDK JSON-RPC) |
| **Verizon Fios** | **G1100** | Actiontec Broadcom BCM63168 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ac) | ⚠️ Legacy Web Auth |
| **Spectrum / Charter** | **SAC2V1K** | Sagemcom IPQ4019 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ac) | ❌ Read-Only Discovery |
| **AT&T** | **BGW-210 / BGW-320** | Humax / Nokia BCM4908 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (Wi-Fi 6) | ❌ Read-Only Discovery |
| **Comcast Xfinity** | **XB7 / XB8** | Technicolor / CommScope | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (Wi-Fi 6E) | ❌ Read-Only Discovery |
| **Asus** | **RT-AX88U / RT-AX86U** | Broadcom BCM4908 / AsusWRT | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | 🛠️ Community Driver Planned |
| **Netgear** | **Nighthawk RAX80/RAX120** | Broadcom / Qualcomm | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | 🛠️ Community Driver Planned |
| **OpenWrt** | **Any router on 21.02+**| MIPS / ARM / x86_64 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (All bands) | 🛠️ `ubus` Driver In Progress |
| **Ubiquiti** | **UniFi Dream Machine (UDM)**| Annapurna AL324 ARM64 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (UniFi APs) | 🛠️ Community Driver Planned |
| **Generic / Unknown** | **Any Router / Hotspot** | Any Architecture | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (With Wi-Fi NIC) | ⚠️ Layer-2 Passive Only |

---

## 📡 2. Wireless Adapter Compatibility for WIDS (RF Monitor Mode)

While LAN discovery and host identification run over any Ethernet or Wi-Fi adapter without special drivers, **Wireless Intrusion Detection (WIDS)** requires a Wi-Fi chipset capable of 802.11 Monitor Mode on Linux:

| Chipset | Example Adapters | Frequency Bands | Monitor Mode Support | Packet Injection |
| :--- | :--- | :--- | :--- | :--- |
| **MediaTek MT7921 / MT7922** | Alfa AWUS036AXM, internal M.2 | 2.4 GHz / 5 GHz / 6 GHz (Wi-Fi 6E) | ✅ Native Kernel 5.18+ | ✅ Supported |
| **MediaTek MT7612U** | Alfa AWUS036ACM | 2.4 GHz / 5 GHz (802.11ac) | ✅ Native Kernel (`mt76x2u`) | ✅ Supported |
| **Qualcomm Atheros ath9k** | Alfa AWUS036NHA, TP-Link TL-WN722N v1 | 2.4 GHz (802.11n) | ✅ Native Kernel (`ath9k_htc`) | ✅ Supported |
| **Intel AX200 / AX210 / AX1675**| ThinkPad, Dell XPS, Framework M.2 | 2.4 GHz / 5 GHz / 6 GHz | ✅ Native Kernel (`iwlwifi`) | ⚠️ Sniffing Only (No Injection) |
| **Realtek RTL8812AU / RTL8814AU**| Alfa AWUS036ACH, AWUS1900 | 2.4 GHz / 5 GHz | ⚠️ Requires DKMS Driver | ✅ Supported with DKMS |
| **Generic Wired Ethernet** | Realtek, Intel, Broadcom GbE | N/A (Wired 802.3) | ❌ N/A (Scout Mode Auto-Fallback) | ❌ N/A |

---

## 📖 Related Documentation
* [08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md](08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md) — Understanding heterogeneous router networks and writing custom drivers.
* [01_OSTIUM_GUIDE_QUICKSTART.md](01_OSTIUM_GUIDE_QUICKSTART.md) — 5-minute setup and CLI walkthrough.
