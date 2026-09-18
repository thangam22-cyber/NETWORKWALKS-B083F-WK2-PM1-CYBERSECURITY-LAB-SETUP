# 🛰️ ReconX — Footprinting & Network Discovery Lab

### Week 02 · Target Reconnaissance & Subnet Discovery

> **Map the Surface. Discover the Network. Understand the Target.**

A hands-on cybersecurity reconnaissance laboratory focused on **OSINT, DNS intelligence, web technology fingerprinting, WAF detection, HTTP analysis, and subnet host discovery** using Kali Linux.

<p align="center">

![Kali Linux](https://img.shields.io/badge/Kali%20Linux-Rolling-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-Network%20Discovery-1F6FEB?style=for-the-badge)
![OSINT](https://img.shields.io/badge/OSINT-Reconnaissance-8A2BE2?style=for-the-badge)
![Networkwalks](https://img.shields.io/badge/Networkwalks-Internship-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-2EA44F?style=for-the-badge)

</p>

---

## 🧭 Mission Brief

Week 02 focuses on understanding how security professionals build an initial picture of a target before deeper security testing.

The laboratory was divided into two reconnaissance tracks:

```text
                    ┌─────────────────────────┐
                    │       KALI LINUX        │
                    │     RECON WORKSTATION   │
                    └────────────┬────────────┘
                                 │
                ┌────────────────┴────────────────┐
                │                                 │
                ▼                                 ▼
       ┌──────────────────┐             ┌──────────────────┐
       │  EXTERNAL RECON  │             │  NETWORK RECON   │
       │                  │             │                  │
       │ networkwalks.com │             │  10.0.0.0/24     │
       └────────┬─────────┘             └────────┬─────────┘
                │                                │
                ▼                                ▼
       Domain Intelligence                Host Discovery
       DNS Enumeration                    Nmap Ping Sweep
       Web Fingerprinting
       WAF Detection
       HTTP Analysis
                │                                │
                └────────────────┬───────────────┘
                                 ▼
                       ┌────────────────────┐
                       │  RECON INTELLIGENCE│
                       │  & DOCUMENTATION   │
                       └────────────────────┘
🎯 Objectives
Area	Objective
🔎 Domain Recon	Collect publicly available domain information
🌐 DNS Analysis	Identify DNS and infrastructure records
🧬 Technology Fingerprinting	Identify web technologies and server components
🛡️ WAF Detection	Determine whether a web application firewall is present
📡 HTTP Analysis	Inspect HTTP response headers and status
🗺️ Network Discovery	Identify active hosts within the authorized subnet
📝 Evidence Collection	Preserve command outputs and screenshots
🧰 Recon Toolkit
┌──────────────┬────────────────────────────────────┐
│ WHOIS        │ Domain / registrar intelligence    │
│ WhatWeb      │ Web technology fingerprinting     │
│ NSLookup     │ DNS resolution                    │
│ cURL         │ HTTP response analysis             │
│ Wafw00f      │ WAF fingerprinting                │
│ DNSRecon     │ DNS record enumeration            │
│ Nmap         │ Network host discovery            │
└──────────────┴────────────────────────────────────┘
🔍 Track 01 — External Footprinting
Target
https://networkwalks.com

The target domain was examined using multiple reconnaissance utilities to build a basic external attack-surface profile.

01 · WHOIS Intelligence
whois networkwalks.com

Collected information included:

Domain registration information
Registrar details
Nameserver information
Domain status information
02 · Web Stack Fingerprinting
whatweb networkwalks.com

Observed technologies:

WordPress
Apache
Bootstrap
03 · DNS Resolution
nslookup networkwalks.com

Observed A Record:

192.232.216.135
04 · HTTP Header Analysis
curl -I https://networkwalks.com

Observed response:

HTTP/2 200 OK

HTTP headers and cookie-related response information were also observed during the analysis.

05 · WAF Fingerprinting
wafw00f https://networkwalks.com

Observed WAF:

ModSecurity (SpiderLabs)
06 · DNS Enumeration
dnsrecon -d networkwalks.com

Observed DNS record categories:

SOA
MX
TXT / SPF
SRV
🗺️ Track 02 — Local Network Discovery
Authorized Laboratory Scope
Network : 10.0.0.0/24
Addresses : 256
Method : ICMP / Host Discovery
Tool : Nmap
Host Discovery
sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt

The scan output was saved locally for documentation and verification.

Review Results
cat nmap_result.txt

This provided a list of responsive hosts detected within the authorized laboratory network.

🧩 Reconnaissance Findings
Investigation	Tool	Observation
Domain Intelligence	WHOIS	Registration & nameserver data
Web Fingerprinting	WhatWeb	WordPress / Apache / Bootstrap
DNS Resolution	NSLookup	192.232.216.135
HTTP Analysis	cURL	HTTP/2 200 OK
WAF Detection	Wafw00f	ModSecurity (SpiderLabs)
DNS Enumeration	DNSRecon	SOA / MX / TXT / SRV
Host Discovery	Nmap	10.0.0.0/24 examined
🛠️ Lab Troubleshooting
🌐 DNS & Routing Failure
Initial Symptoms
Temporary failure in name resolution
Network is unreachable

The Kali VM required network configuration restoration after the environment restart.

Network Recovery
sudo ip addr flush dev eth0
sudo ip addr add 10.0.0.2/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 10.0.0.1 dev eth0

DNS resolver configuration:

echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf

After restoring the interface, route, and resolver configuration, connectivity-dependent reconnaissance commands could be executed.

🖥️ Zenmap → Nmap CLI

Zenmap produced GUI/deprecation warnings and the detailed scan workflow was slower than required for the host-discovery objective.

The laboratory therefore switched to the command-line workflow:

sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt
Why CLI?
✓ Faster execution
✓ Direct terminal output
✓ Easy evidence collection
✓ Reproducible commands
✓ Simple result logging
🧠 What This Lab Demonstrated
🔎 Reconnaissance

Understanding how publicly accessible information can reveal elements of a target's infrastructure.

🌐 DNS Intelligence

Learning how different DNS record types contribute to infrastructure mapping.

🧬 Fingerprinting

Identifying technologies exposed by a web application through passive and active inspection.

🛡️ Defensive Visibility

Recognizing security controls such as Web Application Firewalls during authorized assessment.

🗺️ Network Enumeration

Using Nmap host discovery to identify responsive systems within a controlled subnet.

🛠️ Linux Networking

Practicing manual configuration of:

IP Address
Network Interface
Default Route
DNS Resolver
📸 Evidence & Screenshots

All major laboratory activities were documented using terminal screenshots.

screenshots/
│
├── 01-network-config-whatweb.png
├── 02-nslookup-curl.png
├── 03-wafw00f-dnsrecon.png
├── 04-nmap-scan-start.png
└── 05-nmap-scan-complete.png
Evidence Mapping
Screenshot	Evidence
01	Network recovery + WhatWeb
02	DNS resolution + HTTP headers
03	WAF + DNS enumeration
04	Nmap scan execution
05	Nmap completion / results
📁 Repository Structure
NETWORKWALKS-B083F-WK2-RECONNAISSANCE/
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
🔐 Authorization & Responsible Use

This laboratory was conducted as part of an authorized cybersecurity internship exercise.

Authorized Scope
External Target
└── networkwalks.com

Internal Laboratory Network
└── 10.0.0.0/24

The techniques and commands documented here should only be used against systems and networks where explicit authorization has been provided.

👤 Lab Information
Field	Details
Author	M. Thangamani
Program	Cybersecurity Internship
Organization	Networkwalks
Batch	B083F
Intern ID	NW-83-711
Week	02
Modules	W2-PM1 & W2-PM5
Environment	Kali Linux / VirtualBox
Status	Completed
⚡ Recon Mindset
        ENUMERATE
             ↓
         IDENTIFY
             ↓
          ANALYZE
             ↓
         DOCUMENT

Good security starts with knowing what is exposed.

🛡️ Cybersecurity Internship · Week 02

Reconnaissance • OSINT • DNS • Fingerprinting • Network Discovery
