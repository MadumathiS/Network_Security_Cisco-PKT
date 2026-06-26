# **NVIDIA STRATUM MASTER NETWORK MODERNIZATION & STAGING RECORD**

**Project ID:** NVIDIA-REG-RD-2026-1106

**Target Environment:** NVIDIA Regional Hub — Brussels, Belgium

**Prepared By:** Madumathi Singaraju (Lead Infrastructure Architect & Technical Project Manager)

**Partner Organization:** BeCode Hub

**Execution Window:** Strict 14 Calendar Days

**Approved Budget Ceiling:** €300,000.00 EUR

**Date:** June 2026

TABLE OF CONTENTS

1. IT Company Relocation Site Survey Plan & Physical Layout  
2. Service Agreement for IT Relocation and Infrastructure Upgrade  
3. Quotation for IT Infrastructure Upgrade and Relocation Services  
4. Executive Project Topology Documentation & Justification  
5. Verified Live running-config & CLI Staging Guide  
6. Incident Breakdown, Root Cause Analysis (RCA) & Verification Matrix

1\. IT COMPANY RELOCATION SITE SURVEY PLAN & PHYSICAL LAYOUT

### **1.1 Project Goal**

To ensure the new facility’s physical and logical infrastructure completely supports the operational demands of the NVIDIA Regional Hub prior to asset relocation, successfully eliminating deployment downtime and guaranteeing a deterministic transition baseline.

### **1.2 Operational Survey Scope & Physical Space Audit**

A comprehensive on-site facility audit was conducted to map structural layout constraints and copper/fiber medium run pathways:

* Main Wiring Closet: Serves as the centralized star distribution hub. Houses the core multi-layer switch stack, edge firewall, primary patch panels, and centralized network-attached resources.  
* IT Office (VLAN 60): Provisioned with horizontal structural runs back to the core wiring closet for administrative network orchestration, monitoring, and live fabric management.  
* Production Room (VLAN 10): Outfitted with high-density Cat6a drops designed to handle high-bandwidth R\&D traffic and heavy distributed computational workloads.  
* Support A (VLAN 40\) & Support B (VLAN 20): Dedicated endpoint office areas containing room switches. Disconnected drop links (PC6–PC9) and local shared printers have been completely re-terminated to re-establish wire-line connectivity.  
* Study Room (VLAN 30\) & Management Office (VLAN 50): Operational environments isolated into distinct logical domains to split the facility's broadcast layout, mitigate frame collisions, and restrict broad lateral visibility.

2\. SERVICE AGREEMENT CONTRACT FOR IT RELOCATION AND INFRASTRUCTURE UPGRADE

This binding contract governs the engineering terms, structural parameters, and support mandates between the Service Provider (Stratum, represented by Madumathi Singaraju) and the client steering panels (NVIDIA).

* Execution Track: All physical medium laydown, logical addressing structure alignment, device hardening, and verification staging are executed within a compressed 14-calendar-day runway.  
* Sourcing Logistics: To ensure component legitimacy and compliance, all structural components, core nodes, and computing fleets are natively procured from enterprise wholesale providers via Dustin Belgium.  
* Maintenance SLA: A 5-Year Continuous Maintenance and Technical SLA Package is natively bundled into the baseline cost, guaranteeing remote configuration audits, system status reporting, off-site automated backups, and prioritized ticket response for 60 consecutive months.  
* Governing Law: Jurisdiction rests strictly under the regional laws of Belgium (Brussels Region).

3\. QUOTATION FOR IT INFRASTRUCTURE UPGRADE AND RELOCATION SERVICES

3.1 Itemized Capital Expenditure (CapEx)

Sourced via Dustin Belgium wholesale channels.

* Dell Computing Fleet (Precision 3660/3460 & OptiPlex lines)  
  Quantity: 48 | Total Cost: €104,950.00  
* Core & Edge Networking Gear (1x Edge Firewall, 1x Cisco L3 MLS, 7x Cisco L2 Switches, Racks, Smart UPS)  
  Quantity: 10 | Total Cost: €36,400.00  
* Enterprise Software Suite (5x Windows Server Std, 3-Year Unified UTM Security Suite, PRTG Licenses)  
  Quantity: 1 | Total Cost: €14,040.00  
* Structured Cabling Media (66x Cat6a Copper Straight-Through Runs, Patch Panels, Management Trays)  
  Quantity: 1 | Total Cost: €15,006.00  
* Hardened Peripheral Fleet (Network-Connected Enterprise Multi-Function Printers)  
  Quantity: 4 | Total Cost: €18,000.00

TOTAL EQUIPMENT COST: €188,396.00

3.2 Specialized Engineering Labor Breakdown

* IT Manager: 80 Hours | Cost: €12,800.00  
* Network Administrator: 120 Hours | Cost: €15,600.00  
* Lead Network Technician: 160 Hours | Cost: €16,000.00  
* Cyber Security Manager: 45 Hours | Cost: €7,654.00  
* Lead Security Technician: 50 Hours | Cost: €5,250.00  
* Lead RADIUS/AAA Configuration Specialist: 40 Hours | Cost: €6,000.00

TOTAL TURNKEY LABOR VALUE: €63,304.00

### **3.3 Financial Reconciliation Summary**

* Subtotal Hardware Procurement CapEx: €188,396.00 EUR  
* Subtotal Turnkey Staging Labor Allocation: €63,304.00 EUR  
* 5-Year Operational SLA Support Package (OpEx): €35,800.00 EUR  
* TOTAL ESTIMATED PROJECT PRICE TO NVIDIA: €287,500.00 EUR  
* Financial Note: Final project cost rests exactly €12,500.00 below the maximum approved €300k budget ceiling, yielding a net gross profit margin of 25.63% (€73,700.00).

4\. EXECUTIVE PROJECT TOPOLOGY DOCUMENTATION & JUSTIFICATION

4.1 Architectural Design Justification

The network utilizes a Collapsed Core Star Architecture designed to optimize throughput and secure data boundaries across distinct environments.

* Cisco ASA 5506-X Edge Firewall: Acts as the perimeter isolation boundary. Enforces real-time stateful deep packet inspection and segments the network into an Outside Zone, an Inside Zone, and a hardened Perimeter DMZ Zone.  
* Cisco Catalyst 3650 Multilayer Switch (L3 Core): Acts as the high-speed distribution anchor. This chassis routes Inter-VLAN traffic locally in hardware at wire speed, bypassing the throughput bottlenecks of legacy trunking architectures. It maintains the primary Switch Virtual Interfaces (SVIs) and runs core auxiliary helper functions.  
* Cisco Catalyst 2960-24TT Switches (L2 Access): Deployed across distinct physical rooms to provide secure endpoint aggregation. These devices enforce localized broadcast domain containment per department, running edge filters to block rogue spanning-tree configurations and prevent frame analysis.

### **4.2 Verified Core Network Addressing Map**

\[ 🌐 ISP ROUTER \]

|

▼ (Outside Zone)

\[ 🔥 ASA 5506-X FIREWALL \]

/

(Inside Zone) (DMZ Zone)

| |

\[ 🏢 MULTILAYER SWITCH \] \[ 🔒 DMZ ACCESS SWITCH \]

/ | | | \\ |

FTP DNS DHCP AAA iSCSI \[🌐 HTTP Web\]

(.3) (.10) (.2) (.5) (.6) (192.168.90.2)

|

\+---(802.1Q Trunk Access Lines to Room Switches)

|

\+---+-------+-------+-------+-------+-------+

| | | | | |

▼ ▼ ▼ ▼ ▼ ▼

Production Supp\_B Study Supp\_A Mgmt IT\_Dept

(VLAN 10\) (VLAN 20)(VLAN 30)(VLAN 40)(VLAN 50)(VLAN 60\)

* Production (VLAN 10): Address Range 192.168.10.0 | Mask 255.255.255.0 | Gateway 192.168.10.1 | Scope: Dynamic Allocations (.2 to .254)  
* Support\_B (VLAN 20): Address Range 192.168.20.0 | Mask 255.255.255.224 | Gateway 192.168.20.1 | Scope: Dynamic Allocations (.2 to .30)  
* Study (VLAN 30): Address Range 192.168.30.0 | Mask 255.255.255.224 | Gateway 192.168.30.1 | Scope: Dynamic Allocations (.2 to .30)  
* Support\_A (VLAN 40): Address Range 192.168.40.0 | Mask 255.255.255.224 | Gateway 192.168.40.1 | Scope: Dynamic Allocations (.2 to .30)  
* Management (VLAN 50): Address Range 192.168.50.0 | Mask 255.255.255.224 | Gateway 192.168.50.1 | Scope: Dynamic Allocations (.2 to .30)  
* IT Department (VLAN 60): Address Range 192.168.60.0 | Mask 255.255.255.240 | Gateway 192.168.60.1 | Scope: Dynamic Allocations (.2 to .14)  
* Server Pool (VLAN 70): Address Range 192.168.70.0 | Mask 255.255.255.240 | Gateway 192.168.70.1 | Scope: DHCP (.2), FTP (.3), AAA (.5), iSCSI (.6), DNS (.10)  
* Transit Link (N/A): Address Range 192.168.80.0 | Mask 255.255.255.252 | Gateway: Point-to-Point | Scope: Core-MLS Switch (.1) to ASA Firewall (.2)  
* Perimeter DMZ (N/A): Address Range 192.168.90.0 | Mask 255.255.255.0 | Gateway 192.168.90.1 | Scope: HTTP Web Server (.2)

5\. VERIFIED LIVE RUNNING-CONFIG & CLI STAGING GUIDE

The production-ready configuration below is retrieved from the terminal console memory of the central Core Multilayer Switch:

hostname Core-MLS

aaa new-model

aaa authentication login RADIUS\_AUTH group radius local

ip routing

username admin privilege 15 password nvidia\_admin\_pass

spanning-tree mode pvst

interface GigabitEthernet1/0/1

description Link Facing ASA 5506-X Inside Port

no switchport

ip address 192.168.80.1 255.255.255.252

interface GigabitEthernet1/0/2

description Core Infrastructure Server Access Ports

switchport access vlan 70

switchport mode access

switchport nonegotiate

interface GigabitEthernet1/0/6

description Core Trunk Links to Leaf Switches

switchport trunk allowed vlan 10,20,30,40,50,60,70

switchport mode trunk

switchport nonegotiate

interface Vlan10

ip address 192.168.10.1 255.255.255.0

ip helper-address 192.168.70.2

interface Vlan20

ip address 192.168.20.1 255.255.255.224

ip helper-address 192.168.70.2

interface Vlan30

ip address 192.168.30.1 255.255.255.224

ip helper-address 192.168.70.2

interface Vlan40

ip address 192.168.40.1 255.255.255.224

ip helper-address 192.168.70.2

interface Vlan50

ip address 192.168.50.1 255.255.255.224

ip helper-address 192.168.70.2

interface Vlan60

ip address 192.168.60.1 255.255.255.240

ip helper-address 192.168.70.2

interface Vlan70

ip address 192.168.70.1 255.255.255.240

ip route 192.168.90.0 255.255.255.0 192.168.80.2

ip access-list extended NVIDIA\_SECURITY\_ACL

permit ip 192.168.60.0 0.0.0.15 host 192.168.70.6

deny ip 192.168.10.0 0.0.0.255 host 192.168.70.6

deny ip 192.168.20.0 0.0.0.31 host 192.168.70.6

deny ip 192.168.30.0 0.0.0.31 host 192.168.70.6

deny ip 192.168.40.0 0.0.0.31 host 192.168.70.6

deny ip 192.168.50.0 0.0.0.31 host 192.168.70.6

permit ip any any

radius server NVIDIA\_RADIUS

address ipv4 192.168.70.5 auth-port 1645

key nvidia\_secure\_key

line vty 0 15

login authentication RADIUS\_AUTH

6\. INCIDENT BREAKDOWN, ROOT CAUSE ANALYSIS (RCA) & VERIFICATION MATRIX

### **6.1 Comprehensive Engineering Troubleshooting Log**

#### **6.1.1 HTTP Web Server Subnet Mask Misalignment**

* Symptom: Client terminals pulled valid DHCP leases but connections to [https://stratum.nvidia](https://www.google.com/search?q=https://stratum.nvidia) timed out immediately.  
* Root Cause: The HTTP Server was manually assigned a narrow /28 mask mimicking the internal server pool. Because the ASA firewall’s DMZ interface governs a full /24 segment (192.168.90.1), the web server dropped inbound communication due to local network boundary confusion.  
* Resolution: Corrected the server’s static parameter layout to line up perfectly with the DMZ interface parameters: IP Address: 192.168.90.2, Subnet Mask: 255.255.255.0, Default Gateway: 192.168.90.1.

#### **6.1.2 Point-to-Point Route Failure on Transit Link**

* Symptom: Office clients passed internal traffic locally, but traffic destined for the perimeter DMZ space dropped at the core switch layer.  
* Root Cause: Global IP routing was unconfigured on the Core-MLS switch, and it lacked an explicit path mapping destination traffic out over the dedicated point-to-point transit connection (192.168.80.0/30).  
* Resolution: Enabled global hardware routing on the Core switch and mapped the static routing table entry to point directly to the ASA’s inside interface IP:  
   Core-MLS(config)\# ip routing  
  Core-MLS(config)\# ip route 192.168.90.0 255.255.255.0 192.168.80.2

#### **6.1.3 Asymmetric Routing & Stateful Firewall Inbound Policy Drops**

* Symptom: Workstation requests successfully reached the DMZ nodes, but reply packets timed out at the firewall boundary.  
* Root Cause: The ASA firewall's routing engine had no backward pointers for individual /24 and /27 internal subnets, causing its stateful packet inspection mechanism to drop return packets. Additionally, no interface-level access control lists were bound inbound to permit traffic passing across different security zones.  
* Resolution: Formulated a clean aggregate summary route on the ASA pointing down to the Core Switch, created explicit inbound interface access groups, and mapped them to the inside and dmz interfaces:  
   ciscoasa(config)\# route inside 192.168.0.0 255.255.0.0 192.168.80.1  
   ciscoasa(config)\# access-list INSIDE\_TO\_DMZ\_ACL extended permit ip 192.168.0.0 255.255.0.0 192.168.90.0 255.255.255.0  
  ciscoasa(config)\# access-group INSIDE\_TO\_DMZ\_ACL in interface inside  
  ciscoasa(config)\# access-list DMZ\_INBOUND extended permit ip 192.168.90.0 255.255.255.0 192.168.0.0 255.255.0.0  
  ciscoasa(config)\# access-group DMZ\_INBOUND in interface dmz

6.2 Technical Verification Sign-Off Matrix

* DNS Name Resolution: Run "nslookup stratum.nvidia" from internal workstation command prompts. Queries DNS Server 192.168.70.10 and accurately maps the domain to the DMZ web server IP 192.168.90.2. Status: SUCCESSFUL / Active.  
* Web Landing Page Access: Open web browser app and navigate to [https://stratum.nvidia](https://www.google.com/search?q=https://stratum.nvidia). Stateful firewall check allows the request; the server cleanly renders the custom secure web portal. Status: SUCCESSFUL / Active.  
* iSCSI SAN Hardening: From an unauthorized production PC, run: "ping 192.168.70.6". Packets are dropped immediately by the Core switch's outbound access control list. Status: SUCCESSFUL / Active.

**Sign-Off Authority:** Compiled, verified, and baseline-locked solely by Lead Infrastructure Architect Madumathi Singaraju.

