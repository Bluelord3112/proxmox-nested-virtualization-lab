# Hyper-V Removal and Virtualization Fix (Windows 11)

## Overview

Proxmox VE failed to boot inside VirtualBox due to Windows 11 virtualization conflicts. Hyper-V and Virtualization-Based Security (VBS) were preventing VirtualBox from accessing hardware virtualization (VT-x/AMD-V).

---

## Symptoms

- Black screen after starting Proxmox installer  
- No GUI or CLI output  
- System appears frozen  

---

## Root Cause

- Hyper-V enabled at boot level  
- Virtual Machine Platform enabled  
- Windows Hypervisor Platform enabled  
- Memory Integrity (Core Isolation) active  
- VirtualBox running without hardware virtualization access  

---

## Resolution Steps

### 1. Disable Hypervisor at Boot

```powershell
bcdedit /set hypervisorlaunchtype off

```
### 2. Disable Virtualization-Based Security

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard /v EnableVirtualizationBasedSecurity /t REG_DWORD /d 0 /f

```
### 3. Disable VBS Launch Type

```powershell
bcdedit /set vsmlaunchtype off

```
### 4. Disable Windows Features
Disabled via Windows Features: 
- Hyper-V
- Virtual Machine Platform
- Windows Hypervisor Platform
- Windows Subsystem for Linux
- Containers
- Windows Sandbox

### 5. Disable Memory Integrity
  Windows Security → Device Security → Core Isolation
  Memory Integrity: OFF

### 6. Reboot System
  Full restart required to apply changes.

### 7. Verification

```powershell
systeminfo | findstr /i hypervisor

```
Expected result: no output
