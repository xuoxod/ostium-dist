# 🏛️ OSTIUM — Operational Scope, Technical Limits & Target Beneficiaries

> **Specification Standard:** `OSTIUM-SPEC-SCOPE-01`  
> **Security Protocol:** `SOVEREIGN-SANITIZE-01`  
> **Target Audience:** Home network operators, independent engineers, security researchers, multi-dwelling network administrators  

---

## 🎯 1. Target Beneficiaries: Who Benefits Most from Ostium?

Modern consumer routers and ISP-provided gateways operate as black boxes. They provide either zero real-time visibility into wireless station activities or force operators into invasive proprietary cloud apps that harvest telemetry, monetize network data, and charge monthly subscriptions for basic perimeter security.

`ostium` was engineered from the ground up to liberate operators with an autonomous, zero-cloud, local-first perimeter sentinel. The platform provides maximum value to four key user archetypes:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                          OSTIUM PRIMARY BENEFICIARY ARCHETYPES                         │
├──────────────────────────────┬─────────────────────────────────────────────────────────┤
│ ARCHETYPE                    │ PRIMARY BENEFIT & OPERATIONAL SCENARIO                  │
├──────────────────────────────┼─────────────────────────────────────────────────────────┤
│ 1. Shared Residential Wi-Fi  │ • Multi-dwelling units (duplexes, tenant apartments).   │
│    Operators (Neighbor/Guest)│ • Whitelisting neighbor devices (Smart TVs, handsets).  │
│                              │ • Distinguishing legitimate neighbor streaming from     │
│                              │   unauthorized drive-by eavesdroppers or rogue probes.  │
├──────────────────────────────┼─────────────────────────────────────────────────────────┤
│ 2. Smart Home & IoT Fleet    │ • High-density smart homes (smart plugs, cameras, TVs). │
│    Managers                  │ • Continuous L2/L3 inventory tracking via SQLite WAL.   │
│                              │ • Alerting when unrecognized IoT MACs appear on LAN.    │
├──────────────────────────────┼─────────────────────────────────────────────────────────┤
│ 3. Remote Engineers &        │ • Zero-trust perimeter awareness for home offices.      │
│    Sovereign Operators       │ • Zero external cloud telemetry or third-party SDKs.    │
│                              │ • Single static binary (Musl PIE) with zero dependencies.│
├──────────────────────────────┼─────────────────────────────────────────────────────────┤
│ 4. Wireless Security         │ • Mathematical 802.11 deauthentication burst detection. │
│    Auditors & Researchers    │ • 802.11w Protected Management Frames (PMF) auditing.   │
│                              │ • Empirical threat forensic logging and rapid response. │
└──────────────────────────────┴─────────────────────────────────────────────────────────┘
```

### Archetype 1: Shared Residential Wi-Fi (The Cooperative Network)
In shared residential environments—such as sharing a high-speed fiber connection with an upstairs neighbor, family member, or tenant—the operator faces a distinct challenge:
- Legitimate third-party devices (e.g. neighbor Smart TVs, streaming dongles, and mobile phones) connect and disconnect intermittently throughout the day.
- Traditional commercial security systems either flag all unknown devices as hostile intruders or allow everything indiscriminately without audit logging.
- `ostium` provides the **Persistent Friendly Alias Registry** (`ostium alias`) and **Granular Trust Model** (`ostium trust-all` / `ostium status`). Operators permanently map neighbor devices (`"Neighbor TCL 4K TV"`, `"Neighbor Galaxy Handset"`) into local encrypted state, ensuring neighbor entertainment systems stream uninhibited while unauthorized strangers attempting to piggyback are immediately identified, flagged, and ejected.

### Archetype 2: High-Density IoT Environments
Modern homes host dozens of low-power appliances, many of which use randomized MAC addresses or obscure Wi-Fi chipsets:
- `ostium` resolves randomized MAC obfuscation by interrogating internal gateway reverse DNS PTR lease records over sub-millisecond UDP Port 53 queries.
- Discovered devices are matched against IEEE OUI manufacturer databases and persistent SQLite historical records.
- Sleep-state IoT sensors (waking only on 802.11 DTIM intervals) are cataloged through non-disruptive IPv6 All-Nodes multicast solicitations.

---

## ⚠️ 2. Technical Boundaries & Operational Limits

In accordance with sovereign software integrity, `ostium` documents its precise operational limits and engineering trade-offs transparently.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                         OSTIUM THREE-TIER CAPABILITY MATRIX                            │
├─────────┬──────────────────────────────┬────────────────────────┬──────────────────────┤
│ TIER    │ CAPABILITY                   │ HARDWARE REQUIREMENT   │ WIRELESS INTERFACE   │
├─────────┼──────────────────────────────┼────────────────────────┼──────────────────────┤
│ TIER 1  │ Zero-Privilege Scout Scan    │ Any standard NIC       │ Client / Ethernet    │
│ TIER 2  │ WIDS RF Threat Radar         │ Monitor Mode chipset   │ mac80211 / nl80211   │
│ TIER 3  │ Carrier Gateway Ejection     │ Supported Gateway RPC  │ Remote RPC via LAN   │
└─────────┴──────────────────────────────┴────────────────────────┴──────────────────────┘
```

### Limit 1: Tier 1 Zero-Privilege Discovery Boundaries
- **Local Broadcast Domain**: Tier 1 discovery operates strictly within the broadcast domain (L2/L3 subnet) of the network adapter. It cannot traverse non-routed VLAN boundaries or upstream ISP WAN interfaces without routing privileges.
- **Firewalled Endpoints**: Stations that strictly drop all ICMP Echo Requests and do not communicate on the L2 broadcast domain will only be discovered when they generate network traffic (via kernel neighbor ARP/NDP observation) or request DHCP lease renewal.

### Limit 2: Tier 2 RF Monitoring (WIDS) Hardware Requirements
- **Physical Monitor Mode Requirement**: Capturing raw 802.11 management frames (Deauth, Disassoc, Beacon, Probe Request) requires a wireless network interface controller (NIC) that supports **802.11 Monitor Mode** (e.g. Qualcomm Atheros ath9k/ath10k, MediaTek mt76, Realtek rtl8812au).
- **Graceful Fallback**: If `ostium` is executed on a host with a standard managed Wi-Fi adapter or over wired Ethernet, the WIDS subsystem automatically and safely degrades to high-speed L2/L3 Scout discovery without panicking or crashing.
- **Physical Proximity**: RF detection is governed by physical radio signal propagation. If a rogue attacker is transmitting wardriving deauthentication frames on a remote channel or outside the physical reception range of the monitoring antenna, those frames cannot be intercepted.

### Limit 3: Tier 3 Gateway Telemetry & Hardware ACL Ejection
- **Gateway Driver Compatibility**: Hardware-level station disassociation (`ostium eject`) and hardware MAC access control list (ACL) manipulation require direct communication with the gateway's management interface.
- **Pre-Integrated Out-of-the-Box**: Carrier Wi-Fi 6/6E QSDK gateways (Qualcomm Networking Pro / WNC CR1000B platforms) are natively supported via asynchronous RPC.
- **Closed Proprietary Consumer Mesh**: Consumer mesh routers with closed proprietary cloud backends (e.g., Google Nest Wi-Fi, Amazon eero) that do not expose local RPC, SSH, or administrative APIs cannot be commanded to disassociate stations at the hardware radio layer. For such networks, `ostium` provides clean asynchronous Rust driver traits allowing operators to connect custom OpenWrt, pfSense, OPNsense, or RouterOS scripts.
- **Self-Preservation Protection**: Under no circumstances will `ostium` permit an operator to eject or blacklist their own host machine. The `HostIdentity::assert_not_self()` invariant mathematically asserts machine identity before dispatching any disassociation or ACL blacklist command.

### Limit 4: Scope of Security Inspection
- `ostium` is an **L2/L3 Network Sentinel and 802.11 WIDS Platform**, **NOT** a Deep Packet Inspection (DPI) sniffer.
- `ostium` does not intercept, decrypt, or analyze HTTPS/TLS application payloads.
- `ostium` does not replace enterprise border firewalls (such as `bastion`) or authoritative defensive DNS sinks (such as `aegis`). Instead, it forms the frontline Layer 2/3 wireless perimeter shield.

---

## 🌍 3. Platform Architecture & Multi-OS Agnostic Strategy

While production releases (`v0.1.0`, `v0.1.1`, and `v0.1.2`) provide first-class native pre-compiled binaries across **Linux (x86_64 and aarch64)**, **Windows (x86_64)**, and **macOS (Apple Silicon & Intel)**, the overarching architecture of Ostium is designed for radical multi-OS agnosticism.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        CROSS-PLATFORM ARCHITECTURAL TOPOLOGY                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│                       SHARED AGNOSTIC RUST CORE (100% PORTABLE)                         │
│  • SQLite WAL State Machine (`ostium-db`) • Sliding Window Math (`ostium-wids`)         │
│  • RFC 1035 UDP DNS Parser (`ostium-scout`) • IEEE OUI Classifier (`ostium-fingerprint`)│
│  • Ratatui ANSI Command Bridge (`ostium watch`) • Alias Registry Engine                │
├──────────────────────────┬──────────────────────────────┬──────────────────────────────┤
│ LINUX PLATFORM (TIER 1)  │ WINDOWS PLATFORM (TIER 2)    │ MACOS PLATFORM (TIER 2)      │
├──────────────────────────┼──────────────────────────────┼──────────────────────────────┤
│ • Native Musl Static PIE │ • Native Win32 Console       │ • Universal Mach-O Binary    │
│ • /proc/net/fib_trie     │ • Win32 GetAdaptersAddresses │ • getifaddrs / sysctl routing│
│ • Netlink / AF_PACKET    │ • Windows Npcap / WinPcap    │ • BPF (Berkeley Packet Filter│
│ • mac80211 / nl80211     │ • Native WlanAPI (NativeWifi)│ • CoreWLAN Framework         │
└──────────────────────────┴──────────────────────────────┴──────────────────────────────┘
```

### Why Linux Native First?
1. **Direct Kernel Transparency**: Linux provides zero-overhead access to kernel network neighbor tables (`/proc/net/fib_trie`, `/proc/net/arp`) without proprietary driver wrappers or closed-source system calls.
2. **Deterministic Musl Static Compilation**: Statically linked Musl binaries (`x86_64` and `aarch64`) run identically across Debian, Ubuntu, Alpine, Fedora, Arch, and custom embedded OpenWrt systems without runtime dependencies (`glibc`, dynamic linkers, or C runtime bloat).
3. **Open 802.11 Monitor Mode Standards**: The Linux `nl80211` / `mac80211` wireless stack allows uninhibited raw frame capture for WIDS analysis.

### Multi-OS Testbed Strategy (Linux, Windows & macOS)
The sovereign engineering workspace actively maintains physical Linux workstations, physical Windows systems, and macOS virtualization testbeds. This multi-OS fleet enables empirical validation across diverse operational topologies:

1. **Windows OS Deployment**:
   - The core engine (database, OUI lookup, DNS reverse resolver, and TUI) compiles natively to `x86_64-pc-windows-gnu` / `x86_64-pc-windows-msvc`.
   - Host identity discovery bridges to Win32 `GetAdaptersAddresses` and `GetIpNetTable`.
   - WIDS packet ingestion bridges to Npcap or the native Windows `WlanAPI` subsystem.
2. **macOS Deployment**:
   - Compiles cleanly to `aarch64-apple-darwin` (Apple Silicon) and `x86_64-apple-darwin`.
   - Host identity utilizes BSD `getifaddrs` and routing sockets.
   - Passive frame capture interfaces with `/dev/bpf*` and Apple's `CoreWLAN` framework.
3. **Cross-Compilation Infrastructure**:
   - Releases are compiled deterministically using `cargo-zigbuild` and static toolchains.
   - All cryptographic release signatures (`SOVEREIGN-SIGN-01`) and SHA-256 manifests remain identical across target platforms.

---

## 🔒 4. Data Sovereignty & Anti-Cloud Guarantee

1. **Zero Public Cloud Telemetry**: `ostium` contains zero tracking SDKs, zero analytics beacons, and zero connection attempts to third-party cloud infrastructure.
2. **Local Cryptographic Custody**: All station records, neighbor aliases, and intrusion incident reports remain strictly on the host filesystem in `~/.config/ostium/ostium.db` (SQLite WAL mode).
3. **Deterministic Clean Removal**: Deleting `~/.config/ostium` instantly and completely erases all local state and audit history without residual registry or background daemon sprawl.
