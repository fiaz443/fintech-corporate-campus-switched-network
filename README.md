# FinTech Corporate Campus & Cloud Gateway Network (Day 22 Lab)

## 📌 Project Overview
This repository hosts a sophisticated Cisco Packet Tracer network simulation replicating a high-performance FinTech corporate campus architecture. This lab moves beyond traditional Router-on-a-Stick setups, implementing advanced Enterprise best practices.

The key objectives were to consolidate Inter-VLAN routing onto a Layer 3 Core Multilayer Switch (SVI) to offload the edge router, implement link aggregation (LACP EtherChannel) between Core and Access tiers for bandwidth redundancy, and establish dynamic OSPF routing between the campus core and a simulated Cloud Gateway. This ensures seamless, high-speed, and resilient connectivity for internal users to simulated public cloud services (8.8.8.8).

---

## 🛠️ Network Architecture & Design Best Practices
The topology is designed with a multi-tier modular approach, focusing on performance, scalability, and security:

* *Core/Distribution Layer (Layer 3 Switching):* A high-performance Cisco Multilayer Switch (Core-SW1) acts as the campus backbone. It performs all Inter-VLAN routing using Switch Virtual Interfaces (SVIs), providing line-rate switching speed between departments.
* *Access Layer (Link Aggregation):* Departmental devices connect via an Access Switch (Acc-SW1), linked to the Core via a logical LACP EtherChannel bundle (channel-group 1). This combines multiple physical Gi0 links into one, doubling bandwidth and preventing logical loop formation while providing link-level redundancy.
* *Edge Routing & Cloud Gateway:* A simulated ISP router (Edge-R1) connects the campus to a public cloud network. This link forms a transit network running OSPF for dynamic route propagation, allowing internal campus prefixes to reach the public cloud server (8.8.8.8).
* *Segmentation:* Departments (Operations, Developers, Management) are strictly isolated into distinct VLAN broadcast domains.

---

## 🗺️ Topology Diagram & Documentation
(Ensure your screenshot filename matches the image name below, or use GitHub's drag-and-drop feature to insert your diagram link here)

![Topology Diagram](topology.png)

---

## ⚙️ Key Configuration Highlights (Day 22 Lab Focus)

### 1. LACP EtherChannel Configuration (Core-SW1 <-> Acc-SW1)
Doubles bandwidth and provides link redundancy using open-standard LACP.

```cisco
! On Core-SW1
interface range GigabitEthernet0/1 - 2
 channel-group 1 mode active
! On Acc-SW1
interface range GigabitEthernet0/1 - 2
 channel-group 1 mode active
[4:53 AM, 8/31/2026] 🙂Fی@Z AحmEd 🇵🇰: ! Enable Layer 3 Routing
ip routing

! Configure SVI for VLAN 10 (Operations)
interface vlan 10
 ip address 10.10.10.1 255.255.255.0
 no shutdown
[4:53 AM, 8/31/2026] 🙂Fی@Z AحmEd 🇵🇰: ! Router configuration
interface GigabitEthernet0/24
 description Link to Edge-R1 (Transit Network)
 ip address 10.10.0.1 255.255.255.252

! OSPF process configuration
router ospf 1
 network 10.10.0.0 0.0.0.3 area 0
 network 10.10.10.0 0.0.0.255 area 0
 network 10.10.20.0 0.0.0.255 area 0
 network 10.10.99.0 0.0.0.255 area 0
