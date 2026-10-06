# Network Topology

## Lab Network

The laboratory uses an isolated VMware network:

`192.168.51.0/24`

## Hosts

| Host            | Role                                | IP Address       |
| --------------- | ----------------------------------- | ---------------- |
| Ubuntu Manager  | Wazuh Manager / Indexer / Dashboard | `192.168.51.131` |
| Ubuntu Endpoint | Monitored company server            | `192.168.51.135` |
| Kali Linux      | Analyst / controlled adversary      | `192.168.51.145` |

## Logical Flow

```text
                    Lab Network
                  192.168.51.0/24
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
 Ubuntu Manager    Ubuntu Endpoint     Kali Linux
 192.168.51.131    192.168.51.135    192.168.51.145
        │                │                │
        │         Wazuh Agent           │
        │                │                │
        └───────────────►│◄──────────────┘
                         │
                  Endpoint Activity
                         │
                         ▼
                    Wazuh Manager
                         │
                         ▼
                     Detection
                         │
                         ▼
                   Investigation
```

## Network Roles

### Ubuntu Manager

Central security monitoring infrastructure.

### Ubuntu Endpoint

Represents a monitored organizational server and is the primary source of endpoint evidence.

### Kali Linux

Used to generate controlled activity and validate detection and investigation capabilities.

## Security Boundary

The environment is intended to remain isolated from production systems.

Attack simulations must only target systems explicitly included in this laboratory.

## Current Connectivity

The three systems have confirmed network connectivity across the laboratory network.

Wazuh Agent communication between the Ubuntu Endpoint and Wazuh Manager is operational.
