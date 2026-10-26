# Lab 2 — A Windows guest, and a network between them

Pair: None </br>   
Driver first half: Tamerlan Shakenov (Me) </br>
Machine: HP Victus 16-e0XXX (Laptop), Windows 10 Professional, 32 GB RAM, 8 cores 16 threads, VMware Workstation Pro, guest Ubuntu Server 26.04.1 LTS + Windows 11 Enterprise eval. </br>
Date: 27.10.2026 </br>

## What I did

1. Downloaded Windows 11 Enterprise evaluation. Licence: https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise

2. Created lab02-windows: 2 vCPU, 4 GB, 64 GB thin, UEFI + Secure Boot, vTPM.

3. Configured network mode to VMware NAT (VMnet8) for both guests to keep VMs in an isolated local network while still allowing them to share the host's internet connection. Checked IP configuration on Ubuntu Server guest:
```bash
shakenov@lab01-shakenov:~\$ ip -br a
lo               UNKNOWN        127.0.0.1/8 ::1/128 
ens33            UP             192.168.122.129/24 fe80::20c:29ff:fe47:4955/64 
```

4. Discovered that Ping Ubuntu → Windows timed out because Windows Defender Firewall blocks incoming ICMP echo requests by default on public network profiles. Executed the rule command inside administrative PowerShell on the Windows guest:
```powershell
PS C:\Windows\system32> New-NetFirewallRule -DisplayName "Allow ping in (ICMPv4)" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow

Name                  : {9a25b340-9a3c-4cf2-8356-9d6cbf7d86f7}
DisplayName           : Allow ping in (ICMPv4)
Enabled               : True
Profile               : Any
Platform              : Any
Direction             : Inbound
Action                : Allow
```

5. Verified connectivity by launching an automated verification check from Git Bash via SSH:
```bash
ssh shakenov@192.168.122.129 "ping -c 4 192.168.122.130; ping -c 4 ubuntu.com" >> ping-both-ways.txt
```

6. Set up memory overcommitment by pushing both guests' RAM profiles to 20480 MB (20 GB) each on a 32 GB host system, then generated active load inside the Ubuntu terminal to test resource constraints:
```bash
shakenov@lab01-shakenov:~\$ sudo apt update
Hit:1 http://ubuntu.com resolute InRelease
Get:2 http://ubuntu.com resolute-security InRelease [137 kB]
Fetched 3,101 kB in 3s (1,086 kB/s)
43 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

## Network diagram
              Internet
                 |
            Host laptop
                 |
     [VMware NAT · VMnet8 · 192.168.122.0/24]
          /                        \
    lab01-ubuntu                  lab02-windows
    192.168.122.129               192.168.122.130

## Result

| Metric | Before | After |
|---|---|---|
| Host RAM in use | 8.4 GB | 30.1 GB |
| Available | 22.9 GB | 1.2 GB |
| Committed / Swap used | 10.7 GB / 36.2 GB | 36.4 GB / 39.1 GB |

### What I noticed
The host operating system resource footprint experienced a major degradation curve when physical RAM consumption maximized at 96%. Windows initialized rapid disk allocation extensions within the system pagefile structure, scaling active allocation metrics to 36.4 GB out of 39.1 GB. Interface interactivity presented heavy micro-stuttering behaviors and window responsiveness latency on the host desktop environment, though the underlying kernel stayed operational without entering a total freezing crash.

## What did not work the first time

* **Symptom:** Executing target output redirect scripts encountered shell subsystem errors stating `No such file or directory`.
* **Root Cause:** Terminal working contexts were nested within the active remote SSH terminal directory inside the Ubuntu subsystem filesystem instead of utilizing the local path context structure managed by the host environment.
* **Resolution:** Terminated the active SSH subshell tracking profile with an `exit` procedure, verified path layouts inside native Git Bash environments, and issued directory-relative targets directly using native host shell variables.

## Evidence

- [evidence/ipconfig-windows.txt](evidence/ipconfig-windows.txt) — Proves the assigned IPv4 interfaces and local subnet boundaries on the Windows guest configuration.
- [evidence/ip-addr-linux.txt](evidence/ip-addr-linux.txt) — Proves the assigned network adapter configuration and native address structures for the active Ubuntu host.
- [evidence/ping-both-ways.txt](evidence/ping-both-ways.txt) — Proves bi-directional echo resolution over host-to-guest subnets alongside functional internet routing metrics.
- [evidence/host-memory-before.png](evidence/host-memory-before.png) — Visual record of host system performance monitors displaying standard resource states with virtual machines stopped.
- [evidence/host-memory-after.png](evidence/host-memory-after.png) — Visual record confirming host resource saturation, paging exhaustion, and active hardware constraints under total thread overcommit.
