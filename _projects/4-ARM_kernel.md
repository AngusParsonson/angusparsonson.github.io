---
title: "Kernel for ARM 32bit Cortex-A8 Processor"
excerpt: Using POSIX for the general architecture, I built a kernel for an ARM operating system with an age-based priority scheduling algorithm to run user processes. I also completed an implentation of the dining philosophers problem on the OS (using a mutex lock and semaphores). This project was completed using C. <br/><img src='/images/projects/ARM_kernel/arm_cortex-A8.jpeg'>"
collection: projects
---

<div style="text-align: justify">
Using POSIX for the general architecture, I built a kernel for an ARM operating system with an age-based priority scheduling algorithm to run user processes. I also completed an implentation of the dining philosophers problem on the OS (using a mutex lock and semaphores). This project was completed using C.
</div>

## Implementation

During development, I used QEMU to emulate the ARM Cortex-A8 Processor.

The kernel was built to run simple programs concurrently using time slicing. A global time was set to return control to the kernel via an interrupt. The user could also choose which programs to run using the keyboard. I used a PCB (program control block) to keep track of processes, the layout of which was a slightly simpler version of the standard:

<img src='/images/projects/ARM_kernel/pcb.jpg' width=250>

So each process had a **process state** (running, waiting etc.), a **PID** (process ID and parent process ID), registers (for memory) and a program counter (to restore swapped process out of memory i.e. remember what position we were in the program for context switching).

The priority age-based scheduler ensured all current programs would be run based on their priority and how long it had been since they last ran. The priority of a program could be changed by the user via a **nice** system call. The basic algorithm worked as follows: 
- After every context switch:
    1. The currently running program have its age decreased to zero.
    2. All other programs have their ages incremented by their priority (defined a default value at startup or a custom value by the user at runtime).
    3. The program with the highest age is then ran until the next context switch.

## System Calls
**Fork** - the process in question creates a copy of itself.

**Exec** - runs a new process in place of the currently running process.

**Exit** - terminates a process execution.

**Yield** - returns control to the kernel and calls the scheduling algorithm.

**Nice** - changes the process in question's priority.

Using the kernel, I added basic support for inter-process communication to solve the dining philosophers problem. Upon execution, it forked to spawn 16 new philosopher child processes which could interact with each other via IPC (inter-process communication). I used a mutex lock with semaphore wait and semaphore post functions. I also utilised the semaphore wait and post functions with an array of forks.

## Key Concurrency and OS Concepts
**Synchronisation**: using atomic operations to ensure correct operation of cooperating threads. Multiple processes join up or handshake in order to reach agreement.

**Critical section**: a section of code, or collection of operations, in which only one thread may be executing at a given time.

**Mutual Exclusion**: mechanisms used to create critical sections.

**Context switch**: the kernel does time slicing of the processor and assigns one single thread to be in running state. It swaps this process by saving the current state and loading the state of another based on a scheduling queue using an algorithm.

**Traps**: events occurring in the current thread that cause a change of control into the operating system, there are three ways to do this: System call, error, page fault.

**Interrupts**: events occurring outside the current thread that cause a state to switch into the operating system, e.g. keyboard events, timer etc.
