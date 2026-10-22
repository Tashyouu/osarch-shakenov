# Lab 1 — Your first virtual machine

Pair: none   Driver first half: Tamerlan Shakenov
Machine: HP Victus 16-e0XXX (Laptop), Windows 10 Professional, VMware Workstation Pro, guest Ubuntu Server 26.04.1 LTS
Date: 22.10.2026

## What I did
1. Installed VMware Workstation Pro 17, version 26.0.1.25688693. Licence: https://blogs.vmware.com/cloud-foundation/2024/11/11/vmware-fusion-and-workstation-are-now-free-for-all-users/
2. Created VM lab01-ubuntu: 4 vCPU, 8 GB RAM, 25 GB dynamic SSD disk, NAT network.
3. Installed Ubuntu Server, took snapshot clean-install.
4. Broke it: `sudo sed -i 's/dhcp4: true/dhcp4: false/' /etc/netplan/*.yaml` && `sudo netplan apply`
5. Rolled back to clean-install: Used VMware Snapshot Manager to revert the virtual machine state to clean-install snapshot.

## Resource justification
I allocated 4 vCPU and 8 GB of RAM to the virtual machine because this is more than sufficient for a headless Ubuntu Server to run smoothly on my modern laptop (since it has 8 cores and 16 threads, and 32 GB of RAM), while leaving most of the resources for the Windows 10 host system to function without any lag at all.

## Result
The VM boots; the break-and-rollback cycle is shown in the evidence below.

## What did not work the first time
Initially, when I tried to run the SSH command from my host laptop, the resulting text file was completely empty. It turned out that the network connection to the VM was down because I changed the DHCP settings incorrectly and applied the network configuration before saving the output. To fix this, I used VMware Snapshot Manager to roll back to the clean-install state, double-checked my VM's active IP with 'ip -br a', and successfully reran the command directly without losing connection.

## Evidence
- [evidence/vm-settings.png](evidence/vm-settings.png) — CPU, memory and disk as configured
- [evidence/guest-release.txt](evidence/guest-release.txt) — Ubuntu Server is running in the guest
- [evidence/broken.png](evidence/broken.png) — the guest in its broken state
- [evidence/restored.png](evidence/restored.png) — the guest in its restored state
- [evidence/snapshot-list.png](evidence/snapshot-list.png) — the snapshot, after the rollback
