# Ticket 2: Kitting out the new office

## 1. What was asked

> From: Daniyar K. (Director)
> Subject: computers for the new office
>
> We are opening the second office and need four computers: accountant, sales, web developer, designer. Budget 3,000,000 tenge for everything. Everyone says we should just buy the same four machines, is that right? Make sure they last a few years. Monitors included.

## 2. What was actually needed

**Four identical machines is wrong.** The four jobs load the computer in four different ways, so the same money has to go to a different part in each machine.

- The accountant mostly needs reliability, a good screen and a licensed Windows for accounting software. Their CPU and GPU sit idle.
- The sales manager lives in a browser with many tabs and video calls, so memory runs out first.
- The web developer runs a code editor, containers or virtual machines, a local database and several browsers at once, so memory and SSD speed run out first.
- The designer works with large images or video and needs accurate colours, so the money goes to the monitor, and to GPU and memory.

One identical machine would be too weak for the developer and designer, and wasted money for the accountant.

**What the request left out**

1. Does 3,000,000 tenge cover only the computers and monitors, or also the operating system, keyboards, mice, cables and a UPS? The request only says "monitors included".
2. How long is "a few years"? This decides the platform: a socket and memory type that can take more memory or a newer CPU later.
3. Which software does each person run? Accounting software in Kazakhstan often needs Windows. A designer's tools may need a certain GPU.
4. Do the four machines have to be delivered, and by when? Prices have to come from a vendor that delivers to Kazakhstan.
5. Does the office already have monitors, network and desks?

**What I would ask Daniyar, and what I assumed instead**

| Question | Assumption I used |
|---|---|
| Does the budget include OS and accessories? | Yes. The 3,000,000 tenge includes monitors and licensed Windows. Keyboards, mice, webcams, a UPS and cables are not in the totals. budget.csv lists only the machine, the monitor and the operating system, so these are an open question for the director. |
| How many years is "a few"? | 5 years. So I chose a platform with an upgrade path (AM5 socket, DDR5 memory). |
| Which software? | Accountant: Windows-only accounting software. Others: browser, office suite and the tools listed in builds.md. |
| Delivery | Only vendors that deliver in Kazakhstan, each price dated. |
| Existing equipment | None, everything is bought new. |

## 3. What I did

1. Went from role to load to bottleneck to trade-off (see [builds.md](builds.md)), and only then picked parts.
2. Checked compatibility as in Lab 3: CPU socket and board socket, memory type, M.2 slot and NVMe SSD, total power and power supply. PCPartPicker was used only for compatibility and power. It does not price in tenge.
3. Took every price in tenge from a Kazakhstan vendor listing (mostly Flexcom.kz, found through the e-katalog.kz and alfa.kz price pages), with the date, and saved a dated screenshot (see [budget.csv](budget.csv)).
4. Every machine has at least 16 GB memory, a 1 TB SSD, a licensed operating system and a monitor.
5. The biggest finding was the price of memory: a 32 GB DDR5 kit (from 332 235 KZT) cost more than three Ryzen 5 7600 on the day I checked, so memory decisions decided the budget. The prices moved while I worked: my first 32 GB price was 230 168 KZT, and I had to rebuild the budget around the new one. The margin left is only 87 220 KZT.

**Totals (from budget.csv, prices taken 2026-11-22)**

| Role | Total, KZT |
|---|---|
| Accountant | 556 841 |
| Sales | 541 235 |
| Web developer | 802 161 |
| Designer | 1 012 543 |
| **Grand total** | **2 912 780 (budget 3 000 000, margin 87 220)** |

### Answer 1: which machine do I upgrade first, in 18 months?

Web developer, the memory. The board has four memory slots and only two are used, so I can add a second 2 x 16 GB kit and reach 64 GB without throwing anything away (a 32 GB kit now costs 332 235 KZT or more, so adding a second kit later is a real cost, but it needs no new board or CPU). Containers, virtual machines and a local database are the load that grows fastest, and on this machine memory is the first resource to run out. The AM5 platform and DDR5 chosen in section 2 are what make this a simple add-on.

### Answer 2: the same four as pre-built machines

I could not get verified pre-built prices for four comparable machines on 2026-11-22: the Kazakhstan vendor pages I could reach showed either gaming machines with a different part mix or prices I could not confirm, and I will not put a made-up number in the table. The comparison below is therefore about what changes, not about the exact price gap.

What the company gains by buying pre-built: one seller who tests and delivers the machine, a single warranty and support contact for the whole computer, and no assembly time for me (four machines by hand is hours of work).
What it loses: choice of parts and the upgrade path. The pre-built listing I looked at on alfa.kz (item 8583273) had one 16 GB memory module (single channel, so no dual-channel memory), an entry-level A620 board and a graphics card nobody asked for, which is a typical way pre-builts save money in the wrong place for an office. It also loses control over which licensed Windows is included and whether the monitor is included.
**Pre-built prices (fill in from the vendor page, same day, with a screenshot named prebuilt-*.png):**

| Role | Pre-built model (vendor) | Price, KZT | Date | Link | Built-by-me price (budget.csv) |
|---|---|---|---|---|---|
| Accountant | TODO | TODO | 2026-11-22 | TODO | 556 841 |
| Sales | TODO | TODO | 2026-11-22 | TODO | 541 235 |
| Web developer | TODO | TODO | 2026-11-22 | TODO | 802 161 |
| Designer | TODO | TODO | 2026-11-22 | TODO | 1 012 543 |

The price difference is the one thing the text above does not answer. The exact quote must be added from a vendor listing with a date before this report can be called complete.

### Answer 3: who complains about performance within a month, and about what?

The designer, about large files and video. I put the money into the monitor and into a GeForce RTX 5060 with 8 GB of video memory, and kept the SSD at the 1 TB minimum. A heavy project (large layered images or video timelines) will run out of 8 GB of video memory, and a year of project files will fill 1 TB. A 16 GB card was too expensive for the budget (the RTX 5060 Ti 16 GB was listed at 366 990 KZT on e-katalog.kz, about twice the 5060), so this is the trade-off most likely to hurt first.

## 4. Evidence

| File | What it proves |
|---|---|
| [budget.csv](budget.csv) | Every part, its price in tenge, the vendor page and the date the price was taken. |
| [builds.md](builds.md) | Load, bottleneck, trade-off and total for each of the four machines. |
| [evidence/pcpartpicker-links.md](evidence/pcpartpicker-links.md) | Compatibility and power check for each build, with the PCPartPicker list links. |
| [evidence/vendor-quotes/price-log.md](evidence/vendor-quotes/price-log.md) | Text log of every price: vendor, price, link and where it was seen on 2026-11-22. |
| evidence/vendor-quotes/ (screenshots) | Dated screenshots of the prices, named as listed in the README inside that folder. |

## 5. What I would do differently

I would check the price of the most expensive part first. I picked the platform and the roles, and only later found that a 32 GB DDR5 kit costs more than twice the CPU, which forced me to redo the trade-offs and left a margin of only 87 220 KZT. Next time I would look up memory, SSD and GPU prices before choosing anything else.
I would also record each price at the moment I see it, with a screenshot and the vendor name. The same Ryzen 5 7600 appeared at two different prices in two listings, and for the Ryzen 7 7700 BOX I only had the lowest of five offers and not the name of the shop, which I had to leave to be checked.
