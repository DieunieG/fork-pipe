# fork-pipe
## Submission — Q1 & Q2 
When a process calls fork(), the parent and child don’t actually share the normal memory they use for variables. The stack and heap start out as copies (copy-on-write), so they look the same at first, but once either process writes to them, it gets its own private version. The only thing they truly share are any shared-memory segments that were intentionally created for that purpose—those stay visible to both and updates show up on both sides. For mobile concurrency, iOS mainly uses Grand Central Dispatch (GCD) and OperationQueue so I can put work on background queues with priorities and keep UI work on the main queue. Android used to lean on Threads with Handlers/Loopers, but the modern approach is Kotlin Coroutines with Dispatchers (Main/IO/Default), plus WorkManager for reliable background jobs and Foreground Services for ongoing user-visible tasks—again, UI updates belong on the main thread. The hard parts that concurrency adds are: (1) race conditions, where two things touch the same state at the same time and I get inconsistent results; (2) deadlocks/priority issues, where threads block each other or a low-priority holder stalls a high-priority task; and (3) lifecycle and battery pressure, like work outliving a screen, leaking resources, or draining power. Using GCD/Operations/Coroutines, keeping data immutable when possible, and syncing shared state carefully helps avoid those pitfalls.

## Q3 — Source code
- C++ program

### How the program works (brief)
- Creates 20 random integers.
- **Parent** finds the min in indices **0–9**.
- **Child** finds the min in **10–19** and **writes it into a pipe**.
- Parent **reads** the child’s min, prints both PIDs, and the **overall minimum**.
