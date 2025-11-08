# PowerShell Attack Simulation & Detection with Sysmon + MITRE ATT&CK

## Project summary
This project simulates PowerShell-based attacker activity in an isolated lab and implements detections and automated responses using Sysmon, Azure Log Analytics, Microsoft Sentinel, and (optionally) Microsoft Defender for Endpoint. Each analytic rule is mapped to MITRE ATT&CK technique IDs and accompanied by a short SOC L1 triage runbook.

## Objectives
- Simulate lab-safe PowerShell adversary techniques (T1059.001).  
- Collect telemetry: Sysmon (process create, process access), Windows Security, PowerShell ScriptBlock logs.  
- Build KQL hunting queries -> Sentinel analytic rules -> incidents.  
- Create a Logic App playbook to isolate host and notify SOC channels.  
- Provide documentation, screenshots, and a short demo video for portfolio/resume.

## Architecture (high level)
- Attacker VM (Linux) — runs simulations -> initiates PowerShell commands on Windows target (RDP/WinRM/SSH for test).  
- Windows target VM — Sysmon + AMA -> Log Analytics Workspace (LAW) -> Microsoft Sentinel.  
- Defender for Endpoint (optional) -> EDR telemetry + isolation API.  
- Sentinel analytic rules trigger Logic App playbooks to isolate and create incidents.

## Repo structure
See folder tree in the repo root. Key files:
- `configs/sysmon.xml` — recommended Sysmon config for PowerShell detection.
- `kql/01-hunting-powershell.kql` — hunting queries.
- `playbooks/isolate-host-logicapp.json` — Logic App template for host isolation.
- `docs/soc_l1_runbook.md` — L1 triage runbook.

## Prerequisites
- Azure subscription with permissions to create resources (Sentinel, LAW, Logic Apps).  
- Windows VM (isolated lab) with admin rights.  
- Linux attacker VM to run safe tests.  
- `git` and (optionally) `gh` CLI if you prefer to create a GitHub repo from CLI.

## Quick start
1. Clone this repo (or create it locally).  
2. Populate `configs/sysmon.xml`.  
3. Install Azure Monitor Agent + deploy Sysmon on the Windows VM.  
4. Enable PowerShell Script Block Logging via Group Policy / Local Policy.  
5. Connect LAW to Sentinel and enable required connectors (Windows Security, Sysmon, Defender for Endpoint if available).  
6. Run a low-noise PowerShell simulation from the attacker VM (see `tests/simulations.md`).  
7. Validate events in Log Analytics: `Sysmon EventID 1` and `EventID 10` and PowerShell ScriptBlock logs (4104).  
8. Import KQL hunting queries and turn the most reliable into Sentinel analytic rules.  
9. Create Logic App playbook and attach to rule for automated response.

## Deliverables
- Sysmon config (`configs/sysmon.xml`)  
- KQL rules and saved Sentinel analytic rule JSON (`kql/alerts/`)  
- Logic App playbook JSON (`playbooks/`)  
- SOC L1 runbook (`docs/soc_l1_runbook.md`)  
- Evidence: screenshots and a short demo video (`assets/`)

## MITRE mapping
- Primary: T1059.001 — PowerShell  
- (Add any additional techniques your detections cover)

## Verification checklist
- [ ] Sysmon events with CommandLine are visible in Log Analytics.  
- [ ] PowerShell ScriptBlock (4104) events are visible.  
- [ ] Hunting query returns simulated events.  
- [ ] Sentinel analytic rule fires and optionally triggers playbook.

## Notes & safety
Run all offensive/simulation steps only in this isolated lab (no production systems). Follow the `tests/simulations.md` for safe commands.

## Contact / Author
Your Name — short sentence about you and a link to resume / LinkedIn / portfolio.

