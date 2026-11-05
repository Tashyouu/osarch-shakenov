# Ticket 1 — New developer starts Monday, needs a test machine

## 1. What was asked

> "Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks"

---

## 2. What was actually needed

### What the request left out
The original request from the Office Manager (Aigerim T.) was non-technical and omitted several critical engineering specifications:

* **Operating System Requirements:** It did not specify which operating system(s) or development stack the new employee needs (Linux server, Windows client, both, containers, etc.).
* **Hardware & Resource Limits:** It omitted host capacity constraints (available RAM, CPU cores/threads, free SSD space).
* **Network Scope:** It did not clarify whether the environment requires external internet access, isolated internal-only networking, or both, and how it should interact with the campus network.
* **Recovery expectations:** It did not define what “without breaking anything” actually means in practice (rollback mechanism, clean baseline, documentation of the restore process).

### Where the request was wrong
* **"Nothing special":** A usable developer test environment in 2026 is not a single basic VM. It requires a properly structured dual-OS sandbox (Ubuntu Server for backend/tooling + Windows 11 for client-side testing) with correct resource allocation, networking that works on the campus network, and a documented recovery path.
* **"Without breaking anything":** Simply installing a virtual machine does not protect state. Achieving a safe testing sandbox requires baseline snapshots and an explicit, documented rollback procedure. Without that, the first failed experiment forces a full reinstall.

### What I assumed instead
* **Stack:** A standard dual-environment stack — Ubuntu Server LTS (backend, networking labs, compilation) + Windows 11 Enterprise Evaluation (frontend/client GUI testing).
* **Host Hardware Budget:** A workstation with 32 GB RAM, 1 TB SSD, and an 8-core / 16-thread processor (based on the machine used for Lab 0).
* **Networking & Security:** Guests need internet access while avoiding IP/MAC conflicts on the university network that filters by physical MAC address. Therefore NAT was chosen over Bridged.
* **Recovery model:** Clean baseline snapshots must exist before any testing begins so the environment can be returned to a known-good state in under a minute.

### Questions I would have asked Aigerim
1. Which operating systems and tools does the new developer actually need on day one (Ubuntu only, Windows only, both, Docker, specific IDEs)?
2. What is the exact host machine that will run the VMs (RAM, free disk space, CPU)?
3. Does the test environment need internet access, and must it work on the campus network without triggering MAC filtering?
4. How critical is quick recovery after a broken experiment? Do we need documented snapshots and a one-click rollback procedure?
5. Are we allowed to use evaluation/trial licences (Windows 11 Enterprise 90-day eval), or must everything be fully licensed?

### Licences and reader
* VMware Workstation Pro is free for personal, educational and commercial use (no licence key required as of 2024/2025 policy change by Broadcom).
* Windows 11 Enterprise Evaluation is a free 90-day fully-featured trial from Microsoft Evaluation Center. After 90 days it enters a reduced-functionality state.
* Ubuntu Server LTS is free and open source.
* The primary reader of `BUILD.md` is a non-technical person (Office Manager). Therefore the guide is written as a step-by-step procedure with screenshots and explicit commands rather than as an engineer’s notes.

---

## 3. What I did

1. Audited host hardware capacity (32 GB RAM / ~1 TB SSD / 8 cores 16 threads) and free space on the system drive.
2. Downloaded official ISOs for Ubuntu Server LTS and Windows 11 Enterprise Evaluation.
3. Provisioned two virtual machines in VMware Workstation Pro with customized hardware parameters (UEFI + vTPM for Windows 11).
4. Configured NAT virtual networking so both guests share the host’s authorised IP/MAC and obtain internet access without campus MAC filtering issues.
5. Installed both operating systems and verified inter-guest + internet connectivity.
6. Enabled the required Windows Defender Firewall rule for ICMPv4 so ping works in both directions.
7. Created clean baseline snapshots for instant rollback.
8. Documented the entire process in `BUILD.md` so a non-technical person can repeat it.

### Resource Allocation Strategy
* **Host Total Capacity:** 32 GB RAM, ~1 TB SSD, 8 CPU cores / 16 logical threads.
* **Ubuntu Server Guest:** 8 GB RAM, 4 vCPUs, 25 GB thin-provisioned disk.
* **Windows 11 Evaluation Guest:** 4 GB RAM, 2 vCPUs, 64 GB thin-provisioned disk (Windows 11 minimum is 64 GB).
* **Host Overhead:** Combined guest footprint is 12 GB RAM and 6 vCPUs. The host retains ~20 GB RAM and 10 logical threads, which keeps the host responsive.

### Justification of Engineering Decisions (Trade-offs & Alternatives)

| Decision Area | Chosen Setting | Alternative Considered | Trade-off Analysis & Justification |
| :--- | :--- | :--- | :--- |
| **Networking Mode** | **NAT** | Bridged Networking | Bridged would give each guest its own campus IP and MAC. The university network filters by physical MAC address and blocks unknown devices. NAT makes the guests share the host’s already-authorised IP/MAC, so internet works and inter-VM communication is still possible. |
| **Disk Provisioning** | **Thin Provisioning** | Pre-allocated (thick) disks | Pre-allocated disks would instantly consume 89 GB. Lab 0 previously showed only ~46 GB free on the host system drive. Thin provisioning grows on demand up to the configured limits and avoids filling the drive immediately. Additionally, I have updated the Lab 0 up-to-date, providing the current free disk space (~220 GB) |
| **Resource Allocation** | **8 GB / 4 vCPU (Ubuntu) + 4 GB / 2 vCPU (Windows)** | Equal split or heavier overcommit | Ubuntu receives more resources because backend compilation and container work are heavier. Windows 11 Evaluation with 4 GB RAM is sufficient for light client/GUI testing. Combined allocation leaves comfortable headroom for the host. |
| **Firmware & Security (Windows 11)** | **UEFI + vTPM enabled** | Legacy BIOS / no TPM | Windows 11 requires UEFI and TPM 2.0. Without them the installer refuses to proceed or the evaluation image runs with limitations. |
| **Firewall Rule** | **Enable ICMPv4 Echo Request only** | Disable entire Windows Firewall | Disabling the firewall is insecure. Enabling only the “File and Printer Sharing (Echo Request – ICMPv4-In)” rule allows diagnostic pings while keeping the rest of the firewall active. |

---

## 4. Evidence

* [01-new-vm-wizard.png](evidence/01-new-vm-wizard.png) — Proves the New Virtual Machine Wizard was used and the correct ISO was selected.
* [02-disk-settings.png](evidence/02-disk-settings.png) — Verifies the virtual disk size and thin-provisioning settings for both guests.
* [03-network-adapter.png](evidence/03-network-adapter.png) — Confirms NAT was selected for the network adapter.
* [04-guest-uname.txt](evidence/04-guest-uname.txt) — Full terminal output of `uname -a` and `lsb_release -a` proving the Ubuntu guest is running the expected kernel and distribution.
* [05-ping-between-guests.txt](evidence/05-ping-between-guests.txt) — ICMP results showing connectivity in both directions (Ubuntu ↔ Windows) plus successful internet reachability from both guests.
* [06-snapshot-list.png](evidence/06-snapshot-list.png) — Snapshot Manager showing the clean baseline snapshot exists for both machines.
* [07-reproduction-log.txt](evidence/07-reproduction-log.txt) — Planned peer reproduction log (not yet completed).

---

## 5. What I would do differently

Manually documenting a GUI-based installation in VMware for non-technical staff does not scale and is error-prone. In a production or team setting I would replace the entire procedure with a single `Vagrantfile` (or a Packer template) that builds the identical twin-sandbox environment with one command (`vagrant up`). That removes the need for the end-user to interact with the hypervisor UI at all and guarantees bit-for-bit reproducibility.