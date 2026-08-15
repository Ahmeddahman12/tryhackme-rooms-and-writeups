# Room: VulnNet - Active (TryHackMe Write-up)

* **Prepared By:** Ahmed Dahman Saleh[cite: 2]
* **Date:** April 2026[cite: 2]
* **Difficulty:** Medium[cite: 2]

---

## 📌 Introduction
This report documents the security assessment and penetration testing phases conducted on the **VulnNet: Active** target machine. The primary objective was to identify potential entry points, evaluate service vulnerabilities, and demonstrate the impact of misconfigurations within a Windows-based environment[cite: 2].

---

## 🔍 1. Enumeration & Scanning
Initial network enumeration against the Windows target revealed several open services, prioritized for investigation[cite: 2]:
* **Redis Service:** Port `6379`[cite: 2]
* **SMB Service:** Ports `135`, `139`, `445` (Microsoft Windows RPC / SMB)[cite: 2]

Targeted enumeration using `nmap` scripts and `smbclient` showed that anonymous login was initially successful, followed by deeper enumeration to map shares and user accounts[cite: 2].

---

## 🛠️ 2. Redis Service Exploitation & Information Leak
* **Access:** Connected directly to the Redis `2.8.2402` instance using `redis-cli`[cite: 2].
* **Information Leak:** Executed `info` and `CONFIG GET dir` commands, which uncovered the internal system path and a specific local username: `enterprise-security`[cite: 2].
* **Post-Exploitation Attempt:** Leveraged Redis configuration commands to attempt writing a custom file (`flag.txt`) to the target user's desktop directory[cite: 2].

---

## 🔓 3. Credential Brute-Forcing & Spraying
* **Hydra Attempt:** An initial brute-force attack against the SMB service using `hydra` and the `rockyou.txt` wordlist for the administrator account encountered protocol constraints and 'invalid reply' errors due to connection handling[cite: 2].
* **NetExec Switch:** Switched to **NetExec (`nxc`)** to effectively target the Windows 10 / Server 2019 machine within the `vulnnet.local` domain[cite: 2]. After resolving encoding compatibility with the wordlist, systematic checks were performed, recording multiple `STATUS_LOGON_FAILURE` responses during the authentication process[cite: 2].

---
