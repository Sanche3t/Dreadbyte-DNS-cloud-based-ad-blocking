# DreadByte DNS 🛡️

Self-hosted, network-wide ad and tracker blocking DNS server built with **AdGuard Home**, deployed on a free-tier cloud VM. Part of the broader **DreadByte** security project.

No browser extensions. No per-device setup. Ads and trackers get blocked at the DNS level, before they ever load — across every device on the network.

---

## 📌 Overview

**Goal:** Deploy a self-hosted DNS server that blocks ads/trackers network-wide.

**Why:** Learn cloud infrastructure, Linux service management, firewall configuration, and DNS-level security — practical building blocks for cybersecurity work — while also getting rid of ads on every device at home.

**Tech Stack:**
- **Cloud Provider:** Scaleway (Stardust free-tier VM)
- **OS:** Ubuntu 26.04 LTS
- **Software:** AdGuard Home
- **Tools:** SSH, UFW Firewall, Scaleway Security Groups

---

## 🔧 Implementation

### 1. Cloud VM Setup
Provisioned a free-tier **Stardust** VM on Scaleway (1 vCPU, 1 GB RAM, Ubuntu 26.04) and connected via SSH.

![VM Details](screenshots/01-scaleway-vm-details.png)
![SSH Connection](screenshots/02-ssh-connect.png)

### 2. AdGuard Home Installation
Installed AdGuard Home and set it up as a `systemd` service so it runs persistently and restarts automatically. Verified it was live and healthy with `systemctl status`.

```bash
sudo systemctl status AdGuardHome
```

![AdGuard Home Service Status](screenshots/03-adguard-systemctl-status.png)

### 3. Firewall & Security
Locked down the server so only the necessary ports were exposed — SSH, DNS, and the admin panel — using UFW, layered with Scaleway's cloud-level security groups for defense in depth.

```bash
sudo ufw allow 22/tcp
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
sudo ufw allow 3000/tcp
sudo ufw enable
```

![UFW Status](screenshots/04-ufw-status.png)
![Scaleway Security Group](screenshots/05-scaleway-security-group.png)

### 4. Device Configuration
Pointed device DNS settings to the VM's IP address so filtering applies automatically across the network — no extensions or per-browser configuration needed.

![Device DNS Settings](screenshots/06-device-dns-settings.png)

### 5. Verification
Tested the setup against [adblock-tester.com](https://adblock-tester.com) to confirm ads, trackers, and analytics scripts were actually being blocked in practice.

**Result: 89/100**

![AdBlock Tester Result](screenshots/07-adblock-tester-result.png)

---

## 💡 Key Learnings

| Skill | What I Learned |
|---|---|
| **Cloud Computing** | Provisioned and managed a free-tier VM on Scaleway; handled SSH access, public IP, and security groups |
| **Linux Administration** | Managed AdGuard Home as a systemd service; verified service health via `systemctl` |
| **Firewall Configuration** | Wrote UFW rules to expose only required ports; layered with cloud security groups |
| **DNS Security** | Configured AdGuard Home as a network-wide DNS-level ad/tracker blocker |
| **Verification & Testing** | Used adblock-tester.com to empirically validate blocking effectiveness (89/100) |

---

## 📊 Results

- **Before:** Ads and trackers loading on every device — phone, laptop, smart TV.
- **After:** Network-wide blocking verified via adblock-tester.com, scoring 89/100.
- **Cost:** ~$0 (free-tier VM).

---

## 🔗 Notes

- IP addresses in screenshots are redacted/cropped for security — don't expose a live server's public IP.
- This project is part of the larger **DreadByte** initiative, which also includes a Chrome security extension (URL scanning, HTTPS enforcement, redirect tracking).
