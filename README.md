# Hybrid-Identity-Governance-AD-to-Entra 
This project documents the deployment of a **Hybrid Identity Environment**, bridging on-premises Windows Server infrastructure with **Microsoft Entra ID (Azure AD)**. The architecture focuses on the **Zero Trust framework**, implementing **Multifactor Authentication (MFA)**, **Just-In-Time (JIT) administration**, automated endpoint management via **Intune**, and robust **Disaster Recovery (DR)** protocols.

## Table of Contents
* [Project Overview](#Hybrid-Identity-Governance-AD-to-Entra)
* [Topology Diagram](#Topology)
* [Tools & Technologies](#Tools-used)
* [Implementation Process](#Process)
* [Challenges & Troubleshooting](#Challenges--Troubleshooting)
* [Key Takeaways](#What-I-Learned-and-Applied)

## Tools used
- **Windows Server 2022**
- **Active Directory**
- **Microsoft Entra**
- **Windows Defender**
- **Intune**
- **VMWare**
- **Powershell**
- **DNS & DHCP**

## Topology
<img width="1408" height="768" alt="topology" src="https://github.com/user-attachments/assets/fe4a09c3-94d4-4eb0-84d6-5a52341454a1" />

## Challenges & Troubleshooting
- **Virtual Network Connectivity (VMware NAT Service): * Issue:** Encountered an "Ethernet Disconnected" error on the Windows  Server VM, preventing IP discovery and domain configuration.
  - **Resolution:** Identified that the `VMware NAT Service` on the host machine had stopped. Manually restarted the service via `services.msc`, which successfully restored the virtual network bridge and allowed the server to pull a valid IP.
- **DNS Resolution during Domain Joins: * Issue:** New client VMs were unable to find the Domain Controller during the domain join process.
  - **Resolution:** Diagnosed the issue as a DNS mismatch. Manually configured the client's IPv4 settings to point directly to the Windows Server’s static IP as the Primary DNS server, ensuring the client could resolve the `lab.local` domain.
- **NTFS vs. Share Permission Conflicts: * Issue:** Users were able to see folders but could not modify files, despite having "Modify" permissions in the Security tab.
  - **Resolution:** Discovered that the "Share" permissions were set to "Read Only" for Everyone, which was overriding the NTFS Security settings. Changed Share permissions to "Full Control" and used the Security tab (NTFS) to handle the granular access, which resolved the conflict.
- **Intune Enrollment & TPM Requirements: * Issue:** Initial attempts to enroll the Windows Client into Intune failed due to hardware verification errors.
  - **Resolution:** Enabled Virtual TPM and Secure Boot in the VMware hardware settings. This satisfied the hardware security requirements for Intune and allowed for successful MDM enrollment and BitLocker policy application.
- **PowerShell Script Logic (Duplicate Objects): * Issue:** The initial bulk-user import script failed when encountering duplicate usernames or existing OU paths.
  - **Resolution:** Reverted to a clean state using a VM snapshot, refined the CSV data for unique identifiers, and adjusted the script logic to verify OU existence before attempting creation, resulting in a 100% success rate for the 100-user import.

## What I Learned and Applied
- **The Principle of Least Privilege (PoLP):** I gained a deep understanding of how to minimize a company's "attack surface" by using Role-Based Access Control (RBAC) and Access-Based Enumeration. I learned that users should only see and access exactly what they need to do their jobs—nothing more.
- **Modern Identity Governance (Zero Trust):** Beyond just setting up passwords, I learned how to implement a Zero Trust architecture. By using **Just-In-Time (JIT)** access through PIM and **Conditional Access** policies, I learned how to protect a tenant even if a user's password is compromised.
- **Hybrid Infrastructure Architecture:** I learned how to manage the "bridge" between legacy on-premises systems (Active Directory) and modern cloud environments (Entra ID). This included understanding UPN suffixes, synchronization cycles, and how to manage a single identity across two different platforms.
- **The Importance of Redundancy & Recovery:** This project taught me that "backups" are not a single step but a multi-layered strategy. I learned the difference between hardware redundancy (RAID 1), accidental deletion protection (**Shadow Copies/Recycle Bin**), and total disaster recovery (**Full System Backups**).
- **Endpoint Security Lifecycle:** I gained hands-on experience in the full lifecycle of a device—from automated enrollment via **Intune**, to deploying software (7-zip), enforcing security compliance (BitLocker/USB blocks), and eventually performing a remote wipe to protect company data.
- **Automation as a Force Multiplier:** By using **PowerShell** to provision 100 users instantly, I learned how automation reduces human error and allows IT departments to scale efficiently.

## Process
<details>
<summary>Click to expand: Phase 1 - Infrastructure & RAID Setup</summary>
  
- **Server Provisioning:** Allocation of resources and conversion from Evaluation to Retail mode to ensure environment stability. (ss)
- **RAID 1 Implementation:** Configured a mirrored volume across two dedicated drives to ensure hardware-level redundancy for the OS and Active Directory database. (ss)
- **Failure Simulation:** Performed a "Pull-the-Plug" test by removing a drive from the VM. Verified data persistence on the surviving mirror and documented the process of initializing a new drive to rebuild the array. (ss)
- **Network Persistence:** Resolved a VMware NAT service failure (ss) and configured a static IPv4 stack with loopback DNS to ensure DC stability. (ss)
</details>

<details>
<summary>Click to expand: Phase 2 - Identity Architecture & RBAC (On-Premises)</summary>

(Put your text and screenshots here)

</details>

<details>
<summary>Click to expand: Phase 3 - Defensive GPOs & Data Recovery</summary>

(Put your text and screenshots here)

</details>

<details>
<summary>Click to expand: Phase 4 - The Hybrid Bridge (Entra ID Integration)</summary>

(Put your text and screenshots here)

</details>

<details>
<summary>Click to expand: Phase 5 - Modern Management & Incident Response (Intune/Defender)</summary>

(Put your text and screenshots here)

</details>

