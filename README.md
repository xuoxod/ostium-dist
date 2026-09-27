# 🦀 OSTIUM — Sovereign Home Gateway Sentinel & WIDS Defense Platform

> **Namespace:** `ostium` | **Release Version:** `v0.1.0`  
> **Platform Target:** Linux (x86_64 Standalone Executable)  
> **Commercial Hub:** [RMediaTech Sovereign Edge Arsenal](https://rmediatech.com)  

---

## 🏛️ What Is Ostium?

**Ostium** is an autonomous, zero-dependency Rust security platform engineered to emancipate home and enterprise network edges from opaque carrier black-box control. 

Designed specifically for carrier-grade optical gateways (such as the **Verizon Fios CR1000B, CR1000A, and G3100**), Ostium audits, monitors, and protects your wireless airspace and network perimeter in real time.

```text
================================================================================
  OSTIUM GATEWAY TELEMETRY STATUS [IP: 192.168.1.1]
================================================================================
  WAN Link Status:   ONLINE_ACTIVE
  Roundtrip Latency: 57.54 ms
  Optical Connected: YES (Active)
  Active Devices:    18
--------------------------------------------------------------------------------
  MAC ADDRESS        IP ADDRESS            VENDOR                       STATUS
--------------------------------------------------------------------------------
  58:CE:2A:3E:42:7F  192.168.1.57          ★ THIS HOST (xuoux)          TRUSTED
  78:67:0E:BA:7E:74  192.168.1.1           Wistron NeWeb Corp (WNC)     TRUSTED
  90:39:5F:01:25:E0  192.168.1.153         Amazon (AmazonPlug01R2)      TRUSTED
  9E:8C:C6:F3:B7:80  192.168.1.168         Xueux (Android Phone)        TRUSTED
  0C:70:43:B3:47:54  192.168.1.182         Sony Interactive (PS5)       TRUSTED
  68:E1:DC:A7:14:D6  192.168.1.183         Buffalo NAS (LS210D4D6)      TRUSTED
  84:C8:A0:6D:A8:88  192.168.1.185         TCL TV (50Q550G)             TRUSTED
================================================================================
```

---

## 🚀 Quick Download & Verification

Pre-compiled, statically linked binaries are available directly in this repository:

```bash
# 1. Download the release binary and checksum
curl -LO https://github.com/xuoxod/ostium-dist/raw/main/bin/ostium-v0.1.0-linux-x86_64
curl -LO https://github.com/xuoxod/ostium-dist/raw/main/bin/ostium-v0.1.0-linux-x86_64.sha256

# 2. Verify cryptographic SHA-256 integrity
sha256sum -c ostium-v0.1.0-linux-x86_64.sha256

# 3. Make executable
chmod +x ostium-v0.1.0-linux-x86_64
sudo mv ostium-v0.1.0-linux-x86_64 /usr/local/bin/ostium
```

---

## ⚡ Core Capabilities

1. **Carrier Hardware Fingerprinting (`ostium identify`)**:
   Automatically identifies router ODM (Wistron NeWeb Corp vs Arcadyan), SoC architecture (Qualcomm IPQ8072A Quad-Core ARM64), and Linux kernel version.
2. **Dual-Stack Active Node Discovery (`ostium scan`)**:
   Awakens sleeping mobile stations via IPv6 `ff02::1` multicast and sweeps IPv4 subnets concurrently in $<200\text{ms}$.
3. **Automated Reverse DNS & Alias Management (`ostium alias`)**:
   Overcomes Android/iOS MAC Randomization by resolving DHCP hostnames directly via router DNS in $<1\text{ms}$.
4. **Mathematical Self-Preservation**:
   Identifies your host workstation, highlights it in Cyan, and prevents accidental self-ejections or hardware ACL lockouts.
5. **Wireless Intrusion Detection (WIDS)**:
   Detects wardriving sweeps, 802.11 deauthentication floods, Evil Twins, and audits 802.11w PMF protection.
6. **Interactive Terminal Operations Dashboard (`ostium watch`)**:
   High-performance Ratatui TUI with arrow-key station selection, one-key rogue ejections (`[K]`), hardware ACL blacklisting (`[B]`), and celestial chime notifications (`[C]`).

---

## 📖 Public Documentation & Guides

* [01. Quickstart Guide](docs/01_OSTIUM_GUIDE_QUICKSTART.md)
* [02. Supported Hardware & Carrier Router Matrix](docs/02_OSTIUM_GUIDE_HARDWARE_MATRIX.md)
* [03. Wardriving Defense & 802.11w PMF Protocol](docs/03_OSTIUM_GUIDE_WARD_DRIVE_DEFENSE.md)
* [04. Interactive TUI Operations Reference](docs/04_OSTIUM_GUIDE_TUI_REFERENCE.md)
* [05. Dual-Stack Discovery Architecture](docs/05_OSTIUM_GUIDE_DUAL_STACK_DISCOVERY.md)
* [06. Device Naming, Reverse DNS & Aliases](docs/06_OSTIUM_GUIDE_DEVICE_NAMING_AND_ALIASES.md)
* [07. Host Self-Awareness & Self-Preservation Security](docs/07_OSTIUM_GUIDE_SELF_PRESERVATION_SECURITY.md)

---

## ⚖️ License & Distribution

* **Distribution Package:** Provided by [RMediaTech](https://rmediatech.com) for authorized subscribers and sovereign operators.
* **Core Engine:** Proprietary Sovereign Rust Architecture (`xuoxod/ostium`).
