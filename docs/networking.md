# Networking Configuration

## Overview

All virtual machines are connected using a Linux bridge (vmbr0) inside Proxmox.

---

## Configuration

- Bridge: vmbr0  
- Connected to physical network interface  
- All VMs attached to vmbr0  

---

## Behavior

- All systems share the same subnet  
- IPs assigned via DHCP or static configuration  
- Full inter-VM communication enabled  

---

## Verification

### IP check
```bash
ip a
```
### Connectivity test
```bash
ping <target-ip>
```
### ARP table
```bash
arp -a
```

## Result
- Layer 2 connectivity confirmed
- Layer 3 connectivity confirmed
- Stable communication between all virtual machines
