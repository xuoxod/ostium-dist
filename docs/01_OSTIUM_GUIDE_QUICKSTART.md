# 🚀 01_OSTIUM_GUIDE_QUICKSTART

> **Target Audience:** Home Network Owners, Systems Engineers, Security Researchers  
> **Platform Support:** Linux (x86_64, aarch64), macOS (Apple Silicon / Intel)  

---

## ⚡ 1. One-Line Installation

Download the verified, statically linked single binary from RMediaTech or GitHub:

```bash
# Download and install to /usr/local/bin
curl -sSL https://rmediatech.com/api/v1/download/ostium -o ostium
chmod +x ostium
sudo mv ostium /usr/local/bin/
```

Verify SHA-256 integrity:
```bash
ostium --version
# Output: ostium 0.1.0 (RMediaTech Sovereign Platform)
```

---

## 🧭 2. First Run: Identify Your Router

Run the non-intrusive hardware fingerprinter to discover your router model, ODM manufacturer, and kernel architecture:

```bash
ostium identify
```

Example Output:
```
================================================================================
  OSTIUM GATEWAY HARDWARE FINGERPRINT
================================================================================
  Gateway IP:        192.168.1.1
  Gateway MAC:       78:67:0E:BA:7E:74
  ODM Manufacturer:  Wistron NeWeb Corp (WNC)
  Carrier Family:    Verizon Fios
  Model Profile:     CR1000B (WNC 10GbE Wi-Fi 6E)
  SoC Architecture:  Qualcomm IPQ8072A (Quad-Core ARM64 Cortex-A53)
  Kernel ABI:        Linux 5.4 QSDK Carrier Kernel
  QSDK Compatible:   YES
  Web Title:         Fios Router
================================================================================
```

---

## 📡 3. Live TUI WIDS Radar

Launch the real-time terminal user interface to view active Wi-Fi stations, track deauth bursts, and manage rogue clients:

```bash
ostium watch
```

* Press `[K]` to instantly kick a rogue station.
* Press `[B]` to permanently ban a MAC from the router hardware ACL.
* Press `[P]` to audit 802.11w Protected Management Frames.
* Press `[Q]` to exit cleanly.
