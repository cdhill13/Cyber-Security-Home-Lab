# Network Reconnaissance Lab

## Objective

The objective of this lab was to practice network reconn and
service enumeration against a target within an isolated VirtualBox 
environment.

## Lab Environment

| System | Role | IP Address |
| Kali Linux | Security workstation | 192.168.56.101 |
| Ubuntu Server | Target | 192.168.56.103 |

Both systems communicate through a VirtualBox Host-Only network,
keeping the target isolated from the external network.

## Tools Used

- Kali Linux
- Ubuntu Server
- VirtualBox
- Nmap

## Methodology
### 1. Connectivity Testing

I first verified that the target was reachable from the Kali Linux
system using ICMP.

Command:

    ping -c 4 192.168.56.103

The target responded successfully with 0% packet loss.

### 2. Initial Port Scan

I performed a standard Nmap scan to identify commonly exposed TCP
services.

Command:

    nmap 192.168.56.103

The scan identified TCP port 22 as open and associated with SSH.

### 3. Service and Version Detection

After identifying SSH, I performed service/version detection.

Command:

    nmap -sV 192.168.56.103

Nmap identified the SSH service as OpenSSH running on Ubuntu Linux.

### 4. Full TCP Port Scan

I scanned the complete TCP port range to determine whether services
were listening outside Nmap's default port selection.

Command:

    nmap -p- 192.168.56.103

The scan found:

- 65,534 closed TCP ports
- TCP/22 open
- No additional open TCP ports

## Findings

### SSH Service

**Port:** TCP/22  
**State:** Open  
**Service:** SSH  
**Implementation:** OpenSSH  
**Operating System:** Ubuntu Linux

SSH was the only TCP service identified on the target during the
full-port scan.

## Analysis

The initial scan demonstrated how basic network recon can
identify exposed services on a target system.

Service/version detection provided additional information about the
software behind the exposed port, while the full TCP scan verified
that no additional TCP services were listening outside Nmap's default
scan range.

This demonstrates why reconn typically progresses from broad
service discovery into more targeted enumeration.

## Evidence

Raw Nmap results are available in the `scans/` directory:

- `initial-scan.txt`
- `service-scan.txt`
- `full-tcp-scan.txt`

## Skills Practiced

- Network recon
- TCP port scanning
- Service enumeration
- Service/version detection
- Nmap
- Linux networking
- VirtualBox network isolation

## Lessons Learned

This lab demonstrated the difference between identifying an open port,
identifying the service associated with that port, and enumerating the
specific software providing that service.

It also demonstrated the importance of scanning beyond Nmap's default
port selection when assessing a system's exposed TCP services.

