# Back-End iSCSI Storage Security Verification Log

**Date Verified:** July 2026  
**Status:** 🟩 PASSED  

This log documents the verification metrics used to protect the enterprise back-end storage pool infrastructure node (`192.168.70.6`) from unauthorized discovery, structural lateral movement, or data-plane network disruption.

---

## 🧪 Test Execution & Results

### Test 4.1: Switchport Physical Integrity Audit
*   **Action:** Ran an interface query on the central **Core-MLS** where the storage array terminates:
    ```text
    show running-config interface fastEthernet 0/24
    ```
*   **Observed Behavior:** Interface is locked exclusively to access `Vlan70` and runs optimized Spanning-Tree `portfast` routines to guarantee high availability.

### Test 4.2: Standard Subnet Inbound Shielding
*   **Action:** Opened the Command Prompt on a **Production PC** (`VLAN 10`)[cite: 1] and attempted to ping the storage node:
    ```cmd
    ping 192.168.70.6
    ```
*   **Observed Behavior:** Terminal returned 100% packet loss (`Request timed out`). 
*   **Security Assessment:** Successful. Extended access control structures intercept and drop storage-bound data streams right at the client SVI ingress layer.

### Test 4.3: Administrative Path Validation
*   **Action:** From an authorized **IT Dept PC** (`VLAN 60`)[cite: 1], executed a diagnostic connection tracking test to the node:
    ```cmd
    ping 192.168.70.6
    ```
*   **Observed Behavior:** Connection instantly completed with low-latency replies (`<1ms`).
*   **Security Assessment:** Successful. The IT administration cluster retains explicit, unhindered visibility to handle storage area network (SAN) operations and resource monitoring.

---

## 📸 Verification Evidence
Side-by-side terminal verification proving standard client drop vs. administrative allowance:

![iSCSI Storage Verification Matrix](screenshots/iscsi_validation.png)