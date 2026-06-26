# **NETWORKING\_ANALYSISTECHNICAL REVIEW RECORD**

POST-IMPLEMENTATION REVIEW, SECURITY VALIDATION, AND ROOT CAUSE ANALYSIS (RCA) Project Identifier: NVIDIA-REG-RD-2026-1106 Target Environment: Regional Hub \- Brussels, Belgium Lead Systems Integrator: Technical Support & Engineering Taskforce Documentation State: Final Baseline Engineering Record

1\. SYSTEM ARCHITECTURE BLUEPRINT

This section outlines the finalized logical and physical design states for the corporate facility network layer. Following comprehensive architectural staging and multi-zone diagnostic verification, the network has been transitioned to a fully routed, stable, stateful, and deterministic environment.

1.1 Completed Departmental VLSM Matrix

* Production Zone: VLAN ID 10 | Network Range 192.168.10.0 | Subnet Mask Size 255.255.255.0 (/24) | Default Gateway IP 192.168.10.1 | Host Scope: Dynamic IP Allocations (.2 to .254)  
* Support\_B Zone: VLAN ID 20 | Network Range 192.168.20.0 | Subnet Mask Size 255.255.255.224 (/27) | Default Gateway IP 192.168.20.1 | Host Scope: Dynamic IP Allocations (.2 to .30)  
* Study Zone: VLAN ID 30 | Network Range 192.168.30.0 | Subnet Mask Size 255.255.255.224 (/27) | Default Gateway IP 192.168.30.1 | Host Scope: Dynamic IP Allocations (.2 to .30)  
* Support\_A Zone: VLAN ID 40 | Network Range 192.168.40.0 | Subnet Mask Size 255.255.255.224 (/27) | Default Gateway IP 192.168.40.1 | Host Scope: Dynamic IP Allocations (.2 to .30)  
* Management Zone: VLAN ID 50 | Network Range 192.168.50.0 | Subnet Mask Size 255.255.255.224 (/27) | Default Gateway IP 192.168.50.1 | Host Scope: Dynamic IP Allocations (.2 to .30)  
* IT Department Zone: VLAN ID 60 | Network Range 192.168.60.0 | Subnet Mask Size 255.255.255.240 (/28) | Default Gateway IP 192.168.60.1 | Host Scope: Dynamic IP Allocations (.2 to .14)  
* Server Pool Zone: VLAN ID 70 | Network Range 192.168.70.0 | Subnet Mask Size 255.255.255.240 (/28) | Default Gateway IP 192.168.70.1 | Host Scope: Central DHCP (.2), FTP (.3), AAA (.5), iSCSI Storage Target (.6), DNS (.10)  
* Transit Link Zone: VLAN ID N/A | Network Range 192.168.80.0 | Subnet Mask Size 255.255.255.252 (/30) | Default Gateway IP: Point-to-Point | Host Scope: Core-MLS Switch (.1) to ASA Firewall (.2)  
* Perimeter DMZ Zone: VLAN ID N/A | Network Range 192.168.90.0 | Subnet Mask Size 255.255.255.0 (/24) | Default Gateway IP 192.168.90.1 | Host Scope: HTTP Web Server (.2)

2\. CHRONOLOGICAL ENGINEERING TROUBLESHOOTING LOG & INTERVENTIONS

During deployment and commissioning phases, systematic physical and logical path failures were isolated and remediated. Below is the technical Root Cause Analysis (RCA) and resolution ledger for all issues encountered.

2.1 Incident 1: Physical Layer Dropouts (Support\_A Offline)

* Symptom: Workstations and endpoints inside the Support\_A sector could not communicate locally or reach the network core.  
* Root Cause: Physical layer disconnects. The physical straight-through cabling layout on the access switch (Switch1) for endpoints PC6 through PC9 and the local printing appliance was missing or unmapped.  
* Remediation & Resolution: Terminated physical straight-through Cat6A lines from endpoints to access ports FastTerminal ports. Forced administrative status to active (no shutdown).

2.2 Incident 2: Subnet Mask Discrepancies & SVI Gateway Mismatches

* Symptom: Workstations obtained IP addresses via DHCP but completely failed to ping across zone bounds or communicate with the DMZ Web Portal (192.168.90.2).  
* Root Cause: Subnet mask misalignment between the DHCP server scopes and the Core Switch Virtual Interfaces (SVIs). The pools issued narrow /27 masks (255.255.255.224), whereas the switch interfaces had mismatched default allocations, forcing the endpoints to point to an unreachable local gateway address.  
* Remediation & Resolution: Synchronized all switch gateways to match the DHCP scope structure. Reconfigured the Core-MLS CLI for Vlan20, Vlan30, Vlan40, and Vlan50 SVI parameters securely.

2.3 Incident 3: Asymmetric Routing & Firewall Boundary Drops

* Symptom: Internal subnets successfully hit the DMZ Server, but packet traces showed an instant timeout on the return path.  
* Root Cause: Asymmetric stateful routing on the perimeter Cisco ASA Firewall. The firewall's routing engine had no backward pointers for individual /24 and /27 subnets, causing its stateful packet inspection engine to drop return packets coming from the dmz or inside links.  
* Remediation & Resolution: Implemented a broad internal aggregate summary route on the firewall to encapsulate all internal subnets cleanly under a single next-hop pointer pointing back to the Core switch:  
   route inside 192.168.0.0 255.255.0.0 192.168.80.1

2.4 Incident 4: HTTP Web/FTP Server Relocation

* Symptom: Attempting to run ftp 192.168.70.3 returned an immediate session timeout error.  
* Root Cause: Dual-layer configuration error. First, the server was executing on a completely different zone (DMZ network at 192.168.90.3), rendering the target .70.3 address nonexistent. Second, the ASA firewall lacked active application inspection protocols for FTP's secondary dynamic data port constraints.  
* Remediation & Resolution: Target connections were redirected to the correct IP address. Concurrently, stateful FTP application protocol inspection was forced into the global security policy on the firewall:  
   policy-map global\_policy  
   class inspection\_default  
   inspect ftp

2.5 Incident 5: Peripheral Device Gateway Configuration Failures

* Symptom: Multi-function printers across office zones were completely inaccessible from other segments, and running Cisco IOS CLI interface assignments threw an error.  
* Root Cause: Printer devices had manual static gateway mappings pointing to foreign subnets, and engineers were trying to run physical port configurations directly inside the wrong switch CLI prompts.  
* Remediation & Resolution: Flipped the printers to run dynamic DHCP addressing or re-mapped their static parameters to line up perfectly with their respective local subnets. Executed the appropriate command terminal parameters on the leaf switches.

3\. HARDENED SECURITY & ISOLATION REALIGNMENT

To support the modern perimeter parameters specified in the corporate contract while ensuring zero data degradation, strict extended access control filters are actively deployed across the core multi-layer switch fabric.

3.1 Backend Block-Storage Hardening (NVIDIA\_SECURITY\_ACL)

Applied outbound on the core storage gateway (VLAN 70), this policy restricts raw block-level iSCSI disk access (192.168.70.6) strictly to authorized administrative stations, preventing lateral exploitation paths.

Core-MLS\# show access-lists Extended IP access list NVIDIA\_SECURITY\_ACL

10 permit ip 192.168.60.0 0.0.0.15 host 192.168.70.6 (Matches logged)

20 deny ip 192.168.10.0 0.0.0.255 host 192.168.70.6

30 deny ip 192.168.20.0 0.0.0.31 host 192.168.70.6

40 deny ip 192.168.30.0 0.0.0.31 host 192.168.70.6

50 deny ip 192.168.40.0 0.0.0.31 host 192.168.70.6

60 deny ip 192.168.50.0 0.0.0.31 host 192.168.70.6

70 permit ip any any (General Traffic Clear)

4\. DIAGNOSTIC SIGN-OFF VERIFICATION METRICS

The total environment has been fully cleared against all verification test parameters:

* Name Resolution Test: nslookup stratum.nvidia successfully queries the server at 192.168.70.10 and resolves to the DMZ Web Server IP address.  
* Web Landing Page Access: Workstations cleanly render the custom landing page via HTTP using the corporate domain name link.  
* iSCSI Protection: Attempts to contact the raw storage block from the production subnets are successfully dropped by the access switch fabric, while IT terminals retain clean, low-latency communication paths.

