# Principles of Scheduling

## Table of Contents

- [Principles of Scheduling](#principles-of-scheduling)
  - [Table of Contents](#table-of-contents)
  - [Scheduling Overview](#scheduling-overview)
  - [Long-term vs. Short-term Scheduling](#long-term-vs-short-term-scheduling)
    - [Priority for Scheduling](#priority-for-scheduling)
  - [Preemptive vs Non-preemptive Scheduling](#preemptive-vs-non-preemptive-scheduling)

## Scheduling Overview

## Long-term vs. Short-term Scheduling

**Long-term scheduling** decides when a process should enter the ready sate and start competing for the CPU

**Short-term scheduling** decides which of the ready processes should run next on the CPU

![long_vs_short](/notes/assets/scheduling/long_vs_short.PNG)

### Priority for Scheduling

A process's **priority** is a numerical value that indicates the importance of the process relative to other processes. The scheduler uses this value to decide which process to run next

The **arbitration** rule decides which process should proceed if two or more processes have the same priority

Scheduling is typically based on the following parameters:

| Parameter            | Explanation                                                                                                              |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| arrival              | time when the process enters the ready list                                                                              |
| departure            | time when the process leaves the ready list by either entering the blocked or suspended state or by terminating all work |
| attained CPU time    | amount of CPU time used by the process since arrival                                                                     |
| real time in system  | amount of actual time the process has spent in the system since arrival                                                  |
| total CPU time       | amount of CPU time the process will consume between arrival and departure                                                |
| external priority    | numeric priority value assigned to the process explicitly at time of creation                                            |
| deadline             | time which the process must be completed by                                                                              |
| period               | time interval during which a periodically repeating computation must be completed                                        |
| other considerations | resource requirements of a process                                                                                       |

## Preemptive vs Non-preemptive Scheduling

**Preemptive scheduling** may stop the currently running process and choose another process to run. This decision is made when:

- a new process enters the ready list
- a previously blocked or suspended process re-enters the ready list
- the OS periodically interrupts the current process to give other processes a chance to run

**Non-preemptive scheduling** allows a running process to continue until the process terminates or blocks on a resource

![preemtpive_vs_nonpreemptive](/notes/assets/scheduling/preemtpive_vs_nonpreemptive.PNG)
