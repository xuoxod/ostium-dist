# 🦀 OSTIUM — Sovereign Home Gateway Sentinel & WIDS Defense Platform

> **Namespace:** `ostium` | **Platform Target:** Linux (x86_64 / ARM64 Musl)  
> **Directives:** Inherits `AGY-RULE-SOVEREIGN-FLAGSHIP-01` & `AGY-RULE-SOVEREIGN-SECURITY-01`  
> **Status:** PRODUCTION READY • 100% TDD TESTED • RED-TEAM ADVERSARIAL VERIFIED  

---

## 🏛️ 1. Project Topology & Crate Registry

`ostium` enforces strict **One-Job-Principle (OJP)** boundaries with zero bloat across 7 modular crates:

| Crate | Directory | Sovereign Role |
| :--- | :--- | :--- |
| **`ostium-core`** | [`crates/ostium-core`](crates/ostium-core) | Domain models, `HostIdentity` self-awareness, self-preservation invariant, forensic logging. |
| **`ostium-fingerprint`** | [`crates/ostium-fingerprint`](crates/ostium-fingerprint) | "Which Router Is This?" Layer-2 OUI lookup, ARP probe, HTTP signatures, magic header dissector. |
| **`ostium-wids`** | [`crates/ostium-wids`](crates/ostium-wids) | Wireless Intrusion Detection System, deauth burst velocity math, 802.11w PMF validator. |
| **`ostium-carrier-rpc`** | [`crates/ostium-carrier-rpc`](crates/ostium-carrier-rpc) | Gateway telemetry polling & hardware ACL disassociation dispatcher. |
| **`ostium-scout`** | [`crates/ostium-scout`](crates/ostium-scout) | Dual-stack IPv4/IPv6 active discovery, RFC 1035 UDP reverse DNS resolver, alias manager. |
| **`ostium-tui`** | [`crates/ostium-tui`](crates/ostium-tui) | Reactive Ratatui/Crossterm terminal UI factory (Station radar, threat stream, cursor navigation). |
| **`ostium-redteam`** | [`crates/ostium-redteam`](crates/ostium-redteam) | Adversarial attack fixtures (Deauth blizzard, MAC whack-a-mole, OUI fuzzing, self-targeting). |
| **`ostium` (cli)** | [`src/main.rs`](src/main.rs) | Unified sovereign binary (`identify`, `whoami`, `status`, `scan`, `watch`, `alias`, `audit`, `eject`). |

---

## 🧭 2. Fast Commands

```bash
# 1. Non-intrusively identify your home router hardware (e.g. Verizon CR1000B QSDK)
ostium identify

# 2. Display host machine network identity, interfaces, and self-preservation status
ostium whoami

# 3. Discover all active nodes via dual-stack IPv4 sweep and IPv6 multicast
ostium scan

# 4. Inspect operational gateway status, WAN link, and optical RX power
ostium status

# 5. Launch the live interactive Ratatui WIDS Radar & Station Sentinel dashboard
ostium watch

# 6. Assign friendly nicknames to devices (by MAC or IP)
ostium alias 192.168.1.168 "Rick's Android Phone"
ostium aliases

# 7. Approve all current legitimate devices into the trusted whitelist
ostium trust-all

# 8. Run a comprehensive wireless security audit (PMF 802.11w, Rogue MAC scan)
ostium audit

# 9. Disassociate and blacklist a rogue MAC in the gateway ACL
ostium eject 00:C0:CA:99:12:44
```

---

## 📚 3. Architectural Specifications

* [00_OSTIUM_SPEC_CHARTER.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/00_OSTIUM_SPEC_CHARTER.md)
* [01_OSTIUM_SPEC_WORKSPACE_TOPOLOGY.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/01_OSTIUM_SPEC_WORKSPACE_TOPOLOGY.md)
* [02_OSTIUM_SPEC_ROUTER_FINGERPRINT.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/02_OSTIUM_SPEC_ROUTER_FINGERPRINT.md)
* [03_OSTIUM_SPEC_WIDS_DEFENSE.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/03_OSTIUM_SPEC_WIDS_DEFENSE.md)
* [04_OSTIUM_SPEC_TUI_FACTORY.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/04_OSTIUM_SPEC_TUI_FACTORY.md)
* [05_OSTIUM_SPEC_CARRIER_RPC.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/05_OSTIUM_SPEC_CARRIER_RPC.md)
* [06_OSTIUM_SPEC_REDTEAM_POC.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/06_OSTIUM_SPEC_REDTEAM_POC.md)
* [07_OSTIUM_SPEC_DUAL_STACK_SCOUT.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/07_OSTIUM_SPEC_DUAL_STACK_SCOUT.md)
* [08_OSTIUM_SPEC_SELF_AWARENESS_AND_INVARIANTS.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/08_OSTIUM_SPEC_SELF_AWARENESS_AND_INVARIANTS.md)
* [09_OSTIUM_SPEC_REVERSE_DNS_AND_DEVICE_CLASSIFICATION.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/specs/09_OSTIUM_SPEC_REVERSE_DNS_AND_DEVICE_CLASSIFICATION.md)

---

## 📖 4. Public End-User Guides

* [01_OSTIUM_GUIDE_QUICKSTART.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/01_OSTIUM_GUIDE_QUICKSTART.md)
* [02_OSTIUM_GUIDE_HARDWARE_MATRIX.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/02_OSTIUM_GUIDE_HARDWARE_MATRIX.md)
* [03_OSTIUM_GUIDE_WARD_DRIVE_DEFENSE.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/03_OSTIUM_GUIDE_WARD_DRIVE_DEFENSE.md)
* [04_OSTIUM_GUIDE_TUI_REFERENCE.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/04_OSTIUM_GUIDE_TUI_REFERENCE.md)
* [05_OSTIUM_GUIDE_DUAL_STACK_DISCOVERY.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/05_OSTIUM_GUIDE_DUAL_STACK_DISCOVERY.md)
* [06_OSTIUM_GUIDE_DEVICE_NAMING_AND_ALIASES.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/06_OSTIUM_GUIDE_DEVICE_NAMING_AND_ALIASES.md)
* [07_OSTIUM_GUIDE_SELF_PRESERVATION_SECURITY.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/07_OSTIUM_GUIDE_SELF_PRESERVATION_SECURITY.md)
* [08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md)

---

## 🌐 5. Router Diversity & Compatibility In The Wild

> **Architectural Transparency:** No single security tool can interface with every proprietary router API in the wild out-of-the-box. `ostium` addresses this through a decoupled 3-tier capability model:

1. **Tier 1: Universal Zero-Privilege Discovery (100% of All Routers & Networks)**
   * Works unconditionally on **any** router, switch, or mesh system (Netgear, Asus, Linksys, TP-Link, Eero, Google Nest, pfSense, OPNsense, Ubiquiti UniFi, MikroTik, Cisco, OpenWrt, DD-WRT, and carrier gateways from Verizon, AT&T, Comcast Xfinity, Spectrum, etc.).
   * Passive kernel ARP table analysis (`/proc/net/arp`), sub-millisecond RFC 1035 UDP port 53 Reverse DNS PTR lease harvesting, dual-stack IPv4 sweep/IPv6 multicast, IEEE OUI identification, and local alias overrides. No passwords or API keys needed.
2. **Tier 2: Wireless Intrusion Detection (WIDS) & Threat Radar**
   * Works on any wireless network using a Linux-compatible **802.11 Monitor Mode** Wi-Fi adapter (Alfa, Intel AX200/AX210, MediaTek MT7921/MT7612U). Detects deauth floods, rogue APs, and PMF 802.11w compliance. (Gracefully falls back to pure Scout mode over wired Ethernet).
3. **Tier 3: Carrier Hardware Telemetry & Layer-2 Hardware ACL Ejection**
   * Hardware ACL disassociation requires router-specific APIs. Currently supported out-of-the-box for **Verizon Fios CR1000A/B, G3100, and E3200**.
   * **Cloner & Contributor Friendly:** Easily extensible via the `CarrierRpcClient` trait in `crates/ostium-carrier-rpc`. See [08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md](file:///home/emhcet/private/projects/desktop/rust/ostium/docs/public/08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION.md) for step-by-step instructions.
