# Lab 6 - Live processes and memory pressure

Pair: None
Date: 22.11.2026
Machines: VMware Workstation with two guests: Windows 11 "lab02-windows" and Ubuntu Server 26.04.1 LTS "lab01-ubuntu" (hostname lab01-shakenov, 4 vCPU, 7.2 GiB RAM, no swap)

Note on time: Ubuntu logs are in UTC. Example: 17:04 on Ubuntu is 22:04 in Almaty (UTC+5).

## Part 1 - One process, three tools

Process chosen: `ping 127.0.0.1` (Windows guest: `PING.EXE`, Ubuntu guest: `ping`).

Why: the guide suggests a program that exists on both guests. I first tried `python -m http.server 8000`, but Python was not found on the Windows machine where I tried it (`python` printed nothing, `py` was not recognized), so I switched to `ping`. It is built into both systems, needs no installation, and runs forever with `ping -t` on Windows and in the background with `&` on Linux.

Important detail: Task Manager and Process Explorer looked at the same Windows process (PID 10572) at the same time. htop and ps looked at the Linux process (PID 3356). These are two different processes of the same program on two different guests, which the guide allows.

Commands used on Ubuntu:

```bash
ping 127.0.0.1 > /dev/null &
pgrep -a ping
ps -o pid,ppid,user,rss,vsz,nlwp,cmd -p 3356
sudo ls /proc/3356/fd | wc -l
sudo ls -l /proc/3356/fd
htop        # F3 search "ping", then t for tree view
```

Command used on the Windows guest (cmd): `ping -t 127.0.0.1`

### Comparison table

| | Task Manager (Details tab) | Process Explorer | htop / ps |
|---|---|---|---|
| PID | 10572 | 10572 | 3356 |
| Parent (name and PID) | not shown | cmd.exe (8748) | bash (2218) |
| "Memory" as shown by default | 512 K (private working set) | 5 452 K (Working Set column) | RES 3 140 KB |
| Private memory | 512 K (private working set), 880 K (commit size) | Private Bytes 880 K, WS Private 480 K | no separate column |
| Working set / RES | 5 452 K | 5 452 K | RES 3 140 KB, SHR 2 972 KB |
| Virtual size | not shown as such (commit size 880 K is the closest) | not shown by default | VIRT / VSZ 8 068 KB |
| Handles / file descriptors | 82 handles | 82 handles | 5 file descriptors |
| Threads | 3 | 3 | 1 (nlwp) |

Small differences between the two Windows tools for private working set (512 K and 480 K) come from the two screenshots being taken a moment apart. The numbers of a live process move a little.

What the 5 file descriptors on Linux are (from `ls -l /proc/3356/fd`):
- 0 and 2: the terminal (`/dev/pts/0`)
- 1: `/dev/null` (I sent the output there with `> /dev/null`)
- 3 and 4: two network sockets

### Where the tools disagree

1. **The parent process.** Task Manager has no column for it at all. Process Explorer shows it as the line above in the tree (cmd.exe), and in htop tree view (`t`) it is `-bash`. Without Process Explorer or htop I could not have answered "who started this process".
2. **"Memory" means different things.** Task Manager's default memory number (512 K) is the private working set, which does not count shared parts. Process Explorer's Working Set (5 452 K) and htop's RES (3 140 KB) do include shared libraries. So for one process, 512 K and 5 452 K are both correct, they just measure different things.
3. **Same thing, different names.** Task Manager's "Commit size" (880 K) is exactly the same number as Process Explorer's "Private Bytes" (880 K). htop has no column for it.
4. **VIRT is not memory in use.** htop shows VIRT 8 068 KB for ping, which is the whole address space it may use, not what it actually holds in RAM (that is RES, 3 140 KB).
5. **Handles are not file descriptors.** Windows reports 82 handles for ping, and the lower pane of Process Explorer shows they include files, registry keys, semaphores and threads. Linux reports only 5 file descriptors, because Linux counts only open files and sockets. The numbers 82 and 5 cannot be compared directly.

## Part 2 - Memory pressure, on purpose

Guest: Ubuntu, RAM 7.2 GiB total, swap 0 B (checked with `free -h` and `swapon --show`). I did not lower the RAM. Before starting I took a VMware snapshot of the Ubuntu guest.

Because the guest has no swap, the kernel cannot move memory to disk, so when RAM runs out it has to kill a process.

Steps, in order:

```bash
mkdir -p ~/lab06 && cd ~/lab06
{ echo "=== BEFORE $(date) ==="; free -h; } > memory-pressure.txt
while true; do echo "=== $(date +%T) ==="; free -h; sleep 3; done | tee -a memory-pressure.txt     # second terminal, runs all the time

stress-ng --vm 2 --vm-bytes 70% --timeout 60s      # first terminal
stress-ng --vm 2 --vm-bytes 90% --timeout 60s
stress-ng --vm 2 --vm-bytes 120% --timeout 90s

sudo journalctl -k --since "30 min ago" | grep -i -E -B3 -A8 "out of memory|oom-kill" > ~/lab06/kernel-log-extract.txt
{ echo "=== AFTER (final) $(date) ==="; free -h; } >> ~/lab06/memory-pressure.txt
```

### Numbers (from [evidence/memory-pressure.txt](evidence/memory-pressure.txt))

| | Before (17:00:07) | During, worst moment (17:05:33) | After (17:10:31) |
|---|---|---|---|
| Used | 613 MiB | 6.5 GiB | 607 MiB |
| Available | 6.6 GiB | 751 MiB | 6.6 GiB |
| Swap used | 0 B | 0 B | 0 B |

### What happened at each step

| Step | stress-ng wanted | Time (Ubuntu, UTC) | Used peak | Result |
|---|---|---|---|---|
| 70% | 4.05 GB of 5.78 GB available | 17:01 to 17:02 | 4.8 GiB | passed, failed: 0, finished in 1 min |
| 90% | 5.19 GB of 5.76 GB available | 17:02:30 to 17:03:30 | 5.8 GiB, available down to about 560 MiB | passed, failed: 0, finished in 1 min 1.50 s |
| 120% | 6.92 GB of 5.76 GB available | 17:04:10 to 17:05:40 | 6.5 GiB, available down to 751 MiB | stress-ng says passed, failed: 0, finished in 1 min 31.50 s, but the kernel log shows 19 kills |

Responsiveness: swap stayed at 0 for the whole experiment, so there was no swapping slowdown. The 90% and 120% runs finished 1.5 seconds after the planned time, which is a very small delay. The SSH session stayed usable and I could run the log commands right after the test.

Interesting detail: stress-ng printed `failed: 0` even at 120%. My explanation (not checked in the stress-ng source) is that stress-ng starts a new worker after the kernel kills one, which fits the 19 kills in the log, so from its own point of view the run was fine. Only the kernel log shows what really happened.

### What the kernel did

From [evidence/kernel-log-extract.txt](evidence/kernel-log-extract.txt) (285 lines with context). First kill:

```
Nov 22 17:04:13 lab01-shakenov kernel: stress-ng-vm invoked oom-killer: ... oom_score_adj=1000
Nov 22 17:04:13 lab01-shakenov kernel: Out of memory: Killed process 4224 (stress-ng-vm) total-vm:3686112kB, anon-rss:3546528kB, ... oom_score_adj:1000
```

My explanation:
- **Which process:** `stress-ng-vm`, the memory worker of stress-ng. The kernel killed 19 of them in total, the first at 17:04:13 (PID 4224) and the last at 17:05:34 (PID 4320).
- **Why this one:** it held a lot of memory (anon-rss 3546528 kB, about 3.4 GiB, from the line above), and the log shows `oom_score_adj=1000`, which is the highest value and makes a process the first candidate to be killed. The line `global_oom` means the whole machine ran out of memory, not just one limited group.
- **When:** 17:04:13 to 17:05:34 on Ubuntu, which matches `memory-pressure.txt`: at 17:04:11 used memory jumped to 5.9 GiB and available fell to 458 MiB, and at 17:05:42 used memory fell back to 667 MiB after the last worker was gone.
- **One more thing visible in the numbers:** at 17:04:14 the `buff/cache` column dropped from 1.1 GiB to 114 MiB. The file cache was released at the same moment as the first kill.

### Windows

Command (run in PowerShell on the Windows 11 guest):

```powershell
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-Resource-Exhaustion-Detector'} -MaxEvents 5 | Format-List TimeCreated, Id, Message
```

Result: `No events were found that match the specified selection criteria.` (see [evidence/windows-eventlog.txt](evidence/windows-eventlog.txt)). This is a valid result: the Windows guest never ran out of virtual memory, and I did not push it into memory pressure.

What I would expect and when: Windows writes event ID 2004 from `Resource-Exhaustion-Detector` when the system runs low on virtual memory (commit charge reaches the limit). The event names the programs that used the most memory. Windows does not kill a process the way the Linux OOM killer does. It warns, new memory allocations start to fail, and the event records who was using the most. So the same class of event as the Linux OOM line exists, but Linux picks and kills a victim, Windows only reports.

## What did not work the first time

1. **No Python on Windows.** `python -m http.server 8000` printed nothing, and `py` gave "name not recognized". Fix: I switched the process to `ping`, which exists on both systems.
2. **Commands typed into a busy terminal.** On Ubuntu `python3 -m http.server 8000` was still running in the terminal, so the next commands I typed went into that server and did nothing. Fix: Ctrl+C to stop the server first.
3. **Permission denied on `/proc/3356/fd`.** Listing the open files of ping without `sudo` failed and `wc -l` printed 0. Fix: `sudo ls /proc/3356/fd | wc -l` gave 5.
4. **htop keys did nothing.** F5 seemed dead, and the screenshot showed the search line `Search: ping` still open at the bottom. Fix: Enter to close the search, then `t` to turn on the tree view. The process list also showed a second htop in state T (stopped), which I closed with `pkill htop`.
5. **Wrong password entered.** `sudo` said "Authentication failed" once, and the first `scp` said "Permission denied, please try again". Both worked on the second try.
6. **Missing file name for `wc -l`.** I ran a different command in place of the one that counted lines, and `wc -l` without a file name just waits for input, so the terminal looked frozen. Fix: Ctrl+C. The log file itself was already saved correctly (285 lines). Because of this mix-up the AFTER line was written into `memory-pressure.txt` more than once (17:07:43, 17:09:39 and the final one at 17:10:31). The last one is the real "after".
7. **The first `scp` with two files printed nothing and copied nothing.** Fix: copy the files one by one.
8. **I did the Windows part on the wrong machine first.** I ran Task Manager, Process Explorer and the event log query on my Windows 10 host instead of the Windows 11 guest, which the brief does not allow. Fix: I repeated all three on the Windows 11 guest and replaced the screenshots and the log file.

## Evidence

- [evidence/task-manager.png](evidence/task-manager.png) - Details tab of Task Manager on the Windows 11 guest with the PING.EXE row and the columns PID, private working set, working set, commit size, handles, threads
- [evidence/process-explorer.png](evidence/process-explorer.png) - Process Explorer on the Windows 11 guest with the process tree (PING.EXE under cmd.exe), memory columns and the lower pane with handles
- [evidence/htop.png](evidence/htop.png) - htop tree view with `ping 127.0.0.1` (PID 3356) under bash
- [evidence/memory-pressure.txt](evidence/memory-pressure.txt) - `free -h` before, every 3 seconds during, and after the three stress-ng runs
- [evidence/kernel-log-extract.txt](evidence/kernel-log-extract.txt) - kernel OOM killer lines (19 kills of stress-ng-vm)
- [evidence/windows-eventlog.txt](evidence/windows-eventlog.txt) - the Windows event ID 2004 query on the guest and its empty result
