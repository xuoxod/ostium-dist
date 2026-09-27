# 🌐 05_OSTIUM_GUIDE_DUAL_STACK_DISCOVERY

> **Product:** Ostium Sovereign Sentinel  
> **Audience:** Operators, Security Auditors, Network Administrators  
> **Topic:** Dual-Stack IPv4 & IPv6 Active Node Discovery  

---

## 🧐 Why Traditional Network Scanners Miss Modern Devices

If you have ever run standard network discovery tools (like passive ARP lookups), you may have noticed that half your devices appear to be missing. 

Modern home networks are full of devices that:
* Operate almost exclusively over **IPv6** link-local addresses.
* Enter low-power **Wi-Fi sleep states (802.11 DTIM)** when idle.
* Never send IPv4 broadcast ARP packets unless they are actively loading a webpage.

---

## 🚀 How Ostium Finds Every Device in Under 200ms

Ostium features a specialized **Dual-Stack Scout Engine** designed to awaken and index all active nodes without requiring administrator or root privileges:

```text
$ ostium scan

Executing Dual-Stack Active Sweep across 192.168.1.0/24 & IPv6 Multicast (wlp0s20f3)...
================================================================================
  OSTIUM DUAL-STACK NETWORK NODE DISCOVERY [18 ACTIVE NODES]
================================================================================
  MAC ADDRESS        IP ADDRESS            VENDOR                       STATUS
--------------------------------------------------------------------------------
  58:CE:2A:3E:42:7F  192.168.1.57          ★ THIS HOST (xuoux)          TRUSTED
  78:67:0E:BA:7E:74  192.168.1.1           Wistron NeWeb Corp (WNC)     TRUSTED
  90:39:5F:01:25:E0  192.168.1.153         Amazon (AmazonPlug01R2)      TRUSTED
  ...
  9E:8C:C6:F3:B7:80  192.168.1.168         Xueux (Android Phone)        TRUSTED
  0C:70:43:B3:47:54  192.168.1.182         Sony Interactive (PS5)       TRUSTED
  68:E1:DC:A7:14:D6  192.168.1.183         Buffalo NAS (LS210D4D6)      TRUSTED
  84:C8:A0:6D:A8:88  192.168.1.185         TCL TV (50Q550G)             TRUSTED
================================================================================
```

### 1. IPv6 All-Nodes Multicast Solicitations (`ff02::1`)
Ostium transmits an ICMPv6 all-nodes neighbor solicitation frame across your local Wi-Fi interface. The wireless access point broadcasts this through its DTIM interval, waking up sleeping smartphones, game consoles, and smart appliances.

### 2. High-Speed Asynchronous IPv4 Subnet Sweeper
In parallel with the IPv6 multicast, Ostium initiates asynchronous non-blocking connection probes to every IPv4 host on the `/24` subnet. This forces the operating system kernel to populate its Layer-2 neighbor tables.

### 3. Unified Kernel Neighbor Harvesting
Ostium reads the kernel's neighbor cache, de-duplicates entries across IPv4 and IPv6, resolves IEEE OUI manufacturer hardware signatures, and queries local reverse DNS.

---

## 🛠️ Command-Line Flags

```bash
# Scan a custom subnet and interface:
ostium scan --subnet 192.168.1 --interface wlp0s20f3

# Output raw JSON for script integration:
ostium scan --json
```
