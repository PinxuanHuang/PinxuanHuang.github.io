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

## What is a Frame?

A **frame** refers to the entire data packet that is sent or received during communication.

## The UART Frame Structure

In UART communication, a typical frame follows a specific, sequential order of bits. While the core structure is standard, several parameters are configurable via the UART peripheral's registers:

- **Start Bit:** Signals the beginning of the communication frame. It is always held **LOW** for a duration of exactly 1 bit.
- **Data Bits:** The actual data payload being transmitted, sent from the Least Significant Bit (LSB) to the Most Significant Bit (MSB). The payload length is typically configurable between **5 to 9 bits**.
- **Parity Bit (Optional):** Used for basic error checking. It consumes exactly 1 bit. If enabled, the hardware can be configured to use either an **even parity** or **odd parity** mechanism.
- **Stop Bit:** Signals the end of the frame. It is always held **HIGH**. The duration of the stop condition is configurable, typically to **1, 1.5, or 2 bit** lengths.

## What is Baud Rate?

The **baud rate** defines how fast data is sent over a serial line. It is most commonly expressed in units of **bits per second (bps)**.

The primary requirement for successful serial communication is that both the transmitting and receiving devices must be configured to operate at the exact same baud rate. If there is a mismatch, the receiver will sample the data line at the wrong times, leading to corrupted data.

### Common Baud Rates

Baud rates can theoretically be set to almost any value, provided both devices support it. However, standard rates are typically used.

## Bit Duration and Timing

You can determine the exact time it takes to transmit a single bit (i.e., how long the transmitter holds the serial line HIGH or LOW for a given bit).

As you increase the baud rate, the duration of each bit becomes smaller, meaning your overall data packet is transmitted much more quickly.

### Calculation Example (9600 bps)

If the baud rate is 9600 bps, the duration of a single bit is calculated as:

- 1 / 9600 approx 0.00010416 seconds, or roughly **104 microseconds (µs)**.
- Therefore, transmitting 4 bits of data would take approximately **416 µs** .

## Hardware Limitations

While higher baud rates mean faster data transfer, there are physical and architectural limits to how fast data can be sent. The maximum achievable baud rate is heavily dependent on the **peripheral clock frequency** of the UART hardware.

## The Role of Synchronization Bits

In asynchronous serial communication, synchronization bits are special bits transferred with each chunk of data. Because there is no shared clock line, these bits—specifically the **Start** and **Stop** bits—are essential for marking the precise beginning and end of a data packet.

## Start Bit

Every UART frame begins with exactly **one** start bit.

- **Line Transition:** The start bit is indicated by the data line transitioning from its default idle state (**HIGH**) to an active **LOW** state.

## Stop Bit(s)

A UART frame concludes with one or more stop bits, which return the data line to its idle state by holding it **HIGH**.

- **Configurability:** While there is always only one start bit, the number of stop bits is configurable.
- **Typical Usage:** Most standard applications use **1** stop bit.
- **High-Speed Usage:** If your application operates at a very high baud rate (e.g., in the megabits per second range), it is often recommended to configure the hardware to insert **2** stop bits to give the receiver adequate time to process the frame.
