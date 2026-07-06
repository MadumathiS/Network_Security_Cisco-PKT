# Inter-VLAN Routing & Isolation Verification Log

**Date Verified:** July 2026  
**Status:** 🟩 PASSED  

This log documents structural enforcement of department workstation segregation across the collapsed core multi-layer switch line cards[cite: 1], ensuring zero lateral traffic flow while maintaining IT administrative override visibility.

---

## 🧪 Test Execution & Results

### Test 3.1: IT Department Traversal Overrides
*   **Action:** Opened the Command Prompt on the **IT Dept PC** (`VLAN 60`)[cite: 1]. Executed diagnostic sweeps to other subnets:
    ```cmd
    ping 192.168.10.14
    ping 192.168.50.7
    ```
*   **Observed Behavior:** Pings resolved successfully with 0% packet loss. 
*   **Security Assessment:** Successful. Return-path ACL overrides allow peer nodes to answer IT queries while maintaining their default local protections.

### Test 3.2: Lateral Department Blocking (Peer Isolation)
*   **Action:** From a **Production PC** (`VLAN 10`)[cite: 1], attempted to ping a workstation inside the **Study Sector** (`VLAN 30`)[cite: 1]:
    ```cmd
    ping 192.168.30.3
    ```
*   **Observed Behavior:** `Request timed out` (100% loss).
*   **Security Assessment:** Successful. Lateral department communication is entirely intercepted and discarded at the Core-MLS line interface boundary.

---

## 📸 Verification Evidence
![IT Administration Access Paths](screenshots/it_ping_success.png)