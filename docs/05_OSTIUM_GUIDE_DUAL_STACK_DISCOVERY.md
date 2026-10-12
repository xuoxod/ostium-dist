# 🌐 05_OSTIUM_GUIDE_DUAL_STACK_DISCOVERY

> **Target Audience:** Systems Administrators, Network Architects, Security Auditors  
> **Topic:** Dual-Stack IPv4 & IPv6 Active Node Discovery and Low-Power Wakeup Mechanics  

---

## 🧐 The Challenge with Standard Discovery Tools

Standard network discovery tools (such as simple passive ARP lookups) frequently fail to detect modern client devices:

1. **IPv6 Link-Local Operation:** Modern mobile operating systems and IoT devices increasingly route communication over IPv6 link-local addresses without emitting legacy IPv4 broadcast ARP requests.
2. **802.11 DTIM Sleep Cycles:** Battery-powered mobile devices and smart appliances enter deep wireless sleep states, waking only during Access Point Delivery Traffic Indication Message (DTIM) beacon intervals.
3. **Passive Silence:** Devices remain silent on the network until the user actively initiates outbound traffic.

---

## 🚀 Dual-Stack Scout Engine Architecture

`ostium` solves this problem through an asynchronous **Dual-Stack Scout Engine** operating without requiring administrative privileges:

```text
$ ostium scan

Executing Dual-Stack Active Sweep across 10.0.0.0/24 & IPv6 Multicast (wlan0)...
================================================================================
  OSTIUM DUAL-STACK NETWORK NODE DISCOVERY [ACTIVE NODES]
================================================================================
  MAC ADDRESS        IP ADDRESS            VENDOR / HOSTNAME            STATUS
--------------------------------------------------------------------------------
  00:11:22:33:44:01  10.0.0.57             ★ THIS HOST (sentinel-node)  TRUSTED
  00:11:22:33:44:00  10.0.0.1              Carrier Gateway Wi-Fi 6E     TRUSTED
  00:11:22:33:44:10  10.0.0.153            IoT Smart Plug               TRUSTED
  00:11:22:33:44:20  10.0.0.168            Mobile-Station (Handset)     TRUSTED
  00:11:22:33:44:30  10.0.0.182            Media Console                TRUSTED
  00:11:22:33:44:40  10.0.0.183            Network Attached Storage     TRUSTED
  00:11:22:33:44:50  10.0.0.185            Connected Smart Display      TRUSTED
================================================================================
```

### 1. IPv6 All-Nodes Multicast Solicitations (`ff02::1`)
`ostium` transmits an ICMPv6 all-nodes neighbor solicitation frame across the active wireless interface. The wireless access point transmits this frame across the next DTIM beacon interval, waking sleeping mobile devices, media consoles, and smart appliances simultaneously.

### 2. High-Speed Asynchronous IPv4 Subnet Sweeper
In parallel with the IPv6 multicast frame, `ostium` dispatches non-blocking connection probes across the local subnet. This prompts the operating-system kernel to populate its Layer-2 neighbor cache.

### 3. Unified Kernel Neighbor Harvesting & Reverse DNS Resolution
`ostium` interrogates the kernel neighbor table, de-duplicates entries across IPv4 and IPv6, resolves IEEE OUI manufacturer hardware signatures, and performs sub-millisecond RFC 1035 UDP reverse DNS queries directly against the gateway's DHCP lease cache.

---

## 🛠️ Command-Line Usage

```bash
# Scan a specific subnet and interface:
ostium scan --subnet 192.168.1 --interface wlan0

# Generate structured JSON output for automated ingestion:
ostium scan --json
```
