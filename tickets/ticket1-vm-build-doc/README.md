# Ticket 1 — New developer starts Monday, needs a test machine

## 1. What was asked
> "Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks"

---

## 2. What was actually needed

### What the request left out
The original request from the Office Manager (Aigerim T.) was non-technical and omitted critical engineering specifications:
* **Operating System Requirements:** The request did not specify which operating system or dev stack the new employee requires.
* **Hardware & Resource Limits:** It omitted host capacity constraints (CPU, RAM, SSD space).
* **Network Scope:** It did not clarify whether the environment requires external internet access or isolated internal access.

### Where the request was wrong
* **"Nothing special":** A modern developer setup requires a properly structured multi-OS environment (both Linux Server and Windows client) rather than a single basic setup.
* **"Without breaking anything":** Standard VM installation alone does not prevent breaking state. Achieving a safe testing sandbox strictly requires baseline snapshots and rollback mechanisms.

### What I assumed instead
* **Stack:** Assumed a standard dual-environment stack requiring an Ubuntu Server LTS VM (backend) and a Windows 11 Enterprise Evaluation VM (frontend/client).
* **Host Hardware Budget:** Evaluated a host workstation with 32 GB RAM, 1024 GB (1 TB) SSD, and an 8-Core / 16-Thread processor.
* **Networking & Security:** Assumed guests need internet access while avoiding IP/MAC conflicts on the campus network.

---

## 3. What I did

1. Audited host hardware capacity (32 GB RAM / 1024 GB SSD / 8 Cores 16 Threads) to establish optimal resource allocations.
2. Downloaded official ISOs for Ubuntu Server LTS and Windows 11 Enterprise Evaluation.
3. Provisioned two virtual machines in VMware Workstation Pro with customized hardware parameters.
4. Configured NAT virtual networking to ensure inter-guest communication and external internet connectivity while bypassing campus MAC filtering.
5. Resolved Windows Firewall ICMP blocking to restore ping connectivity between guests.
6. Created clean baseline snapshots for instant rollback.
7. Validated the guide by having a peer execute `BUILD.md` on a secondary machine.

### Resource Allocation Strategy
* **Host Total Capacity:** 32 GB RAM, 1024 GB SSD (1 TB), 8 CPU Cores / 16 Logical Threads.
* **Ubuntu Server Guest:** 8 GB RAM, 4 vCPUs, 25 GB Thin Disk (Optimized for backend dev & compilation workloads).
* **Windows 11 Evaluation Guest:** 4 GB RAM, 2 vCPUs, 64 GB Thin Disk (Configured for lightweight client/GUI testing).
* **Host Overhead & Budgeting:** 12 GB RAM total and 6 vCPUs allocated across both VMs. The host OS retains 20 GB RAM (~62.5% of total capacity) and 10 logical CPU threads, eliminating disk swapping and ensuring seamless host performance.

### Justification of Engineering Decisions (Trade-offs & Alternatives)

| Decision Area | Chosen Setting | Alternative Considered | Trade-off Analysis & Justification |
| :--- | :--- | :--- | :--- |
| **Networking Mode** | **NAT** | Bridged Networking | **Trade-off:** Bridged assigns a direct campus IP but is blocked by campus MAC filtering rules. NAT shares the host's IP, bypassing MAC filtering while providing full internet and inter-VM connectivity. |
| **Disk Provisioning** | **Thin Provisioning** | Pre-allocated Disk | **Trade-off:** Pre-allocated disks offer slightly higher I/O performance but instantly consume 89 GB on the host SSD. Thin provisioning grows dynamically up to set limits (25 GB Ubuntu / 64 GB Win11), conserving host storage. |
| **Resource Allocation** | **8 GB RAM / 4 vCPU (Ubuntu) & 4 GB RAM / 2 vCPU (Win11)** | Equal 4 GB / 2 vCPU Allocation or Overcommitting Host | **Trade-off:** Allocating 8 GB RAM and 4 vCPUs to Ubuntu Server prioritizes backend compilation and container workloads, while 4 GB RAM and 2 vCPUs for Windows 11 Eval is sufficient for client GUI testing. The combined VM footprint (12 GB RAM / 6 vCPUs) leaves 20 GB RAM and 10 CPU threads for the host. |
| **Firewall Rule** | **Inbound ICMPv4 Allow** | Disabling Firewall Entirely | **Trade-off:** Disabling the firewall compromises guest security. Explicitly enabling the `File and Printer Sharing (Echo Request - ICMPv4-In)` rule allows ping diagnostic traffic safely. |

---

## 4. Evidence
* [01-new-vm-wizard.png](evidence/01-new-vm-wizard.png) - Proves correct virtual machine creation and OS mapping in the VMware configuration wizard.
* [02-disk-settings.png](evidence/02-disk-settings.png) - Verifies the virtual disk provisioning bounds to prevent host storage overflow.
* [03-network-adapter.png](evidence/03-network-adapter.png) - Proves the selection of the NAT network adapter in VMware settings for isolated external access.
* [04-guest-uname.txt](evidence/04-guest-uname.txt) - Pure text terminal log verifying the exact running kernel architecture of the Ubuntu guest.
* [05-ping-between-guests.txt](evidence/05-ping-between-guests.txt) - Raw ICMP output proving full network visibility and link layer connectivity between the Ubuntu and Windows 11 guests.
* [06-snapshot-list.png](evidence/06-snapshot-list.png) - Proves a baseline snapshot was captured in VMware for instant rollback in case the dev breaks the system.
* [07-reproduction-log.txt](evidence/07-reproduction-log.txt) - Plain text verification log proving that a second user successfully executed `BUILD.md` on another machine (To be done soon).

## 5. What I would do differently
Manually documenting a GUI-based installation in VMware for non-technical office staff does not scale well and is prone to human error. In a production setting, I would write an automated Vagrantfile or a script using HashiCorp Packer. This would allow spinning up the exact same twin-sandbox environment using a single command (`vagrant up`), completely removing the hypervisor complexity for the end-user.