# 🛡️ Incident Response: Redtail Botnet Intrusion Detection & Mitigation

## 📝 Overview
This project documents a real-world security incident investigation. I identified and mitigated an automated **Redtail Botnet** attack targeting a Dockerized SSH Honeypot (Cowrie). The investigation covers log analysis, malware forensics, and active incident response.

---

## 🔍 Incident Timeline (Feb 28, 2026)
* **00:47:35 UTC**: Initial intrusion detected (Session `48972f8491da`).
* **09:03:28 UTC**: Second wave of attack for persistence (Session `763a86d75a77`).
* **09:30 UTC**: Investigation initiated via terminal log analysis.
* **10:00 UTC**: Mitigation deployed (IP Blocking & System Hardening).

---

## 🛠️ Technical Analysis & Evidence

### 1. Indicators of Compromise (IoCs)
* **Attacker IP**: `130.12.180.51` (Omegatech LTD, US) - Identified as **High Risk**.
* **Malware Family**: Redtail (Cryptojacker).
* **C2 Servers**: Linked to nodes in Japan and Australia.

### 2. Malicious Payloads (SHA-256 Hashes)
I extracted the following hashes from the honeypot's download directory:
* `048e374baac36d8cf68dd32e48313ef8eb517d647548b1bf5f26d2d0e2e3cdc7` (Redtail Binary)
* `d46555af1173d22f07c37ef9c1e0e74fd68db022f2b6fb3ab5388d2c5bc6a98e` (`clean.sh` Evasion Script)

### 3. Attack Tactics (TTPs)
* **Defense Evasion**: Used `chattr -ia` to unlock and modify `authorized_keys`.
* **Persistence**: Injected a static SSH RSA key (`rsa-key-20230629`) for backdoor access.
* **Resource Hijacking**: Automated scripts disabled competing miners (e.g., `c3pool_miner`) to maximize CPU usage.
* **Anti-Forensics**: Executed `rm -rf /tmp /var/tmp /dev/shm` to erase execution traces.

---

## 🚀 Incident Response & Mitigation
To secure the system, I performed the following steps:
1.  **Firewall Configuration**: Installed and enabled **UFW**.
2.  **Network Blocking**: Permanently banned the attacker's IP:
    ```bash
    sudo ufw deny from 130.12.180.51
    ```
3
