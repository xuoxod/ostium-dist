# 📜 Changelog — OSTIUM Defense Platform

All notable changes to the `ostium` project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.1.1] - 2026-10-12

### 🛡️ Added
- **Private SQLite WAL Persistence Engine (`crates/ostium-db`)**:
  - Implemented high-concurrency, zero-sprawl SQLite database engine with Write-Ahead Logging (WAL) enabled at `~/.config/ostium/ostium.db`.
  - Schema tables: `stations` (L2/L3 inventory, RSSI, trust status), `lease_history` (DHCP lifecycle telemetry), `aliases` (persistent friendly device nicknames), and `wids_incidents` (tamper-evident intrusion audit trail).
  - Sub-millisecond atomic transactional queries with zero external database server dependencies.
- **Autonomous Perimeter Sentinel (`ostium sentinel` / `ostium guard`)**:
  - Continuous foreground and background daemon monitor with configurable evaluation intervals (`--interval <seconds>`).
  - Automated rogue station detection: actively cross-references active stations against the trusted database whitelist.
  - Autonomous hardware defense: automatically dispatches 802.11 disassociation RPCs and hardware MAC ACL blacklisting to the network gateway.
  - Immutable incident recording: automatically commits incident logs with timestamps, MAC, IP, and threat classification into `wids_incidents`.
  - Tactical audio integration: triggers `gemini-chime` upon detecting or mitigating intrusion events.
- **Enhanced Device Discovery & Fingerprinting**:
  - Expanded IEEE OUI hardware fingerprinting engine for HiSense Smart TVs (`88:B8:63`), Amazon Fire TV and Echo devices, Samsung Galaxy handsets, and TCL Smart Displays.
  - Asynchronous RFC 1035 UDP reverse DNS PTR queries to router port 53, stripping randomized MAC evasion by resolving DHCP hostnames.
- **Crucible Empirical Security Verification**:
  - Verified all 47 adversarial unit and integration tests across formula injection, ANSI log poisoning, sliding-window deauth flood defense, and allocation boundary guardrails.
  - Published comprehensive audit report in [`docs/09_OSTIUM_CRUCIBLE_EMPIRICAL_AUDIT.md`](docs/09_OSTIUM_CRUCIBLE_EMPIRICAL_AUDIT.md).
- **Scope, Limits & Beneficiaries Specification**:
  - Published [`docs/10_OSTIUM_SCOPE_LIMITS_AND_BENEFICIARIES.md`](docs/10_OSTIUM_SCOPE_LIMITS_AND_BENEFICIARIES.md) defining operational tiers, target beneficiaries (shared Wi-Fi, smart homes), and multi-OS roadmap (Linux native, Windows, macOS testbeds).
- **Ground-Floor Multi-OS Platform Abstraction Layer (PAL)**:
  - Completely refactored OS interactions into clean platform modules (`linux.rs` and `windows.rs`).
  - Windows host identity engine parsing native `%COMPUTERNAME%` and `ipconfig /all` adapter tables.
  - Windows default route discovery via `route print 0.0.0.0` and hardware MAC resolution via `arp -a`.
  - Windows ARP neighbor cache harvesting with automated broadcast/multicast rejection and hex validation.
- **Pure-Rust Bitwise Radix IP / CIDR Engine (`crates/ostium-core/src/radix.rs`)**:
  - High-performance $O(k)$ prefix trie engine derived from the sovereign `bastion` standard.
  - Microsecond bitwise lookup performance ($<85\text{ns}$ per lookup) for single IPs and CIDR blocks (`/24`, `/16`, `/8`, etc.).
  - Formal Parity and Crucible adversarial test suite (`tests/parity_radix_trie_tests.rs`).
- **Crucible Appliance Air-Gap & Isolation Invariant (`OSTIUM-SPEC-INVARIANT-10`)**:
  - Established formal architectural invariant strictly air-gapping live production DNS (`aegis`) and carrier appliances from automated test harnesses, fuzzers, and stress sweeps.
  - Confined all Crucible testing to in-memory fixtures, loopbacks, and isolated virtual machines (`citadel-win1`).
- **Native Multi-OS Release Binaries**:
  - Linux `x86_64`: `ostium-v0.1.1-linux-x86_64` (9.0 MB, Musl Static PIE).
  - Linux `aarch64`: `ostium-v0.1.1-linux-aarch64` (6.2 MB, Musl Static Stripped).
  - Windows `x86_64`: `ostium-v0.1.1-windows-x86_64.exe` (13 MB, Standalone PE32+ Console Executable).
  - Cryptographic verification via SHA-256 manifests and detached GPG Ed25519 signatures (`SOVEREIGN-SIGN-01`).
- **Crucible Empirical Security Verification (57/57 Passed)**:
  - Total test battery expanded to 57 automated tests passing 100% green across Linux and Windows targets.


### 🔧 Changed
- **Mathematical Self-Preservation Invariant**: Hardened `HostIdentity::assert_not_self()` preventing self-targeting, accidental lockouts, or automated blacklisting of the operator workstation.
- **CLI Commands Upgraded to Persistent DB**:
  - `ostium scan`: Discovers network stations and automatically persists inventory to SQLite WAL.
  - `ostium trust-all`: Promotes all currently active stations into the trusted whitelist in `ostium.db`.
  - `ostium alias <id> <name>`: Persists friendly device aliases directly into the database.
  - `ostium aliases`: Displays all active registered device nicknames.
  - `ostium history`: Inspects historical station lease events and appearances.
  - `ostium incidents`: Reviews recorded WIDS intrusion attempts and mitigation actions.
  - `ostium db-stats`: Displays SQLite WAL storage metrics and record counts.
- **Air-Gap Documentation Sanitization (`SOVEREIGN-SANITIZE-01`)**: Scrubbed all live operational IPs, physical MAC addresses, and hostnames across all documentation.

---

## [0.1.0] - 2026-09-26

### 🚀 Added
- **Initial Sovereign Release**:
  - **Tier 1 Universal Zero-Privilege Discovery**: Dual-stack IPv4 sweep and IPv6 all-nodes multicast ping; Linux neighbor table inspection; IEEE OUI vendor resolution.
  - **Tier 2 Wireless Intrusion Detection System (WIDS)**: Sliding-window deauthentication burst velocity accumulator; 802.11w Protected Management Frames (PMF) compliance audit; Rogue AP detection.
  - **Tier 3 Gateway Telemetry & Layer-2 ACL Ejection**: Qualcomm Atheros QSDK carrier Wi-Fi 6/6E gateway telemetry (CPU, RAM, Optical RX Power, WAN status); Remote station disassociation and hardware MAC blacklisting.
  - **Interactive Terminal User Interface (`ostium watch`)**: Full-screen Ratatui ANSI console dashboard featuring real-time station monitor, WIDS threat radar, and one-key station ejection.
- **Core Documentation**:
  - Published guides 01 through 08 covering Quickstart, Hardware Matrix, Wardrive Defense, TUI Reference, Dual-Stack Discovery, Device Naming, Self-Preservation, and Router Diversity.
- **Cryptographic Provenance**:
  - GPG detached signing and SHA-256 integrity verification.
