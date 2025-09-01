# Hybrid-Windows-Linux-
Hybrid Windows &amp; Linux Server Lab – Proof of Concept demonstrating AD DS, DNS, DHCP, IIS, VPN with RADIUS, SMB/Samba shares, and Apache web hosting. Includes configuration screenshots and verification from internal and external clients.
# Advanced Server Project – Hybrid Windows/Linux Lab

## 📖 Overview
This project was completed as part of **Assignment 2: Advanced Server**.  
The goal was to design and configure a hybrid server environment that integrates both **Windows Server** and **Linux** services.

The proof-of-concept demonstrates:
- Centralized authentication using **Active Directory**
- Secure **VPN access with RADIUS authentication**
- Hosting **websites on Windows IIS and Linux Apache**
- Providing **file shares via SMB and Samba**
- Integrating **Windows and Linux into the same domain**
- Verifying access from both **internal** and **external clients**

---

## 🏗️ Network Topology
The lab consisted of:
- **Windows Domain Controller (DC)**  
  - AD DS, DNS, DHCP, IIS, NPS (RADIUS), SMB “sales” share  
- **Windows VPN/RRAS Server**  
  - NAT gateway, VPN endpoint, DHCP failover partner  
- **Linux Server (CentOS 9)**  
  - Samba share, Apache websites, SSH access with sudo user  
- **Internal Client (Windows 10)**  
  - Domain joined, tests services as normal users  
- **External Client (Windows 10)**  
  - VPN-connected, manages servers via RSAT, SSH to Linux  

---

## ⚙️ Configuration Steps

### 1. Windows Domain Controller
- Built new AD domain: **firstname.com**
- Imported users and groups via PowerShell script (`usermaker.ps1`)
- Installed **DHCP** with a 100-IP scope
- Configured **DNS** forward lookup zone
- Installed **IIS** and hosted two websites:
  - `http://public.firstname.com` → anonymous access
  - `https://private.firstname.com` → restricted to WEBusers (Windows Authentication)
- Configured **SMB share** at `C:\shares\sales`
  - Sales → Modify  
  - Accounting & Management → Read  
  - Admins → Full control
- Installed **NPS (RADIUS)** for VPN authentication

### 2. Windows VPN/RRAS Server
- Configured **NAT** for Internet access  
- Enabled **VPN** with RADIUS auth against DC  
- Set up **DHCP failover** with DC (60/40 split)

### 3. Linux Server
- Joined domain `firstname.com`
- Configured **Samba share** at `/shares/accounting`:
  - Accounting → RW  
  - Others → Read  
- Installed **Apache** websites:
  - `http://customers.firstname.com` (public)
  - `http://coolkids.firstname.com` (restricted to WEBusers)
- Created local sudo-enabled user **boss** for SSH

### 4. Internal Client (Windows 10)
- DHCP client, domain joined
- Verified access to:
  - Public & private IIS websites
  - Samba + SMB shares with correct permissions
  - Linux Apache websites via hostname

### 5. External Client (Windows 10)
- Connected successfully to VPN
- Verified access to:
  - All web services and shares
  - Linux SSH with **boss** user (`sudo firewall-cmd --list-all`)
  - Managed servers remotely via **RSAT** as `meadmin`

---

## 🗂️ Artifacts
This repo contains screenshots that demonstrate successful configuration:
- ADUC user and group creation
- DNS forward lookup zone
- DHCP scope & failover config
- IIS website bindings and authentication
- SMB share permissions
- RRAS VPN settings
- Samba config (`smb.conf`) and share permissions
- Apache virtual host configs
- Internal client tests (Pascal, Louise)
- External client VPN + SSH tests

---

## 🔍 Platform Comparison
- **Windows Strengths:** Centralized management (AD, DNS, DHCP, VPN + RADIUS). Great for enterprise integration.  
- **Linux Strengths:** Lightweight, flexible web/file hosting (Apache, Samba). Lower resource use and licensing cost.  
- **Conclusion:** A **hybrid approach** is best – Windows for directory/authentication, Linux for web & file services.

---

## 🎓 Lessons Learned
- Configured **DHCP failover** with load balancing
- Set up **VPN with RADIUS authentication**
- Applied **Windows Authentication in IIS**
- Joined **Linux server to AD domain**
- Used **AGLP** for file share permissions
- Tested services with different users for security validation

---
