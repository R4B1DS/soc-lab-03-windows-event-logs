# SOC Lab 03: Windows Event Log Analysis

A hands-on Blue Team lab simulating the work of a Tier 1 SOC analyst: investigating suspicious activity on a Windows host through Windows Event Logs.

![Focus](https://img.shields.io/badge/Focus-SOC_%7C_Blue_Team-blue?style=flat-square)
![Level](https://img.shields.io/badge/Level-Junior-green?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=flat-square&logo=windows&logoColor=white)

## Scenario

The SOC received an alert about suspicious activity on a Windows machine. As the analyst on duty, the goal is to review the Windows Event Logs, identify what happened, determine whether the host was compromised, and recommend the next steps.

## Objectives

- Identify relevant security events in the Windows Event Logs
- Detect suspicious authentication activity
- Build an event timeline
- Map the activity to MITRE ATT&CK
- Recommend containment and hardening actions
- Document the incident in a technical report

## Skills Demonstrated

- Windows Event Log analysis (Security, System, Application)
- Event ID interpretation
- Authentication and account activity investigation
- Indicator of Compromise (IOC) extraction
- Attack timeline reconstruction
- MITRE ATT&CK mapping
- Incident response and technical reporting

## Tools

- Windows Event Viewer
- PowerShell (`Get-WinEvent`)
- MITRE ATT&CK framework

## Key Event IDs

| Event ID | Log | Meaning | Why it matters |
|---|---|---|---|
| 4624 | Security | Successful logon | Confirms access; check logon type and source |
| 4625 | Security | Failed logon | Multiple failures may indicate brute force |
| 4672 | Security | Special privileges assigned | Privileged session started |
| 4688 | Security | New process created | Tracks executed programs |
| 4720 | Security | User account created | Possible persistence |
| 4732 | Security | Member added to a local group | Possible privilege escalation |
| 1102 | Security | Audit log cleared | Possible attempt to hide activity |
| 7045 | System | New service installed | Possible persistence |

> Keep only the Event IDs that appeared in your investigation.

## Methodology

1. **Triage:** confirm the alert and define the time window
2. **Collection:** export or filter the relevant logs
3. **Analysis:** filter by Event ID and look for anomalies
4. **Correlation:** connect related events into a timeline
5. **Classification:** map to MITRE ATT&CK and assess severity
6. **Response:** define containment and prevention actions
7. **Documentation:** write the report

### Example: filtering failed logons with PowerShell

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} |
  Select-Object TimeCreated, Message |
  Format-List
```

### Example: counting events by ID

```powershell
Get-WinEvent -LogName Security |
  Group-Object Id |
  Sort-Object Count -Descending |
  Select-Object -First 10 Count, Name
```

## Key Findings

> Replace the `<...>` values with the results of your investigation.

| Item | Result |
|---|---|
| Host analyzed | `<hostname>` |
| Time window | `<start>` to `<end>` |
| Suspicious events found | `<summary>` |
| Accounts involved | `<usernames>` |
| Source IP(s) / workstation | `<IP or name>` |
| Successful compromise? | `<Yes / No>` |
| Severity | `<Low / Medium / High>` |
| Verdict | `<True positive / False positive>` |

## Timeline

| Time | Event ID | Description |
|---|---|---|
| `<time>` | `<ID>` | `<what happened>` |
| `<time>` | `<ID>` | `<what happened>` |
| `<time>` | `<ID>` | `<what happened>` |

## Indicators of Compromise (IOCs)

| Type | Value |
|---|---|
| Account | `<username>` |
| IP address / host | `<value>` |
| Process / file | `<name or path>` |
| Event pattern | `<e.g., repeated 4625 followed by 4624>` |

## MITRE ATT&CK Mapping

> Keep only the techniques that match what you found.

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Brute Force | [T1110](https://attack.mitre.org/techniques/T1110/) |
| Initial Access | Valid Accounts | [T1078](https://attack.mitre.org/techniques/T1078/) |
| Persistence | Create Account | [T1136](https://attack.mitre.org/techniques/T1136/) |
| Persistence | Create or Modify System Process: Windows Service | [T1543.003](https://attack.mitre.org/techniques/T1543/003/) |
| Defense Evasion | Indicator Removal: Clear Windows Event Logs | [T1070.001](https://attack.mitre.org/techniques/T1070/001/) |

## Incident Response

**Containment**
- Isolate the affected host from the network
- Disable or reset compromised accounts

**Eradication and Recovery**
- Remove unauthorized accounts, services, or scheduled tasks
- Confirm no persistence mechanisms remain
- Restore the system from a known good state if needed

**Prevention**
- Enforce strong passwords and account lockout policies
- Enable MFA for remote access
- Turn on advanced audit policies (process creation, logon events)
- Forward logs to a central SIEM
- Create alerts for repeated 4625 events and for Event ID 1102

## Project Structure

```text
soc-lab-03-windows-event-logs/
├── README.md
├── docs/
│   ├── investigation.md
│   └── incident-report.md
└── evidence/
    └── (screenshots and log samples)
```

## Interview Questions This Lab Prepares For

- What is the difference between Event ID 4624 and 4625?
- What does Event ID 1102 indicate, and why is it a red flag?
- How would you detect a brute force attack in Windows logs?
- What are logon types, and which ones are most suspicious?

## Author

**Nicolas Borges Ocampos**
Cybersecurity student | Aspiring SOC / Blue Team analyst
[LinkedIn](https://www.linkedin.com/in/nicolas-borges-ocampos/)


## AI Assistance

> AI tools were used only to help create and organize this README. The lab, practical work, commands, analysis, and conclusions are my own work.
