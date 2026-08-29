# Network Recon Lab

## Overview

This was the first hands-on project in my cybersecurity home lab.

The goal was to get my Kali Linux and Ubuntu Server VMs communicating over an isolated network and then use Kali to perform basic network recon against the Ubuntu target.

I wanted to get more comfortable with Nmap and understand the difference between finding an open port, identifying the service running on it, and gathering more information about that service.

---

## Lab Setup

| Machine | Role | IP Address |
|---|---|---|
| Kali Linux | Attacker / Security Workstation | 192.168.56.101 |
| Ubuntu Server | Target | 192.168.56.103 |

Both machines are running in VirtualBox and communicate through a Host-Only network.

This lets me practice against the Ubuntu target without exposing it directly to my normal network.

---

## Tools Used

- Kali Linux
- Ubuntu Server
- VirtualBox
- Nmap

---

# What I Did

## 1. Tested Connectivity

Before scanning anything, I wanted to make sure Kali could communicate with the Ubuntu target.

From Kali, I ran:

    ping -c 4 192.168.56.103

The target responded to all four packets with 0% packet loss.

This confirmed that both machines were connected to the Host-Only network and could communicate with each other.

---

## 2. Initial Nmap Scan

Once I knew the target was reachable, I ran a basic Nmap scan:

    nmap 192.168.56.103

The scan found one open TCP port:

    PORT    STATE   SERVICE
    22/tcp  open    ssh

This told me that the target was accepting TCP connections on port 22 and that Nmap associated the port with SSH.

The other 999 TCP ports included in Nmap's default scan were closed.

---

## 3. Service and Version Detection

Knowing that port 22 was open was useful, but I wanted to know more about what was actually running on it.

I used Nmap's service/version detection:

    nmap -sV 192.168.56.103

This gave me more information about the SSH service and identified it as OpenSSH running on Ubuntu Linux.

This helped me understand the difference between simply finding an open port and actually enumerating the service behind that port.

---

## 4. Full TCP Port Scan

The basic Nmap scan checks 1,000 commonly used TCP ports.

I wanted to make sure the target wasn't running another service on a port outside of Nmap's default scan, so I scanned the full TCP port range:

    nmap -p- 192.168.56.103

The scan checked all 65,535 TCP ports.

The result was:

    65,534 closed TCP ports
    1 open TCP port

The only open TCP port was still:

    22/tcp open ssh

This confirmed that SSH was the only TCP service exposed by the target at the time of the scan.

---

# What I Found

### SSH

**Port:** 22/TCP  
**State:** Open  
**Service:** SSH  
**Software:** OpenSSH  
**Operating System:** Ubuntu Linux

SSH was the only open TCP service I found on the target.

Because this is a machine I built myself, I also know why SSH is there: I installed OpenSSH Server while setting up Ubuntu.

Seeing Nmap discover that service from Kali helped connect what was configured on the target with what another machine can actually see over the network.

---

# Scan Results

I saved the raw Nmap output from each stage instead of only keeping screenshots.

The scan files are available in the [`scans`](./scans/) directory:

- [`initial-scan.txt`](./scans/initial-scan.txt)
- [`service-scan.txt`](./scans/service-scan.txt)
- [`full-tcp-scan.txt`](./scans/full-tcp-scan.txt)

---

# What I Learned

This lab helped me understand how network recon builds on itself.

Instead of immediately running a large number of tools, I started simple:

    Confirm target is reachable
              ↓
       Find open ports
              ↓
       Identify services
              ↓
      Identify versions
              ↓
     Check full port range
              ↓
        Analyze results

I also learned that finding an open port is only the beginning. Knowing that port 22 is open tells me something is accessible, but service detection gives me much more useful information about what's actually running behind that port.

Scanning the full TCP range also showed me why I shouldn't assume Nmap's default scan covers every possible service.

---

# Skills Practiced

- Network recon
- TCP/IP fundamentals
- Nmap
- TCP port scanning
- Service enumeration
- Service/version detection
- Linux networking
- VirtualBox networking
- Reading and interpreting scan results
- Documenting technical findings

---

## Next Steps

My next goal is to continue building on this environment instead of treating this as a one-time scan.

I plan to use the same lab to get more experience with:

- Linux security and enumeration
- Network traffic analysis with Wireshark
- Web application security
- Burp Suite
- Python security automation
- Additional intentionally vulnerable targets

---

## Disclaimer

All testing in this lab was performed against systems I own and configured specifically for cybersecurity practice.
