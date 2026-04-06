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
After defining the network scope, the next phase is to identify live assets. I performed a Ping Sweep to map the network without performing a full port scan, which is essential for maintaining a low profile and saving time during the reconnaissance phase.

Command: nmap -sn 192.168.1.0/24

Flag Explanation (-sn): This flag disables port scanning. Nmap sends ICMP echo requests and TCP/ARP pings to determine if a host is online without probing specific services.

Technical Benefit: Using a ping scan is a "stealthier" and faster way to discover active devices compared to a full connect scan, as it avoids triggering alerts for unauthorized port access on many basic monitoring systems.

![image alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/68eb60eaa1baa792bf12ed6cfa56a74abc3bcc20/images/2.ping%20sweep.png)

Results:
The scan successfully identified 4 live hosts within the 192.168.1.0/24 subnet. This allowed me to narrow down the targets for the subsequent service enumeration and vulnerability analysis phases.

### 3️⃣ Service Enumeration & Version Fingerprinting

Once the live hosts were identified, I performed a targeted scan on the primary target (192.168.1.1) to determine the specific services running and their respective versions.

Command: nmap -sV 192.168.1.1

Flag Explanation (-sV): This flag enables version detection. Nmap probes open ports to determine the exact software and version number, which is critical for identifying known vulnerabilities (CVEs).

![image alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/68eb60eaa1baa792bf12ed6cfa56a74abc3bcc20/images/3.sv.png)

Technical Analysis of Results:
The scan revealed that out of 1000 standard TCP ports, 997 are closed (responded with RST packets), leaving 3 active entry vectors:

![image alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/d0627f8f761664f6477017097f4f117d5d53c232/images/3-1.png)

Security Interpretation:
Fingerprinting results strongly suggest this is a network gateway (router). The limited number of open ports indicates a reduced attack surface, following a standard security posture for consumer or small-office networking equipment. 

Save results to a file for reporting 

![image alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/68eb60eaa1baa792bf12ed6cfa56a74abc3bcc20/images/Screenshot%202026-04-06%20200916.png)


### 📊 4️⃣ Documentation of Findings & Security Analysis

After completing the enumeration phase, I consolidated the raw data into a structured format to evaluate the security posture of each discovered asset.

![image alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/68eb60eaa1baa792bf12ed6cfa56a74abc3bcc20/images/4.Report.png)

### 🔍 5️⃣ Investigation & Professional Interpretation

As a Security Analyst, I reflected on the discovered landscape to identify anomalies or potential threats:

Expected Traffic vs. Anomalies: The presence of DNS and HTTP/S on the router, alongside Windows Management services (SMB/RPC) and Splunk on the host, aligns with a standard lab/office environment.

Critical Service Check: I specifically audited the network for legacy or insecure protocols. No instances of FTP (Port 21) or Telnet (Port 23) were detected, which significantly reduces the risk of credential sniffing or clear-text data interception.

Areas of Interest:

SMB (Port 445): While expected on Windows, it remains a primary target for lateral movement. I recommend ensuring this service is only accessible via authenticated sessions.

Splunk (Port 8000): The presence of a SIEM (Splunk) indicates an environment with active logging. This suggests the network is monitored, which is a positive security control.

Unknown Host (192.168.1.3): Although "silent," this host warrants further monitoring to ensure it is an authorized asset.

Summary of Security Posture
The network appears well-maintained with no immediate "low-hanging fruit" vulnerabilities like unencrypted management ports. The primary focus for future hardening should be the SMB and Splunk interfaces on the Windows host to prevent unauthorized access to sensitive logs or file shares.

### 🔍 6️⃣ Extended Tasks: Nmap Deep-Dive


Phase 6.1: Operating System Detection & Fingerprinting
In this phase, I conducted an advanced scan to identify the underlying operating system of the target gateway to better understand its potential vulnerabilities.

Target Selection: I selected the primary gateway 192.168.1.1 from the initial host discovery results.

Command: nmap -O -oN task1_os_detection 192.168.1.1

Flag Explanation (-O): Enables OS detection by analyzing TCP/IP stack fingerprinting.

Flag Explanation (-oN): Redirects the output into a formatted text file (task1_os_detection) for professional documentation.

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/fc34f4c56941664a7f24a88c664719068416f369/images/task.1.png)

OS Fingerprint Results
The scan generated a series of OS guesses based on the target's network response patterns:

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/80409a337ec5d9dc48e69525bdd52c06ee2666c3/images/2.png)






















## ⚖️ Ethical Disclosure
This project was conducted in a strictly private and authorized lab environment.
