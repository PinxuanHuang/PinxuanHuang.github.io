---
sidebar_position: 1
title: GPIO (General-purpose input/output)
sidebar_label: GPIO
---

# GPIO Introduction

There are several ports, and each port contains multiple pins (e.g., 16 pins on Port A).
Each port has its own configuration registers.

## Registers

- Mode
  - Input
  - Output (Push Pull or Open Drain)
  - Alternate Function
  - Analog

```
When it is in Input mode. It can be configured to issue an interrupt to the processor.

When an I/O port is programmed as input, the output buffer is disabled, the schmitt trigger input is activated, the pull-up and pull-down resistors are activated depending on the value of the PUPDR register.

The data present on the I/O pin are sampled into the input data register for every bus clock cycle and read access to the input data register provides the I/O state of that pin.
```

- Output Speed
  - Low Speed
  - Medium Speed
  - High Speed
  - Very High Speed
