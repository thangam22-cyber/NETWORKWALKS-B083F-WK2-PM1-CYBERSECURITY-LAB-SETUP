# 🔐 Cybersecurity Lab — Footprinting & Network Scanning

## Week 02 — Target Reconnaissance & Subnet Discovery Laboratory

A practical cybersecurity laboratory focused on **target footprinting, OSINT, DNS enumeration, web technology identification, WAF detection, and local subnet host discovery** using Kali Linux.

> **Ethical Use:** All activities documented in this project were performed against the designated internship target `networkwalks.com` and the authorized isolated laboratory subnet `10.0.0.0/24` as part of the Networkwalks Cybersecurity Internship curriculum.

---

## 📌 Project Overview

This project documents the execution of **Week 02 laboratory modules** for the **Networkwalks Cybersecurity Internship — Batch B083F**.

The laboratory consists of two major modules:

### Module W2-PM1 — Target Footprinting

Performed reconnaissance and OSINT against the authorized domain:

```text
networkwalks.com
```

The following tools were used:

* WHOIS
* WhatWeb
* NSLookup
* cURL
* Wafw00f
* DNSRecon

### Module W2-PM5 — Network Scanning

Performed active host discovery across the authorized laboratory subnet:

```text
10.0.0.0/24
```

using:

* Nmap

The objective was to identify active hosts and document the resulting network discovery data.

---

# 🎯 Objectives

* Perform passive and active reconnaissance against the authorized target domain.
* Collect domain registration and infrastructure information using `whois`.
* Enumerate DNS records using `dnsrecon`.
* Resolve domain IP addresses using `nslookup`.
* Identify web technologies using `whatweb`.
* Inspect HTTP response headers using `curl`.
* Identify the presence of a Web Application Firewall using `wafw00f`.
* Discover active hosts within the authorized `10.0.0.0/24` subnet.
* Capture and document command output for laboratory verification.
* Develop practical skills in reconnaissance, enumeration, and network troubleshooting.

---

# 🛡️ Purpose of the Laboratory

Reconnaissance is an important initial phase of an authorized security assessment.

This laboratory provides practical experience in:

### 🔎 Passive Reconnaissance

Collecting publicly available information about a domain and its infrastructure.

### 🌐 Active Reconnaissance

Interacting with an authorized target to identify publicly exposed services, technologies, and security controls.

### 🗺️ Network Discovery

Identifying active systems within an authorized network segment.

### 🛠️ Network Troubleshooting

Diagnosing and resolving connectivity, routing, and DNS configuration issues inside the Kali Linux environment.

---

# 🏗️ Lab Architecture

```text
                         ┌──────────────────────────┐
                         │       KALI LINUX VM      │
                         │      MT Linux / eth0     │
                         │       10.0.0.2/24        │
                         └────────────┬─────────────┘
                                      │
                    ┌─────────────────┴─────────────────┐
                    │                                   │
                    ▼                                   ▼
        ┌──────────────────────┐             ┌────────────────────────┐
        │ LOCAL NETWORK SCAN   │             │ EXTERNAL FOOTPRINTING  │
        │                      │             │                        │
        │ Target: 10.0.0.0/24  │             │ Target: networkwalks   │
        │ Tool: Nmap           │             │ Tools: WHOIS            │
        │                      │             │        WhatWeb           │
        │ Host Discovery       │             │        NSLookup          │
        └──────────┬───────────┘             │        cURL              │
                   │                         │        Wafw00f           │
                   ▼                         │        DNSRecon          │
        ┌──────────────────────┐             └───────────┬────────────┘
        │ Active Hosts         │                         │
        │ & Network Discovery  │                         ▼
        └──────────────────────┘             ┌────────────────────────┐
                                             │ Reconnaissance Results │
                                             │                        │
                                             │ • IP Address           │
                                             │ • DNS Records          │
                                             │ • Web Technologies     │
                                             │ • HTTP Headers         │
                                             │ • WAF Information      │
                                             └────────────────────────┘
```

---

# ⚙️ Lab Environment

| Category             | Configuration                                     |
| -------------------- | ------------------------------------------------- |
| Operating System     | Kali Linux Rolling                                |
| Virtualization       | VirtualBox                                        |
| Network Interface    | `eth0`                                            |
| VM IP                | `10.0.0.2/24`                                     |
| Gateway              | `10.0.0.1`                                        |
| External Target      | `networkwalks.com`                                |
| Internal Target      | `10.0.0.0/24`                                     |
| Primary Scanner      | Nmap                                              |
| Reconnaissance Tools | WHOIS, WhatWeb, NSLookup, cURL, Wafw00f, DNSRecon |

---

# 🧰 Tooling & Key Findings

| Tool       | Purpose                          | Key Observation                              |
| ---------- | -------------------------------- | -------------------------------------------- |
| `whois`    | Domain and registrar information | Domain registration / nameserver information |
| `whatweb`  | Web technology fingerprinting    | WordPress, Apache, Bootstrap                 |
| `nslookup` | DNS resolution                   | `192.232.216.135` A record                   |
| `curl`     | HTTP header inspection           | `HTTP/2 200 OK` and response headers         |
| `wafw00f`  | WAF detection                    | ModSecurity (SpiderLabs) detected            |
| `dnsrecon` | DNS enumeration                  | SOA, MX, TXT/SPF and SRV records             |
| `nmap`     | Host discovery                   | Active hosts within `10.0.0.0/24`            |

---

# 🪜 Lab Execution

## Module 1 — Target Footprinting (W2-PM1)

### 1. WHOIS Enumeration

Used WHOIS to collect domain registration and nameserver information.

```bash
whois networkwalks.com
```

**Purpose:**

* Domain registration information
* Registrar details
* Nameserver information
* Domain status information

---

### 2. Web Technology Detection

Used WhatWeb to identify technologies used by the target website.

```bash
whatweb networkwalks.com
```

**Observed technologies included:**

* WordPress
* Apache
* Bootstrap

---

### 3. DNS Resolution

Used NSLookup to resolve the target domain.

```bash
nslookup networkwalks.com
```

**Observed A Record:**

```text
192.232.216.135
```

---

### 4. HTTP Header Inspection

Used cURL to inspect the HTTP response headers.

```bash
curl -I https://networkwalks.com
```

**Observed response:**

```text
HTTP/2 200 OK
```

The response also provided information regarding HTTP headers and cookies.

---

### 5. WAF Detection

Used Wafw00f to identify whether a Web Application Firewall was present.

```bash
wafw00f https://networkwalks.com
```

**Observed WAF:**

```text
ModSecurity (SpiderLabs)
```

---

### 6. DNS Enumeration

Used DNSRecon to enumerate available DNS records.

```bash
dnsrecon -d networkwalks.com
```

**Observed record types included:**

* SOA
* MX
* TXT / SPF
* SRV

---

# 🌐 Module 2 — Subnet Host Discovery (W2-PM5)

The authorized laboratory subnet was scanned using Nmap.

### Nmap Ping Sweep

```bash
sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt
```

The `-sn` option performs host discovery without conducting a traditional port scan.

The `-oN` option saves the output in normal Nmap format.

### View Saved Results

```bash
cat nmap_result.txt
```

### Scan Scope

```text
Network: 10.0.0.0/24
Total Addresses: 256
Purpose: Active Host Discovery
```

The resulting output was used to identify live endpoints within the authorized laboratory network.

---

# 🐞 Challenges Faced & Solutions

## Problem 1 — DNS Resolution and Network Connectivity

### Issue

Several reconnaissance commands initially returned errors such as:

```text
Temporary failure in name resolution
Network is unreachable
```

### Cause

After restarting the Kali Linux VM, the network interface configuration, default route, and DNS resolver configuration required restoration.

### Resolution

The network interface and routing configuration were manually restored:

```bash
sudo ip addr flush dev eth0
sudo ip addr add 10.0.0.2/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 10.0.0.1 dev eth0
```

DNS resolution was then configured using:

```bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

After restoring the configuration, connectivity-dependent reconnaissance commands were executed successfully.

---

## Problem 2 — Zenmap GUI Instability

### Issue

Zenmap displayed deprecation warnings and the detailed scanning process was slower than expected. The topology visualization also did not provide the required laboratory output.

### Resolution

The GUI-based workflow was discontinued for this exercise and the Nmap command-line interface was used instead:

```bash
sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt
```

The CLI approach provided:

* Faster execution
* Direct terminal output
* Easy result logging
* Reproducible commands
* Simple documentation

---

# 🧠 Technical Concepts Learned

### 1. OSINT & Reconnaissance

Learned how publicly available domain information can be collected using tools such as:

```text
WHOIS
DNSRecon
NSLookup
```

### 2. Web Technology Fingerprinting

Used WhatWeb to identify technologies and frameworks exposed by a web application.

### 3. WAF Fingerprinting

Used Wafw00f to identify the presence and type of Web Application Firewall protecting the authorized target.

### 4. DNS Enumeration

Learned how DNS records such as:

```text
A
SOA
MX
TXT
SRV
```

can provide useful infrastructure information during an authorized security assessment.

### 5. Network Discovery

Used Nmap's ping sweep functionality to identify active hosts within a subnet.

### 6. Linux Network Troubleshooting

Practiced manually configuring:

```text
IP Address
Network Interface
Default Gateway
DNS Resolver
```

using Linux networking commands.

---

# 📸 Proof of Execution

The following screenshots document the execution of the laboratory activities:

```text
1. network-config-whatweb.png
   → Network configuration repair and WhatWeb output

2. nslookup-curl.png
   → DNS resolution and HTTP header inspection

3. wafw00f-dnsrecon.png
   → WAF detection and DNS enumeration

4. nmap-scan-start.png
   → Nmap subnet discovery execution

5. nmap-scan-complete.png
   → Nmap scan completion and summary
```

---

# 📊 Final Results

| Assessment Area       | Tool     | Result                                      |
| --------------------- | -------- | ------------------------------------------- |
| Domain Reconnaissance | WHOIS    | Domain and nameserver information collected |
| Web Fingerprinting    | WhatWeb  | WordPress, Apache and Bootstrap identified  |
| DNS Resolution        | NSLookup | `192.232.216.135` resolved                  |
| HTTP Inspection       | cURL     | `HTTP/2 200 OK` observed                    |
| WAF Detection         | Wafw00f  | ModSecurity (SpiderLabs) identified         |
| DNS Enumeration       | DNSRecon | SOA, MX, TXT/SPF and SRV records identified |
| Subnet Discovery      | Nmap     | `10.0.0.0/24` scanned                       |
| Result Logging        | Nmap     | Output saved to `nmap_result.txt`           |

---

# 📁 Project Structure

```text
Week-02-Footprinting-Network-Scanning/
│
├── README.md
│
├── screenshots/
│   ├── network-config-whatweb.png
│   ├── nslookup-curl.png
│   ├── wafw00f-dnsrecon.png
│   ├── nmap-scan-start.png
│   └── nmap-scan-complete.png
│
└── results/
    └── nmap_result.txt
```

---

# 👤 Author

**M. Thangamani**

**Program:** Cybersecurity Internship — Networkwalks
**Batch:** B083F
**Intern ID:** NW-83-711
**Week:** 02
**Modules:** W2-PM1 & W2-PM5

---

## 🔐 Lab Summary

```text
RECON → DISCOVER → ANALYZE → DOCUMENT
```

This laboratory strengthened practical understanding of **cybersecurity reconnaissance, OSINT, DNS enumeration, web fingerprinting, WAF identification, network discovery, and Linux network troubleshooting** in an authorized lab environment.
