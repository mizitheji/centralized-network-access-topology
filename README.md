# 🌐 Centralized-network-access-topology

This repository documents the **cloud-based network lab topology** running on Proxmox VE with routing, VLANs, DHCP, wireless integration, monitoring, and role-based authentication.  
The lab simulates a centralized infrastructure with branch routers, access points, and centralized identity management.

---

## 📖 Topology Overview

- **Proxmox VE** hosts the following virtual machines:
  - **ISE** – Cisco Identity Services Engine (`192.168.100.10`)
  - **WLC** – Wireless LAN Controller (`192.168.100.20`)
  - **AD/DNS** – Active Directory & DNS Server (`192.168.100.30`)
  - **PRTG** – Network Monitoring (`192.168.100.40`)

- **R1** acts as the **core router** connected to the Proxmox servers in **Area 0**.  
- **R2, R3, R4** are **branch routers**, each providing local VLANs and DHCP.  
- **Access Points (APs)** connect to **R2** and **R4**, managed by the **WLC** in the core.  
- **OSPF** is used to interconnect all routers across different areas.  

---

## 🗂 IP Addressing

### Core & WAN Links
| Link | Network | R1 | Branch Router |
|------|---------|----|---------------|
| Core LAN | `192.168.100.0/24` | `.1` | Servers |
| R1–R2 | `192.168.2.0/30` | `.1` | R2 = `.2` |
| R1–R3 | `192.168.3.0/30` | `.1` | R3 = `.2` |
| R1–R4 | `192.168.4.0/30` | `.1` | R4 = `.2` |

### Branch VLANs
- **R2 VLANs**  
  - `10.2.2.0/24` – MGMT (Vlan 1)
  - `172.20.2.0/24` – WIRED (Vlan 10)
  - `172.19.2.0/24` – WIRELESS (Vlan 20)

- **R3 VLANs**  
  - `10.3.3.0/24` – MGMT (Vlan 1)
  - `172.20.3.0/24` – WIRED (Vlan 10)

- **R4 VLANs**  
  - `10.4.4.0/24` – MGMT (Vlan 1)
  - `172.20.4.0/24` – WIRED (Vlan 10)
  - `172.19.4.0/24` – WIRELESS (Vlan 20)

---

## 🔑 Authentication & Access Control

### **Users**
- Credentials stored in **Active Directory (AD)**.  
- Used for:  
  - WiFi authentication (via APs + WLC + ISE)  
- Restrictions:  
  - **Users cannot log in to switches, routers, and WLC.**  

### **Admins**
- Credentials managed by **Cisco ISE**.  
- Admins authenticate with **their own ID** to:  
  - Switches  
  - Routers  
  - WLC  
- Role-based access control (RBAC):  
  - **Read-only role** for junior admins  
  - **Full access role** for senior admins  

---

## 📡 Wireless Integration

- **Access Points (APs)** connect to **R2** and **R4**.  
- VLANs are trunked from routers to AP switches for **SSID-to-VLAN mapping**.  
- **WLC (192.168.100.20)** centrally manages APs via CAPWAP.  
- **ISE (192.168.100.10)** enforces network access policies (802.1X, posture, profiling).  
- **AD** provides user credentials for wireless login.  

---

## ⚙️ Key Configurations

- **Router-on-a-stick** on R2, R3, R4 for inter-VLAN routing.  
- **DHCP** on each branch router for local VLANs.  
- **AD/DNS (192.168.100.30)** is the DNS server for all devices.  
- **ISE** handles **admin login authentication** and device access policies.  
- **OSPF Areas**:  
  - Area 0 → Core LAN (R1 + Proxmox servers)  
  - Area 2 → R1–R2 link  
  - Area 3 → R1–R3 link  
  - Area 4 → R1–R4 link  

---

## 📊 Monitoring

- **PRTG** (`192.168.100.40`) monitors routers, switches, servers, and APs.  
- ICMP and SNMP sensors track health and performance.  
- Alerts are configured for:  
  - Device down  
  - High CPU/memory usage  
  - Interface errors  

---

## ✅ Notes

- Each branch router handles DHCP for its VLANs.  
- Users authenticate to the network via **AD credentials**.  
- Admins authenticate to network devices via **ISE credentials**.  
- End-to-end connectivity and authentication must be verified for both **wired and wireless clients**.  

---
