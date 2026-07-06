# Application Access Matrix Verification Log

**Date Verified:** July 2026  
**Status:** 🟩 PASSED  

This log validates infrastructure-layer enforcement of application access profiles matching the security matrix rules for DHCP, DNS, Web, and stratified FTP access tiers.

---

## 📊 Access Control Policy Matrix

| Source Department / VLAN | DHCP (`.70.2`) | DNS / Web (`.70.1` / `.5`) | FTP Access (`.70.3`) | iSCSI Storage (`.70.6`) | Inter-VLAN Routing (Workspaces) | IT Mgmt Subnet (`.60.0/28`) |
| :--- | :---: | :---: | :--- | :---: | :---: | :---: |
| **IT Department** (`VLAN 60`) | 🟩 Allowed | 🟩 Allowed | 🟩 Full Access | 🟩 Allowed | 🟩 Allowed | 🟩 Full Access |
| **Management** (`VLAN 50`) | 🟩 Allowed | 🟩 Allowed | 🟩 Full Access | 🟥 Denied | 🟥 Denied | 🟩 Replies Only |
| **Production** (`VLAN 10`) | 🟩 Allowed | 🟩 Allowed | 🟩 Full Access | 🟥 Denied | 🟥 Denied | 🟩 Replies Only |
| **Study Sector** (`VLAN 30`) | 🟩 Allowed | 🟩 Allowed | 📘 Read-Only | 🟥 Denied | 🟥 Denied | 🟩 Replies Only |
| **Support-A** (`VLAN 40`) | 🟩 Allowed | 🟩 Allowed | 🟥 Denied | 🟥 Denied | 🟥 Denied | 🟩 Replies Only |
| **Support-B** (`VLAN 20`) | 🟩 Allowed | 🟩 Allowed | 🟥 Denied | 🟥 Denied | 🟥 Denied | 🟩 Replies Only |

---

## 🧪 Test Execution & Results

### Test 2.1: Full FTP Access (Production & Management)
*   **Action:** Opened the Command Prompt on a **Production PC** (`VLAN 10`) and initiated an FTP session to the central server:
    ```cmd
    ftp 192.168.70.3
    ```
*   **Observed Behavior:** Connection established immediately (`220- Welcome to PT Ftp server`). Logged in with user credentials and executed file reads/writes successfully.
*   **Security Assessment:** Matches Matrix (`Full Access`).

### Test 2.2: Read-Only FTP Access Enforcement (Study Sector)
*   **Action:** Opened the Command Prompt on a **Study PC** (`VLAN 30`). Logged into the FTP server (`192.168.70.3`) and ran a directory list followed by an upload command:
    ```cmd
    dir
    put sample.txt
    ```
*   **Observed Behavior:** The `dir` command returned the file system cleanly. However, the `put` data-write command stalled indefinitely and timed out.
*   **Security Assessment:** Matches Matrix (`Read-Only`). Port 21 (FTP Control) is permitted, but Port 20 (`ftp-data`) is actively dropped by `STUDY_INBOUND_ACL`.

### Test 2.3: Explicit FTP Denial (Support-A & Support-B)
*   **Action:** From **Support-A** (`VLAN 40`) and **Support-B** (`VLAN 20`) PCs, executed `ftp 192.168.70.3`.
*   **Observed Behavior:** Connection stalled immediately at `Trying to connect...` and failed.
*   **Security Assessment:** Matches Matrix (`Denied`).

---

## 📸 Verification Evidence
Terminal captures showing read-only enforcement and total application drops:

![Full FTP Access](screenshots/full_ftp_access.png)
![Study FTP Read Only](screenshots/ftp_study_readonly.png)
![Support FTP Denied](screenshots/ftp_support_denied.png)