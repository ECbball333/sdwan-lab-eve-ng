# Phase 0.5 – Root CA Server Setup

This phase covers the creation of a local Root Certificate Authority (CA) to issue and sign certificates for all Cisco SD-WAN controllers in my lab environment.

---

## 🎯 Objectives

- Build a basic Root CA server on a Cisco IOSv router
- Generate a self-signed root certificate
- Sign CSR files from vManage, vSmart, and vBond
- Upload signed certs into vManage GUI
- Enable secure control-plane establishment

---

## 🧱 Lab Setup

| Role       | Hostname    | IP Address       | Notes              |
|------------|-------------|------------------|--------------------|
| Root CA    | `root-ca`   | `192.0.2.200/24` | IOSv or Linux node |
| Controllers | Various     | `192.0.2.x`      | All in VPN 0       |

---

## 🔧 IOSv Root CA Configuration

### 1. **Create RSA Key**
```bash
hostname root-ca
crypto key generate rsa general-keys modulus 2048

