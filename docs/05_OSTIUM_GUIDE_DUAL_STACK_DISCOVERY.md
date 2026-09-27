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

Executing Dual-Stack Active Sweep across 192.168.1.0/24 & IPv6 Multicast (wlan0)...
================================================================================
  OSTIUM DUAL-STACK NETWORK NODE DISCOVERY [ACTIVE NODES]
================================================================================
  MAC ADDRESS        IP ADDRESS            VENDOR / HOSTNAME            STATUS
--------------------------------------------------------------------------------
  58:CE:2A:XX:XX:XX  192.168.1.57          ★ THIS HOST (sentinel-node)  TRUSTED
  78:67:0E:XX:XX:XX  192.168.1.1           Carrier Gateway Wi-Fi 6E     TRUSTED
  90:39:5F:XX:XX:XX  192.168.1.153         IoT Smart Plug               TRUSTED
  9E:8C:C6:XX:XX:XX  192.168.1.168         Mobile-Station (Handset)     TRUSTED
  0C:70:43:XX:XX:XX  192.168.1.182         Media Console                TRUSTED
  68:E1:DC:XX:XX:XX  192.168.1.183         Network Attached Storage     TRUSTED
  84:C8:A0:XX:XX:XX  192.168.1.185         Connected Smart Display      TRUSTED
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
