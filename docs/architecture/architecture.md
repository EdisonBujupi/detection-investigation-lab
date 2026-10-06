# Detection & Investigation Lab — Architecture

## 1. Purpose

The Detection & Investigation Lab is a reproducible security operations environment designed to study how endpoint activity becomes security evidence.

The system follows the complete operational path:

**Endpoint Activity → Telemetry → Detection → Investigation → Decision → Response → Verification**

The objective is not to demonstrate individual security tools in isolation. The objective is to understand how an analyst uses multiple sources of evidence to determine what actually happened.

---

## 2. Architecture Goals

The environment must allow an analyst to:

1. Observe activity occurring on an endpoint.
2. Collect relevant telemetry.
3. Detect potentially suspicious behavior.
4. Investigate the underlying activity.
5. Correlate evidence from multiple sources.
6. Distinguish security incidents from normal operational problems.
7. Determine impact and confidence.
8. Decide on an appropriate response.
9. Verify whether the response was successful.
10. Document the investigation so another analyst can reproduce the reasoning.

---

## 3. Core Components

### Wazuh Manager

Responsibilities:

* Receive endpoint telemetry.
* Process and decode events.
* Apply detection rules.
* Generate alerts.
* Provide centralized visibility for investigations.

Address:

`192.168.51.131`

---

### Ubuntu Endpoint

Responsibilities:

* Represent a monitored company server.
* Generate authentic operating-system telemetry.
* Execute controlled test activity.
* Provide process, authentication, file and network evidence.

Address:

`192.168.51.135`

Telemetry sources will include:

* Linux system logs
* Authentication events
* Process execution
* Linux Audit
* Network activity
* File activity
* Wazuh Agent telemetry

---

### Kali Linux

Responsibilities:

* Controlled analyst and adversary workstation.
* Generate reproducible activity against the monitored environment.
* Perform reconnaissance and controlled attack simulations.
* Validate whether detections provide useful evidence.

Address:

`192.168.51.145`

Kali activity must remain controlled and attributable to a specific experiment.

---

## 4. Telemetry Pipeline

```text
Ubuntu Endpoint
      │
      ├── System Logs
      ├── Authentication
      ├── Process Activity
      ├── Linux Audit
      ├── Network Activity
      └── File Activity
              │
              ▼
        Wazuh Agent
              │
              ▼
        Wazuh Manager
              │
              ▼
       Detection Rules
              │
              ▼
           Alert
              │
              ▼
        Investigation
              │
              ▼
     Analyst Decision
              │
       ┌──────┴──────┐
       ▼             ▼
    Response       Benign /
       │           Operational
       ▼
   Verification
```

---

## 5. Investigation Model

Every investigation follows the same reasoning cycle:

**Observe → Classify → Hypothesize → Identify Missing Evidence → Investigate → Correlate → Assess Impact → Decide → Respond → Verify → Document**

The system should not encourage conclusions based on a single alert.

An alert is a starting point for investigation, not proof of compromise.

---

## 6. Design Principles

### Evidence before conclusions

The analyst must identify the evidence supporting a conclusion.

### Normal before abnormal

The environment should establish normal endpoint behavior before suspicious behavior is evaluated.

### Detection is not an incident

A detection identifies activity requiring investigation. The investigation determines whether an incident actually occurred.

### Context determines severity

The same technical event may have different significance depending on the user, process, host, destination, timing and business context.

### Reproducibility

Experiments must be repeatable.

### Analyst reasoning is explicit

Investigations should record:

* What was observed.
* What was expected.
* What was suspicious.
* What evidence was missing.
* Which hypotheses were considered.
* Which evidence eliminated hypotheses.
* Why the final decision was made.

### Automation supports analysis

Automation should reduce repetitive work while leaving security decisions explainable and auditable.

---

## 7. Planned Capability Development

### Phase 1 — Endpoint Visibility

Establish reliable telemetry from the Ubuntu endpoint.

Primary objective:

> Determine which user executed which process and when.

---

### Phase 2 — Endpoint Context

Add additional context around:

* Process ownership
* User sessions
* Network connections
* File activity
* Authentication
* Parent/child processes

---

### Phase 3 — Detection Engineering

Develop and validate detections based on observed telemetry.

Each detection should define:

* Detection objective
* Data source
* Trigger condition
* Expected behavior
* False-positive conditions
* Investigation requirements
* Severity rationale

---

### Phase 4 — Controlled Attack Scenarios

Generate reproducible activity from Kali.

Scenarios will cover areas such as:

* Reconnaissance
* Authentication abuse
* Suspicious execution
* Persistence
* File manipulation
* Network communication

---

### Phase 5 — Investigation

Turn alerts into complete investigations.

Each case should contain:

* Initial alert
* Timeline
* Evidence
* Hypotheses
* Correlation
* Impact assessment
* Analyst decision
* Supporting evidence

---

### Phase 6 — Response

Test controlled response procedures:

* Containment
* Eradication
* Recovery
* Verification
* Lessons learned

---

### Phase 7 — Automation

Introduce automation only after the underlying detection and investigation workflow is reliable.

Potential capabilities include:

* Alert enrichment
* Evidence collection
* Case creation
* Analyst notification
* Repetitive investigation tasks

---

## 8. Success Criteria

The laboratory is successful when an analyst can start with an alert and answer:

**What happened?**

**Where did it happen?**

**Who or what initiated it?**

**When did it happen?**

**What evidence supports the conclusion?**

**What else could explain the activity?**

**What is the impact?**

**Is this a security incident?**

**What action should be taken?**

**Did the response work?**
