# Security VM Setup

## Objective
Create a dedicated security monitoring machine.

## Operating System
Ubuntu Server 24.04 LTS

## Role
This machine is responsible for:

- Running Suricata IDS
- Running Wazuh Agent
- Monitoring traffic between Kali and Metasploitable2

## Hardware Allocation

| Resource | Value |
|-----------|------|
| RAM | 2 GB |
| CPU | 1 Processor, 2 Cores |
| Disk | 30 GB |
| Network | Host-only (VMnet1) |

## Status

- [x] Ubuntu ISO downloaded
- [x] VM created
- [x] Host-only network configured
- [ ] Ubuntu installation completed
- [ ] Suricata installed
- [ ] Wazuh agent configured
