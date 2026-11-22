# Ticket 2: four builds

> Prices are in [budget.csv](budget.csv), taken on 2026-11-22 (source and vendor for every row). Screenshots of the prices are in evidence/vendor-quotes/.
> Numbers marked "estimate" are my estimates, not measurements.

**Common platform for all four: AMD AM5 with DDR5.** One socket and one memory type means any machine can take a newer CPU or more memory later, which is what "last a few years" needs. The board (MSI PRO B650M-P) has 4 memory slots and 2 M.2 slots. The cheaper DDR4 / AM4 platform has no upgrade path left.
Every machine has: at least 16 GB memory, a 1 TB NVMe SSD, a licensed Windows 11 Pro and one monitor.
Compatibility checked as in Lab 3: CPU socket and board socket (AM5), board memory type (DDR5), M.2 slot and NVMe SSD, total power and power supply. See [evidence/pcpartpicker-links.md](evidence/pcpartpicker-links.md).

**Price finding that shaped every build:** on the day I checked, a 32 GB DDR5 kit (from 289 119 KZT) cost almost three Ryzen 5 7600 (99 927 KZT), and 16 GB (169 335 KZT) was already more than the CPU. The Kingston 32 GB price rose from 230 168 to 332 235 during the work; the developer and designer use a cheaper Apacer kit and the sales machine has 16 GB. Memory is the most expensive part of the machine, so every decision about memory is a decision about money.

## Build 1: accountant

**Load:** accounting software, Excel with large files, a browser, mail. Eight hours a day, one or two things at a time, no video editing and no virtual machines.

**Bottleneck:** not the CPU or GPU, they idle. What fails here is reliability and the screen: a crash loses a day of work, and eight hours of reading numbers needs a comfortable IPS monitor and a backup.

**Trade-off:** Ryzen 5 7600 with integrated graphics (no graphics card) and 16 GB instead of 32 GB. 32 GB would cost about 120 000 KZT more and this load does not use it. I kept a decent IPS monitor instead of the cheapest one.
**Alternative not taken:** 8 GB of memory. Rejected, the hard requirement is 16 GB, and Windows plus accounting software plus Excel would swap on 8 GB.

**Parts:** Ryzen 5 7600 BOX, MSI PRO B650M-P, 16 GB DDR5-6000 (2 x 8 GB), Kingston NV3 1 TB, MSI MAG A550BN 550 W, FSP CMT223S case, LG 24MR400-B 23.8 inch IPS, Windows 11 Pro.
**Power check:** estimate about 100 to 150 W under load, so a 550 W unit has a large margin.
**Total:** 599 799 KZT (budget.csv, accountant rows)

## Build 2: sales manager

**Load:** CRM in the browser, 20 to 30 tabs, Excel, several hours of video calls a day, often all at once (estimate).

**Bottleneck:** memory. Browser tabs plus a video call plus Excel can use around 10 GB or more (estimate), and on 16 GB the margin is small. A 32 GB kit costs about 120 000 KZT more than 16 GB, which the budget could not take. When memory runs out the machine starts swapping, and that happens in the middle of a customer call. The CPU is mostly idle.

**Trade-off:** same Ryzen 5 7600 as the accountant and 16 GB (2 x 8 GB). 32 GB would cost about 120 000 KZT more at today's price, so I accepted a small memory margin and saved on the monitor: a 23.8 inch Philips at 36 594 KZT. The board keeps 2 free slots, so memory can be added later.
**Alternative not taken:** 32 GB. Rejected on price. A Ryzen 7 with 16 GB was also rejected: the CPU would not be the problem, memory would.

**Parts:** Ryzen 5 7600 BOX, MSI PRO B650M-P, 16 GB DDR5-6000 (2 x 8 GB), Kingston NV3 1 TB, MSI MAG A550BN 550 W, FSP CMT223S case, Philips 24E2N1110 23.8 inch IPS, Windows 11 Pro. A webcam and headset are not included (see README, section 2).
**Power check:** estimate about 100 to 150 W.
**Total:** 584 193 KZT (budget.csv, sales rows)

## Build 3: web developer

**Load:** code editor, a browser with many tabs, Docker containers or virtual machines, a local database, sometimes several of these at once (estimate).

**Bottleneck:** memory first, then storage speed. Containers and a virtual machine each hold their own memory, and the database and build tools read and write a lot. After that comes the CPU, when builds run.

**Trade-off:** Ryzen 7 7700 OEM without cooler (8 cores for builds and VMs; a separate AM5 cooler is added), 32 GB of memory in 2 of 4 slots so it can grow to 64 GB later, and a 27 inch QHD monitor for more screen space. I accepted no graphics card, because the developer does not need a GPU.
**Alternative not taken:** a Ryzen 9. Rejected, it costs much more and the extra cores would be used only a small part of the day. I also considered Ubuntu instead of Windows to save the licence (102 659 KZT), but the company may need Windows-only tools, so Windows 11 Pro with WSL2 is the safer choice.

**Parts:** Ryzen 7 7700 OEM + ID-COOLING SE-802-SD cooler, MSI PRO B650M-P, 32 GB DDR5-5200 (2 x 16 GB, Apacer Panther), Kingston NV3 1 TB, MSI MAG A550BN 550 W, FSP CMT223S case, Iiyama ProLite XB2797QSU-B1 27 inch QHD IPS, Windows 11 Pro.
**Power check:** estimate about 150 to 200 W.
**Total:** 759 629 KZT (budget.csv, web developer rows)

## Build 4: designer

**Load:** large images or video, layers, colour work. Eight hours a day, mostly in one heavy application.

**Bottleneck:** the screen first (colour accuracy decides whether the work is correct), then the graphics card and memory, then the SSD for big files.

**Trade-off:** the budget goes to a 27 inch QHD IPS monitor with 99% sRGB (116 074 KZT) and a GeForce RTX 5060 8 GB (189 832 KZT). The CPU is a Ryzen 7 7700 and not a faster one, the SSD is 1 TB, the minimum, and the monitor is not a calibrated professional model with wide gamut. The other three machines get less so this one can have a GPU.
**Alternative not taken:** a cheaper monitor with a stronger graphics card. Rejected: a fast card on a screen with wrong colours gives fast but wrong results.

**Parts:** Ryzen 7 7700 OEM + ID-COOLING SE-802-SD cooler, MSI PRO B650M-P, 32 GB DDR5-5200 (2 x 16 GB, Apacer Panther), Kingston NV3 1 TB, INNO3D RTX 5060 Twin X2 8 GB, PCCooler YK650 650 W, FSP CMT223S case, ASUS BE27AQ 27 inch QHD IPS, Windows 11 Pro.
**Power check:** estimate about 250 to 350 W with the graphics card; the card maker recommends a 550 W unit, so 650 W has a margin.
**Total:** 970 011 KZT (budget.csv, designer rows)

## Grand total

| Build | Total, KZT |
|---|---|
| Accountant | 599 799 |
| Sales | 584 193 |
| Web developer | 759 629 |
| Designer | 970 011 |
| **Grand total** | **2 913 632 (budget 3 000 000, margin 86 368)** |

The margin is only 86 368 KZT, about 2.9% of the budget. It covers no keyboards, mice, UPS or cables.
