# 🛡️ 03_OSTIUM_GUIDE_WARD_DRIVE_DEFENSE

> **Target Audience:** Systems Engineers, Network Operators, Security Practitioners  
> **Topic:** Defending Edge & Home Networks Against RF Attackers and Handshake Harvesters  

---

## 🎯 1. Threat Profile: 802.11 Deauthentication Exploits

Wardriving and rogue RF reconnaissance typically target Wi-Fi Access Points (BSSIDs) to capture authentication materials:

1. **Active Deauthentication:** An attacker within physical RF range broadcasts spoofed Layer-2 **802.11 Deauthentication Management Frames** claiming to originate from the legitimate gateway.
2. **Forced Client Disassociation:** Target stations (laptops, mobile phones, IoT devices) are abruptly dropped from the wireless medium.
3. **Handshake Harvesting:** Upon automatic reconnection, the attacker captures the standard **4-Way EAPOL Handshake**.
4. **Offline Cryptanalysis:** The captured handshake is moved off-site for high-velocity dictionary or mask-based hash attacks.

---

## 🛡️ 2. Defense Mechanisms

### A. Mathematical Velocity Trapping (`DeauthTrap`)
`ostium` executes a sliding-window rate evaluation across raw 802.11 management frames:
* Arrival of $\ge 5$ deauthentication or disassociation frames within a $250\text{ms}$ interval triggers immediate threshold breach detection.
* The system dispatches an audible alert tone, writes forensic metadata to the local security audit log, and flags the source MAC for defensive quarantine.

### B. Protocol-Level Immunity: 802.11w Protected Management Frames (PMF)
`ostium` audits whether the gateway enforces **802.11w (PMF)**:
* Under PMF, unicast and broadcast management frames are integrity-protected using **AES-128 CMAC (BIP - Broadcast Integrity Protocol)**.
* Spoofed management frames lacking valid cryptographic keys are discarded at the physical radio layer before reaching the operating system kernel.
* Attackers cannot force disconnections, neutralizing automated handshake-harvesting scripts.

### C. Autonomous Station Sentinel & ACL Isolation
If an unauthorized or rogue station attempts association, the sentinel classifies the station in $< 200\text{ms}$ and dispatches an immediate hardware ACL disassociation command, severing the rogue station's Layer-2 network access.
