# Cyber Security Home Lab

Personal cybersecurity lab designed to develop hands-on experience in offensive security, networking, Linux administration, and security automation.

This repository documents the development of an isolated virtual security lab and the practical exercises performed within it.

---

## Objectives

- Develop practical penetration testing skills
- Strengthen networking fundamentals
- Develop Linux administration skills
- Practice web application security
- Build security automation tools with Python
- Document security findings and remediation

---

## Lab Environment

| System | Role | Configuration |
|---|---|---|
| Kali Linux | Security Workstation | VirtualBox VM |
| Ubuntu Server | Target System | VirtualBox VM |
| VirtualBox | Hypervisor | Host-Only Lab Network |

### Network Architecture

    Internet
       |
       |
    Kali Linux
    ├── NAT Interface
    │   └── Internet Access
    │
    └── Host-Only Interface
        192.168.56.101
             |
             | VirtualBox Host-Only Network
             |
        Ubuntu Server
        192.168.56.103

The target environment is isolated using a VirtualBox Host-Only network. Kali maintains a separate NAT interface for Internet access while communicating with lab targets through the isolated interface.

---

# Labs

## 1. Network Recon

My first lab focused on basic network recon inside an isolated VirtualBox environment.

I used Nmap to discover open ports, identify the services running on them, perform service/version detection, and verify the target's full TCP port range.

**Main skills practiced:**
- Host connectivity testing
- TCP port scanning
- Service enumeration
- Nmap service/version detection
- Full TCP port scanning

**Status:** Completed
➡️ [View the Network Recon Lab](./1-Network-Recon/README.md)

---

## 2. Linux Security & SSH Enumeration

This lab focused on Linux security fundamentals and learning how to gather information about a target both from inside the system and remotely from Kali.

I practiced Linux users and groups, permissions, ownership, processes, services, `sudo`, and systemd. I then treated the Ubuntu server like a black-box target and performed host discovery and SSH enumeration.

**Main skills practiced:**
- Linux users, groups, and privileges
- File permissions and ownership
- Processes and services
- Host discovery
- SSH service enumeration
- SSH authentication enumeration
- SSH host-key fingerprinting
- Basic vulnerability research
- CVE analysis

**Status:** In Progress
➡️ [View the Linux Security & SSH Enumeration Lab](./2-Linux-Security/README.md)

---

# Lab Areas

## Networking

Network recon, service enumeration, packet analysis, and TCP/IP fundamentals.

**Status:** Completed

## Linux Security

Linux administration, users and groups, permissions, processes, services, and privilege escalation.

**Status:** In Progress

## Web Security

HTTP, authentication, authorization, input validation, Burp Suite, and common web vulnerabilities.

**Status:** Planned

## Security Automation

Python projects that automate security-related tasks and make parts of my workflow more efficient.

**Status:** Planned

## Network Analysis

Capturing and analyzing network traffic with Wireshark.

**Status:** Planned

---

# Tools & Technologies

## Currently Using

- Kali Linux
- Ubuntu Server
- VirtualBox
- Nmap
- OpenSSH / SSH
- Linux command-line tools
- Git
- GitHub

## Coming Up

- Wireshark
- Burp Suite
- Python security tools
- Additional vulnerable lab environments

---

# How I Document My Labs

For each lab, I try to document:

1. What I wanted to learn
2. How I set up the environment
3. What tools I used
4. What I did and why
5. What I found
6. Evidence and command output
7. What the findings mean
8. How the issue could be fixed when applicable
9. What I learned from the lab

I also keep raw scan results and other useful output so the repository shows both the results and the process I used to get there.

---

# Current Repository Structure

   Cyber-Security-Home-Lab/
│
├── README.md
│
├── 1-Network-Recon/
│   ├── README.md
│   └── scans/
│       ├── initial-scan.txt
│       ├── service-scan.txt
│       └── full-tcp-scan.txt
│
└── 2-Linux-Security/
    ├── README.md
    └── evidence/
        ├── host-discovery.txt
        ├── ssh-service-scan.txt
        ├── ssh-algorithms.txt
        ├── ssh-auth-methods.txt
        └── ssh-hostkey-fingerprints.txt

---

## Disclaimer

Everything in this repository is performed in systems I own or environments where I have permission to test. This lab is built for learning and practicing cybersecurity.
