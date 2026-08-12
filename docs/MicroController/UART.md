---
sidebar_position: 1
title: UART & USART
sidebar_label: UART
---

# Introduction

An introduction to UART and USART, explaining their core functions, key differences, hardware components, and how they handle serial data communication.

## What are UART and USART?

At their core, both UART and USART are simply pieces of hardware built into microcontrollers that convert **parallel data into serial data** for transmission. Modern microcontrollers typically come equipped with USART modules, allowing developers the flexibility to use them in either mode.

- **UART:** Universal Asynchronous Receiver Transmitter.
- **USART:** Universal Synchronous Asynchronous Receiver Transmitter.

## Key Differences

The primary distinction between the two lies in their timing and synchronization capabilities.

| Feature             | UART (Asynchronous Mode)                                                         | USART (Synchronous Mode)                                                    |
| :------------------ | :------------------------------------------------------------------------------- | :-------------------------------------------------------------------------- |
| **Clock Signal**    | No separate clock line.                                                          | Clock is sent separately alongside the data stream (similar to SPI or I2C). |
| **Synchronization** | Uses synchronization bits (**Start** and **Stop** bits) framing the useful data. | No Start/Stop bits required, as the shared clock syncs the transmission.    |
| **Efficiency**      | Lower (extra bandwidth consumed by non-data Start/Stop bits).                    | Higher (transmits pure data streams without framing overhead).              |

```text
Physical Interfaces
Unlike Ethernet or standard USB, UART/USART do not have a specific, standardized physical port.
Instead, they are commonly used in conjunction with physical interface standards and transceivers like **RS-232** or serial-to-USB converters.
```

## Typical Hardware Components

A standard USART hardware module typically consists of several internal blocks to manage the data flow and timing:

- **Baudrate Generator:** Generates the specific timing (baud rate) required for data communication.
- **TX and RX Shift Registers:** Responsible for shifting data bits out (Transmit) or shifting data bits in (Receive) one by one.
- **Transmit and Receive Control Blocks:** Manage the state and flow of the communication.
- **Transmit and Receive Buffers:** Temporarily hold the data being sent or received.
- **FIFO (First-In, First-Out) Buffer Memory:** An advanced feature that queues multiple bytes of data, drastically reducing CPU overhead.

## Basic Bi-Directional Communication (No Flow Control)

When hardware flow control is not used, UART requires a minimum of just two pins for bi-directional communication:

- **TX (Transmit):** Used to send data out of the UART module.
  - **Idle State:** When no data is being transmitted, the TX line is strictly held **HIGH**.
- **RX (Receive):** Used to receive data into the UART module.
  - **Start Bit Detection:** The UART module continuously samples the RX line. When it detects a transition that signifies a **start bit**, frame reception kicks in and continues until the module detects the idle line again.

## Hardware Flow Control (RTS and CTS)

To manage data flow and prevent buffer overruns, UART can utilize hardware flow control, which introduces two additional **active-low** pins:

- **CTS (Clear To Send):** This pin controls the transmission behavior. The UART module will only transmit data on the TX line if its CTS pin is pulled **LOW** by the external device. If it is HIGH, transmission is held until the pin is asserted LOW.
- **RTS (Request To Send):** The device uses this line to inform the connected device that it needs or is ready for data. It does this by asserting the line **LOW**.

### Device Interconnection

In a standard hardware flow control setup, the control lines are cross-connected between the two communicating devices:

- **Device A's RTS** connects to **Device B's CTS**.
- **Device B's RTS** connects to **Device A's CTS**.

**How it works:** When Device A wants data from Device B, Device A asserts its RTS pin LOW. This physically pulls Device B's CTS pin LOW, granting Device B the hardware permission to begin transmitting data on its TX line.
