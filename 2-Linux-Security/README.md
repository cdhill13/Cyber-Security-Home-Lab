# Linux Security & SSH Enumeration Lab

## Overview

This lab focused on understanding Linux permissions, users, services, processes, and how to perform basic SSH enumeration from the outside of a target.

I used my Ubuntu Server as the target and Kali Linux as the security workstation.

Part of the lab was done from inside the Ubuntu system to understand how Linux handles permissions and services. The second part was done from Kali as a black-box style assessment, where I treated the Ubuntu machine like an unknown target and only used information exposed over the network.

---

## Lab Environment

| Machine | Role | IP Address |
|---|---|---|
| Kali Linux | Security Workstation | 192.168.56.101 |
| Ubuntu Server | Target | 192.168.56.103 |

Both systems communicate through a VirtualBox Host-Only network.

---

# Linux Security Fundamentals

## Users and Privileges

I started by learning how Linux identifies users and controls their privileges.

Some of the commands I used were:

    whoami
    id
    sudo -l

From this I learned:

- `whoami` shows the current user
- `id` shows the user's UID, GID, and group memberships
- being part of the `sudo` group allows an authorized user to run commands with elevated privileges
- being able to use `sudo` does not mean the user is always operating as root

I also learned the principle of least privilege, where users and processes should only have the permissions they actually need.

---

## File Permissions

I practiced reading Linux permissions such as:

    -rwxr-x---

I learned that permissions are split between:

    Owner | Group | Others

and that:

    r = read
    w = write
    x = execute

I also practiced modifying permissions with `chmod`.

Examples:

    chmod u+x test.sh
    chmod 700 test.sh
    chmod 740 test.sh

This helped me understand both symbolic permissions and numeric permissions.

---

## File Ownership

I also practiced changing file ownership with:

    chown

For example:

    sudo chown root test.sh

This showed me that file access depends on both:

- who owns the file
- what permissions the owner, group, and others have

---

## Processes and Services

I learned the difference between a process, service, protocol, and port.

Example:

    sshd
      ↓
    SSH
      ↓
    TCP
      ↓
    Port 22

I used commands such as:

    ps aux
    systemctl status
    ss -ltn
    ss -ltnp

This helped me connect running services to the actual processes behind them.

I also learned that Ubuntu can use systemd socket activation for SSH.

In this setup:

    ssh.socket

was listening on port 22 even when:

    ssh.service

was inactive.

Once an SSH connection was made, the SSH server process was started to handle the session.

---

## Service Management

I practiced using `systemctl` to understand how Linux services are managed.

Commands included:

    systemctl status
    systemctl start
    systemctl stop
    systemctl restart
    systemctl enable
    systemctl disable

The main difference I learned was:

    start / stop
    → affects the service right now

    enable / disable
    → controls whether the service starts automatically at boot

I also used:

    systemctl list-units --type=service --state=running

to view currently running services.

---

# Host Discovery

Before scanning individual systems, I learned how to discover which systems were alive on the lab network.

I used:

    nmap -sn 192.168.56.0/24

This performed host discovery without doing a port scan.

From this I learned the difference between:

    Host discovery
    → Which systems are reachable?

    Port scanning
    → What ports are open on a specific system?

After powering on the Ubuntu target, Nmap discovered:

    192.168.56.103

as an active host.

---

# SSH Enumeration

After discovering the target, I performed a basic port scan:

    nmap 192.168.56.103

The scan found:

    22/tcp open ssh

I then performed service and version detection:

    nmap -sV 192.168.56.103

This identified OpenSSH running on Ubuntu Linux.

---

## SSH Cryptographic Algorithms

I used Nmap's SSH enumeration script:

    nmap -p 22 --script ssh2-enum-algos 192.168.56.103

This showed the cryptographic algorithms supported by the SSH server.

The output included categories such as:

- key exchange algorithms
- host key algorithms
- encryption algorithms
- MAC algorithms
- compression methods

The main thing I learned from this was not to memorize every algorithm name, but to understand what each category represents and identify anything that may be outdated or worth researching further.

---

## SSH Authentication Methods

I also checked which authentication methods the server allowed:

    nmap -p 22 --script ssh-auth-methods --script-args="ssh.user=labuser" 192.168.56.103

The server allowed:

    publickey
    password

This showed me that password authentication being enabled increases the attack surface, but does not automatically mean the server is vulnerable.

---

## SSH Host Keys

I used:

    ssh-keyscan 192.168.56.103

to remotely retrieve the SSH server's public host keys.

The server advertised:

- RSA
- ECDSA
- ED25519

I then converted the keys into SHA256 fingerprints using:

    ssh-keyscan 192.168.56.103 2>/dev/null | ssh-keygen -lf -

This helped me understand that SSH host keys are used to verify the identity of the server.

A changed host key can be caused by things like rebuilding the server or regenerating keys, but it can also be a warning sign that the connection should be verified before continuing.

---

# Vulnerability Research

After identifying the OpenSSH version, I researched whether the installed version was associated with known vulnerabilities.

The Ubuntu target was running:

    OpenSSH 10.2p1
    Ubuntu package: 1:10.2p1-2ubuntu3.5

Ubuntu published a security update containing fixes in:

    1:10.2p1-2ubuntu3.6

This showed that the installed package was behind the current security revision. :contentReference[oaicite:0]{index=0}

I also reviewed CVE-2026-73283.

The issue affects `sshd` and involves SSH tunnel-forwarding restrictions, but the attack requires an attacker who already has an authorized SSH key on the system. :contentReference[oaicite:1]{index=1}

Because of that requirement, I would not consider this CVE a useful path for gaining initial unauthenticated access to the target.

This helped me understand an important difference between:

    A software version being associated with a CVE

and

    A vulnerability actually being exploitable in the current environment

---

# What I Learned

This lab helped me better understand the connection between Linux security and network enumeration.

Some of the main things I learned were:

- how Linux users, groups, and privileges work
- how file permissions and ownership affect access
- how `sudo` changes the privilege level of a command
- how services relate to running processes
- how listening ports expose services to the network
- how to discover hosts on a subnet
- how to enumerate SSH remotely
- how SSH authentication and host keys work
- how to research vulnerabilities without assuming a CVE automatically means a system is exploitable

One of the biggest lessons from this lab was:

    Enumeration is not the same as exploitation.

Finding an open port, enabled feature, or outdated package gives me something to investigate, but I still need to verify whether the condition is actually vulnerable and whether the required attack conditions are present.

---

# Evidence

Raw output from the exercises and enumeration performed during this lab is saved in the [`evidence`](./evidence/) directory.

- [Host Discovery](./evidence/host-discovery.txt)
- [SSH Service and Version Scan](./evidence/ssh-service-scan.txt)
- [SSH Cryptographic Algorithms](./evidence/ssh-algorithms.txt)
- [SSH Authentication Methods](./evidence/ssh-auth-methods.txt)
- [SSH Host-Key Fingerprints](./evidence/ssh-hostkey-fingerprints.txt)

These files contain the original command output used to support the findings documented in this lab.

---

## Next Steps

Next I plan to continue with:

- deeper Linux enumeration
- searching for interesting files and permissions
- service and configuration enumeration
- identifying possible privilege escalation paths
- practicing against intentionally vulnerable systems

---

## Disclaimer

All testing documented in this lab was performed against systems I own and configured specifically for cybersecurity practice.
