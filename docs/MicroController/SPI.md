---
sidebar_position: 1
title: SPI (Serial Peripheral Interface)
sidebar_label: SPI
---

# SPI (Serial Peripheral Interface) Introduction

SPI is a **synchronous serial communication protocol** used in embedded systems to connect **one Master** device to **one or more Slave** devices (e.g., sensors, EEPROMs, SD cards, displays, Wi-Fi chips).

---

## Core Concepts

- **Master-Slave Architecture:** One master controls the communication and initiates all data transfers.
- **Synchronous Protocol:** Data transfer is synchronized by a clock signal generated solely by the Master.
- **Alternative Protocols:** Differs from other common protocols like I2C, CAN, Ethernet, USB, and RS232/RS485.

---

## The 4 Essential SPI Bus Pins

### 1. MOSI (Master Out Slave In)

- **Type:** Data Pin
- **Direction:** Master -> Slave
- **Function:** Carries data sent from the Master to the Slave.

### 2. MISO (Master In Slave Out)

- **Type:** Data Pin
- **Direction:** Slave -> Master
- **Function:** Carries data sent from the Slave to the Master.

### 3. SCK / SCLK (Serial Clock)

- **Type:** Clock Line
- **Direction:** Master -> Slave
- **Function:** Synchronizes data bits. SPI cannot function without this clock.
- **Debug Tip:** If communication fails, always check if the Master is generating this clock.

### 4. NSS / SS (Slave Select)

- **Type:** Control Pin (No data)
- **Direction:** Master -> Slave
- **Function:** Used by the Master to select which individual Slave to talk to. Optional if there is only one Slave.
