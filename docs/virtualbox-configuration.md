

# VirtualBox Configuration for Proxmox VE

## Overview

Correct VirtualBox configuration is required for stable nested virtualization.

---

## System Settings

- Chipset: PIIX3  
- EFI: Disabled  
- I/O APIC: Enabled  

---

## CPU

- Minimum: 2 cores  
- PAE/NX: Enabled  
- VT-x/AMD-V: Enabled  

---

## Memory

- Minimum: 4096 MB  

---

## Display

- Graphics Controller: VMSVGA  
- Video Memory: 128 MB  
- 3D Acceleration: Disabled  

---

## Storage

- Disk size: 32 GB or more  
- Format: VDI (dynamic)  

---

## Network

- Adapter type: Bridged  
- Adapter model: Intel PRO/1000 MT Desktop  

---

## Common Issues

Incorrect configuration may result in:

- Black screen during boot  
- Installer freeze  
- Kernel panic  
