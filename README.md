# sdwan-lab-eve-ng
# Cisco SD-WAN EVE-NG Lab

This project documents a full-featured Cisco SD-WAN deployment built in an offline EVE-NG environment. The lab includes dual ISP simulation, ZTP-based onboarding, and 3 remote sites with routing integration.

## 🚀 Lab Goals

- Build and validate a 3-site Cisco SD-WAN topology
- Simulate dual transport (INET/MPLS) using IOSv routers
- Deploy all SD-WAN controllers (vManage, vSmart, vBond)
- Implement ZTP onboarding using a local ZTP server
- Practice real-world edge routing with BGP and OSPF
- Document the journey step-by-step for reproducibility

---

## 🧱 Topology Overview

### Nodes
- **vManage** (Manager Node)
- **2x vSmart** (Control Plane)
- **2x vBond** (Orchestrators, 1 used for ZTP server)
- **3x cEdge (Catalyst 8000v)** – one per branch site
- **2x IOSv Routers** simulating INET and MPLS ISPs
- (Optional) **ZTP Server** hosted on vBond or Linux VM

> A `.drawio` topology diagram will be added here soon.

---

## 🔄 Lab Phases

### ✅ Phase 0 – ZTP Server Setup
- [ ] Create a simple ZTP web server (Python HTTP or Apache)
- [ ] Host bootstrap config
- [ ] Simulate Cisco ZTP cloud with DNS override (`ztp.viptela.com` → vBond IP)

### 🛠️ Phase 1 – Controller Deployment
- [ ] Deploy vManage (large memory, minimum 32GB)
- [ ] Deploy vBond (x2) and vSmart (x2)
- [ ] Configure system IPs, organization name, site IDs
- [ ] Ensure mutual reachability across controllers (VPN 0)

### 🌐 Phase 2 – Transport Simulation (INET/MPLS)
- [ ] Configure 2 IOSv routers as NATed ISP devices
- [ ] Assign different public IP ranges per router
- [ ] Test NAT, route reachability, and color settings

### 🏠 Phase 3 – Site Deployment (Branches)
- [ ] Create Branch1, Branch2, and HQ (DC)
- [ ] Onboard each using ZTP or manual bootstrap
- [ ] Establish overlay (OMP, TLOCs) to all controllers

### 📡 Phase 4 – Routing and VPNs
- [ ] Configure VPN 10 in each site
- [ ] Test inter-site routing with OSPF/BGP
- [ ] Implement DIA, firewall zones, policies (optional)

---

## 📸 Screenshots & Diagrams

_Topology diagrams, CLI outputs, and UI screenshots will go here._

---

## 🛠️ Tools & Versions

| Component   | Version     |
|------------|-------------|
| EVE-NG     | Pro or Community |
| Cisco SD-WAN | 20.15.1     |
| vManage    | 32 vCPU / 64 GB RAM |
| cEdge      | Catalyst 8000v |
| ISP Routers | IOSv 15.x     |

---

## ✍️ Notes

All configurations, screenshots, and diagrams are stored in the following folders:

