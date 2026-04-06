# 🛡️ Network Scanning & Host Enumeration with Nmap

![Nmap](https://img.shields.io/badge/Tool-Nmap-blue?style=for-the-badge&logo=nmap)
![Kali Linux](https://img.shields.io/badge/Platform-Kali_Linux-lightgrey?style=for-the-badge&logo=kali-linux)

## 🚀 Project Overview
This project demonstrates professional infrastructure discovery and security auditing techniques using **Nmap**. The primary objective was to identify the attack surface of a local network by enumerating active hosts, identifying open ports, and fingerprinting services/OS versions.

> **Key Skill:** Reconnaissance & Threat Modeling for SOC Analysts.
## 🛠️ Tech Stack & Tools
* **Operating System:** Kali Linux (Rolling Edition)
* **Scanner:** Nmap (Network Mapper)
* **Documentation:** Markdown & VS Code
* **Environment:** Private Lab / Controlled Subnet

---

## ⚙️ Execution Workflow

### 1️⃣ Network Discovery & Scoping
Before scanning, I identified the network boundaries to ensure an authorized and efficient scan.
* **Local Gateway:** `192.168.1.1`
* **Subnet Range:** `192.168.1.0/24`
* **Command:** `ip a` or `ifconfig`

  ![image alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/098b9199b20885f31362ed041233a90fbfef6548/ipconfig.png)

### 2️⃣ Host Discovery (Ping Sweep)
To minimize noise and avoid detection by basic firewalls, I performed a host discovery scan to map live assets.
```bash
nmap -sn 192.168.1.0/24

---

## 📊 Summary of Findings

| Host IP | Status | Open Ports | Identified Services |
| :--- | :--- | :--- | :--- |
| **192.168.1.1** | Up | 53, 80, 443 | Router (DNS, HTTP, HTTPS) |
| **192.168.1.3** | Up | None | Unknown Device (Likely hardened) |
| **192.168.1.22**| Up | 135, 445, 8000| Windows Host (SMB, Splunk) |

> [!IMPORTANT]
> **Key Security Insight:** Port 445 (SMB) was found open on host 192.168.1.22. In a real-world scenario, this would be a high-priority target for lateral movement or exploit attempts (e.g., EternalBlue).

---

## 🖼️ Evidence

![image alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/098b9199b20885f31362ed041233a90fbfef6548/ipconfig.png)

---

## ⚖️ Ethical Disclosure
This project was conducted in a strictly private and authorized lab environment.
