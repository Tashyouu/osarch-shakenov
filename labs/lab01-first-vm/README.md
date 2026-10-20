# Lab 1 — Your first virtual machine

Pair: DAMIR NIYAZBEK (My pair)
Driver first half: TAMERLAN SHAKENOV (Me)
Machine: HP Victus 16-e0XXX (Laptop)
Date: 20.10.2026

## What I did

Decided the resources:
Steps, with commands in code blocks. Not prose about the steps.

```
<command>
<output, trimmed>
```

## Result
Resource allocation:
vCPU - 2 cores - justification: I decided to give it 2 cores since my CPU has 8 cores and 16 threads. </br>
Memory - 8 GB - justification: Since my laptop has 32 GB RAM, there's no issue with memory allocation for VM.
Disk - 25 GB (dynamically allocated) - justification: I cleared up some space on my disk for the Virtual Machine, and now it should have no issue for host machine to allocate more space in the future.
Network - NAT - justification: I selected NAT to allow the virtual machine to access the internet through the host machine's network connection.

## What did not work the first time

At least one thing. What I saw, what it turned out to be, what I did about it.

## Evidence

- [evidence/](evidence/) — one line per file on what it proves
