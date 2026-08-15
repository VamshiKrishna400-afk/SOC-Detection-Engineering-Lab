# 02 — Hardware & Virtualization Requirements

## 1. Physical Host Requirements

| Resource | Requirement |
|---|---|
| Host Operating System | Windows / Linux |
| CPU | 8+ logical cores recommended |
| RAM | 32 GB minimum / 64 GB preferred |
| Storage | 500 GB+ SSD recommended |
| Network | Ethernet or Wi-Fi |
| Virtualization | Intel VT-x / AMD-V enabled |
| Hypervisor | Oracle VirtualBox |
| Network Mode | Bridged Adapter |

### Usage

The physical host provides the CPU, RAM, storage, and network resources required to run the SOC virtual machines. Hardware-assisted virtualization must be enabled to run multiple VMs efficiently.

---

## 2. Virtual Machine Resource Allocation

### VM 1 — Windows 10

| Resource | Configuration |
|---|---|
| VM Name | Windows 10 x64 |
| Operating System | Windows 10 x64 |
| CPU | 4 cores |
| RAM | 6 GB |
| Storage | 50 GB |
| Network Adapter | Bridged Adapter |
| IP Address | [Lab IP] |
| Purpose | Monitored Endpoint |
| Role | Security Telemetry Source |

### Usage

Windows 10 acts as the primary monitored endpoint. It generates Windows Security, System, Application, and Sysmon telemetry that can be forwarded to Splunk and IBM QRadar for monitoring, detection, and investigation.

---

### VM 2 — Ubuntu

| Resource | Configuration |
|---|---|
| VM Name | Ubuntu-64bit |
| Operating System | Ubuntu Linux 64-bit |
| CPU | 2 cores |
| RAM | 3 GB |
| Storage | 30 GB |
| Network Adapter | Bridged Adapter |
| IP Address | [Lab IP] |
| Purpose | Linux Endpoint |
| Role | Security Telemetry Source |

### Usage

Ubuntu acts as a Linux endpoint within the SOC lab. It generates Linux authentication, system, SSH, and application telemetry that can be forwarded to Splunk and IBM QRadar for monitoring and investigation.

---

### VM 3 — IBM QRadar

| Resource | Configuration |
|---|---|
| VM Name | QRadar |
| Operating System | IBM QRadar |
| CPU | 8 cores |
| RAM | 8 GB |
| Storage | 250 GB |
| Network Adapter | Bridged Adapter |
| IP Address | 192.168.29.55 |
| Purpose | SIEM |
| Role | Security Event Collection, Correlation and Monitoring |

### Usage

IBM QRadar acts as one of the two SIEM platforms in the SOC lab. It receives security events, processes and correlates them, applies detection rules, generates offenses, and supports security investigations.

> **Note:** QRadar resource requirements depend on the specific QRadar version and deployment type. The final CPU, RAM, and storage configuration should be verified against the requirements for the exact QRadar version being used.

---

### VM 4 — Kali Linux

| Resource | Configuration |
|---|---|
| VM Name | Kali Linux |
| Operating System | Kali Linux |
| CPU | 2 cores |
| RAM | 3 GB |
| Storage | 40 GB |
| Network Adapter | Bridged Adapter |
| IP Address | [Lab IP] |
| Purpose | Security Testing |
| Role | Controlled Attack Simulation and Security Assessment |

### Usage

Kali Linux acts as the primary controlled attacker/security-testing workstation. Activities performed within the authorized lab generate security events that can be observed by Windows and Ubuntu and detected by Splunk and QRadar.

---

### VM 5 — Parrot OS + Splunk

| Resource | Configuration |
|---|---|
| VM Name | Parrot OS |
| Operating System | Parrot Security OS |
| CPU | 4 cores |
| RAM | 4 GB |
| Storage | 80 GB |
| Network Adapter | Bridged Adapter |
| IP Address | [Lab IP] |
| Purpose | Splunk SIEM and Security Analysis |
| Role | Splunk Server and Security Analysis Workstation |

### Usage

Parrot OS hosts the Splunk platform used as the second SIEM in the SOC lab.

Splunk is responsible for:

- Security log ingestion
- Event searching
- Detection development
- Alert generation
- Dashboard creation
- Security event analysis
- Incident investigation

Parrot OS can also provide security-testing and analysis capabilities when required.

---

## 3. Virtualization Requirements

| Resource | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Hardware Virtualization | Intel VT-x / AMD-V |
| Network Type | Bridged Adapter |
| Promiscuous Mode | Enabled |
| VM Communication | Local LAN |
| Host Resources | Shared between VMs |

### Usage

VirtualBox provides the virtualization layer for the SOC lab. Each operating system runs as a virtual machine while sharing the physical host's CPU, RAM, storage, and network resources.

The bridged network allows the virtual machines to participate in the same local network as the physical host.

Promiscuous mode is enabled as part of the lab's network-visibility configuration.

---

## 4. VM Resource Summary

| VM | CPU | RAM | Storage | Primary Usage |
|---|---:|---:|---:|---|
| Windows 10 | 4 cores | 6 GB | 50 GB | Monitored Endpoint |
| Ubuntu | 2 cores | 3 GB | 30 GB | Linux Telemetry Source |
| IBM QRadar | 8 cores | 8 GB | 250 GB | SIEM |
| Kali Linux | 2 cores | 3 GB | 40 GB | Security Testing |
| Parrot OS + Splunk | 4 cores | 4 GB | 80 GB | Splunk SIEM |

---

## 5. SOC Lab Usage

The virtual machines work together to create the SOC home-lab environment:

```text
                         Local Network
                              |
                         VirtualBox
                              |
          +-------------------+-------------------+
          |          |         |         |        |
          v          v         v         v        v
     Windows 10   Ubuntu    QRadar    Kali    Parrot OS
     Endpoint     Linux      SIEM     Testing    |
          |          |                           |
          |          |                           v
          |          |                         Splunk
          |          |                           |
          +----------+---------------------------+
                     |
              Security Telemetry
                     |
             +-------+-------+
             |               |
             v               v
           Splunk          QRadar
             |               |
             +-------+-------+
                     |
                Detections
                     |
                   Alerts
                     |
                Investigation
                     |
             MITRE ATT&CK Mapping
                     |
              Incident Response
