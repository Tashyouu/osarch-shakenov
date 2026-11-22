# Compatibility and power check

**What this file proves:** each of the four builds was checked for compatibility (socket, memory type, M.2 slot, case and board size) and for power before choosing the power supply. PCPartPicker does not price in tenge, so it is used only for compatibility and power. All prices are in [../budget.csv](../budget.csv).

| Check | Accountant | Sales | Web developer | Designer |
|---|---|---|---|---|
| CPU socket and board socket | Ryzen 5 7600 (AM5) on B650M-P (AM5): OK | same: OK | Ryzen 7 7700 (AM5) on B650M-P: OK | same: OK |
| Memory type | DDR5 kit on DDR5 board: OK | OK | OK | OK |
| M.2 NVMe SSD | Kingston NV3 (M.2 2280, PCIe 4.0) in board M.2 slot: OK | OK | OK | OK |
| Board in case | mATX board in ATX mid tower: OK | OK | OK | OK |
| Display output | CPU has integrated graphics, board has video outputs: monitor connects to the board | same | same | graphics card used |
| Estimated draw (PCPartPicker) | 149 W | 149 W | 170 W | 315 W |
| Power supply | 550 W | 550 W | 550 W | 650 W (the RTX 5060 page recommends at least 550 W) |

**PCPartPicker share links.** PCPartPicker does not price in tenge, so its dollar prices are ignored here. It was used only for the compatibility check and the power estimate. Differences from the real builds are listed in the last column: PCPartPicker does not have every part sold in Kazakhstan, and the operating system and the monitor are priced in budget.csv, not in the lists.

| Build | PCPartPicker link | Estimated wattage | Warnings shown | Differences from the real build |
|---|---|---|---|---|
| Accountant and sales (same parts) | https://pcpartpicker.com/list/xpy3sK | 149 W | none (only the standard note that some physical constraints, such as cooler clearance, are not checked) | Memory: Kingston FURY Beast RGB 16 GB DDR5-6000 CL36 instead of the non-RGB CL30 kit (same type, DDR5 2 x 8 GB). Case: FSP CMT260 instead of FSP CMT223S, which is not in the database (both ATX mid tower). |
| Web developer | https://pcpartpicker.com/list/WKy3sK | 170 W | none (only the standard disclaimer about physical constraints). The cooler mount shows AM5, so the ID-COOLING SE-802-SD V3 fits this socket. | Memory: Apacer Panther RGB 32 GB DDR5-5200 CL38 instead of the non-RGB Panther. Case: FSP CMT260 instead of CMT223S (not in the database). |
| Designer | https://pcpartpicker.com/list/fwTqw3 | 315 W | none (only the standard disclaimer about physical constraints). The graphics card power connector is matched by the power supply. | Graphics card: Inno3D Twin X2 OC RTX 5060 8 GB (the build uses the non-OC Twin X2, same chip and memory). Power supply: MSI MAG A650BN 650 W instead of PCCooler YK650 650 W (not in the database, same class). Memory: Apacer Panther RGB instead of non-RGB. Case: FSP CMT260 instead of CMT223S. |
