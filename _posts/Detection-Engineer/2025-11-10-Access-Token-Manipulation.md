---
title: " Access Token Manipulation "
classes: wide
header:
  teaser: /assets/images/digital-forensics/Access-Token-Manipulation-pic/.png
ribbon:
description: " This technique is highly dangerous because it's acts as another user without knowing the password and that's enables stealthy lateral movement or Uses legitimate Windows APIs and authentication flows and that's bypasses many endpoint detections or Often does not create clear security events	Low noise with normal traffic."
categories:
  - Detection-Engineer
toc: true
---


# Overview

Access Token Manipulation (MITRE ATT&CK: T1134) used by threat actors to impersonate other users and move laterally inside Windows Active Directory environments. Instead of stealing plaintext passwords, attackers abuse Windows' built-in authentication mechanisms to reuse cached credentials, Kerberos tickets, NTLM hashes, or new logon sessions.

This technique is highly dangerous because it's acts as another user without knowing the password and that's enables stealthy lateral movement or Uses legitimate Windows APIs and authentication flows and that's bypasses many endpoint detections or Often does not create clear security events	Low noise with normal traffic.

Access Token Manipulation isn’t a vulnerability, it's abusing how Windows is designed.

# Key Background Concepts

**Access Token** Represents who you are and what permissions you have and based on this Controls which actions/processes you can perform

**Logon Session Authentication** context created when a user logs in, an attacker can abuse it to Hold cached credentials like hashes or 
Kerberos tickets


# Attacker Goal

After initial compromise (phishing,..), attackers run code under a low-privileged token. Their goal becomes:

Replace or manipulate their token so Windows thinks they are a more privileged user.

That enables:

- Lateral movement (to servers, domain controllers, internal services)

- Privilege escalation without cracking passwords

# Three General Manipulation Paths (Attacker Options)

- Steal an existing token that's allow to	Impersonate a logged-in privileged.
- Create a new logon session using credentials found elsewhere
- Modify cached credentials inside LSASS memory.

![error](/assets/images/Detection-Engineer/Access-Token-Manipulation-pic/attacker-goals-1.png)

# Variants of Access Token Manipulation (T1134 Sub-Techniques)

## New Credentials (Using LogonUser / runas /NETONLY)

NETONLY (also called NewCredentials) is a Windows authentication behavior used when a process is started with different credentials for network authentication only, while locally the process continues running under the original user’s token.
Does not validate the credentials unless a remote request occurs

Typical abuse:

``runas /netonly /user:domain\admin cmd.exe``

**Logs to look for:**   
- Event ID 4648 (logon with explicit credentials)
- Event ID 4624 with LogonType = 9

**Suspicious when:**   
- runas /netonly used from non-admin workstation  
- Low-privilege user spawning powershell.exe or cmd.exe with NETONLY credentials
- NETONLY creation followed by:
    - SMB connection
    - RDP authentication
    - File share access

***NETONLY + Network activity = HIGH-RISK***

## Pass-The-Ticket (PTT – Kerberos abuse)

Kerberos is a network authentication protocol used in Active Directory environments. Its main purpose is to let users and services prove their identity over the network without sending passwords.

Think of Kerberos as a secure ticketing system, similar to checking in at an airport:

You prove who you are once  
You receive tickets  
You use the tickets to access resources 

**Authentication Flow**

1. User logs in   
2. User enters username + password  
3. Authentication Service (AS) verifies password   
4. If successful → user receives TGT (Ticket Granting Ticket)
5. User needs to access a service ( file share, SQL server)
6. TGT is presented to Ticket Granting Service (TGS)
7. KDC issues a Service Ticket (ST) specific to the requested service
8. User sends the Service Ticket to the service
9. Service decrypts the ticket using its own service account key
10. User is granted access (based on permissions)

![error](/assets/images/Detection-Engineer/Access-Token-Manipulation-pic/tgt.png)

If an attacker steals tickets from memory (LSASS), they can impersonate you without password.

**password never travels over the network.**   
**Kerberos uses encryption + tickets instead.**  

## Pass-The-Hash (PtH – NTLM authentication hijack)

Pass-The-Hash(PTH) lets attackers authenticate as a user by reusing that user’s NTLM hash (stolen from memory or disk), enabling lateral movement without knowing the password, detection focuses on NTLM network logons and LSASS access.

### How it works

- Compromise a host (phishing, exploit, or existing foothold).

- Dump credentials from memory (commonly from lsass.exe) to obtain NTLM password hashes. Tools like **Mimikatz** historically do this.

- Use the hash to authenticate to other Windows hosts/services that accept NTLM (SMB, WMI, RPC, WinRM, etc.), typically via tools or native APIs that accept hashes instead of passwords.

- Gain access on the target host under the victim user’s privileges and continue lateral movement.

***attacker never needs the cleartext password once they have the hash.***

## Overpass-The-Hash (OPtH – hybrid between Kerberos and NTLM)

OPtH is when an attacker takes a stolen NTLM password hash (not the actual password) and uses it to trick the domain authentication system into giving them a Kerberos ticket. That ticket works like a travel pass — once they have it, they can access many services across the network as that user, without ever knowing the real password.

## How it differs from PtH and PTT (short)

**PtH (Pass-The-Hash):** attacker reuses an NTLM hash directly to authenticate via NTLM protocols (SMB, WMI,..).

**PTT (Pass-The-Ticket):** attacker injects a stolen Kerberos ticket (TGT or service ticket) into a session.

**OPtH:** takes an NTLM hash and uses it to obtain a new Kerberos TGT (so the attacker converts a hash → TGT), then uses Kerberos for lateral movement.

### workflow

- Attacker obtains an NTLM hash (from LSASS memory,).
- Then initiates an Authentication Service Request to the KDC (Domain Controller) but uses the hash in place of the password for the required proof.
- If the KDC accepts the authentication proof built from the hash, it issues a TGT.
- The attacker now holds a TGT for that account and can request service tickets (TGS) to access other services or hosts.

Think of it as:

Pass-the-Hash → Pass-the-Ticket      
This grants full Single Sign-On capabilities across the domain.

# Detection Strategy (Defensive View)

Abnormal logon types (LogonType = 9 / New Credentials)	Strong token manipulation indicator
LSASS access with PROCESS_VM_READ or WRITE	Required for PtH / OPtH
Multiple TGTs for same user / machine	Pass-The-Ticket behavior
Unusual parent-child process creation (e.g., runas → powershell.exe)	Abuse of CreateProcessWithTokenW

High-value logs to monitor:
4624 / 4648 / 5140 / 4672
Sysmon Event IDs: 1, 10, 13


# Detection Playbook

 **Primary detection signals**

- Unexpected TGT issuance (Event 4768) for an account without a corresponding interactive logon.
- Rapid issuance of service tickets (Event 4769) shortly after a TGT for the same account.
- LSASS memory access or credential-dumping indicators on the host.
- Kerberos requests using weak crypto types or unusual patterns.
- New or unusual service access from accounts that typically don’t access those services.

 **Immediate triage**

- Record alert context: hosts, time window, user accounts, DCs involved, process names, any EDR alerts.
- Confirm Kerberos events: pull DC logs for 4768/4769 around the timestamp. Note source workstation and requester IP.
- Check EDR for LSASS access on the suspected origin host (memory read/write, injected processes, suspicious tools).
- Map process tree on the origin host (parent -> child processes) and capture command-lines.
- Do not immediately change anything that would destroy volatile evidence (collect first).

 **Containment**

- Isolate the origin host (network or block lateral protocols) while preserving EDR/forensic access.
- Block suspicious account sessions: block or disable accounts only after confirming ownership and coordinating with business owners.
- Block lateral protocols (SMB, RPC, WMI, WinRM) from the host at network level to stop further misuse.
- Apply temporary conditional access for the impacted account(s) (force re-auth/MFA) if possible.

 **Investigation & evidence collection**

- Collect logs: DC Security logs (4768/4769), origin host Security/System/Application logs, Sysmon, EDR telemetry, and network flows.
- Capture volatile data: EDR memory snapshot of suspicious processes (where permitted).
- Timeline: initial compromise → LSASS activity → (TGT request) → TGS requests → lateral connections.
- Search environment: hunt for same user TGT issuance from other hosts, and similar TGT request anomalies.
- Preserve chain of custody; record who accessed which evidence.

 **Eradication (next steps once evidence collected)**

- Reimage compromised hosts if LSASS was accessed or credential dumping is confirmed.
- Reset credentials for compromised accounts.
- Invalidate tickets / sessions: purge Kerberos tickets ( user ticket cache) and force re-authentication where possible.
- Rotate sensitive keys (KRBTGT rotation only as part of major compromises and with AD/Directory SME planning).

 **Recovery & hardening (post-incident)**

- return hosts only after reimage, patching, and validation.
- Restrict NTLM and disable legacy crypto (disable RC4, enforce strong Kerberos crypto).
- Audit and reduce the number of privileged accounts and their use on endpoints.
- Review service account practices, unique passwords, constrained delegation, and no shared local admin hashes.

 **Detection tuning & monitoring**

- Alert on 4768 where Account requested a TGT from a host that is not the user’s workstation or without interactive logon.
- Alert on unusual 4769 immediately after a suspicious 4768 for high-value accounts.
- Monitor EDR for LSASS access events and correlate to Kerberos events.
- Baseline normal Kerberos behavior per account to reduce false positives.

# Sigma Rules

## Sigma Rule — NETONLY / NewCredentials (runas /netonly)
```yaml
title: Access Token Manipulation - NewCredentials / NETONLY (RunAs)
id: a1c7128e-55fa-4c0e-8f6c-e1a4f54b01e8
status: experimental
description: Detects usage of NETONLY / NewCredentials via runas.exe where a process is started with alternate credentials for network authentication only.
author: Asem Ashraf
date: 2025-11-09
references:
  - https://attack.mitre.org/techniques/T1134/
tags:
  - attack.t1134
  - attack.credential_access
  - attack.lateral_movement
  - windows

logsource:
  product: windows
  service: security

detection:
  selection_newcredentials:
    EventID:
      - 4624
      - 4648
    LogonType: 9

  selection_runas_netonly:
    EventID: 4688
    CommandLine|contains:
      - "/netonly"
      - "runas /netonly"

  condition: selection_newcredentials or selection_runas_netonly

falsepositives:
  - Legitimate administrative usage of runas /netonly
level: high
```

## Sigma Rule — Suspicious Kerberos TGT / Service Ticket Activity (Pass-the-Ticket)
```yaml
title: Access Token Manipulation - Kerberos TGT or Service Ticket Abuse (PTT)
id: 1bc7e7fd-7c92-4bd7-8e59-5e02bc6cdf1f
status: experimental
description: Detects suspicious Kerberos authentication activity that may indicate Pass-the-Ticket or forged ticket attacks.
author: Asem Ashraf
date: 2025-11-09
references:
  - https://attack.mitre.org/techniques/T1550/
tags:
  - attack.t1550
  - attack.lateral_movement
  - attack.kerberos
  - windows

logsource:
  product: windows
  service: security

detection:
  selection_kerberos_tgt:
    EventID: 4768
    TicketOptions|contains: "0x40"

  selection_kerberos_service:
    EventID: 4769
    ServiceName|exists: true

  condition: selection_kerberos_tgt or selection_kerberos_service

falsepositives:
  - Kerberos renewals and scheduled job authentication
level: medium
```

## Sigma Rule — Credential Dumping Tool Execution (Rubeus / Mimikatz)
```yaml
title: Access Token Manipulation - Credential Tooling Execution (Rubeus / Mimikatz)
id: 7b517daa-9094-45e4-82f5-f7e3d65d2962
status: experimental
description: Detects execution of tools commonly used for ticket extraction or manipulation such as Rubeus and Mimikatz.
author: Asem Ashraf
date: 2025-11-09
tags:
  - attack.t1003.001
  - attack.credential_access
  - windows

logsource:
  product: windows
  category: process_creation

detection:
  selection_tooling_execution:
    EventID:
      - 4688
      - 1      # Sysmon
    CommandLine|contains:
      - "rubeus"
      - "mimikatz"
      - "sekurlsa"
      - "asktgt"

  condition: selection_tooling_execution

falsepositives:
  - Security testing or Red Team activity
level: high
```

## Sigma Rule — LSASS Memory Access (Tokens / Ticket Extraction)

```yaml
title: Access Token Manipulation - LSASS Access (Suspicious Memory Read)
id: d1f57ab3-1453-470e-9dd2-b3fb395a63a7
status: experimental
description: Detects suspicious process accessing LSASS memory, often associated with token manipulation or credential dumping.
author: Asem Ashraf
date: 2025-11-09
tags:
  - attack.t1003.001
  - attack.credential_access
  - windows

logsource:
  product: windows
  service: sysmon

detection:
  selection_lsass_access:
    EventID: 10
    TargetImage|endswith: "\\lsass.exe"
    GrantedAccess|contains:
      - "0x1fffff"

  condition: selection_lsass_access

falsepositives:
  - Security products legitimately accessing LSASS
level: high
```
