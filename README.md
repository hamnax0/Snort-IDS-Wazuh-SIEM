# 🛡️ Snort IDS + Wazuh SIEM Integration Lab

> A complete Network Intrusion Detection and SIEM integration lab simulating
> real-world SOC operations using Snort IDS v2.9.20 and Wazuh SIEM on VMware.



**Project Overview**

| | |
|---|---|
| **Project Title** | Snort IDS + Wazuh SIEM Integration Lab |
| **Course** | Network Security Lab |
| **Tools Used** | Snort v2.9.20, Wazuh SIEM, VMware, Kali Linux |

---

**Lab Architecture**

| **Component** | **VM** | **IP Address** | **Role** |
|---|---|---|---|
| Attacker | Kali Linux | 192.168.121.129 | Launch attacks / simulate threat actor |
| Defender | Ubuntu Linux | 192.168.121.132 | Snort IDS + Wazuh Manager + Dashboard |

Both VMs connected via VMware NAT networking on subnet 192.168.121.0/24.

---

**Data Flow Pipeline**---

**Snort Configuration**

| | |
|---|---|
| **Version** | Snort 2.9.20 GRE (Build 82) |
| **Interface** | ens33 |
| **HOME_NET** | 192.168.0.0/16 |
| **Rules File** | /etc/snort/rules/local.rules |
| **Alert Format** | Fast format → /var/log/snort/alert |
| **Run Mode** | Daemon (-D flag) |

---

**Custom Snort Detection Rules**

| **SID** | **Rule Name** | **Detects** | **How It Works** |
|---|---|---|---|
| 1000001 | ICMP Ping Detected | Ping / ICMP flood | Alerts on any ICMP packet to HOME_NET |
| 1000002 | Nmap SYN Scan | TCP port scanning | Detects 20+ SYN packets in 3 seconds from same source |
| 1000003 | Nmap Aggressive Scan | OS/service detection | Matches SYN+FIN flag combo used by Nmap -A |
| 1000004 | Nikto Web Scan | Web vulnerability scan | Looks for "Nikto" string in HTTP headers |
| 1000005 | SSH Brute Force | Password attacks on SSH | Detects 5+ SYN packets to port 22 in 10 seconds |

---

**Wazuh SIEM Components**

| **Component** | **Service Name** | **Purpose** |
|---|---|---|
| Wazuh Manager | wazuh-manager | Core engine — processes and correlates alerts |
| Wazuh Indexer | wazuh-indexer | OpenSearch database — stores all alert data |
| Wazuh Dashboard | wazuh-dashboard | Web UI at https://192.168.121.132 |

---

**Attacks Launched from Kali Linux**



| **#** | **Attack** | **Tool** | **Command** | **Purpose** |
|---|---|---|---|---|
| 1 | ICMP Flood | ping | `sudo ping -f 192.168.121.132` | DoS — 262,000+ packets |
| 2 | TCP SYN Scan | Nmap | `sudo nmap -sS 192.168.121.132` | Stealthy port discovery |
| 3 | Aggressive Scan | Nmap | `sudo nmap -A -T4 192.168.121.132` | OS + version + service detection |
| 4 | Web Vulnerability Scan | Nikto | `nikto -h http://192.168.121.132` | Web server vulnerability scan |
| 5 | SSH Brute Force | Hydra | `hydra -l root -P rockyou.txt ... ssh` | Credential stuffing attack |
| 6 | UDP Flood | hping3 | `sudo hping3 --udp -p 53 --flood ...` | DNS amplification simulation |
| 7 | TCP SYN Flood | hping3 | `sudo hping3 -S --flood -p 80 ...` | HTTP DoS attack |
| 8 | Xmas Scan | Nmap | `sudo nmap -sX 192.168.121.132` | FIN+PSH+URG flag scan |
| 9 | FIN Scan | Nmap | `sudo nmap -sF 192.168.121.132` | Stealth firewall bypass scan |
| 10 | NULL Scan | Nmap | `sudo nmap -sN 192.168.121.132` | No-flag stealthy scan |

---

**Wazuh Alert Classification**


| **Rule ID** | **Description** | **Level** | **Meaning** |
|---|---|---|---|
| 20100 | First time this IDS alert is generated | 8 | New/unique attack signature seen for first time |
| 20101 | IDS event | 6 | Recurring Snort alert matching known pattern |

Alert count increased from 25 → 33+ during testing with real-time dashboard updates.

---

**Key Configuration Files**

| **File Path** | **Purpose** |
|---|---|
| `/etc/snort/rules/local.rules` | Custom Snort detection rules written for this lab |
| `/etc/snort/snort.conf` | Main Snort config — HOME_NET, rule paths, output |
| `/var/ossec/etc/ossec.conf` | Wazuh config — localfile block to monitor Snort logs |
| `/var/log/snort/alert` | Snort alert output file monitored by Wazuh |

---

**Repository Contents**

| **File** | **Description** |
|---|---|
| `Snort_Wazuh_Lab_Report.pdf` | Complete lab report |
| `Snort_Wazuh__Lab_project_ppt.pptx` | Lab presentation |

---

**Skills Demonstrated**

| | |
|---|---|
| **Network Traffic Analysis** | Packet inspection and protocol analysis using Snort |
| **IDS Rule Writing** | Custom Snort rules for real-world attack patterns |
| **SIEM Log Integration** | Snort → Wazuh pipeline configuration |
| **Attack Simulation** | 10 real attack types from Kali Linux |
| **Alert Triage** | Wazuh dashboard analysis and event classification |

---

**License**

Academic project — all documentation is original work produced by the team.
