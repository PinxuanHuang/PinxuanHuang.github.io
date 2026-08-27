---
sidebar_position: 1
title: CAN
sidebar_label: CAN
---

# Introduction

An introduction to the Controller Area Network (CAN) protocol

## What is a CAN Bus?

At its most basic level, the CAN bus is a robust serial communication network. Its primary job is to **reliably connect Electronic Control Units (ECUs)** while minimizing physical wiring. It accomplishes this by being:

- **Multi-Master:** Any node on the network can transmit data whenever the bus is idle, eliminating the need for a central master controller.
- **Broadcast-Based:** Messages are transmitted to the entire network rather than to specific, targeted node addresses.

## The Mechanics of Communication

Imagine multiple ECUs needing to share critical data on the same shared wire simultaneously.

1.  **The Broadcast:** When a node has data (e.g., engine RPM), it constructs a frame containing a unique **Identifier** and broadcasts it to the entire bus.
2.  **The Arbitration:** If two nodes transmit at the exact same time, the bus uses a non-destructive bitwise arbitration process. The message with the lower identifier value is granted the highest priority and wins control of the bus.
3.  **The Acceptance:** Every node on the network "hears" the winning message. Each node's hardware filter checks the message identifier to decide whether to process the data or simply ignore it.

## Noise Immunity and Error Handling

When transmitting data in a moving vehicle or a noisy factory floor, signal integrity is critical. This robustness is achieved through the **physical and logical design** of the CAN protocol.

- **Differential Signaling:** The CAN bus transmits data by sending identical but inverted voltage signals across two twisted wires (CAN High and CAN Low). This makes the network highly immune to external electromagnetic interference.
- **Error Confinement:** If a node detects continuous transmission errors or physical faults, it will automatically isolate and disconnect itself from the network (Auto Bus-Off) to prevent crashing the rest of the system.
