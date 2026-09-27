# 🌐 08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION

> **Namespace:** `ostium::docs::router_diversity`  
> **Target Audience:** Passer-byers, GitHub Cloners, Security Researchers, and Open-Source Contributors  
> **Guiding Principle:** Absolute Architectural Transparency — No Silver Bullets in Heterogeneous Networks  

---

## 🧭 1. Executive Summary: What Works Everywhere vs. What Requires Hardware Integration

When evaluating or deploying `ostium` across real-world home, prosumer, and enterprise networks, it is essential to understand that **there is no universal standard router API in the wild**. Router vendors (Netgear, Asus, Linksys, TP-Link, Eero, Google Nest, Ubiquiti UniFi, MikroTik, pfSense, OPNsense, Cisco, OpenWrt, and ISP carrier gateways from Verizon, AT&T, Comcast Xfinity, Spectrum, and CenturyLink) utilize wildly disparate architectures, protocols, security policies, and administrative interfaces.

To deliver immediate value without requiring brittle vendor-specific hacks, `ostium` is architected across three decoupled operational tiers:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   OSTIUM THREE-TIER COMPATIBILITY ARCHITECTURE          │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 1: UNIVERSAL ZERO-PRIVILEGE DISCOVERY (100% OF ALL NETWORKS)      │
│  • Passive ARP Kernel Interrogation (/proc/net/arp)                   │
│  • RFC 1035 UDP Port 53 Reverse DNS PTR Lease Harvesting               │
│  • Dual-Stack IPv4 Warm & IPv6 Multicast Scout                         │
│  • Sub-microsecond IEEE OUI Vendor Fingerprinting                      │
│  • Local Host Identity Self-Preservation & Friendly Aliases            │
│  ► WORKS ON: Any router, switch, mesh node, or hotspot in existence.   │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 2: WIRELESS INTRUSION DETECTION SYSTEM (WIDS / RF MONITORING)     │
│  • 802.11 Deauth Blizzard Velocity Math (Sliding Window Algorithm)     │
│  • Protected Management Frames (802.11w PMF) Audit                     │
│  • Evil Twin & BSSID Spoofing Detection                                │
│  ► REQUIREMENT: Any Linux-compatible 802.11 Monitor Mode adapter.      │
│  ► FALLBACK: If running over Ethernet or Client Wi-Fi, Scout Mode runs │
│              passively without RF monitoring.                          │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 3: HARDWARE GATEWAY TELEMETRY & ACTIVE ACL EJECTION               │
│  • Gateway CPU, Memory, Optical RX Power, WAN Link Telemetry           │
│  • Real-time Layer-2 Station Ejection & Hardware MAC Blacklisting      │
│  ► CURRENT SUPPORT: Verizon Fios CR1000A/B, G3100, E3200 (QSDK JSON-RPC)│
│  ► EXTENSIBLE: Clean Rust CarrierRpcClient Trait for OpenWrt, AsusWRT, │
│                pfSense, UniFi, and RouterOS contributions.             │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🔍 2. Real-World Router Diversity & Environmental Edge Cases

If you clone or deploy `ostium` on an arbitrary network in the wild, you may encounter specific network behaviors. Below is an engineering breakdown of why they happen and how `ostium` handles them:

### A. AP Client Isolation (Guest Networks & Public Wi-Fi)
* **The Scenario:** You run `ostium scan` or `ostium watch` in a coffee shop, hotel, airport, or on a home router's "Guest Wi-Fi" network.
* **The Symptom:** Only your own machine (`127.0.0.1` / your local IP) and the default gateway appear. No other stations show up.
* **The Reality:** AP Client Isolation (Layer-2 isolation) explicitly blocks station-to-station traffic at the wireless radio layer. The access point drops broadcast and unicast ARP/IP packets between connected wireless clients to protect users from each other.
* **Ostium's Behavior:** Ostium detects that the ARP table only contains the default gateway and safely informs the user without hanging or timing out.

### B. Carrier Gateways with Disabled / Stub Local Reverse DNS
* **The Scenario:** You run `ostium scan` on certain locked-down ISP carrier gateways (such as select AT&T BGW or Comcast Xfinity xFi gateways).
* **The Symptom:** Device IP addresses and MAC addresses are detected instantly, but hostnames show as empty or `Unknown`.
* **The Reality:** The router's embedded DNS forwarder does not serve PTR records for local DHCP leases over UDP port 53, or it forwards all DNS queries to upstream public DNS servers which have no knowledge of RFC 1918 private IP addresses (`192.168.x.x`).
* **Ostium's Solution:** 
  1. Ostium tries RFC 1035 UDP reverse DNS queries directly against the gateway IP with a strict 250ms timeout.
  2. If the gateway refuses to resolve PTR records, Ostium falls back to Layer-2 IEEE OUI vendor resolution.
  3. You can permanently name any device using the local alias manager:
     ```bash
     ostium alias 192.168.1.168 "Rick's Android Phone"
     ```
     This alias is stored locally in `~/.config/ostium/aliases.json` and immediately surfaces in all CLI commands and the Ratatui TUI radar.

### C. Private / Randomized MAC Addresses (iOS 14+, Android 10+, Windows 11)
* **The Scenario:** Modern smartphones and laptops connect to Wi-Fi using randomized MAC addresses ("Private Wi-Fi Address").
* **The Symptom:** The MAC vendor displays as `Locally Administered / Private MAC` rather than `Apple Inc.` or `Samsung Electronics`.
* **Ostium's Advantage:** Because `ostium-scout` queries the router's internal DHCP lease table via reverse DNS, it frequently discovers the true operating-system hostname assigned during DHCP negotiation (e.g. `Rick-Pixel-8`, `MacBook-Pro-M2`), unmasking randomized MAC addresses without intrusive packet sniffing!

### D. Subnet Boundaries and Multi-VLAN Corporate Networks
* **The Scenario:** The network uses multiple subnets or enterprise VLANs (e.g., Management on `10.0.1.0/24`, IoT on `10.0.20.0/24`, Staff on `10.0.50.0/24`).
* **The Reality:** Standard ARP probes cannot cross Layer-3 router hops without an ARP proxy or routed relay.
* **Ostium's Behavior:** Ostium automatically binds to your primary active network interface and scans its native broadcast domain. Multi-interface scanning can be performed sequentially across interfaces.

---

## 🛠️ 3. How to Extend Ostium for Your Router (Cloner's Guide)

Ostium is built from the ground up to be community-driven and modular. If your router is not a Verizon Fios CR1000 or G3100, you can easily write a driver for it!

### Step 1: Implement the `CarrierRpcClient` Trait
In `crates/ostium-carrier-rpc/src/`, all router drivers implement this asynchronous trait:

```rust
use async_trait::async_trait;
use ostium_core::{GatewayTelemetry, OstiumError};

#[async_trait]
pub trait CarrierRpcClient: Send + Sync {
    /// Probe the gateway to verify if this driver matches the hardware
    async fn probe(&self) -> Result<bool, OstiumError>;

    /// Fetch gateway operational health, optical RX power, and WAN status
    async fn get_telemetry(&self) -> Result<GatewayTelemetry, OstiumError>;

    /// Issue an active hardware ACL disassociation / blacklist command
    async fn eject_mac(&self, mac: &str) -> Result<bool, OstiumError>;
}
```

### Step 2: Add Router Fingerprint Signatures
In `crates/ostium-fingerprint/src/dissector.rs`, add your router's hardware markers:
* Default MAC OUI prefix (e.g., `00:1A:2B` for Asus, `E4:F0:42` for Netgear)
* HTTP `Server` banner or HTML title tag
* Unique JSON API response headers or favicon hashes

### Step 3: Verify with TDD and Submit a PR
Every driver in `ostium` is backed by unit and red-team integration tests. Add mock JSON fixtures in `crates/ostium-carrier-rpc/tests/` and run:
```bash
cargo test -p ostium-carrier-rpc
```
Then submit a PR to [`https://github.com/xuoxod/ostium`](https://github.com/xuoxod/ostium)!

---

## 📋 4. Roadmap for Community Driver Support

| Category | Target Platform | Target Protocol | Priority |
| :--- | :--- | :--- | :--- |
| **Open Source** | **OpenWrt (21.02+)** | `ubus` JSON-RPC over HTTP/HTTPS | High |
| **Prosumer** | **pfSense / OPNsense** | REST API / `pfctl` table manipulation | High |
| **Prosumer** | **Ubiquiti UniFi** | UniFi Controller v7+ REST API | High |
| **Consumer** | **AsusWRT / Merlin** | `httpd` hook / NVRAM query | Medium |
| **Enterprise** | **MikroTik RouterOS** | RouterOS REST API (v7+) | Medium |
| **Carrier** | **AT&T BGW-210 / BGW-320**| Web session authentication | Community |
| **Carrier** | **Comcast Xfinity XB7 / XB8**| xFi Gateway session / UPnP | Community |

---

## 🛡️ 5. Architectural Invariant Reminder

Regardless of the router brand, firmware version, or network environment:
1. **Zero Workstation Risk:** Ostium's `HostIdentity` engine guarantees that the host running Ostium can **never** be targeted, blacklisted, or ejected.
2. **Graceful Degradation:** If any advanced feature (WIDS or Carrier RPC) lacks requisite hardware or API access, Ostium degrades gracefully to universal passive discovery.
3. **Forensic Integrity:** All scans, threats, and actions are logged deterministically without external cloud dependencies.
