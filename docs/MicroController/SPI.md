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

## Protocol Comparison Table (for example)

| Protocol   | Max Speed             | Max Distance          | Signaling Type     | Ideal Scenario          |
| :--------- | :-------------------- | :-------------------- | :----------------- | :---------------------- |
| **SPI**    | **High** (~25 Mbps)   | **Short** (< 10 feet) | Single-Ended (TTL) | Same PCB / Board        |
| **I2C**    | **Low** (~3.4 Mbps)   | **Medium** (> SPI)    | Single-Ended (TTL) | Same PCB / Board        |
| **RS-485** | **Medium** (~10 Mbps) | **Long** (1000+ feet) | Differential       | Inter-device / Factory  |
| **CAN**    | **Medium**            | **Long**              | Differential       | Automotive / Industrial |

---

### 1. SPI: The Speed King for Short Distances

- **High Data Rates:** Max speed can reach **Peripheral Clock / 2** (e.g., 25 Mbps on a 50 MHz clock).
- **Distance Capped:** Limited to **10 feet or less** due to single-ended (TTL) signaling.
- **Best Use Cases:** High-frequency sensors, displays, and high-speed serial Flash/EEPROM memory.

### 2. I2C: Complex Features but Slower Speed

- **Slower Throughput:** Capped around 3.4 Mbps even in High-Speed mode.
- **Feature Rich:** More complex protocol than SPI, allowing better device management over slightly longer distances.
- **Best Use Cases:** Low-speed sensor data collection on the same board.

### 3. Long-Distance Protocols: RS-485 & CAN

- **Noise Immunity:** Utilize **differential signaling** to cancel out environmental noise.
- **Extended Range:** Can reliably cover hundreds or thousands of feet (e.g., 100ft to 1000ft+).
- **Best Use Cases:** Automotive networks, factory automation, and building/home automation.

---

#### Summary

- **Single PCB Environment:** Use **SPI** or **I2C** to connect microcontrollers to onboard chips.
- **Large Area / Inter-Device Environment:** Use **CAN**, **Ethernet**, or **RS-485** to connect separate modules or systems.
