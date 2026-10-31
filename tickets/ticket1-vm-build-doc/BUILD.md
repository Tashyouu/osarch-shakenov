# VMware Workstation Build Document

This guide is designed for non-technical users to safely spin up a twin-sandbox test environment (Ubuntu Linux & Windows 11) using VMware Workstation.

## Step 0: Prerequisites & Software Installation
Before configuring the virtual environments, you must download and install the hypervisor software, then fetch the operating system installer files (ISO images). 

### 1. Install the Hypervisor (VMware Workstation Pro)
* Go to the [Official Broadcom Product Announcement](https://vmware.com) and download the VMware Workstation Pro installer for your host operating system. *(Note: As of November 2024, VMware Workstation is completely free for personal, educational, and commercial use).*
* Launch the downloaded installer executable (`.exe` on Windows or `.dmg` on macOS).
* Follow the installation wizard prompts (accept the license agreement, choose the default installation path, and click **Finish**). 
* Restart your host laptop if prompted by the installer to properly initialize virtual network drivers.

### 2. Download Operating System ISO Images
Download the following installation files and save them into a dedicated folder on your hard drive. **Crucial:** Do not attempt to double-click, open, or extract these files; VMware will read them as virtual discs automatically.
* **Ubuntu Server LTS ISO:** Download the 64-bit Linux server distribution file from the [Ubuntu Server Download Portal](https://ubuntu.com).
* **Windows 11 Enterprise ISO:** Download the 90-day fully-featured evaluation image from the [Microsoft Evaluation Center](https://microsoft.com).

---

## Step 1: Base Configuration & Initialization
Once VMware Workstation is successfully installed and your ISO files are ready, launch the VMware application from your desktop shortcut to begin building the guests.

1. In the main VMware window, click on **Create a New Virtual Machine** (or press `Ctrl+N`).
2. Choose the **Typical (recommended)** configuration type and click **Next**.
3. Select **Installer disc image file (iso)**, click **Browse**, and navigate to your downloaded Ubuntu Server ISO. Click **Next**.
4. Name the machine clearly as `lab01-ubuntu`. Allocate **8192 MB (8 GB) RAM** and **4 CPU cores** away from the default presets to match development workloads.
5. Repeat this exact process for the second machine: select the Windows 11 ISO, name it `lab02-windows`, and allocate **4096 MB (4 GB) RAM** with **2 CPU cores**.

Once both creation processes are complete, you will see both functional guest environments initialized inside your VMware library sidebar and ready for their first clean boot, as shown below: <br>
![](evidence/01-new-vm-wizard.png)



## Step 2: Storage Allocation
1. Set the virtual disk size to **25 GB** for Ubuntu.
2. Set the virtual disk size to **64 GB** for Windows 11.

*Architectural Choice:* Windows 11 has strict storage requirements (minimum 64 GB) to handle system files, temporary storage, and cumulative Windows Updates comfortably without triggering system crashes. A server Ubuntu environment, however, operates efficiently within a tight 25 GB partition under typical development testing loads.
*(Below is an example of the storage configuration interface captured from the lab02-windows setup)* <br>
![](evidence/02-disk-settings.png)

## Step 3: Networking Configuration
1. Open the virtual machine settings in VMware and select **Network Adapter**.
2. Strictly select the **NAT** (Network Address Translation) option. <br>
![](evidence/03-network-adapter.png)

*Why NAT and not Bridged:* The university teaching network filters connected devices strictly by physical MAC address. A bridged setup forces the virtual machine to broadcast its own unique MAC address to the local network switch, which results in an immediate internet lockout by the campus firewall. By using VMware's NAT engine, the guest machines share your host laptop's authorized IP and MAC address, gaining seamless internet access without network conflicts.

## Step 4: Environment Verification
1. Boot both machines to confirm they can reach the internet and see each other.
2. Take a system snapshot in VMware to save the clean state.
*(Below is the baseline snapshot tree verified via the Snapshot Manager for the Windows instance)* <br>
![](evidence/06-snapshot-list.png)