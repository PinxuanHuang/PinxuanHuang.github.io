---
sidebar_position: 1
title: LowPower
sidebar_label: LowPower
---

# Introduction

An introduction to Low Power mode with MCU(STM32)

## Types of Low-Power Modes

A Cortex-M-based microcontroller typically provides two categories of low-power modes:

- **Processor-specific low-power modes**
  - Defined by the ARM Cortex-M processor architecture.

- **MCU/vendor-specific low-power modes**
  - Implemented by the microcontroller vendor, such as STMicroelectronics.

## MCU Operating Modes

At a high level, an MCU can operate in two major modes:

- **Run Mode**
- **Low-Power Mode**

## Run Mode

- The processor clock is active.
- The CPU continuously executes instructions.
- Peripherals and clocks operate according to their configuration.
- Power is continuously consumed while the processor is running.

In many simple firmware applications, the MCU remains in an infinite loop:

```c
while (1)
{
    // Application processing
}
```

If there is no useful work to perform, the CPU may simply continue executing the loop.

This is commonly referred to as an **idle loop**.

## Why Use Low-Power Modes?

Instead of continuously executing an idle loop, the processor can enter a **sleep or low-power state**.

Conceptually:

```text
Run Mode
   |
   | No work to perform
   v
Low-Power Mode
   |
   | Interrupt / Wake-up event
   v
Run Mode
```

This allows the MCU to reduce power consumption while waiting for an event.

## Cortex-M processors processor-level sleep categories:

- Normal Sleep
- Deep Sleep

- The SLEEPDEEP bit in SCB->SCR selects which sleep category is requested.
  Sleep can be entered using: - WFI - WFE - Sleep-on-Exit

```text
Key Point:
Arm Cortex-M defines how the processor requests and enters sleep, while the MCU vendor defines what actually happens to clocks, memories, peripherals, and power domains during that sleep state.
```

## Low-Power Mode Power Saving Tips

- Disable unused peripheral clocks
- Configure GPIOs as analog mode
- Minimize ISR execution time
