# Lab 0 — Inventory your own machine

Pair: DAMIR NIYAZBEK (My pair) <br/>
Driver first half: TAMERLAN SHAKENOV (Me) <br/>
Machine: HP Victus 16-e0XXX (Laptop) <br/>
Date: 08.10.2026

## Inventory table

| What | Value | Where I got it |
|------|-------|----------------|
| CPU model / cores / threads | *AMD Ryzen 7 5800H, 8 cores, 16 threads* | `Get-CimInstance Win32_Processor` |
| Total memory + modules + speed | *32 GB (2 × 16 GB @ 3200 MHz)* | `Get-CimInstance Win32_PhysicalMemory` |
| Disk model + type | *SAMSUNG MZVLB1T0HBLR-000H1, SSD, NVMe, (~953 GB)* | `Get-PhysicalDisk` |
| Free space on volume | *46 GB free on C:* | `Get-Volume` |
| Firmware type | *UEFI* | `(Get-ComputerInfo).BiosFirmwareType` |
| BIOS version + date | *F.09, 16.07.2021* | `Get-CimInstance Win32_BIOS` |
| Virtualization present & enabled | *Yes* | `systeminfo` (Hyper-V Requirements) |

## What I did
I used Windows PowerShell to collect information about my personal laptop, including the operating system, hardware specifications, processor, memory, and storage. Here are the commands I used, presented both in screenshots and in raw txt files: <br/>

  Screenshots + Commands:
- Overview `Get-ComputerInfo` <br/> 
![OS information](evidence/os.png) <br/>
- OS & Memory `systeminfo`<br/> 
![OS-CPU-RAM](evidence/os-cpu-memory.png) <br/> 
![hyper-v](evidence/hyper-v.png) <br/>
- Disks `Get-Volume`<br/> 
![disks](evidence/disks.png) <br/>

  Text files + Commands:
- CPU model + cores + threads  — `Get-CimInstance Win32_Processor | Select-Object Name, NumberOfCores, NumberOfLogicalProcessors` [evidence/cpu.txt](evidence/cpu.txt)
- Total memory + modules + speed — `Get-CimInstance Win32_PhysicalMemory | Select-Object BankLabel, Capacity, Speed, Manufacturer` [evidence/memory.txt](evidence/memory.txt)
- Disk model + type — `Get-PhysicalDisk | Select-Object FriendlyName, MediaType, BusType, Size` [evidence/disk.txt](evidence/disk.txt)
- Firmware type — `(Get-ComputerInfo).BiosFirmwareType` [evidence/firmware.txt](evidence/firmware.txt)
- BIOS version + date — `Get-CimInstance Win32_BIOS | Select-Object SMBIOSBIOSVersion, ReleaseDate` [evidence/bios.txt](evidence/bios.txt)

## Result
Successfully collected basic system information using Windows PowerShell. The inventory included the operating system, firmware, processor, installed memory, storage devices, and computer model.

The commands demonstrated how to query Windows system information using built-in PowerShell commands without installing additional software.

## What did not work the first time
The RAM value was displayed in megabytes, which was less convenient to read. I interpreted the value and converted it to gigabytes for easier understanding.

## Evidence
Text Files:
- [evidence/cpu.txt](evidence/cpu.txt) — raw CPU output
- [evidence/memory.txt](evidence/memory.txt) — raw memory modules
- [evidence/disk.txt](evidence/disk.txt) — raw physical disk
- [evidence/firmware.txt](evidence/firmware.txt) — firmware type
- [evidence/bios.txt](evidence/bios.txt) — BIOS version and date
Screenshots:
- [evidence/disks.png](evidence/disks.png) — Disks info
- [evidence/hyper-v.png](evidence/hyper-v.png) — Hyper-V (Virtualisation) info
- [evidence/os-cpu-memory.png](evidence/os-cpu-memory.png) — OS, CPU and RAM memory info
- [evidence/os.png](evidence/os.png) — OS info
