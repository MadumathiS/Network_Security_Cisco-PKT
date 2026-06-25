# Network-Security

<img width="100" height="100" alt="image" src="https://github.com/user-attachments/assets/34cac26b-e79a-4eb9-92f8-fa8ea67d4865" />

Network Security project designed and simulated using Cisco Packet Tracer, demonstrating a **collapsed core star topology enterprise network architecture**. The design focuses on secure segmentation, centralized routing, and controlled inter-department communication using VLANs, ACLs, and firewall enforcement.

All access-layer switches are connected in a star formation to a central Layer 3 core switch, which handles inter-VLAN routing and network policy enforcement.

---

## 🔐 Key Features


- Collapsed Core Star Topology design
- VLAN-based segmentation and isolation
- Inter-VLAN routing via Layer 3 core switch
- Access Control Lists (ACLs) for traffic control
- Edge firewall for perimeter security
- Centralized server services (DNS, DHCP, HTTP, RADIUS)
- Secure enterprise printing (dedicated VLAN)
- Department-wise isolation and security zoning
- Scalable enterprise network design

---

## 🧱 Network Architecture

### Topology Type
**Collapsed Core Star Topology**

### Layers

- **Core Layer (Collapsed Core Switch - Layer 3)**
  - Inter-VLAN routing
  - Central policy enforcement
  - Network backbone connectivity

- **Access Layer (Star Topology)**
  - Multiple Layer 2 switches
  - Each department connects to core switch

- **Edge Layer**
  - Firewall for external traffic control
  - Security boundary enforcement

- **Server Room (MDF)**
  - Central services and infrastructure systems

---

## 🖥️ Network Clusters

- Server Room (Core Services & Storage)
- Production Cluster
- IT Administration Cluster
- Management Cluster
- Support A Cluster
- Support B Cluster
- Study Cluster

---

## 📡 Technologies Used

- Cisco Packet Tracer
- Cisco Layer 2 Switches
- Cisco Layer 3 Switch (Core)
- VLANs & Subnetting
- ACL (Access Control Lists)
- Firewall Security Rules
- DHCP, DNS, HTTP, RADIUS Services

---

## 🚀 Objectives

- Design and implement a secure enterprise network
- Enforce VLAN-based segmentation
- Prevent unauthorized inter-department access
- Centralize routing using a collapsed core switch
- Simulate real-world enterprise security architecture

---


## 📁 Project Structure
```
Network-Security/

│
├── README.md
├── configs/
│ ├── core-switch-config-layout.txt
│ ├── core-switch-config.txt
│ ├── firewall-config.txt
│ ├── server-configs.txt
│ └── access-switch-configs/
│ ├── support-a-switch.txt
│ ├── support-b-switch.txt
│ ├── production-switch.txt
│ ├── study-switch.txt
│ ├── management-switch.txt
│ └── it-switch.txt
│
├── documentation/
| ├── nvidia.pkt
│ ├── nvidia_stratum_master_report.pdf
│ └── networking_analysis.pdf
│
└── screenshots/
├── topology-overview.png
├── firewall-setup.png
├── server-room.png
└── ping-tests.png

```

---

## 🖼️ Screenshots

> Add Packet Tracer visuals here

- ![Network Topology](screenshots/topology-overview.png)
  
- Vlan Structure
```
========================================================================================================
                                         [ 🌐 ISP ROUTER ]
                                                 │
                                                 ▼ (Outside Zone)
                                      [ 🧱 ASA 5506-X FIREWALL ]
                                          │              │
                (Inside Zone) ────────────┘              └──────────── (Perimeter DMZ Zone)
                       │                                                 │
                       ▼                                                 ▼
            [ 🧠 CORE-MLS MULTILAYER SWITCH ]                     [ 🔒 DMZ ACCESS SWITCH (2960) ]
                       │                                                 │
                       ├──► [ 💾 DNS Server (.10) ]                      ├──► [ 🌐 HTTP Web Server (.2) ]
                       ├──► [ 💾 DHCP Server (.2) ]                      └──► [ 📂 FTP Storage Server (.3) ]
                       ├──► [ 💾 AAA/RADIUS Node (.5) ]
                       └──► [ 💾 iSCSI Storage Target (.6) ]
                       │
   ┌───────────┬───────┴───┬───────────┬───────────┬───────────┐  
   │           │           │           │           │           │   (802.1Q Fiber Trunk Lines)
   ▼           ▼           ▼           ▼           ▼           ▼
[ Switch1 ] [ Switch2 ] [ Switch3 ] [ Switch4 ] [ Switch5 ] [ Switch6 ]
 (PROD)      (SUPP-B)    (STUDY)     (SUPP-A)    (MGMT)       (IT)
 (VLAN 10)   (VLAN 20)   (VLAN 30)   (VLAN 40)   (VLAN 50)    (VLAN 60)
========================================================================================================

```
- ![Firewall configuration](screenshots/firewall-setup.png)
  
- ![Server room setup](screenshots/server-room.png)
  
- ![Connectivity test results](screenshots/ping-tests.png)

---

## ⚙️ How to Run the Project

1. Open Cisco Packet Tracer
2. Load the file:
```
nvidia.pkt
```
3. Wait for devices to initialize
4. Verify:
- VLAN assignments
- DHCP allocation
- Inter-VLAN routing
- Firewall rules
5. Test connectivity using:
- `ping`
- `tracert`

---

## 🔐 Security Design Summary

- VLAN isolation between departments
- Controlled inter-VLAN routing via ACLs
- Firewall filtering for external traffic
- Centralized authentication (RADIUS-ready design)
- Secure server segmentation in dedicated network zone

---

## 📊 Outcome

This project demonstrates a fully functional enterprise network with:

- Scalable collapsed core star topology
- Strong segmentation and isolation
- Secure inter-department communication
- Centralized routing and services
- Real-world enterprise security implementation

---

## 📌 License

Educational / Academic Cisco Packet Tracer Simulation Project
