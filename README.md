Proxmox VE Lab on VirtualBox (Windows 11)

A practical virtualization lab demonstrating Proxmox VE deployment, troubleshooting, and multi-VM networking on Windows 11 using VirtualBox.

Overview

This project documents the setup of a virtualized infrastructure lab using Proxmox VE running inside Oracle VirtualBox on Windows 11. It includes detailed troubleshooting of boot issues, system-level configuration, and deployment of multiple virtual machines connected through a shared network.

The goal of this project is to simulate a realistic virtualization environment while demonstrating practical debugging and infrastructure setup skills.

Objectives
Deploy Proxmox VE inside VirtualBox using nested virtualization
Diagnose and resolve critical boot issues (black screen, kernel freeze)
Disable Hyper-V and virtualization conflicts on Windows 11
Build a multi-VM lab environment
Configure networking for full communication between virtual machines

Architecture
[ Windows 11 Host ]
        |
   VirtualBox (Type 2 Hypervisor)
        |
   Proxmox VE (Nested Hypervisor)
        |
     vmbr0 (Linux Bridge)
        ├── Kali Linux
        ├── Ubuntu Server
        └── Windows 10 Pro
Key Challenge

During the installation process, the Proxmox installer consistently failed with a black screen immediately after boot.

Symptoms
Boot menu appeared normally
Selecting installer resulted in a black screen
No output in both GUI and CLI modes
Root Cause

The issue was caused by Windows 11 virtualization conflicts:

Hyper-V still active in the system
Virtualization Based Security (VBS)
Memory Integrity (Core Isolation)
VirtualBox running without direct access to hardware virtualization
Solution

A full system-level remediation was required:

Disabled Hyper-V using bcdedit
Disabled Virtualization Based Security via registry
Disabled Windows features:
Hyper-V
Virtual Machine Platform
Windows Hypervisor Platform
Windows Subsystem for Linux
Disabled Memory Integrity in Windows Security
Performed full system reboot
Verified hypervisor was no longer active
Verification
systeminfo | findstr /i hypervisor

Expected result: no output

Automation Script

The process is automated using a PowerShell script:

scripts/disable_hyperv_autocheck.ps1

This script:

Disables Hyper-V and VBS
Removes conflicting Windows features
Performs post-reboot validation
VirtualBox Configuration

The following configuration was required for stable operation:

EFI disabled
Chipset: PIIX3
Graphics controller: VMSVGA
3D acceleration disabled
CPU: minimum 2 cores
RAM: minimum 4 GB
VT-x / AMD-V enabled
Nested paging enabled
Proxmox Installation

After resolving virtualization conflicts:

Installer booted successfully
Both GUI and CLI installers worked
System installed without errors
Web Interface
https://<IP_ADDRESS>:8006
Replace <IP_ADDRESS> with the IP address assigned to your Proxmox host.

Networking Configuration

A Linux bridge (vmbr0) was used to connect all virtual machines.

All VMs are attached to vmbr0
Each VM receives an IP address in the same subnet
Communication is enabled across all systems
Results
Layer 2 connectivity confirmed via ARP
Layer 3 connectivity confirmed via ICMP (ping)
Stable communication between all virtual machines
Virtual Machines
Kali Linux

Role: Security testing environment
Status: Fully operational and network-connected

Ubuntu Server

Role: Infrastructure and service host
Status: Fully operational with network connectivity

Windows 10 Pro

Role: Client system
Notes: Required VirtIO driver during installation
Status: Fully operational and network-connected

Validation
All virtual machines successfully obtained IP addresses
Ping tests between all systems were successful
ARP tables confirmed Layer 2 communication
Proxmox web interface accessible and stable
Security Note

This project is a lab environment. All IP addresses and configurations are generic and do not expose any real infrastructure.

Disclaimer

This setup is intended for learning and testing purposes only. Running Proxmox inside VirtualBox is not recommended for production environments due to limitations of nested virtualization.

Future Improvements
Isolated network segments for security testing
Docker deployment on Ubuntu Server
Monitoring stack (Prometheus and Grafana)
VLAN configuration and network segmentation
License

MIT License
