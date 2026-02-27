# 🔍 Network Traffic Analysis & ARP Spoofing Lab

## 📌 Project Overview
This project demonstrates a real-world cybersecurity lab setup using two virtual machines to simulate network traffic analysis and an ARP spoofing attack. The lab was conducted in a controlled environment using VMware Workstation Pro.

| Role | Machine | IP Address |
|------|---------|------------|
| 🔴 Attacker | Kali Linux | 192.168.0.164 |
| 🔵 Victim | Ubuntu 24.04 | 192.168.0.246 |

---

## 🎯 Objectives
- Set up an isolated virtual cybersecurity lab using VMware Workstation
- Capture and analyze normal network traffic using Wireshark
- Simulate a Man-in-the-Middle (MITM) attack using ARP Spoofing
- Detect and analyze ARP spoofing traffic patterns in Wireshark
- Understand how attackers intercept network communications

---

## 📁 Project Files
- Network connectivity screenshots (ping tests)
- Wireshark traffic capture screenshots
- ARP Spoofing attack and detection screenshots

---

## 🛠️ Tools & Technologies Used
- **VMware Workstation Pro** — Virtual machine environment
- **Kali Linux** — Attacker machine
- **Ubuntu 24.04 LTS** — Victim machine
- **Wireshark** — Network traffic capture and analysis
- **Bettercap** — ARP spoofing tool
- **ifconfig / ping** — Network configuration and connectivity testing

---

## ⚙️ Lab Setup & Network Configuration

Both VMs were configured on a **Bridged Network** so they operate as real devices on the same physical network — simulating a real-world environment.

**Step 1** — Check IP addresses on both VMs using `ifconfig`

**Step 2** — Configure both VMs to Bridged network adapter in VMware settings

**Step 3** — Restart both VMs and verify new IP addresses:
- Kali Linux: `192.168.0.164`
- Ubuntu: `192.168.0.246`

**Step 4** — Verify connectivity by pinging both VMs

---

## 🌐 Phase 1 — Network Connectivity Testing

### ✅ Kali Pings Ubuntu
<p align="center">
  <img src="Kali%20pings%20Ubuntu.png" width="700">
</p>

Kali (192.168.0.164) successfully pinged Ubuntu (192.168.0.246) with 0% packet loss confirming network connectivity.

---

### ✅ Ubuntu Pings Kali
<p align="center">
  <img src="Ubuntu%20pings%20Kali.png" width="700">
</p>

Ubuntu (192.168.0.246) successfully pinged Kali (192.168.0.164) with 0% packet loss confirming bidirectional communication.

---

## 🔍 Phase 2 — Normal Traffic Capture

Wireshark was installed on Ubuntu using the following commands:
```bash
sudo add-apt-repository ppa:wireshark-dev/stable
sudo apt-get update
sudo apt-get install wireshark
sudo wireshark
```

With Wireshark running, Kali pinged Ubuntu to generate traffic. The following protocols were observed and filtered:

| Protocol | Color Code | Description |
|---------|------------|-------------|
| ICMP | Pink | Ping requests and replies between VMs |
| ARP | Yellow | Address Resolution Protocol packets |
| TCP | Dark Gray | ACK traffic |
| Errors | Black | Packets with errors |

### 📸 Ubuntu Captures Traffic from Kali
<p align="center">
  <img src="Ubuntu%20captures%20traffic%20from%20kali.png" width="700">
</p>

Wireshark on Ubuntu showing normal ICMP ping traffic and ARP packets between the two VMs. The Statistics menu was also used to get a visual representation of all captured traffic.

---

### 📸 Kali Captures Traffic from Ubuntu
<p align="center">
  <img src="Kail%20capture%20traffic%20from%20ubuntu.png" width="700">
</p>

Wireshark on Kali showing mixed traffic including ICMP, ARP and TLS packets from Ubuntu's network activity.

---

## ⚠️ Phase 3 — ARP Spoofing Attack (MITM Simulation)

### What is ARP Spoofing?
ARP Spoofing is a Man-in-the-Middle (MITM) attack where the attacker sends fake ARP messages to associate their MAC address with a legitimate IP address. This allows the attacker to intercept, modify, or stop traffic between two devices without their knowledge.

### Attack Commands
Bettercap was installed and configured on Kali Linux:

```bash
# Install Bettercap
sudo apt update && sudo apt install bettercap -y

# Launch Bettercap on the network interface
sudo bettercap -iface eth0

# Set the target (Ubuntu VM)
set arp.spoof.targets 192.168.0.246

# Launch the ARP spoofing attack
arp.spoof on
```

### 📸 ARP Spoofing Attack Launched from Kali
<p align="center">
  <img src="ARP%20Spoofing%20Attack%20Lauch%20from%20Kali.png" width="700">
</p>

Bettercap successfully launched the ARP spoofing attack against Ubuntu (192.168.0.246). The tool identified the target endpoint and began poisoning the ARP table — positioning Kali as a Man-in-the-Middle between Ubuntu and the gateway.

---

### 📸 ARP Spoofing Detected in Wireshark on Ubuntu
<p align="center">
  <img src="ARP%20spoofing%20Detected%20in%20Wireshark%20on%20Ubuntu.png" width="700">
</p>

Wireshark on Ubuntu filtered by **ARP protocol** clearly shows the ARP spoofing attack in progress. The flood of ARP packets from multiple sources indicates ARP table poisoning — a clear Indicator of Compromise (IOC) that a SOC analyst would flag as malicious activity.

---

## 🔐 Security Observations & Analysis

**During Normal Traffic:**
- ICMP packets showed clean ping requests and replies
- ARP traffic was minimal and legitimate
- Traffic patterns were consistent and predictable

**During ARP Spoofing Attack:**
- Sudden flood of ARP packets from the attacker
- Multiple "Who has" ARP requests targeting the victim
- ARP table poisoning visible in Wireshark capture
- Attacker successfully positioned as MITM between victim and gateway

**Key Indicators of Compromise (IOCs):**
- Unusual volume of ARP packets
- Duplicate IP-to-MAC address mappings
- Unexpected ARP replies without corresponding requests

---

## 🛡️ Defensive Recommendations
- Enable **Dynamic ARP Inspection (DAI)** on network switches
- Use **Static ARP entries** for critical devices
- Deploy **network monitoring tools** to detect ARP anomalies
- Implement **VPNs and encrypted communications** to reduce MITM impact
- Use **Intrusion Detection Systems (IDS)** to alert on ARP flooding

---

## 🚀 Skills Demonstrated
- Virtual lab setup and network configuration
- Network traffic capture and protocol analysis
- ARP spoofing attack simulation using Bettercap
- Wireshark filtering and traffic pattern identification
- Threat detection and security analysis
- Technical documentation and findings reporting

---

## 📌 Author
**TemmyNetGuard**
Cybersecurity Student at **Altschool Africa** | ISC² Certified in Cybersecurity (CC)
Building offensive and defensive security skills one lab at a time. 🔐
