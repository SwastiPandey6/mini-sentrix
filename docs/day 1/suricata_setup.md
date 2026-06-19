# Suricata IDS Setup

## Objective

Deploy an Intrusion Detection System capable of detecting attacks generated from Kali Linux.

## Why Suricata?

- Open source IDS/IPS
- Fast and lightweight
- Supports Emerging Threat rules
- Compatible with Wazuh and ELK stack
- Recommended for SRM hackathon environment

## Planned Configuration

Security VM:
Ubuntu Server

Detection Engine:
Suricata

Traffic Monitored:
Host-only network (VMnet1)

Attack Source:
Kali Linux

Target:
Metasploitable2

## Status

- [ ] Install Suricata
- [ ] Update rules
- [ ] Configure interface
- [ ] Start service
- [ ] Detect Nmap scan
- [ ] Forward alerts to Wazuh
