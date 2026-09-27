# 🚀 01_OSTIUM_GUIDE_QUICKSTART

> **Target Audience:** Network Administrators, Security Researchers, and Systems Engineers  
> **Platform Support:** Linux (x86_64, aarch64) • Native Self-Contained Static Binaries  

---

## ⚡ 1. One-Line Installation

Download the verified, statically linked single binary:

```bash
# Download and install to /usr/local/bin
curl -sSL https://github.com/xuoxod/ostium-dist/releases/latest/download/ostium -o ostium
chmod +x ostium
sudo mv ostium /usr/local/bin/
```

Verify SHA-256 integrity and installation:
```bash
ostium --version
# Output: ostium 0.1.0
```

---

## 🧭 2. First Run: Identify Your Gateway

Execute the non-intrusive hardware fingerprinter to discover your router model, ODM hardware platform, and operating-system kernel profile:

```bash
ostium identify
```

Example Output:
```text
================================================================================
  OSTIUM GATEWAY HARDWARE FINGERPRINT
================================================================================
  Gateway IP:        192.168.1.1
  Gateway MAC:       78:67:0E:XX:XX:XX
  ODM Manufacturer:  Wistron NeWeb Corp (WNC)
  Carrier Family:    Carrier Wi-Fi 6E Gateway
  Model Profile:     CR1000B (10GbE Wi-Fi 6E)
  SoC Architecture:  High-Throughput Quad-Core ARM64 Networking Platform
  Kernel ABI:        Linux Embedded Carrier Kernel
  Hardware ACL:      SUPPORTED
  Web Interface:     Standard Gateway Administrative Portal
================================================================================
```

---

## 📡 3. Live Threat Radar & Station Sentinel

Launch the real-time interactive terminal radar to inspect active stations, detect deauthentication attacks, and manage access lists:

```bash
ostium watch
```

* Press **`[K]`** to instantly disassociate a rogue station.
* Press **`[B]`** to permanently append a station to the router's hardware ACL deny list.
* Press **`[P]`** to audit 802.11w Protected Management Frames policy.
* Press **`[Q]`** to cleanly restore the terminal and exit.
