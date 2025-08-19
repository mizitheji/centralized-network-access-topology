# 🌐 Cisco Enterprise Wireless Topology – FlexConnect Deployment

This repository contains configuration and documentation for a **Cisco C9800 WLC** deployed at HQ (R1) with **branch access points** at R2 and R4 using **FlexConnect mode**.

---

## 📌 Why FlexConnect?

In this topology:  
- The **WLC** is centralized at HQ.  
- **Access Points** are deployed at remote branches (R2 and R4).  
- The WAN connects the branches back to HQ.  

If centralized switching is used, branch traffic would **hairpin through HQ**, causing:  
- 🚦 **Increased WAN usage**  
- ⏱️ **Higher latency** (bad for voice/video)  
- ❌ **WAN dependency** (branch users lose access if WAN goes down)  

---

## ✅ FlexConnect Benefits

By enabling **FlexConnect mode** on branch APs:  

- **Local Switching**  
  - Client traffic is bridged directly at R2 and R4  
  - Reduces WAN overhead and improves performance  

- **Centralized Control**  
  - WLC manages WLANs, SSIDs, and security policies globally  

- **Resiliency**  
  - Branch APs continue authenticating clients (cached credentials) even during WAN outages  

- **Scalability**  
  - No need for a WLC at each branch site  

---

## 🛠️ Implementation Overview

- **Flex Profiles** – Define VLAN mappings per branch  
- **Site Tags** – Group APs logically by site (e.g., `R2-Site`, `R4-Site`)  
- **Local Switching** – Keeps client data inside the branch LAN  
- **Centralized Authentication** – Cisco ISE + Active Directory manage user access  

---

## 🎯 Outcome

This design ensures:  
- Efficient wireless traffic flow  
- Centralized policy enforcement  
- High availability at branches  
- Optimized WAN utilization  

---


