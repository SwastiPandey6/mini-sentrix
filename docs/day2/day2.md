# Day 2 – Wazuh Agent Integration and Monitoring

## Objective

Set up Mini-Sentrix components by integrating Wazuh Agent with a remote Wazuh Manager, performing reconnaissance against Metasploitable2, and validating monitoring capabilities.

---

## Environment

### Attacker Machine

* Kali Linux VM
* IP Address: 192.168.88.128 / 10.9.26.54

### Target Machine

* Metasploitable2
* IP Address: 192.168.88.129

### Wazuh Manager

* Docker-based deployment on teammate's Windows laptop
* Version: 4.14.5

### Wazuh Agent

* Ubuntu VM
* Version: 4.14.5

---

## Tasks Performed

### 1. Installed Wazuh Agent

Configured Wazuh repository and installed:

```bash
sudo apt install wazuh-agent
```

Verified installation:

```bash
dpkg -l | grep wazuh-agent
```

---

### 2. Reconnaissance using Nmap

Performed service and OS detection against Metasploitable2:

```bash
sudo nmap -sS -A 192.168.88.129
```

Discovered services:

* FTP (21)
* SSH (22)
* Telnet (23)
* SMTP (25)
* DNS (53)
* HTTP (80)
* Samba (139,445)
* MySQL (3306)
* PostgreSQL (5432)
* VNC (5900)
* IRC (6667)
* Tomcat (8180)

---

### 3. Suricata Monitoring

Observed alerts through:

```bash
sudo tail -f /var/log/suricata/fast.log
```

Detected:

* TLS invalid record type
* Application layer mismatch events
* Traffic anomalies generated during scanning

---

### 4. Networking Troubleshooting

Changed VMware adapter configuration to Bridged mode.

Verified interfaces:

```bash
ip a
```

Obtained:

* ens33 → 192.168.88.132
* ens37 → 10.9.26.54

---

### 5. Remote Wazuh Manager Integration

Connected agent to teammate's Wazuh Manager.

Manager IP:

```
10.9.216.162
```

Modified:

```
/var/ossec/etc/ossec.conf
```

Restarted agent:

```bash
sudo systemctl restart wazuh-agent
```

---

### 6. Version Mismatch Issue

Encountered:

```
Agent version must be lower or equal to manager version
```

Cause:

* Agent version 4.14.5
* Manager version 4.7.5

---

### 7. Upgraded Wazuh Manager

Teammate updated Docker containers:

```bash
docker compose down
docker compose pull
docker compose up -d
```

Manager upgraded to:

```
Wazuh 4.14.5
```

---

### 8. Successful Agent Connection

Verified through:

```bash
sudo tail -f /var/ossec/logs/ossec.log
```

Observed:

```
Connected to the server
Agent is now online
```

---

## Outcome

Successfully established communication between the Ubuntu Wazuh Agent and the remote Docker-based Wazuh Manager.

The setup provides:

* Log collection
* File integrity monitoring
* Security configuration assessment
* Remote agent management

---

## Tools Used

* Wazuh 4.14.5
* Docker
* VMware
* Nmap
* Suricata
* Metasploitable2
* Ubuntu
* Kali Linux
