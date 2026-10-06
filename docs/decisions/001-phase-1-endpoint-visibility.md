# Decision 001 — Phase 1 Endpoint Visibility

**Status:** Baseline assessment complete
**Phase:** 1 — Endpoint Visibility
**Date:** 2026-10-06

---

## 1. Objective

The first capability required by the Detection & Investigation Lab is reliable endpoint process-execution telemetry.

The analyst must eventually be able to answer:

> **Which user executed which process, when did it happen, and what evidence supports that observation?**

This capability will become the foundation for later detection and investigation workflows.

---

## 2. Endpoint Under Assessment

| Property        | Value                     |
| --------------- | ------------------------- |
| Role            | Monitored Ubuntu endpoint |
| Hostname        | `wazuh-agent`             |
| IP address      | `192.168.51.135`          |
| Telemetry agent | Wazuh Agent               |
| Audit subsystem | Linux auditd              |

---

## 3. Initial Assessment

Before modifying the endpoint, the existing audit configuration was inspected.

### Audit service

Command:

```bash
sudo systemctl status auditd --no-pager
```

Result:

```text
auditd.service — active (running)
```

### Audit control utility

Command:

```bash
which auditctl
```

Result:

```text
/usr/sbin/auditctl
```

### Kernel audit status

Command:

```bash
sudo auditctl -s
```

Relevant observations:

```text
enabled 1
lost 0
backlog 0
```

Interpretation:

* Linux auditing is enabled.
* The audit daemon is running.
* No audit events have been reported as lost.
* There is no current audit backlog.

---

## 4. Existing Audit Rules

Command:

```bash
sudo auditctl -l
```

Result:

```text
No rules
```

The persistent configuration was then inspected.

File:

```text
/etc/audit/rules.d/audit.rules
```

Current contents contain only global audit configuration:

```text
-D
-b 8192
--backlog_wait_time 60000
-f 1
```

No event-specific rules are currently defined.

---

## 5. Finding

The endpoint has a functioning audit subsystem but does not currently have rules that instruct auditd to record specific security-relevant events.

This creates an important distinction:

> **Audit infrastructure is operational, but useful audit visibility has not yet been configured.**

The current configuration therefore cannot provide the required process-execution evidence through auditd.

---

## 6. Visibility Gap

The laboratory currently cannot reliably use auditd to answer:

```text
Who executed a process?
What executable was launched?
When was it launched?
What audit event corresponds to that execution?
```

Without this telemetry, later detection and investigation would have to rely on less direct evidence.

---

## 7. Why This Matters

A security monitoring system is only as useful as the evidence available to the analyst.

A Wazuh alert may identify suspicious activity, but an investigation requires additional context.

For example:

```text
Alert
  ↓
What process?
  ↓
Who executed it?
  ↓
When?
  ↓
What command?
  ↓
What happened immediately before/after?
```

The endpoint must therefore produce reliable underlying evidence before detection rules are developed.

---

## 8. Design Requirement

Phase 1 will begin with the smallest useful telemetry capability rather than enabling broad audit collection.

The initial requirement is:

```text
User identity
      +
Process execution
      +
Timestamp
```

Additional telemetry will be introduced only when it provides a defined investigative capability.

---

## 9. Next Decision

The next implementation step is to add a narrowly scoped Linux audit rule for process execution.

The implementation will then be validated locally on the endpoint before the resulting events are integrated into the Wazuh investigation pipeline.

The validation sequence will be:

```text
Audit rule
    ↓
Controlled process execution
    ↓
Raw audit event
    ↓
Verify identity / executable / timestamp
    ↓
Wazuh ingestion
    ↓
Detection and investigation
```

---

## 10. Current State

**Baseline assessment: COMPLETE**

**Audit rule implementation: NOT YET PERFORMED**

**Wazuh integration of new telemetry: NOT YET PERFORMED**

The endpoint will not be modified further until the implementation is explicitly defined and documented.
