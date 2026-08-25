---
sidebar_position: 1
title: Timer
sidebar_label: Timer
---

# Introduction

An introduction to the timer peripheral

## What is a Timer?

At its most basic level, a timer is simply a dedicated hardware peripheral. Its primary job is to **count**. It accomplishes this by either:

- **Up-Counting:** Counting from 0 up to a pre-programmed maximum value.
- **Down-Counting:** Counting from a pre-programmed maximum value down to 0.

## Primary Use Cases

While counting sounds simple, it forms the foundation for many complex tasks, including:

- **Timebase Generation:** Creating precise delays (e.g., waiting exactly 10 milliseconds).
- **Signal Measurement:** Measuring the frequency or time period of incoming waveforms.
- **Pulse Width Measurement:** Determining the exact duration of an input pulse.
- **Waveform Generation:** Producing different output waveforms.
- **PWM (Pulse Width Modulation):** Generating PWM signals for motor control or LED dimming.
- **Peripheral Triggering:** Triggering other hardware events automatically, such as initiating an ADC (Analog-to-Digital Converter) reading.

## The Mechanics of Counting

Imagine you program the timer to count up to a maximum value (the "period") of 5.

1.  **The Count:** When triggered, the timer starts at 0 and increments: 1, 2, 3, 4, 5.
2.  **The Roll-Over:** Upon reaching the pre-programmed value (5), the timer immediately rolls back to 0 and begins counting again.
3.  **The Update Event:** Exactly when the timer rolls over, it generates a hardware signal called an **Update Event**. This event is recorded in the timer's status register and can be configured to trigger a software interrupt, pausing the main program to execute specific code.

## The Clock Frequency and Time Period

When counting from 0 to 1, or 1 to 2, there is a specific physical time gap between each count. This time gap is entirely dependent on the **clock frequency** supplied to the timer peripheral.

- **High Clock Frequency:** The timer has more "energy." The time gap between counts is very small, meaning the timer counts very fast.
- **Low Clock Frequency:** The timer has less "energy." The time gap between counts is larger, meaning the timer counts slowly.
