# Phase 1 – SD-WAN Controller Deployment

This phase covers the deployment and base configuration of Cisco SD-WAN controllers in my EVE-NG lab: **vManage**, **vSmart (x2)**, and **vBond (x2)**.

---

## 🎯 Objectives

- Deploy all SD-WAN controller nodes in EVE-NG
- Configure system parameters: `system-ip`, `site-id`, `organization-name`, and `vbond` IPs
- Establish full IP reachability between controllers in VPN 0
- Validate overlay connectivity and GUI access

---

## 🧱 Controller VM Specifications

| Controller | vCPU | RAM (MB) | Notes |
|------------|------|----------|-------|
| vManage    | 32   | 65536    | Manager node (GUI/API)
| vSmart 1   | 2    | 2048     | Control plane
| vSmart 2   | 2    | 2048     | Control plane (redundant)
| vBond 1    | 1    | 1024     | Orchestrator + ZTP server
| vBond 2    | 1    | 1024     | Orchestrator (redundant)

---

## 🔧 Configuration Summary (All Nodes)

### Common Values:
- `organization-name`: connerco
- `site-id`: unique per node
- `system-ip`: unique per node
- `vbond`: IP address of primary orchestrator
- Interfaces: configured under `vpn 0`

### Example Base Configuration:
```bash
system
 host-name vmanage01
 system-ip 1.1.1.1
 site-id 100
 organization-name ConnerCo
 vbond 192.0.2.10
!
vpn 0
 interface eth0
  ip address 192.0.2.100/24
  no shutdown
!

Each controller was configured with a static route or default gateway in VPN 0 to ensure full reachability between all nodes.

🌐 vManage GUI Access
Accessed via https://<vmanage-ip>:8443

Default credentials:

Username: admin

Password: admin

Completed initial setup wizard (if prompted)

✅ Validation Checklist
Checkpoint	Command / Method	Status
All nodes boot and reachable	ping, show interface	✅
GUI accessible on vManage	Web browser (:8443)	✅
Controller system parameters set	show system status	✅
Inter-controller reachability	ping, static routes	✅
vManage control connections up	show control connections	✅

📌 Notes
One vBond node will also serve as the ZTP server in Phase 0.

The controller overlay has been initialized — all nodes will register edge devices in Phase 3.

Controllers are using NATed interfaces via simulated ISP routers (to be configured in Phase 2).
