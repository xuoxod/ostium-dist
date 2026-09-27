# 🏷️ 06_OSTIUM_GUIDE_DEVICE_NAMING_AND_ALIASES

> **Target Audience:** Systems Operators & Network Administrators  
> **Topic:** Device Hostname Discovery, Randomized MAC Resolution, and Local Aliases  

---

## 📱 The "Private Wi-Fi" Address Challenge

Modern mobile operating systems (Android 10+, iOS 14+, Windows 11) enable **MAC Address Randomization** by default. Instead of presenting authentic manufacturer hardware identifiers, network analyzers encounter anonymous, locally administered MAC addresses:

```text
9E:8C:C6:XX:XX:XX  192.168.1.168  Locally Administered / Private MAC
```

`ostium` bridges this visibility gap through automated DHCP lease harvesting and a persistent local alias registry.

---

## 🔍 1. Sub-Millisecond Gateway Lease Harvesting

When a client device negotiates a DHCP lease with the gateway, it registers its operating-system friendly hostname.

`ostium` transmits non-intrusive RFC 1035 UDP reverse DNS PTR queries directly to the gateway (`<gateway-ip>:53`):
* `192.168.1.168` $\implies$ Resolves PTR $\implies$ Labels device as **`Mobile-Handset-Alpha`**.
* `192.168.1.185` $\implies$ Resolves PTR $\implies$ Labels device as **`Smart-Display-4K`**.
* `192.168.1.183` $\implies$ Resolves PTR $\implies$ Labels device as **`Backup-Vault-Storage`**.
* `192.168.1.153` $\implies$ Resolves PTR $\implies$ Labels device as **`Smart-Appliance-Plug`**.

---

## ✏️ 2. Custom Device Aliases

Operators can assign custom persistent identifiers to any station by IP address or hardware MAC:

```bash
# Assign a persistent friendly alias by IP:
ostium alias 192.168.1.168 "Primary Mobile Device"

# Assign a persistent alias by MAC address (retains identity across DHCP renewals):
ostium alias 0C:70:43:XX:XX:XX "Production Media Console"
ostium alias 68:E1:DC:XX:XX:XX "Secure Network Attached Storage"
```

### Viewing Configured Aliases
```bash
$ ostium aliases

================================================================================
  OSTIUM DEVICE ALIAS REGISTRY [CONFIGURED ENTRIES]
================================================================================
  192.168.1.168            -> Primary Mobile Device
  0C:70:43:XX:XX:XX        -> Production Media Console
  68:E1:DC:XX:XX:XX        -> Secure Network Attached Storage
================================================================================
```

Aliases are stored locally in `~/.config/ostium/aliases.json` and immediately render across all CLI commands and the live terminal radar interface.
