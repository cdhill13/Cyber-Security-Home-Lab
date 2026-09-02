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

# Cyber Security Home Lab

This is my personal cybersecurity home lab where I’m building hands-on experience with offensive security, networking, Linux, and security automation.

I’m using this repository to document the labs I build, the tools I learn, what I find, and what I take away from each project.

---

## Objectives

- Build practical penetration testing skills
- Improve my understanding of networking and TCP/IP
- Get more comfortable working with Linux
- Learn web application security
- Build security tools and automation with Python
- Practice documenting findings and explaining my process

---

## Lab Environment

| System | Role | Setup |
|---|---|---|
| Kali Linux | Attacker / Security Workstation | VirtualBox VM |
| Ubuntu Server | Target | VirtualBox VM |
| VirtualBox | Virtualization | Host-Only Lab Network |

### Network Setup

    Internet
       |
    Kali Linux
    ├── NAT
    │   └── Internet Access
    │
    └── Host-Only Adapter
        192.168.56.101
             |
             | Private Lab Network
             |
        Ubuntu Server
        192.168.56.103

The Kali and Ubuntu machines communicate through a VirtualBox Host-Only network. This keeps the target separated from my normal network while still allowing Kali to communicate with it.

Kali also has a separate NAT adapter for Internet access.

---

# Completed Labs

## 1. Network Recon

My first lab focused on basic network recon and learning how to identify what a target is exposing to the network.

I started by confirming that my Kali machine could communicate with the Ubuntu target. From there, I used Nmap to scan the target, identify open ports, and gather more information about the services running on them.

### What I Practiced

- Testing connectivity between machines
- TCP port scanning
- Network recon
- Service enumeration
- Service and version detection
- Full TCP port scanning
- Reading and understanding Nmap results

### What I Found

My initial Nmap scan found one open TCP port:

    22/tcp open ssh

I then used service/version detection to get more information about the service running on port 22.

Nmap identified it as OpenSSH running on Ubuntu Linux.

Finally, I scanned all 65,535 TCP ports to make sure there weren’t services running on ports outside of Nmap’s default scan.

The full scan confirmed that SSH was the only open TCP service on the target.

➡️ [View the Network Recon Lab](./1-Network-Recon/README.md)

---

# Lab Areas

## Networking

Network recon, service enumeration, packet analysis, and TCP/IP fundamentals.

**Status:** In Progress

## Linux Security

Linux administration, users and groups, permissions, processes, services, and privilege escalation.

**Status:** Planned

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
    └── 1-Network-Recon/
        ├── README.md
        │
        └── scans/
            ├── initial-scan.txt
            ├── service-scan.txt
            └── full-tcp-scan.txt

---

## Disclaimer

Everything in this repository is performed in systems I own or environments where I have permission to test. This lab is built for learning and practicing cybersecurity.
