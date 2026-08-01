---
sidebar_position: 1
title: I2C (Inter-Integrated Circuit)
sidebar_label: I2C
---

An introduction to the Inter-Integrated Circuit (I2C) protocol, exploring its fundamental characteristics, key terminologies, and how it compares to the SPI protocol.

# I2C (Inter-Integrated Circuit) Introduction

The **I2C (Inter-Integrated Circuit)** protocol—pronounced "I squared C" or "I two C"—is a serial data communication protocol designed for integrated circuits that are in close proximity. Compared to SPI, I2C is significantly more complex, featuring built-in rules for handshaking, error handling, and data transmission structuring.

## I2C vs. SPI

| Feature               | I2C Protocol                                                 | SPI Protocol                                                      |
| :-------------------- | :----------------------------------------------------------- | :---------------------------------------------------------------- |
| **Specification**     | Standardized dedicated specification by NXP.                 | No global dedicated specification (proprietary variations exist). |
| **Pin Count**         | Exactly **2 pins** (SDA and SCL) regardless of device count. | Minimum 4 pins (increases dynamically with multiple slaves).      |
| **Targeting**         | **Address-based** (every slave has a unique ID).             | **Hardware-based** (requires dedicated Slave Select / NSS pins).  |
| **Directionality**    | Half-duplex.                                                 | Full-duplex.                                                      |
| **Speed / Data Rate** | Slower (typically 400 KHz to 1 MHz; max 4 MHz).              | Very fast (e.g., up to 20 Mbps, typically Peripheral Clock / 2).  |
| **Multi-Master**      | Native hardware support with automatic arbitration.          | Must be manually handled via software code.                       |
| **Acknowledge (ACK)** | Hardware automatically ACKs every received byte.             | No native automatic ACKing.                                       |
| **Clock Control**     | Slaves can pause the master via **Clock Stretching**.        | Slaves have zero control over the serial clock.                   |

:::tip Use Case Selection
Because SPI is significantly faster than I2C, it is ideal for high data-rate applications (like streaming audio/video samples). I2C is best suited for low data-rate requirements, such as reading static values from basic sensors, where saving physical pins is a priority.
:::

## The 2-Pin Hardware Setup

One of the biggest advantages of I2C is its simplicity at the hardware level. The bus requires only two lines, both of which are pulled high to VCC using pull-up resistors:

- **SCL (Serial Clock):** Carries the clock signal.
- **SDA (Serial Data):** Carries the data payload.
