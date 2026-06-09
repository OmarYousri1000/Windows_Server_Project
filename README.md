# Markdown
# 🌐 Enterprise Network Infrastructure via Windows Server 

This project demonstrates the design, deployment, and automation of a secure, production-grade enterprise network environment built on Windows Server. It simulates a real-world corporate IT infrastructure featuring centralized identity management, dynamic addressing, advanced Group Policy Objects (GPOs), and administrative delegation.

---

## 🚀 Key Project Objectives & Features

### 🏢 1. Active Directory Domain Services (AD DS)
* **Forest Infrastructure:** Deployed a Primary Domain Controller (PDC) managing the forest domain `ITISYSADMIN.com`.
* **Organizational Structure:** Modeled corporate hierarchy by creating dedicated **Organizational Units (OUs)** for key business departments (**IT, HR, and Sales**).
* **Role-Based Provisioning:** Populated each OU with 5 distinct users, designating a specific Department Manager for each.
* **Administrative Delegation:** Granted localized administrative privileges, empowering department managers to manage user credentials and attributes strictly within their respective OUs.

### 🌐 2. Network Core Services (DHCP & DNS)
* **Automated IP Management:** Configured a high-availability DHCP server servicing the `10.0.0.0/24` subnet.
* **Security & Traffic Optimization:** Implemented strict **MAC Filtering** to prevent unauthorized network access, defined dedicated IP **Exclusions**, and configured **Static Reservations** for business-critical nodes.
* **Disaster Recovery:** Formulated routine DHCP database backup strategies for rapid state recovery.

### 🔐 3. Enterprise Control & Desktop Environment Automation (Group Policies)
Designed and deployed fine-grained **Group Policy Objects (GPOs)** to enforce security baselines and desktop standardization across all domain clients:
* **Storage Access Security:** Enforced a company-wide security policy prohibiting all domain users from accessing and exploring local drives (`My Computer` lockdown).
* **Automated Environment Standardization:** * Implemented a corporate **Desktop Wallpaper Policy** via a centralized UNC share path (`\\ITISYSADMIN\SharedFolder\wallpaper.jpg`) to ensure a uniform corporate desktop appearance for all active directory users.
  * *Note: Designed strictly to lock the desktop background while retaining the native system lock screen and welcome screens.*
* **Enterprise Restriction Policy:** Enforced strict multi-stage user constraints, restricting system login hours tied to official business working hours and specific authorized workstations.
* **Automation Scripts:** Deployed startup/logon scripts executing background utilities across all client machines.
* **GPO Lifecycle Management:** Created full backups of GPOs, performed destruction-and-recovery simulations (deleting and restoring "not open drive" policies) to validate backup integrity.

### 📁 4. Centralized Storage & File Services (NTFS, SMB & FSRM)
* **Secure File Sharing:** Provisioned an enterprise-level shared network folder with precise role-based NTFS and SMB share permissions.
* **Storage Allocation Control:** Configured **File Server Resource Manager (FSRM)** to enforce strict storage quotas, capping disk space utilization at `200MB` for designated users (e.g., `IT2`, `Sales2`) while allowing unrestricted allocations for IT administrators.

### 🔄 5. High Availability & Infrastructure Resilience
* **Domain Redundancy:** Promoted and integrated a Child Domain Controller (CDC) to function as a seamless backup/secondary authority, ensuring 100% domain uptime and active directory replication.

---

## 🛠️ Tech Stack & Lab Architecture
* **Server OS:** Windows Server 2019 / 2022
* **Client OS:** Windows 10 Enterprise / Pro
* **Hypervisor:** VMware Workstation (Nested Virtualization Environment)
* **Core Roles:** AD DS, DNS, DHCP, FSRM, Group Policy Management (GPMC)

---

## 📐 Network Topology & OU Design
```text
  [ ITISYSADMIN.com (Forest Root) ]
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
     [ IT ]   [ HR ]   [ Sales ]
        │        │        │
        ├── IT_Mgr  ├── HR_Mgr  ├── Sales_Mgr
        └── 5 Users  └── 5 Users  └── 5 Users
