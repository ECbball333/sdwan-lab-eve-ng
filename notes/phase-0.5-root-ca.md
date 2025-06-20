# Phase 0.5 – Root CA Setup (IOSv 15.7.3)

In this phase, I configured a Cisco IOSv router running IOS 15.7(3) as a local Root Certificate Authority (CA) to sign the CSRs for my Cisco SD-WAN controllers. This setup replaces Cisco's hosted PKI infrastructure in a self-contained lab environment.

---

## 🧱 Lab IP Addressing

| Device     | Role         | IP Address        |
|------------|--------------|-------------------|
| vManage    | Manager      | 100.100.100.100/24 |
| vSmart1    | Controller   | 100.100.100.101/24 |
| vBond1     | Orchestrator | 100.100.100.102/24 |
| ROOT-CA    | Root CA      | 100.100.100.200/24 |

All devices are in VPN 0 with static addressing. NAT will be handled at the ISP layer in Phase 2.

---

## 🎯 Objectives

1. Set up the IOSv router as a local Root CA server
2. Generate a self-signed root certificate
3. Enable enrollment and auto-grant for certificate requests
4. Sign controller CSRs and return valid certificates
5. Upload the signed certificates and root CA cert into vManage

---

## 🔧 Step 1 – Base Configuration on the Root CA Router


hostname ROOT-CA
no aaa new-model
clock timezone EST -5 0
clock summer-time EST recurring
ip name-server 8.8.8.8

username admin privilege 15 secret <password>

interface g0/0
 ip address 100.100.100.200 255.255.255.0
 no shutdown

interface g0/1
 ip address dhcp   ! Used for external terminal (Putty)
 no shutdown

ip http server
ip http secure-server
ip http authentication local

ip route 0.0.0.0 0.0.0.0 100.100.100.1

line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh

ntp master 1
ntp server time.google.com

## 🔐 Step 2 – Configure the Local CA Server

In this step, I configured the IOSv router (running IOS 15.7(3)) to act as a local Root Certificate Authority (CA) using the `crypto pki server` command set. This CA will be used to sign certificate signing requests (CSRs) from my SD-WAN controllers.

This method avoids the need for external tools like OpenSSL or Cisco’s public PKI, and works well in an isolated lab environment.

---

## Step 2.1 Generate RSA Key Pair
Generate a 2048-bit RSA key that the CA server will use to sign controller certificates:
```crypto key generate rsa label PKI modulus 2048```

Verify the key:
```show crypto key mypubkey rsa```

## Step 2.2 Start and Configure the CA Server

Create the CA server, configure its identity, and enable auto-signing:
bash
crypto pki server PKI
 database url flash:
 database level complete
 issuer-name cn=root.connerco.local
 hash sha256
 database archive pkcs12 password Pass2885
 grant auto
 no shutdown
