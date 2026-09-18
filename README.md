# 🔐 Cybersecurity Lab — Footprinting & Network Scanning

### Week 02 — Target Reconnaissance & Subnet Discovery

**Networkwalks Cybersecurity Internship · Batch B083F**

<p align="center">

![Kali Linux](https://img.shields.io/badge/Kali%20Linux-Rolling-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-7.x-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-Network%20Scanning-1F6FEB?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-2EA44F?style=for-the-badge)

</p>

---

## 📌 Project Overview

This project documents the completion of **Week 02 laboratory exercises** for the Networkwalks Cybersecurity Internship.

The laboratory covers two primary activities:

- **W2-PM1 — Target Footprinting**
- **W2-PM5 — Network Scanning**

The first module focuses on reconnaissance and information gathering against the authorized target domain `networkwalks.com`.

The second module focuses on active host discovery across the authorized `10.0.0.0/24` laboratory subnet using Nmap.

---

## 🎯 Objectives

- Perform domain footprinting and OSINT.
- Collect WHOIS and DNS information.
- Identify web application technologies.
- Inspect HTTP response headers.
- Detect the presence of a Web Application Firewall.
- Enumerate DNS records.
- Discover active hosts within the authorized subnet.
- Capture and document command outputs.
- Troubleshoot Kali Linux network connectivity issues.

---

## 🏗️ Lab Architecture

```text
                         ┌─────────────────────────┐
                         │       KALI LINUX        │
                         │      MT Linux VM        │
                         │       10.0.0.2/24       │
                         └────────────┬────────────┘
                                      │
                  ┌───────────────────┴───────────────────┐
                  │                                       │
                  ▼                                       ▼
        ┌─────────────────────┐                 ┌─────────────────────┐
        │  NETWORK SCANNING   │                 │    FOOTPRINTING     │
        │                     │                 │                     │
        │  10.0.0.0/24        │                 │  networkwalks.com   │
        │  Nmap               │                 │                     │
        │  Host Discovery     │                 │ WHOIS               │
        └──────────┬──────────┘                 │ WhatWeb             │
                   │                            │ NSLookup            │
                   ▼                            │ cURL                │
        ┌─────────────────────┐                 │ Wafw00f             │
        │   Active Hosts      │                 │ DNSRecon            │
        │   & Host Details    │                 └──────────┬──────────┘
        └─────────────────────┘                            │
                                                           ▼
                                               ┌─────────────────────┐
                                               │  Reconnaissance     │
                                               │      Results        │
                                               └─────────────────────┘
⚙️ Lab Configuration
Category	Configuration
Operating System	Kali Linux Rolling
Virtualization	Oracle VM VirtualBox
Network Interface	eth0
VM IP Address	10.0.0.2/24
Gateway	10.0.0.1
External Target	networkwalks.com
Internal Network	10.0.0.0/24
Primary Scanner	Nmap
🧰 Tools Used
Tool	Purpose	Key Observation
whois	Domain information	Registrar and nameserver details
whatweb	Web technology detection	WordPress, Apache, Bootstrap
nslookup	DNS resolution	192.232.216.135
curl	HTTP header inspection	HTTP/2 200 OK
wafw00f	WAF detection	ModSecurity (SpiderLabs)
dnsrecon	DNS enumeration	SOA, MX, TXT/SPF, SRV
nmap	Host discovery	10.0.0.0/24
🔎 Module 1 — Target Footprinting
1. WHOIS Enumeration
whois networkwalks.com

Used to collect publicly available domain registration and nameserver information.

2. Web Technology Detection
whatweb networkwalks.com
Observed Technologies
WordPress
Apache
Bootstrap
3. DNS Resolution
nslookup networkwalks.com
Resolved IP Address
192.232.216.135
4. HTTP Header Inspection
curl -I https://networkwalks.com
Observed Response
HTTP/2 200 OK

The response also provided HTTP header and cookie-related information.

5. WAF Detection
wafw00f https://networkwalks.com
Detected WAF
ModSecurity (SpiderLabs)
6. DNS Enumeration
dnsrecon -d networkwalks.com
Observed Records
SOA
MX
TXT / SPF
SRV
🌐 Module 2 — Subnet Host Discovery
Authorized Network
10.0.0.0/24
Nmap Command
sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt

The -sn option performs host discovery without performing a traditional port scan.

The -oN option saves the output to:

nmap_result.txt
View Saved Results
cat nmap_result.txt

The scan examined the complete /24 network containing 256 IP addresses and recorded responsive hosts.

🐞 Challenges Faced & Solutions
Problem 1 — Network & DNS Resolution Failure
Issue

The following errors were encountered:

Temporary failure in name resolution
Network is unreachable
Cause

After restarting the Kali Linux VM, the network interface, default route, and DNS resolver configuration required restoration.

Solution
sudo ip addr flush dev eth0
sudo ip addr add 10.0.0.2/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 10.0.0.1 dev eth0

DNS configuration:

echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf

After restoring the network configuration, the reconnaissance commands were executed successfully.

Problem 2 — Zenmap GUI Issues
Issue

Zenmap generated GUI/deprecation warnings and the detailed scan was slower than required for the host-discovery task.

Solution

The command-line version of Nmap was used:

sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt

This provided direct terminal output and easy result logging.

🧠 Technical Concepts Learned
OSINT & Reconnaissance

Understanding how publicly available domain information can be collected during an authorized security assessment.

DNS Enumeration

Understanding how DNS records can provide information about domain infrastructure.

Web Fingerprinting

Identifying technologies and server components exposed by a web application.

WAF Detection

Identifying the presence of a Web Application Firewall using Wafw00f.

Network Discovery

Using Nmap to identify active hosts within an authorized subnet.

Linux Network Troubleshooting

Manually configuring IP addressing, routing, network interfaces, and DNS resolution.

📸 Proof of Execution

The following screenshots document the laboratory execution:

screenshots/
│
├── 01-network-config-whatweb.png
├── 02-nslookup-curl.png
├── 03-wafw00f-dnsrecon.png
├── 04-nmap-scan-start.png
└── 05-nmap-scan-complete.png
Screenshot	Description
01	Network configuration and WhatWeb output
02	NSLookup and cURL output
03	Wafw00f and DNSRecon output
04	Nmap scan execution
05	Nmap scan completion
📊 Final Results
Module	Activity	Result
W2-PM1	WHOIS	Domain information collected
W2-PM1	WhatWeb	WordPress, Apache and Bootstrap identified
W2-PM1	NSLookup	192.232.216.135 resolved
W2-PM1	cURL	HTTP/2 200 OK observed
W2-PM1	Wafw00f	ModSecurity detected
W2-PM1	DNSRecon	DNS records identified
W2-PM5	Nmap	10.0.0.0/24 scanned
W2-PM5	Result Logging	Output saved to nmap_result.txt
📁 Repository Structure
NETWORKWALKS-B083F-WK2-FOOTPRINTING-NETWORK-SCANNING/
│
├── README.md
│
├── screenshots/
│   ├── 01-network-config-whatweb.png
│   ├── 02-nslookup-curl.png
│   ├── 03-wafw00f-dnsrecon.png
│   ├── 04-nmap-scan-start.png
│   └── 05-nmap-scan-complete.png
│
└── results/
    └── nmap_result.txt
🔐 Ethical Use

All activities were performed within the authorized scope of the internship laboratory.

Authorized Targets
External Target
└── networkwalks.com

Local Laboratory Network
└── 10.0.0.0/24

The documented commands should only be used against systems and networks for which appropriate authorization has been provided.

👤 Author

M. Thangamani

Detail	Information
Program	Cybersecurity Internship
Organization	Networkwalks
Batch	B083F
Intern ID	NW-83-711
Week	02
Modules	W2-PM1 & W2-PM5
Environment	Kali Linux / VirtualBox
Status	Completed
🔐 FOOTPRINT • SCAN • ANALYZE • DOCUMENT
