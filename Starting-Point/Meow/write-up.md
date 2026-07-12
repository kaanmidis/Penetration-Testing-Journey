# HackTheBox — Meow (Starting Point) Write-up

**Difficulty:** Easy | **Category:** Starting Point | **Target IP:** 10.129.104.226 | **Time to Root:** ~15–20 minutes

## Overview

Meow is an introductory "Starting Point" machine on HackTheBox designed to demonstrate one of the most fundamental — yet still surprisingly common — security failures in real-world environments: **unauthenticated or default-credentialed legacy services**. This box specifically highlights the risks of running Telnet, a plaintext, unencrypted remote access protocol, with default factory credentials still in place.

While this is a beginner-level exercise, the underlying vulnerability class (legacy protocols + default credentials) is directly relevant to enterprise and financial-sector environments, where legacy infrastructure, forgotten management interfaces, and unchanged default passwords remain a real attack surface during penetration tests and red team engagements.

## Skills Demonstrated

- Network reconnaissance and service enumeration
- Service fingerprinting with Nmap
- Exploiting default credentials on a legacy remote access protocol
- Basic post-exploitation triage on a Linux host

## Methodology

### 1. Reconnaissance / Enumeration

I started with a fast port scan using Rustscan to identify open ports, followed by a detailed Nmap scan for service/version fingerprinting:

```bash
nmap -Pn -p 23 -sV -sC 10.129.104.226
```

**Result:** Port `23/tcp` open, running a **Telnet** service.

Telnet's presence alone is a red flag — it transmits all traffic, including credentials, in cleartext, and has been considered insecure for remote administration for decades. SSH has been the standard replacement since the late 1990s.

### 2. Initial Access

I connected to the exposed Telnet service:

```bash
telnet 10.129.104.226 23
```

The service accepted the default credential pair (`root` / `root`), granting an immediate interactive shell with **root-level privileges** — no privilege escalation required.

### 3. Post-Exploitation

With shell access established, I confirmed my foothold and collected the objective flags:

```bash
whoami
id
uname -a
ls
cat flag.txt
```

This retrieved both the user and root flags, completing the machine.

## Evidence

| Step              | Screenshot  |
| ----------------- | ----------- |
| Nmap scan results | `meow1.png` |
| Telnet login      | `meow2.png` |
| Flag retrieval    | `meow3.png` |

## Key Takeaways

- **Default credentials are still an active attack vector.** Any exposed service with factory-default logins should be treated as a critical finding during an assessment.
- **Legacy protocols like Telnet should never be exposed**, especially in regulated environments (e.g., banking/finance), where cleartext credential transmission would violate most security and compliance standards (PCI-DSS, ISO 27001, etc.).
- **Enumeration is the foundation of every engagement.** Even a single-port scan revealed the full attack path here — a reminder that thorough recon often matters more than exploit complexity.
- Building a habit of running quick post-exploitation checks (`whoami`, `id`, `uname -a`) helps establish context fast and is good practice to carry into more advanced boxes.

## Why This Matters for My Career Path

I'm working toward a career in cybersecurity within the banking/financial sector, where legacy system exposure and credential hygiene are recurring findings in real audits and penetration tests. Starting with fundamentals like this — recognizing insecure protocols, testing for default credentials, and documenting findings clearly — builds the same habits and reporting discipline expected in professional security assessments.

---

_This write-up documents a HackTheBox Starting Point lab exercise for educational and portfolio purposes._