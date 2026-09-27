# 🌐 08_OSTIUM_GUIDE_ROUTER_DIVERSITY_AND_CONTRIBUTION

> **Target Audience:** Systems Architects, Network Engineers, and Open-Source Contributors  
> **Guiding Principle:** Absolute Architectural Transparency — No Silver Bullets in Heterogeneous Networks  

---

## 🧭 1. Executive Summary: What Works Everywhere vs. What Requires Hardware Integration

When evaluating or deploying network defense tools across real-world enterprise, prosumer, and residential environments, it is essential to recognize that **there is no universal standard router API in the wild**. Router vendors (Netgear, Asus, Linksys, TP-Link, Eero, Google Nest, Ubiquiti UniFi, MikroTik, pfSense, OPNsense, Cisco, OpenWrt, and ISP carrier gateways) utilize disparate operating systems, administrative protocols, security policies, and credential models.

To deliver immediate operational value without brittle vendor-specific workarounds, `ostium` is architected across three decoupled capability tiers:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   OSTIUM THREE-TIER COMPATIBILITY ARCHITECTURE          │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 1: UNIVERSAL ZERO-PRIVILEGE DISCOVERY (100% OF ALL NETWORKS)      │
│  • Passive Linux kernel neighbor table inspection                     │
│  • Sub-millisecond RFC 1035 UDP Port 53 Reverse DNS lease harvesting  │
│  • Dual-stack IPv4 sweep & IPv6 all-nodes multicast discovery          │
│  • Microsecond IEEE OUI hardware manufacturer fingerprinting           │
│  • Host identity self-preservation invariant & persistent aliases      │
│  ► WORKS ON: Any router, switch, mesh node, or hotspot in existence.   │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 2: WIRELESS INTRUSION DETECTION SYSTEM (WIDS / RF MONITORING)     │
│  • 802.11 Deauth Burst Velocity Math (Sliding Window Algorithm)        │
│  • Protected Management Frames (802.11w PMF) Audit                     │
│  • Evil Twin & BSSID Spoofing Detection                                │
│  ► REQUIREMENT: Any Linux-compatible 802.11 Monitor Mode adapter.      │
│  ► FALLBACK: If running over Ethernet or Client Wi-Fi, Scout Mode runs │
│              passively without RF monitoring.                          │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 3: HARDWARE GATEWAY TELEMETRY & ACTIVE ACL EJECTION               │
│  • Gateway CPU, Memory, Optical RX Power, WAN Link Telemetry           │
│  • Real-time Station Disassociation & Hardware MAC Blacklist           │
│  ► CURRENT SUPPORT: Carrier Wi-Fi 6/6E QSDK Gateways                   │
│  ► EXTENSIBLE: Clean asynchronous driver interface for OpenWrt,        │
│                AsusWRT, pfSense, UniFi, and RouterOS contributions.    │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🔍 2. Real-World Router Diversity & Environmental Edge Cases

When deploying across unknown networks, operators should be aware of the following network conditions:

### A. Access Point Client Isolation (Guest Networks & Public Wi-Fi)
* **The Scenario:** Running discovery on public hotspots, hotel networks, or dedicated router "Guest SSIDs".
* **The Symptom:** Only the host machine and the default gateway are visible; other connected stations do not appear.
* **The Reality:** AP Client Isolation (Layer-2 station isolation) explicitly prevents peer-to-peer communication at the radio layer. The access point discards unicast and broadcast frames between wireless clients.
* **System Handling:** The scanner recognizes that only the default gateway responds and reports station counts accurately without timing out.

### B. Gateways with Stub / Disabled Local Reverse DNS
* **The Scenario:** Certain locked-down carrier or enterprise routers do not serve PTR records for local DHCP leases over UDP port 53.
* **The Symptom:** IP addresses and MAC hardware vendors are discovered instantly, but operating-system hostnames remain unpopulated.
* **System Handling:**
  1. The scanner executes an RFC 1035 UDP reverse DNS query against the gateway with an immediate 250ms deadline.
  2. If the gateway does not maintain local PTR mappings, the system falls back to IEEE OUI hardware manufacturer resolution.
  3. Operators can permanently map device names using the local alias registry:
     ```bash
     ostium alias 192.168.1.168 "Primary Mobile Device"
     ```
     Aliases persist across reboots in `~/.config/ostium/aliases.json` and immediately reflect across all interfaces.

### C. Private / Randomized MAC Addresses (iOS, Android, Windows)
* **The Scenario:** Modern client devices randomize MAC addresses upon connecting to Wi-Fi.
* **The Symptom:** Hardware manufacturer resolves as `Locally Administered / Private MAC`.
* **The Solution:** Because the discovery engine queries the gateway's DHCP lease cache, it frequently retrieves the true operating-system hostname declared during initial DHCP handshake negotiation, correlating randomized MACs to their true device names.

---

## 🛠️ 3. Implementing Third-Party Router Drivers (Contribution Guide)

The gateway control interface is decoupled via an asynchronous driver trait. Adding support for a new router platform requires implementing three methods:

```rust
use async_trait::async_trait;

#[async_trait]
pub trait CarrierRpcClient: Send + Sync {
    /// Probe the gateway to verify compatibility
    async fn probe(&self) -> Result<bool, GatewayError>;

    /// Fetch operational telemetry (WAN status, optical power, CPU/memory)
    async fn get_telemetry(&self) -> Result<GatewayTelemetry, GatewayError>;

    /// Issue an active hardware ACL disassociation command
    async fn eject_mac(&self, mac: &str) -> Result<bool, GatewayError>;
}
```

### Community Driver Roadmap
* **OpenWrt (21.02+)**: Driver interfacing with `ubus` via authenticated JSON-RPC.
* **pfSense / OPNsense**: Driver utilizing REST API and `pfctl` table management.
* **Ubiquiti UniFi**: Driver integrating with the UniFi Controller API.
* **AsusWRT**: Driver interfacing with HTTP administration hooks.
