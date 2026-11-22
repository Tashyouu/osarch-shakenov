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
| Delivery | Only vendors that deliver in Kazakhstan, each price dated. The listings show delivery in Almaty, so I assumed the new office is in Almaty. |
| Existing equipment | None, everything is bought new. |

## 3. What I did

1. Went from role to load to bottleneck to trade-off (see [builds.md](builds.md)), and only then picked parts.
2. Checked compatibility as in Lab 3: CPU socket and board socket, memory type, M.2 slot and NVMe SSD, total power and power supply. PCPartPicker was used only for compatibility and power. It does not price in tenge.
3. Took every price in tenge from a Kazakhstan vendor listing (e-katalog.kz offers from shops such as Ants.kz, Shop.kz and Flexcom.kz, and alfa.kz listings; the shop is named in budget.csv or visible in the screenshot), with the date, and saved dated screenshots (see [budget.csv](budget.csv) and the list in evidence/vendor-quotes/README.md).
4. Every machine has at least 16 GB memory, a 1 TB SSD, a licensed operating system and a monitor.
5. The biggest finding was the price of memory: a 32 GB DDR5 kit (from 289 119 KZT, Kingston from 332 235) cost almost three Ryzen 5 7600 on the day I checked, so memory decisions decided the budget. The prices moved while I worked: my first 32 GB price was 230 168 KZT, and I had to rebuild the budget around the new one. The margin left is only 86 368 KZT.

**Totals (from budget.csv, prices taken 2026-11-22)**

| Role | Total, KZT |
|---|---|
| Accountant | 599 799 |
| Sales | 584 193 |
| Web developer | 759 629 |
| Designer | 970 011 |
| **Grand total** | **2 913 632 (budget 3 000 000, margin 86 368)** |

### Answer 1: which machine do I upgrade first, in 18 months?

Web developer, the memory. The board has four memory slots and only two are used, so I can add a second 2 x 16 GB kit and reach 64 GB without throwing anything away (a 32 GB kit now costs 289 119 KZT or more, so adding a second kit later is a real cost, but it needs no new board or CPU). Containers, virtual machines and a local database are the load that grows fastest, and on this machine memory is the first resource to run out. The AM5 platform and DDR5 chosen in section 2 are what make this a simple add-on.

### Answer 2: the same four as pre-built machines

I priced the closest pre-built systems from alfa.kz (Almaty marketplace, delivery in Almaty). Prices seen on 2026-11-22. A pre-built listing has no monitor and the operating system is not stated, so I added the same monitor and Windows 11 Pro that I used in my builds, to compare like with like.

| Role | Pre-built model (alfa.kz) | Pre-built, KZT | + monitor + Windows 11 Pro, KZT | Built by me (budget.csv), KZT | Date | Link |
|---|---|---|---|---|---|---|
| Accountant | PULSER Eco 100, Ryzen 5 5600GT, DDR4 16 GB, SSD 1 TB | 245 329 | 400 188 | 599 799 | 2026-11-22 | https://alfa.kz/pc/computers/amd/ryzen_5/pulser/8539143 |
| Sales | same PULSER Eco 100 (16 GB, the cheapest suitable one) | 245 329 | 384 582 | 584 193 | 2026-11-22 | https://alfa.kz/pc/computers/amd/ryzen_5/pulser/8539143 |
| Web developer | PULSER Advanced 405M, Ryzen 5 7500F, DDR5 32 GB, SSD 1 TB, RTX 5060 | 1 027 500 | 1 226 612 | 759 629 | 2026-11-22 | https://alfa.kz/pc/computers/amd/ryzen_5/pulser/8682058 |
| Designer | same PULSER Advanced 405M (RTX 5060 8 GB) | 1 027 500 | 1 246 233 | 970 011 | 2026-11-22 | https://alfa.kz/pc/computers/amd/ryzen_5/pulser/8682058 |

Sum of the four pre-built options with monitors and Windows: 3 257 615 KZT, over the 3 000 000 budget, against 2 913 632 for my builds. Note that I found no pre-built with 32 GB and no graphics card, so for the developer the pre-built pays for a GPU that is not needed.

**What the company gains by buying pre-built:** one seller who tests and delivers the machine, one warranty and support contact for the whole computer, and no assembly time (four machines by hand is hours of work). For the two cheap roles the pre-built is cheaper: it is an older platform (AM4, DDR4), so the saving comes from a platform without an upgrade path.
**What it loses:** choice of parts and the upgrade path (the cheap office machines are AM4 with DDR4, so a later CPU or memory upgrade means a new board). The expensive ones include a graphics card that the developer does not need, which makes them more expensive than my builds. The seller also chooses the power supply, the case and the memory brand, and Windows is not included (the 405M is sold with DOS, the Eco 100 does not state an operating system).

### Answer 3: who complains about performance within a month, and about what?

The designer, about large files and video. I put the money into the monitor and into a GeForce RTX 5060 with 8 GB of video memory, and kept the SSD at the 1 TB minimum. A heavy project (large layered images or video timelines) will run out of 8 GB of video memory, and a year of project files will fill 1 TB. The sales manager is the runner-up: 16 GB is enough for 20 to 30 tabs and Excel, but with a video call on top it will be close to full (my estimate is about 10 GB, not a measurement), so slowdowns in the middle of a call are the second most likely complaint. A 16 GB graphics card was too expensive for the budget (the RTX 5060 Ti 16 GB was listed at 366 990 KZT on e-katalog.kz, about twice the 5060), so this is the trade-off most likely to hurt first.

## 4. Evidence

| File | What it proves |
|---|---|
| [budget.csv](budget.csv) | Every part, its price in tenge, the vendor page and the date the price was taken. |
| [builds.md](builds.md) | Load, bottleneck, trade-off and total for each of the four machines. |
| [evidence/pcpartpicker-links.md](evidence/pcpartpicker-links.md) | Compatibility and power check for each build, with the PCPartPicker list links. |
| [evidence/vendor-quotes/price-log.md](evidence/vendor-quotes/price-log.md) | Text log of every price: vendor, price, link and where it was seen on 2026-11-22. |
| evidence/vendor-quotes/ (screenshots) | Dated screenshots of the prices, named as listed in the README inside that folder. |

## 5. What I would do differently

I would check the price of the most expensive part first. I picked the platform and the roles, and only later found that a 32 GB DDR5 kit costs almost three times the CPU, which forced me to redo the trade-offs and left a margin of only 86 368 KZT. Next time I would look up memory, SSD and GPU prices before choosing anything else.
I would also record each price at the moment I see it, with a screenshot and the shop name, and trust only the live page. Prices from search results turned out to differ from the live page (a Kingston 32 GB kit was 230 168 KZT in a cached copy and 332 235 KZT live), and I nearly put a DDR4 memory kit into a DDR5 build because the product name looked right. Next time I would check the memory type on the page itself before writing the price down.
