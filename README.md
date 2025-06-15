
# 🛡️ Zero Trust Cybersecurity Lab – Ethical Hacking & Wazuh SIEM

**Author:** Sushant Niroula  
**Type:** Hands-on Offensive Security Lab with Real-Time Detection  
**Goal:** Simulate attacks in a segmented lab to test and monitor Zero Trust architecture and SIEM response using Wazuh, Hydra, Nmap, OSSEC FIM, and more.

---

## 🧰 Lab Infrastructure & VM Details

| VM Name        | OS           | Role                       | IP Address     | Network Mode | Tools Installed                  |
|----------------|--------------|----------------------------|----------------|--------------|----------------------------------|
| Win-Flare      | Windows 10   | Malware Analysis / Target   | 172.160.0.10   | Host-Only    | FlareVM                          |
| Kali-Linux     | Kali Rolling | Attacker Machine            | 10.0.0.131     | NAT          | Hydra, Nmap, Metasploit          |
| Ubuntu-Client  | Ubuntu 24.04 | Wazuh Agent (Endpoint)      | 10.0.0.10      | NAT          | Wazuh Agent, SSH Server          |
| Wazuh-Manager  | Ubuntu 22.04 | Wazuh SIEM Server           | 10.0.0.12      | NAT          | Wazuh Manager, Dashboard         |
| Windows-Agent  | Windows 10   | Wazuh Agent                 | 10.0.0.13      | NAT          | Wazuh Agent                      |

---

## 🗺️ Network Diagram

```
[ Kali VM (10.0.0.131) ] <---> [ NAT Network ] <---> [ Wazuh Manager (10.0.0.12) ]
                                             |
                      ------------------------------------------------------
                      |                         |                          |
            [ Ubuntu Client (10.0.0.10) ]   [ Windows Agent (10.0.0.13) ]  [ Metasploit VM (10.0.0.132) ]

[ Win-Flare (172.160.0.10) ] <---> [ Host-Only Network ]
```

---

## 📷 Screenshots Placeholder

You can include screenshots in the `/images/` directory. Suggested filenames:

![alt text](<Screenshot 2025-06-11 121003.png>) ![alt text](<Screenshot 2025-06-11 155820.png>) ![alt text](<Screenshot 2025-06-11 161920.png>) ![alt text](<Screenshot 2025-06-11 162001.png>) ![alt text](<Screenshot 2025-06-11 164129.png>) ![alt text](<Screenshot 2025-06-11 165654.png>) ![alt text](<Screenshot 2025-06-11 165825.png>)


---

## 🛠️ Simulation & Detection Configuration

### 🔁 Simulate Brute Force Attack with Hydra:

```bash
hydra -l testuser -P /usr/share/wordlists/rockyou.txt ssh://10.0.0.10 -t 4 -f -o hydra_results.txt
```

### 🕵️ Recon Scan using Nmap:

```bash
nmap -sS -A -Pn -T4 10.0.0.10 -oN nmap_scan.txt
```

### 🧪 Attack Simulation Script

```bash
#!/bin/bash
echo "[*] Starting Attack Simulation..."
hydra -l testuser -P /usr/share/wordlists/rockyou.txt ssh://10.0.0.10 -t 4 -f -o hydra_results.txt
nmap -sS -Pn -T4 10.0.0.10 -oN nmap_scan.txt
```

Make executable and run:

```bash
chmod +x simulate_attack.sh
./simulate_attack.sh
```

---

## 📌 Ubuntu Agent File Integrity Monitoring

Example syscheck configuration:

```xml
<syscheck>
  <disabled>no</disabled>
  <scan_on_start>yes</scan_on_start>
  <directories check_all="yes">/etc,/usr/bin,/usr/sbin,/bin,/sbin</directories>
  <frequency>43200</frequency>
</syscheck>
```

Trigger a FIM event:

```bash
sudo touch /etc/fim-test.txt
echo "integrity check" | sudo tee -a /etc/fim-test.txt
sudo /var/ossec/bin/syscheck_control -u
```

---

## 🔐 Security Compliance Mapping

### MITRE ATT&CK Framework

| ATT&CK ID | Name                        | Description                            |
|-----------|-----------------------------|----------------------------------------|
| T1110     | Brute Force                 | Hydra attacks simulate this technique  |
| T1046     | Network Service Scanning    | Nmap scanning                          |
| T1059     | Command and Scripting Interp| Metasploit and Bash-based attacks      |
| T1078     | Valid Accounts              | Reused credentials for SSH access      |

### HIPAA Security Rule Mapping

| HIPAA Control        | Lab Feature                              |
|----------------------|-------------------------------------------|
| §164.308(a)(1)       | Risk Analysis via simulated attack logs   |
| §164.312(b)          | Audit Controls through Wazuh logs         |
| §164.312(c)(1)       | Integrity through FIM monitoring          |
| §164.308(a)(5)       | Workforce Training via Red/Blue exercises |

### PCI-DSS v4.0 Mapping

| PCI Control            | Lab Feature                              |
|------------------------|-------------------------------------------|
| 10.2.x                 | Automated audit logs with Wazuh           |
| 11.2                  | Vulnerability assessment (Nmap, Hydra)    |
| 10.6.1                | Review of security events (SIEM)          |
| 12.10.x               | Incident response preparation             |

---

## 🔒 Zero Trust Enforcement Summary

- Network segmentation via NAT vs Host-Only  
- Wazuh Agents whitelist manager IP only  
- Limited access paths for sensitive endpoints  
- Attackers constrained to pre-configured NAT zone  
- Logs & access controlled to visualize suspicious behavior

---

## 🧾 How to Use This Lab

- Clone the repo to your local machine  
- Setup the VMs with corresponding IPs and network modes  
- Run `simulate_attack.sh` from Kali VM  
- Log into Wazuh Dashboard (`https://10.0.0.12:55000`) and observe alerts

---

## 👨‍💻 Author

**Sushant Niroula**  
Cybersecurity Enthusiast | Ethical Hacking & SIEM | Blue & Red Team Simulation  

---

💬 Feel free to fork, clone, open issues, and contribute!


