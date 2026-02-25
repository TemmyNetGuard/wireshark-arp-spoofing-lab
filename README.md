
```markdown
# 🔍 Network Traffic Analysis & ARP Spoofing Lab

## 📌 Project Overview
This project demonstrates a **real-world cybersecurity lab** using two virtual machines to simulate **normal network traffic analysis** and an **ARP spoofing (MITM)** attack. The entire lab was conducted in a fully isolated environment using **VMware Workstation Pro**.

| Role       | Machine          | IP Address      |
|------------|------------------|-----------------|
| 🔴 Attacker | Kali Linux       | `192.168.0.164` |
| 🔵 Victim   | Ubuntu 24.04 LTS | `192.168.0.246` |

---

## 🎯 Objectives
- Build an isolated virtual cybersecurity lab with VMware Workstation
- Capture and analyze legitimate network traffic using Wireshark
- Perform a Man-in-the-Middle (MITM) attack via ARP Spoofing
- Detect and analyze ARP poisoning patterns in Wireshark
- Understand how attackers can intercept and manipulate network communications

---

## 🛠️ Tools & Technologies Used
- **VMware Workstation Pro** — Virtual machine hypervisor
- **Kali Linux** — Attacker machine
- **Ubuntu 24.04 LTS** — Victim machine
- **Wireshark** — Packet capture & analysis
- **Bettercap** — ARP spoofing & MITM framework
- **ifconfig / ping** — Network configuration & connectivity testing

---

## ⚙️ Lab Setup & Network Configuration
Both VMs were placed on a **Bridged Network** adapter so they behave as real devices on the same local network.

**Step-by-step setup:**
1. Check IP addresses on both machines using `ifconfig`
2. Change both VMs to **Bridged** network adapter in VMware settings
3. Restart VMs and confirm new IPs:
   - Kali Linux → `192.168.0.164`
   - Ubuntu → `192.168.0.246`
4. Verify bidirectional connectivity with ping

### ✅ Kali Pings Ubuntu
<div align="center">
  <img src="Kali pings Ubuntu.png" width="700" alt="Kali successfully pinging Ubuntu">
</div>

**Result:** 0% packet loss — full connectivity confirmed.

### ✅ Ubuntu Pings Kali
<div align="center">
  <img src="Ubuntu pings Kali.png" width="700" alt="Ubuntu successfully pinging Kali">
</div>

**Result:** 0% packet loss — bidirectional communication working.

---

## 🔍 Phase 1 — Normal Traffic Capture
Wireshark was installed on Ubuntu and used to capture baseline traffic while Kali generated ping requests.

**Installation commands (Ubuntu):**
```bash
sudo add-apt-repository ppa:wireshark-dev/stable
sudo apt-get update
sudo apt-get install wireshark -y
sudo wireshark
```

### 📸 Ubuntu Captures Traffic from Kali
<div align="center">
  <img src="Ubuntu captures traffic from kali.png" width="700" alt="Wireshark capture on Ubuntu showing normal traffic from Kali">
</div>

### 📸 Kali Captures Traffic from Ubuntu
<div align="center">
  <img src="Kail capture traffic from ubuntu.png" width="700" alt="Wireshark capture on Kali showing traffic from Ubuntu">
</div>

**Observed protocols (normal traffic):**
| Protocol | Color     | Description                     |
|----------|-----------|---------------------------------|
| ICMP     | Pink      | Ping requests & replies         |
| ARP      | Yellow    | Address Resolution Protocol     |
| TCP      | Dark Gray | ACK packets                     |
| Errors   | Black     | Corrupted / error packets       |

---

## ⚠️ Phase 2 — ARP Spoofing Attack (MITM Simulation)

### What is ARP Spoofing?
ARP Spoofing (also called ARP Poisoning) is a Layer-2 attack where the attacker sends forged ARP messages to associate their own MAC address with the IP address of a legitimate device. This allows the attacker to become the **Man-in-the-Middle** and intercept/modify traffic.

### Attack Execution (on Kali Linux)
```bash
# Install Bettercap
sudo apt update && sudo apt install bettercap -y

# Launch Bettercap
sudo bettercap -iface eth0

# Inside Bettercap:
set arp.spoof.targets 192.168.0.246
arp.spoof on
```

### 📸 ARP Spoofing Attack Launched from Kali
<div align="center">
  <img src="ARP Spoofing.png" width="700" alt="Bettercap running ARP spoofing attack">
</div>

**Bettercap successfully poisoned the ARP table of the Ubuntu victim.**

### 📸 ARP Spoofing Captured in Wireshark (Ubuntu)
<div align="center">
  <img src="ARP spoofing captured.png" width="700" alt="Wireshark showing massive ARP flood from attacker">
</div>

**Clear indicators of attack:**
- Sudden flood of ARP "Who has" and "Is at" replies
- Attacker MAC address repeatedly claiming the gateway IP
- ARP table poisoning visible in real time

---

## 🔐 Security Observations & Analysis

**Normal Traffic:**
- Clean ICMP ping requests/replies
- Minimal, legitimate ARP traffic
- Predictable and consistent patterns

**During ARP Spoofing:**
- Massive increase in ARP packets from attacker
- Duplicate IP-to-MAC mappings
- Attacker positioned between victim and gateway

**Key Indicators of Compromise (IOCs):**
- Unusual volume of ARP traffic
- ARP replies without matching requests
- Rapid changes in ARP cache

---

## 🛡️ Defensive Recommendations
- Enable **Dynamic ARP Inspection (DAI)** on switches
- Use **static ARP entries** for critical devices
- Deploy **IDS/IPS** that monitor ARP anomalies
- Implement **VPNs** or **encrypted protocols** (TLS, SSH)
- Regular network monitoring with Wireshark or Zeek

---

## 🚀 Skills Demonstrated
- Virtual lab architecture & network bridging
- Wireshark packet capture & protocol analysis
- ARP spoofing simulation with Bettercap
- Real-time threat detection
- Professional technical documentation

---

## 📌 Author
**TemmyNetGuard**  
Cybersecurity Student at **Altschool Africa** | ISC² Certified in Cybersecurity (CC)  
Building offensive & defensive security skills one lab at a time. 🔐
```

