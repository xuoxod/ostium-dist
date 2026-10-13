# 🗺️ 02_OSTIUM_GUIDE_HARDWARE_MATRIX

> **Tested Hardware, Gateway Compatibility Registry & RF Interfaces**  
> **Guiding Principle:** Decoupled Zero-Privilege Layer Architecture  

> [!NOTE]
> **Legal Trademark & Open Source Attribution Notice:**  
> All product names, logos, brands, and registered trademarks mentioned in this document (including *Verizon®*, *Fios®*, *AT&T®*, *Spectrum®*, *Comcast Xfinity®*, *Asus®*, *Netgear®*, *Ubiquiti®*, *Intel®*, *Qualcomm®*) are property of their respective trademark holders. Their reference herein is strictly for technical interoperability, hardware architecture classification, and fair-use educational compatibility reference under U.S. Trademark Fair Use doctrine. Ostium is an independent, zero-cloud sovereign open system and is **not** affiliated with, endorsed by, or sponsored by any commercial telecommunications providers. Firmware references relate to standard open-source GPL-licensed Linux kernel stacks published by upstream hardware ODMs and carriers (e.g. [Verizon Open Source](https://github.com/verizon), [Arcadyan GPL](https://openwrt.org), and [OpenWrt](https://openwrt.org)).

---

## 🏷️ 1. Gateway Compatibility Registry (Tier 1 vs Tier 2 vs Tier 3)

| Manufacturer / Platform | Router Model | Hardware Architecture | Layer-2 Discovery | WIDS Detection | Hardware Eject (ACL) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Fiber Carrier Gateway** | **CR1000B** (WNC) | Qualcomm IPQ8072A (ARM64) | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax/6E) | ✅ Full (QSDK JSON-RPC) |
| **Fiber Carrier Gateway** | **CR1000A** (Arcadyan) | Qualcomm IPQ8072A (ARM64) | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax/6E) | ✅ Full (QSDK JSON-RPC) |
| **Fiber Carrier Gateway** | **G3100** (Arcadyan) | Qualcomm IPQ8074 (ARM64) | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | ✅ Full (QSDK JSON-RPC) |
| **Carrier Wi-Fi Extender**| **E3200 / CE1000A**| Arcadyan Wi-Fi 6 Extender | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | ✅ Full (QSDK JSON-RPC) |
| **Carrier Legacy Router** | **G1100** (Actiontec)| Broadcom BCM63168 (MIPS) | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ac) | ⚠️ Legacy Web Auth |
| **Cable / DOCSIS Gateway**| **SAC2V1K** (Sagemcom)| Qualcomm IPQ4019 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ac) | ❌ Read-Only Discovery |
| **Telco Fiber Gateway**   | **BGW-210 / BGW-320** | Broadcom BCM4908 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (Wi-Fi 6) | ❌ Read-Only Discovery |
| **Cable Carrier Gateway** | **XB7 / XB8** (Technicolor)| CommScope / Broadcom | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (Wi-Fi 6E) | ❌ Read-Only Discovery |
| **Asus Prosumer**         | **RT-AX88U / RT-AX86U** | Broadcom BCM4908 / AsusWRT | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | 🛠️ Community Driver Planned |
| **Netgear Prosumer**      | **Nighthawk RAX80/120** | Broadcom / Qualcomm | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (802.11ax) | 🛠️ Community Driver Planned |
| **OpenWrt Sovereign**     | **Any router on 21.02+**| MIPS / ARM / x86_64 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (All bands) | 🛠️ `ubus` Driver In Progress |
| **Ubiquiti UniFi**        | **Dream Machine (UDM)** | Annapurna AL324 ARM64 | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (UniFi APs) | 🛠️ Community Driver Planned |
| **Generic / Unknown**     | **Any Router / Hotspot**| Any Architecture | ✅ Full (Sub-ms DNS/ARP) | ✅ Full (With Wi-Fi NIC) | ⚠️ Layer-2 Passive Only |

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

## 🛡️ 3. Carrier Gateway Operational Notes: CR1000A / CR1000B Hardware Stack (Qualcomm IPQ8072A)

When integrating Ostium alongside an upstream defensive DNS resolver behind Tier 1 fiber carrier gateways (such as Arcadyan/WNC CR1000A/B platforms):

1. **DNS Server Menu Fallacy**: `Network Settings > DNS Server` is strictly an internal hostname resolution alias table (`/etc/hosts`). Setting entries here will not alter client DNS resolution or upstream forwarding.
2. **WAN DNS Isolation**: `Network Settings > Network Connections > Broadband Connection (Ethernet) > Settings > IPv4 DNS` configures the upstream WAN resolver. Private RFC 1918 addresses configured here fail because WAN routing drops packets directed to local bridge interfaces. The router then silently falls back to Secondary DNS.
3. **Local DHCP Customization**: The stock gateway firmware locks out custom DHCP Option 6. For whole-home zero-configuration interception, deploy an autonomous sovereign DHCP service on the Ostium node and set `Network Settings > IPv4 Address Distribution > Edit > DHCP Server: Disabled` on the gateway.
4. **IPv6 Leak Elimination**: Dual-stack clients resolve via carrier IPv6 DNS in parallel. Toggle `Network Settings > IPv6 > 1. Enable IPv6 Support` to **Disabled** to eliminate parallel DNS leaks.

---

## 💻 4. Host Operating System Platform Matrix

| Host OS Tier | Supported Architectures | Kernel Interfaces | Zero-Privilege Scout | WIDS RF Monitor Mode | Hardware ACL Eject |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Linux (Tier 1)** | `x86_64`, `aarch64` (Musl static) | `/proc/net/arp`, Netlink, `mac80211` | ✅ Full (<85ns match) | ✅ Full (Native Monitor Mode) | ✅ Full (QSDK JSON-RPC) |
| **macOS (Tier 2)** | Apple Silicon (`aarch64` M1-M4), Intel (`x86_64`) | `sysctl`, `getifaddrs`, CoreWLAN | ✅ Full (Sub-ms DNS/ARP) | ⚠️ Passive Airwaves | 🛠️ In Progress |
| **Windows (Tier 2)**| `x86_64` (Win 10/11 / Server) | Win32 `GetAdaptersAddresses`, `GetIpNetTable2` | ✅ Full (Sub-ms DNS/ARP) | ⚠️ Scout Fallback | 🛠️ In Progress |

> [!TIP] Multi-OS Operational Invariant
> Regardless of the host OS, Ostium guarantees the **Host Self-Preservation Invariant**: the local machine's physical MAC addresses and assigned IPs are permanently whitelisted and mathematically immune to administrative drops or ACL blacklisting.

---

## 📖 Related Documentation
* [08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md](08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md) — Understanding heterogeneous router networks and writing custom drivers.
* [01_OSTIUM_GUIDE_QUICKSTART.md](01_OSTIUM_GUIDE_QUICKSTART.md) — 5-minute setup and CLI walkthrough.
* [11_OSTIUM_GUIDE_CARRIER_GATEWAY_DNS_AND_WIDS.md](11_OSTIUM_GUIDE_CARRIER_GATEWAY_DNS_AND_WIDS.md) — Carrier gateway defensive integration & high-availability dual-node sinkhole architecture.
* [`ostium-carrier-rpc` Documentation](../../crates/ostium-carrier-rpc/README.md) — Gateway RPC driver architecture and carrier firmware integration.
