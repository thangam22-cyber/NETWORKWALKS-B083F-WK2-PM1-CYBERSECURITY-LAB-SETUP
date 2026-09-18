<div align="center">

# 🔐 Cybersecurity Lab — Footprinting & Network Scanning

### Week 02 — Target Reconnaissance & Subnet Discovery

**Networkwalks Cybersecurity Internship · Batch B083F**

<br>

![Kali Linux](https://img.shields.io/badge/Kali%20Linux-Rolling-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-7.x-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-Network%20Scanning-1F6FEB?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-2EA44F?style=for-the-badge)

</div>

---

## 📌 Project Overview

This project documents the completion of **Week 02 laboratory exercises** for the **Networkwalks Cybersecurity Internship — Batch B083F**.

The laboratory covers two modules:

- **W2-PM1 — Target Footprinting**
- **W2-PM5 — Network Scanning**

The first module focuses on reconnaissance and information gathering against the authorized target domain `networkwalks.com`.

The second module focuses on active host discovery across the authorized `10.0.0.0/24` laboratory subnet using Nmap.

---

<div align="center">

## 🎯 Objectives

</div>

- Perform passive and active reconnaissance against the authorized target.
- Collect domain registration and DNS information.
- Identify web application technologies.
- Inspect HTTP response headers.
- Detect the presence of a Web Application Firewall.
- Enumerate DNS records.
- Discover active hosts within the authorized subnet.
- Capture and document command outputs.
- Troubleshoot Kali Linux network connectivity issues.

---

<div align="center">

## 🏗️ Lab Architecture

</div>

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
        │    Active Hosts     │                 │ DNSRecon            │
        │   & Host Details    │                 └──────────┬──────────┘
        └─────────────────────┘                            │
                                                           ▼
                                               ┌─────────────────────┐
                                               │  Reconnaissance     │
                                               │      Results        │
                                               └─────────────────────┘
```

---

<div align="center">

## ⚙️ Lab Configuration

</div>

| Category | Configuration |
|---|---|
| Operating System | Kali Linux Rolling |
| Virtualization | Oracle VM VirtualBox |
| Network Interface | `eth0` |
| VM IP Address | `10.0.0.2/24` |
| Gateway | `10.0.0.1` |
| External Target | `networkwalks.com` |
| Internal Network | `10.0.0.0/24` |
| Primary Scanner | Nmap |

---

<div align="center">

## 🧰 Tools Used

</div>

| Tool | Purpose | Key Observation |
|---|---|---|
| `whois` | Domain information | Registrar and nameserver details |
| `whatweb` | Web technology detection | WordPress, Apache, Bootstrap |
| `nslookup` | DNS resolution | `192.232.216.135` |
| `curl` | HTTP header inspection | `HTTP/2 200 OK` |
| `wafw00f` | WAF detection | ModSecurity (SpiderLabs) |
| `dnsrecon` | DNS enumeration | SOA, MX, TXT/SPF, SRV |
| `nmap` | Host discovery | `10.0.0.0/24` |

---

<div align="center">

# 🔎 Module 1 — Target Footprinting

</div>

### 1. WHOIS Enumeration

```bash
whois networkwalks.com
```

Used to collect publicly available domain registration and nameserver information.

**Information collected:**

- Domain registration information
- Registrar details
- Nameserver information
- Domain status information

---

### 2. Web Technology Detection

```bash
whatweb networkwalks.com
```

**Observed Technologies:**

```text
WordPress
Apache
Bootstrap
```

---

### 3. DNS Resolution

```bash
nslookup networkwalks.com
```

**Resolved IP Address:**

```text
192.232.216.135
```

---

### 4. HTTP Header Inspection

```bash
curl -I https://networkwalks.com
```

**Observed Response:**

```text
HTTP/2 200 OK
```

The response also provided HTTP header and cookie-related information.

---

### 5. WAF Detection

```bash
wafw00f https://networkwalks.com
```

**Detected WAF:**

```text
ModSecurity (SpiderLabs)
```

---

### 6. DNS Enumeration

```bash
dnsrecon -d networkwalks.com
```

**Observed DNS Records:**

```text
SOA
MX
TXT / SPF
SRV
```

---

<div align="center">

# 🌐 Module 2 — Subnet Host Discovery

</div>

### Authorized Network

```text
10.0.0.0/24
```

### Nmap Host Discovery

```bash
sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt
```

The `-sn` option performs host discovery without performing a traditional port scan.

The `-oN` option saves the output to:

```text
nmap_result.txt
```

### View Saved Results

```bash
cat nmap_result.txt
```

The scan examined the complete `/24` network containing **256 IP addresses** and recorded responsive hosts.

---

<div align="center">

# 🐞 Challenges Faced & Solutions

</div>

## Problem 1 — Network & DNS Resolution Failure

### Issue

```text
Temporary failure in name resolution
Network is unreachable
```

### Cause

After restarting the Kali Linux VM, the network interface, default route, and DNS resolver configuration required restoration.

### Solution

```bash
sudo ip addr flush dev eth0
sudo ip addr add 10.0.0.2/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 10.0.0.1 dev eth0
```

DNS configuration:

```bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

After restoring the network configuration, the reconnaissance commands were executed successfully.

---

## Problem 2 — Zenmap GUI Issues

### Issue

Zenmap generated GUI/deprecation warnings and the detailed scan was slower than required for the host-discovery task.

### Solution

The command-line version of Nmap was used instead:

```bash
sudo nmap -sn 10.0.0.0/24 -oN nmap_result.txt
```

This provided direct terminal output and easy result logging.

---

<div align="center">

# 🧠 Technical Concepts Learned

</div>

### 🔍 OSINT & Reconnaissance

Understanding how publicly available domain information can be collected during an authorized security assessment.

### 🌐 DNS Enumeration

Understanding how DNS records can provide information about domain infrastructure.

### 🧬 Web Fingerprinting

Identifying technologies and server components exposed by a web application.

### 🛡️ WAF Detection

Identifying the presence of a Web Application Firewall using Wafw00f.

### 📡 Network Discovery

Using Nmap to identify active hosts within an authorized subnet.

### 🛠️ Linux Network Troubleshooting

Practicing manual configuration of IP addressing, routing, network interfaces, and DNS resolution.

---

<div align="center">

# 📸 Proof of Execution

</div>

The following screenshots document the laboratory execution:

| Screenshot | Description |
|---|---|
| `01-network-config-whatweb.png` | Network configuration and WhatWeb output |
| `02-nslookup-curl.png` | NSLookup and cURL output |
| `03-wafw00f-dnsrecon.png` | Wafw00f and DNSRecon output |
| `04-nmap-scan-start.png` | Nmap scan execution |
| `05-nmap-scan-complete.png` | Nmap scan completion |

---

<div align="center">

# 📊 Final Results

</div>

| Module | Activity | Result |
|---|---|---|
| W2-PM1 | WHOIS | Domain information collected |
| W2-PM1 | WhatWeb | WordPress, Apache and Bootstrap identified |
| W2-PM1 | NSLookup | `192.232.216.135` resolved |
| W2-PM1 | cURL | `HTTP/2 200 OK` observed |
| W2-PM1 | Wafw00f | ModSecurity detected |
| W2-PM1 | DNSRecon | DNS records identified |
| W2-PM5 | Nmap | `10.0.0.0/24` scanned |
| W2-PM5 | Result Logging | Output saved to `nmap_result.txt` |

---

<div align="center">

# 📁 Repository Structure

</div>

```text
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
```

---

<div align="center">

# 🔐 Ethical Use

All activities were performed within the authorized scope of the internship laboratory.

**Authorized External Target:** `networkwalks.com`

**Authorized Local Network:** `10.0.0.0/24`

The documented commands should only be used against systems and networks for which appropriate authorization has been provided.

</div>

---

<div align="center">

# 👤 Author

### M. Thangamani

**Cybersecurity Internship — Networkwalks**

**Batch B083F · Intern ID: NW-83-711**

**Week 02 · W2-PM1 & W2-PM5**

<br>

![Status](https://img.shields.io/badge/Lab-Completed-2EA44F?style=flat-square)

</div>
