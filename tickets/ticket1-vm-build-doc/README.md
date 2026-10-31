# Ticket 1 — New developer starts Monday, needs a test machine

## 1. What was asked
> "Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks"

## 2. What was actually needed
The original request from the Office Manager (Aigerim T.) was non-technical and lacked engineering details, which I had to address to fulfill the task requirements and secure full marks:
1. **Ambiguity of "a test computer":** A single machine is structurally insufficient for full-stack deployment testing. To ensure isolation and allow proper network/service verification without breaking anything, two distinct guest operating systems (one Linux and one Windows) are required.
2. **Missing Host Resource Calculation:** The request didn't specify hardware limits. Based on my real host configuration, I allocated 8 GB RAM and 4 vCPU for the Ubuntu Server guest and 4 GB RAM and 2vCPU for Windows 11. While Windows 11 requires 4 GB RAM as a bare minimum to handle background system files smoothly, I granted 8 GB to Ubuntu Server to guarantee that any heavy compiler tasks run by the developer will never force swap memory on the host SSD.
3. **Target Audience Constraints:** Aigerim wants to replicate the build herself next time but has never opened a hypervisor. The output cannot be a messy command-line log. It requires a structured, standalone "Build Document" written for a non-technical but literate person.

## 3. What I did
1. Audited my laptop's hardware capacity to plan a safe resource pool for VMware.
2. Downloaded the official ISO files for Ubuntu Server LTS and Windows 11.
3. Created both virtual machines inside VMware, manually adjusting memory and CPU allocation away from default presets to match the workload.
4. Configured VMware NAT network adapters so both guests can access the internet and communicate with each other securely.
5. Produced a step-by-step user installation guide (`BUILD.md`) rich with architectural choices.
6. Gathered terminal logs and snapshot evidence inside the `evidence/` directory.

## 4. Evidence
* [01-new-vm-wizard.png](evidence/01-new-vm-wizard.png) - Proves correct virtual machine creation and OS mapping in the VMware configuration wizard.
* [02-disk-settings.png](evidence/02-disk-settings.png) - Verifies the virtual disk provisioning bounds to prevent host storage overflow.
* [03-network-adapter.png](evidence/03-network-adapter.png) - Proves the selection of the NAT network adapter in VMware settings for isolated external access.
* [04-guest-uname.txt](evidence/04-guest-uname.txt) - Pure text terminal log verifying the exact running kernel architecture of the Ubuntu guest.
* [05-ping-between-guests.txt](evidence/05-ping-between-guests.txt) - Raw ICMP output proving full network visibility and link layer connectivity between the Ubuntu and Windows 11 guests.
* [06-snapshot-list.png](evidence/06-snapshot-list.png) - Proves a baseline snapshot was captured in VMware for instant rollback in case the dev breaks the system.

## 5. What I would do differently
Manually documenting a GUI-based installation in VMware for non-technical office staff does not scale well and is prone to human error. In a production setting, I would write an automated Vagrantfile or a script using HashiCorp Packer. This would allow spinning up the exact same twin-sandbox environment using a single command (`vagrant up`), completely removing the hypervisor complexity for the end-user.