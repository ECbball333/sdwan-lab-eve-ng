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
- `organization-name`: User defined
- `site-id`: unique per node
- `system-ip`: unique per node
- `vbond`: IP address of primary orchestrator
- Interfaces: configured under `vpn 0`

### Example vManage Base Configuration:
```bash
system
 host-name vmanage01
 system-ip 1.1.1.1
 site-id 100
 organization-name connerco
 vbond 100.100.100.102
!
vpn 0
 interface eth0
  ip address 100.100.100.100/24
  no shutdown
!
ip route 0.0.0.0/0 100.100.100.1  # default-gateway
!
vpn 512    # if needed for remote management
 interface eth1
  ip dhcp-client
  no shut
!
Commit and-quit

Each controller was configured with a static route or default gateway in VPN 0 to ensure full reachability between all nodes.

🌐 vManage GUI Access
Accessed via https://<vmanage-ip>:8443

Default credentials:

Username: admin

Password: admin

✅ Validation Checklist
Checkpoint	Command / Method	Status
All nodes boot and reachable	ping, show interface	✅
GUI accessible on vManage	Web browser (:8443)	✅
Controller system parameters set	show system status	✅
Inter-controller reachability	ping, static routes	✅
vManage control connections up	show control connections	✅
```

## 🔧 vBond1 – Base Configuration

This is the CLI base configuration for `vBond1` used in the SD-WAN lab.

```bash
config
system
 host-name vbond1
 system-ip 1.1.1.2
 site-id 102
 organization-name connerco
 vbond  100.100.100.102 local  # Enables vbond role
!
vpn 0
 interface ge0/0
  ip address 100.100.100.102/24
  no shutdown
 tunnel-interface
  allow-service all # ONLY USE IN LAB ENVIRONMENT!
  encapsulation ipsec
!
 ip route 0.0.0.0/0 100.100.100.1
!
exit
!
vpn 512
interface eth0
 ip add dhcp
 no shut
!
Commit and-quit
```
## 🔧 vSmart1 – Base Configuration

This is the base CLI configuration for `vSmart1` in the SD-WAN lab environment. It includes tunnel interface parameters for establishing secure control-plane connections.

```bash
config
system
 host-name vsmart1
 system-ip 1.1.1.3
 site-id 103
 organization-name connerco
 vbond 100.100.100.102
!
vpn 0
 interface eth0
  ip address 100.100.100.101/24
  no shutdown
  tunnel-interface
   encapsulation ipsec
   allow-service all
  exit
!
 ip route 0.0.0.0/0 100.100.100.1
!
exit


