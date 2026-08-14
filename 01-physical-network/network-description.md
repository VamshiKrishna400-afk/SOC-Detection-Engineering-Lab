# 01 — Physical Network

## Network Overview

The following diagram shows the high-level architecture of the SOC home lab.

![Network Overview](./network-overview.png)

## Detailed Home Lab Network Architecture

The following diagram shows the detailed physical network, virtualization layer, bridged networking configuration, and SOC virtual machines.

![Detailed Home Lab Network Architecture](./home-lab-network-diagram.png)

# 01 — Physical Network

## Overview

This section documents the physical network foundation of my
SOC home lab.

The lab is hosted on a physical Windows 11 computer that is
connected to the home network through Wi-Fi.

VMware Workstation is installed on the Windows 11 physical
host and is used to run the virtual machines that make up the
SOC laboratory.

The virtual machines use a bridged network adapter, allowing
them to connect through the physical host's network connection
and communicate with the local network according to their
network configuration.

---

## Network Architecture

The overall network path is:

Internet
↓
Home Wi-Fi Router
↓
Local Wi-Fi Network
↓
Windows 11 Physical Host
↓
VMware Workstation
↓
Bridged Network Adapter
↓
SOC Virtual Machines

### Network Overview

![Network Overview](network-overview.png)

### Detailed Home Lab Network Diagram

![Home Lab Network Architecture](home-lab-network-diagram.png)

---

## 1. Home Network Concept

The SOC home lab uses my existing home Wi-Fi network as the
physical network foundation.

The physical Windows 11 computer connects to the home router
using Wi-Fi.

VMware Workstation runs on this physical computer and hosts
the virtual machines used in the SOC laboratory.

The virtual machines are configured to use bridged networking,
which allows their virtual network interfaces to connect
through the physical host's network connection.

The physical network therefore provides the connectivity
foundation for the virtual SOC environment.

---

## 2. Internet

The Internet provides external network connectivity to the
home network.

The Internet connection is provided through the home Wi-Fi
router.

The Internet is shown in the architecture diagram to
represent the external network from which the home network
receives connectivity.

No personal Internet information or public IP address is
included in this repository.

---

## 3. Home Wi-Fi Router

The home Wi-Fi router provides the local network used by the
physical host.

It provides wireless connectivity between the physical
Windows 11 computer and the local network.

The router also provides the connection between the local
network and the Internet.

For security and privacy reasons, sensitive router
information is not included in this repository.

This includes:

- Wi-Fi passwords
- Router administrator credentials
- Public IP addresses
- Sensitive network configuration
- Other personally identifiable network information

---

## 4. Local Area Network (LAN)

The home Wi-Fi network acts as the local area network (LAN)
for this laboratory environment.

The Windows 11 physical host connects to this LAN using its
Wi-Fi network adapter.

The virtual machines use VMware's bridged network
configuration to connect through the physical host's network
connection.

The local network therefore provides the communication
environment used by the physical host and the virtual
machines.

---

## 5. Physical Host — Windows 11

The physical host is a Windows 11 computer.

This computer provides the hardware resources required to
operate the SOC home lab.

The physical host is connected to the home network through
Wi-Fi.

VMware Workstation is installed on the Windows 11 host and
provides the virtualization platform for the laboratory.

The Windows 11 operating system is therefore the host
operating system, while the other operating systems in the
lab run as virtual machines.

---

## 6. VMware Workstation

VMware Workstation provides the virtualization layer of the
SOC home lab.

It allows multiple operating systems to run independently as
virtual machines on the Windows 11 physical host.

The laboratory contains the following virtual machines:

| Virtual Machine | Primary Role |
|---|---|
| Windows 10 | Endpoint |
| Parrot OS | Splunk SIEM |
| Ubuntu | Linux System |
| IBM QRadar | SIEM |
| Kali Linux | Security Testing |

Each virtual machine has its own virtual hardware and
network interface.

The individual virtual machines and their configurations are
documented in later sections of this project.

---

## 7. Bridged Networking

The virtual machines use a bridged network adapter in VMware
Workstation.

In this configuration, the virtual machine's network
interface is connected through the physical host's network
connection.

In this laboratory, the physical host uses Wi-Fi.

The simplified network path is:

Physical Windows 11 Host
↓
Physical Wi-Fi Network Adapter
↓
VMware Workstation
↓
Bridged Virtual Network Adapter
↓
Virtual Machine

Bridged networking was selected to allow the virtual
machines to participate in the same network environment as
the physical host, subject to the configuration of the
network and the individual virtual machines.

---

## 8. Why Bridged Networking Was Selected

Bridged networking was selected because the SOC laboratory
requires realistic network communication between the
virtual machines and the surrounding network environment.

A bridged configuration allows the virtual machines to use
the physical host's network connection rather than relying
only on an isolated NAT-based virtual network.

This is useful for a SOC laboratory because the systems can
communicate using normal IP networking and can generate
network and security activity that can be used for monitoring,
logging, detection, and investigation.

The choice of bridged networking also makes the network
architecture easier to understand because the virtual
machines are connected through the same physical network
path used by the Windows 11 host.

---

## 9. Promiscuous Mode

Promiscuous mode is enabled as part of the VMware network
configuration for the laboratory.

Promiscuous mode affects which network frames a virtual
network interface is allowed to receive.

It is important to note that enabling promiscuous mode does
not automatically provide visibility into every packet on
the physical Wi-Fi network.

Actual network visibility depends on factors such as:

- Physical network topology
- Wi-Fi adapter capabilities
- VMware network configuration
- Virtual network configuration
- How traffic is delivered to the virtual interface
- The specific monitoring configuration

Therefore, promiscuous mode is documented as part of the
network configuration rather than being described as a
guarantee of complete network traffic visibility.

---

## 10. How the Virtual Machines Communicate

The virtual machines communicate through their configured
virtual network interfaces.

The general communication path is:

Internet
↓
Home Wi-Fi Router
↓
Local Wi-Fi Network
↓
Windows 11 Physical Host
↓
VMware Workstation
↓
Bridged Network Adapter
↓
SOC Virtual Machines

The virtual machines can communicate with one another
according to their IP addresses, firewall configurations,
virtual network settings, and other network controls.

For example, the Windows 10 endpoint can communicate with
other systems in the laboratory when the required network
connectivity is configured.

The SIEM systems can communicate with configured log sources
when the required network and logging configurations are in
place.

---

## 11. SOC Virtual Machines

The physical host runs several virtual machines that form the
SOC laboratory.

### Windows 10

Windows 10 is used as an endpoint system in the laboratory.

It can generate Windows security and system activity that
can be used as telemetry for security monitoring and
investigation.

### Parrot OS

Parrot OS is used as the host operating system for Splunk in
this laboratory.

Splunk provides the SIEM and security monitoring capabilities
used in the project.

### Ubuntu

Ubuntu is used as a Linux system within the laboratory.

It can be used as a Linux system and, where configured, as a
source of system and security logs.

### IBM QRadar

IBM QRadar is deployed as a separate virtual machine.

QRadar provides the second SIEM platform used in the
laboratory for event collection, monitoring, detection, and
investigation.

### Kali Linux

Kali Linux is used as the security testing system.

It is used to generate controlled security-testing activity
against systems that are intentionally included in the
laboratory.

All testing is performed only against authorized laboratory
systems.

---

## 12. Network Design Summary

The physical and virtual network can be summarized as:

```text
Internet
   │
   ▼
Home Wi-Fi Router
   │
   │ Wi-Fi
   ▼
Windows 11 Physical Host
   │
   ▼
VMware Workstation
   │
   ▼
Bridged Network Adapter
   │
   ├── Windows 10
   │
   ├── Parrot OS
   │      └── Splunk
   │
   ├── Ubuntu
   │
   ├── IBM QRadar
   │
   └── Kali Linux