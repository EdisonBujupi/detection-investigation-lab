# Detection & Investigation Lab

A reproducible security operations environment for studying how endpoint telemetry becomes evidence, how detections are investigated, and how analysts distinguish security incidents from normal operational activity.

## Core Workflow

```text
Endpoint Activity
       ↓
Telemetry
       ↓
Detection
       ↓
Investigation
       ↓
Evidence Correlation
       ↓
Analyst Decision
       ↓
Response
       ↓
Verification
```

## Environment

| System          | Role                                       | Address          |
| --------------- | ------------------------------------------ | ---------------- |
| Ubuntu Manager  | Wazuh Manager / Indexer / Dashboard        | `192.168.51.131` |
| Ubuntu Endpoint | Monitored server / Wazuh Agent             | `192.168.51.135` |
| Kali Linux      | Controlled analyst / adversary workstation | `192.168.51.145` |

Network:

`192.168.51.0/24`

## Objectives

* Build reliable endpoint telemetry.
* Understand normal endpoint behavior.
* Develop evidence-based detections.
* Investigate alerts rather than treating them as incidents automatically.
* Correlate evidence from multiple sources.
* Distinguish malicious activity from legitimate operational activity.
* Practice incident response and verification.
* Document investigations in a reproducible format.

## Investigation Principle

> An alert is a starting point for investigation, not proof of compromise.

Every investigation should establish:

* What happened?
* Where did it happen?
* Who or what initiated it?
* When did it happen?
* What evidence supports the conclusion?
* What alternative explanations exist?
* What action should be taken?
* Did the response work?

## Project Status

**Phase 1 — Endpoint Visibility**

The first capability being developed is reliable process-execution telemetry on the Ubuntu endpoint.

The initial question is:

> **Which user executed which process, and when?**
