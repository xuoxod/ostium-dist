# 🏷️ 06_OSTIUM_GUIDE_DEVICE_NAMING_AND_ALIASES

> **Product:** Ostium Sovereign Sentinel  
> **Audience:** End Users & Network Administrators  
> **Topic:** Device Hostname Discovery, Android/iOS Private MACs, and Custom Aliases  

---

## 📱 The "Private Wi-Fi" Challenge

Smartphones running Android 10+ or iOS 14+ feature **Private MAC Address Randomization** by default. Instead of showing hardware from Apple, Samsung, or Google, standard network tools see an anonymous MAC address:

```text
9E:8C:C6:F3:B7:80  192.168.1.168  Private Wi-Fi (Randomized MAC)
```

Ostium solves this problem through automated reverse DNS discovery and custom alias management.

---

## 🔍 1. Automatic Hostname Resolution

When a device connects to your home Wi-Fi gateway (such as a Verizon Fios CR1000A, CR1000B, or G3100), it registers its internal friendly hostname with the router's DNS server during DHCP assignment.

Ostium performs a high-speed reverse DNS PTR query directly against your router (`192.168.1.1:53`):
* `192.168.1.168` $\implies$ Discovers hostname **`Xueux`** $\implies$ Labels device as **`Xueux (Android Phone)`**.
* `192.168.1.185` $\implies$ Discovers hostname **`50Q550G`** $\implies$ Labels device as **`TCL TV (50Q550G)`**.
* `192.168.1.183` $\implies$ Discovers hostname **`LS210D4D6`** $\implies$ Labels device as **`Buffalo NAS (LS210D4D6)`**.
* `192.168.1.153` $\implies$ Discovers hostname **`AmazonPlug01R2`** $\implies$ Labels device as **`Amazon (AmazonPlug01R2)`**.

---

## ✏️ 2. Setting Custom Device Nicknames

If you want to label devices with your own friendly names (e.g., *"Rick's Android Phone"* or *"Living Room PS5"*), use the `ostium alias` command:

```bash
# Assign a nickname by IP address:
ostium alias 192.168.1.168 "Rick's Android Phone"

# Assign a nickname by MAC address (works even if IP changes):
ostium alias 0C:70:43:B3:47:54 "Living Room PlayStation 5"
ostium alias 68:E1:DC:A7:14:D6 "Buffalo Backup Vault"
```

### Viewing Configured Aliases
```bash
$ ostium aliases

================================================================================
  OSTIUM DEVICE ALIAS REGISTRY [3 CONFIGURATIONS]
================================================================================
  192.168.1.168            -> Rick's Android Phone
  0C:70:43:B3:47:54        -> Living Room PlayStation 5
  68:E1:DC:A7:14:D6        -> Buffalo Backup Vault
================================================================================
```

Aliases are saved to `~/.config/ostium/aliases.json` and immediately appear in all `status`, `scan`, and interactive `watch` TUI dashboards.
