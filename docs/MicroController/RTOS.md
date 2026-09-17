---
sidebar_position: 1
title: RTOS
sidebar_label: RTOS
---

# Introduction

An introduction to FreeRTOS

## What Is FreeRTOS?

**FreeRTOS** is an open-source real-time operating system designed for embedded systems and microcontrollers. It provides a small kernel and related services for building applications that must respond predictably to events.

## Core Kernel Services

FreeRTOS lets an application be split into multiple independent **tasks**. The kernel manages when each task runs and provides services for coordinating them.

| Service                  | Purpose                                                                                 |
| ------------------------ | --------------------------------------------------------------------------------------- |
| Task management          | Create, delete, block, resume, and assign priorities to tasks.                          |
| Scheduling               | Select the next runnable task using preemptive, cooperative, or time-sliced scheduling. |
| Priority levels          | Allow time-critical tasks to run before less important work.                            |
| Inter-task communication | Exchange data and coordinate tasks safely.                                              |
| Time management          | Delay tasks and schedule work at defined intervals.                                     |

## Communication and Synchronization

| Mechanism           | Typical use                                                                 |
| ------------------- | --------------------------------------------------------------------------- |
| Queues              | Send data or messages safely between tasks.                                 |
| Binary semaphores   | Signal that an event has occurred; commonly used for task synchronization.  |
| Counting semaphores | Track multiple available resources or repeated events.                      |
| Recursive mutexes   | Provide mutual exclusion when a task may lock the same resource repeatedly. |
| Event groups        | Synchronize tasks based on one or more event bits.                          |
| Task notifications  | A lightweight, fast direct notification sent to a specific task.            |

### Choosing an IPC Mechanism

```text
Need to transfer data?       -> Queue
Need exclusive access?       -> Mutex / semaphore
Need to wait for many flags? -> Event group
Need a fast 1-to-1 signal?   -> Task notification
```

Task notifications usually have less overhead than queues or semaphores because the notification is stored directly in the target task's control block.

## Time and Memory Services

- **Software timers** can be one-shot or periodic. For example, a periodic timer can trigger a sensor sampling task every 1 ms or 10 ms.
- **Dynamic memory allocation** lets the application request memory from the heap. FreeRTOS provides multiple heap-management schemes, so the application can select one that matches its allocation and safety requirements. |

### CMSIS-RTOS Abstraction

CMSIS stands for **Cortex Microcontroller Software Interface Standard**. The CMSIS-RTOS layer offers standardized RTOS APIs, while translating those calls to the underlying RTOS implementation.

```text
Application code
       |
       v
CMSIS-RTOS API
       |
       v
FreeRTOS kernel
```

This separation keeps application code cleaner and makes a future RTOS migration easier when the target RTOS supports the same CMSIS-RTOS API.
