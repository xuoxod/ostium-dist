# 🛡️ 03_OSTIUM_GUIDE_WARD_DRIVE_DEFENSE

> **Defending Home Wi-Fi Against RF Attackers and Handshake Harvesters**  

---

## 🎯 1. How Wardriving Works

Wardriving occurs when an attacker within radio range of your home Wi-Fi scans the RF spectrum for active Access Points (BSSIDs). 

To crack a WPA2 network without knowing the password:
1. The attacker broadcasts spoofed **802.11 Deauthentication Frames** pretending to be your router.
2. Your laptop, phone, or smart TV gets abruptly kicked off the Wi-Fi.
3. The moment your device reconnects, the attacker's sniffer captures the **4-Way EAPOL Handshake**.
4. The attacker takes the captured handshake offline to run GPU hashcat/dictionary attacks.

---

## 🛡️ 2. How Ostium Defeats Wardriving

### A. Mathematical Velocity Trapping (`DeauthTrap`)
Ostium continuously monitors 802.11 management frame arrival rates. When $\ge 5$ deauth frames arrive in $< 250\text{ms}$, Ostium flags an active burst, triggers an audible celestial chime (`gemini-chime`), and logs the offending source MAC address.

### B. Protocol-Level Immunity: 802.11w Protected Management Frames (PMF)
Ostium verifies that your router enforces **802.11w (PMF)**. With PMF enabled:
* Management frames are cryptographically signed using **AES-128 CMAC (BIP)**.
* Spoofed deauth frames lack the cryptographic key and are **silently discarded**.
* Attackers cannot kick your devices offline, rendering automated handshake-harvesting scripts completely useless.

### C. Autonomous Rogue Ejection
If an unauthorized device manages to associate with your Wi-Fi, Ostium's station sentinel flags the MAC in $< 200\text{ms}$ and dispatches an instant disassociation call through the router's hardware ACL, kicking the intruder off the network.
