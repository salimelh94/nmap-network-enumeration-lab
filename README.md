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

Phase 6.2: Stealth Scanning (TCP SYN Scan)
In this task, I performed a TCP SYN Scan (also known as a "Half-Open" scan) on the gateway to compare efficiency and stealthiness against previous service scans.

Command: nmap -sS 192.168.1.1 -oN task2_syn_scan.txt

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/0d9a4c49c1ce79cc4a18265a70cd4796a9b100d9/images/6-2-2-2.png)


Flag Explanation (-sS): This is the default and most popular scan for good reason. It sends a SYN packet but never completes the 3-way handshake, making it harder for simple logging systems to detect while remaining very fast.

Flag Explanation (-oN): Formats the scan results into a standard text file for the project's audit trail.

Technical Comparison & Reasoning 

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/14fedd1c07f7ed1e734137479eab34bc2ea15c2b/images/6-2-2.png)


Analyst Insight: By utilizing the -sS flag, I was able to verify that the three primary ports (53, 80, 443) were responsive without needing to establish a full TCP session. 
This technique is standard in professional assessments to minimize the "noise" generated on the target network's monitoring logs.

After Scan Completion we open the output: task2_syn_scan.txt and we will see results similar to :
![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/8dc75d4babc958c29cf87dfdc0b43d0a64527fa7/images/task2.png)

Phase 6.3: Final Security Assessment & Findings
After comparing the results from the different scanning methodologies, I consolidated the key findings into a final security audit table for the target gateway (192.168.1.1)

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/a8732f988921e20fe13a34ecbc59c73006834698/images/6-3.png)


Final Reconnaissance Conclusion
Consistency: All three open ports match the expected services identified during the OS Detection phase (Fiberhome/Linux Router).

Security Posture: The device follows the Principle of Least Privilege by only exposing essential services.

Hardening Verification: No high-risk or legacy ports were detected, such as SSH (22), Telnet (23), or non-standard UPnP ports. This indicates a solid baseline security configuration.

🔍 6.3 Phase: UDP Service Discovery
To ensure a comprehensive analysis of the target's attack surface, I performed a UDP Scan. Unlike TCP, UDP is a connectionless protocol, which requires a different scanning approach and often results in longer durations due to the lack of session acknowledgments.

Target IP: 192.168.1.1

Command: nmap -sU 192.168.1.1 -oN task3_udp_scan.txt

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/bfdde381b21111f4c60db6b5e3b1a4dd19042205/images/udp-1.png)


Flag Explanation (-sU): Instructs Nmap to perform a UDP scan to identify services like DNS, SNMP, or DHCP that do not use TCP handshakes.

Flag Explanation (-oN): Redirects the output to a standard text file (task3_udp_scan.txt) for the project's audit trail.

The scan identified that the majority of the 1,000 scanned ports are secured, with only one confirmed service active : 53/udp 
nterpretation & Security Assessment
Significance of Port 53/UDP:
Port 53 is designated for the Domain Name System (DNS). Gateways often perform DNS forwarding to enhance local browsing speeds. Seeing this open for both TCP and UDP (as noted in Task 2 and 3) is a standard configuration.

Hardening Verification:
The absence of common UDP services such as SNMP (161) or NTP (123) suggests that the device has been hardened. Many ISP-provided devices hide or filter these services to prevent remote exploitation.

Final Posture:
The router demonstrates a strong security posture by rejecting the vast majority of UDP services and only exposing essential DNS functionality.

### 🔬 7️⃣ Phase: Port State Analysis & Vulnerability Research
In this final phase, I investigated the "Reason" behind port states and executed Nmap Scripting Engine (NSE) scripts to identify potential security flaws.

7.1: Network Response Analysis (--reason)
I used the --reason flag to understand exactly why Nmap categorized ports as open or closed.

Command: nmap --reason -p- 192.168.1.1 -oN task4_reasons.txt

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/da9a312cb2e3293032730e54ce92eb745b37d25e/images/final%201.png)

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/da9a312cb2e3293032730e54ce92eb745b37d25e/images/final%202.png)

Key Finding: Open ports (53, 80, 443) returned a syn-ack with a ttl 64. This confirms a classic Linux-based TCP/IP stack, typical for embedded router firmware.

Closed Ports: 65,532 ports returned a reset (RST) packet, indicating the device is reachable but no service is listening.

7.2: Lightweight Vulnerability Scan
[!CAUTION]
Safety Reminder: Only perform vulnerability scans on authorized systems. These scans can be resource-intensive and may trigger security alerts.

Command: nmap --script vuln 192.168.1.1 -oN task5_vuln_scan.txt

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/4509e35093c37142ee6ea2149f0a473a13869452/images/final%203.png)

Objective: To cross-reference open services against known vulnerability databases (CVEs).


Primary Finding: Slowloris DoS (CVE-2007-6750)
The scan identified a Likely Vulnerable state on both HTTP (80) and HTTPS (443) ports

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/4509e35093c37142ee6ea2149f0a473a13869452/images/4.png)

### 7.3: Validation & Manual Review
As a security analyst, I validated the findings to filter out false positives:

Vendor Identification: The MAC Address 6C:D7:19:36:DE:B8 confirms the manufacturer is Fiberhome Telecommunication.

Custom Firmware: The web interface uses proprietary code rather than standard Apache/Nginx. This often triggers "Generic" vulnerability detections in Nmap.

Security Headers: Manual review confirmed the presence of X-Frame-Options and HSTS, indicating that the vendor has implemented some modern security controls.

🛡️ 8️⃣ Recommended Remediation
To mitigate the identified risks (Slowloris DoS and unauthorized access), I recommend the following security controls:

![images alt](https://github.com/salimelh94/nmap-network-enumeration-lab/blob/44c9a43f946e5e70127eb747016e5326c136d647/images/5.png)


### 🏁 Final Project Conclusion
This Nmap lab provided a full-cycle security assessment of a local network environment. By moving from Basic Discovery to Advanced Vulnerability Scripting, I demonstrated:

Ability to map an attack surface and identify critical assets.

Deep understanding of TCP/UDP protocols and network responses (syn-ack, rst).

Skill in identifying and validating CVE-2007-6750 (Slowloris).

Capacity to provide actionable remediation steps to stakeholders.






## ⚖️ Ethical Disclosure
This project was conducted in a strictly private and authorized lab environment.
