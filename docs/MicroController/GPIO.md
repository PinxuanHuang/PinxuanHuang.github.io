---
sidebar_position: 1
title: GPIO (General-purpose input/output)
sidebar_label: GPIO
---

# GPIO Introduction

```
The following content takes STM32F407 as an example.
```

- There are several ports, and each port contains multiple pins (e.g., 16 pins on Port A).
- Each port has its own configuration registers.

## Registers

- Mode Register
  - Input
  - Output (Push Pull or Open Drain)
  - Alternate Function
  - Analog

```
When it is in Input mode. It can be configured to issue an interrupt to the processor.

When an I/O port is programmed as input, the output buffer is disabled, the schmitt trigger input is activated, the pull-up and pull-down resistors are activated depending on the value of the PUPDR register.

The data present on the I/O pin are sampled into the input data register for every bus clock cycle and read access to the input data register provides the I/O state of that pin.
```

- Output Speed Register
  - Low Speed
  - Medium Speed
  - High Speed
  - Very High Speed

```
 slew rate is defined as the change of voltage or current, or any other electrical or electromagnetic quantity, per unit of time.
```

- Pull up/down Register
  - No pull up/down
  - Pull up
  - Pull down
  - Reserved

- Input Data Register
  - read only and can be accessed in word mode only

```
Input Data Register will be updated for every one AHB1 bus clock cycle.
```

- Output Data Register
  - can be read or write

- Alternate Function (low/high) Register

```
Each pin has 16 different alternate functions (4 bits to config)

The datasheet describes the configurable functions for each pin.
```

## Absolute Maximum Ratings (AMR)

Absolute Maximum Ratings (AMR) define the ultimate physical limits of an electronic component. Exceeding these values will cause permanent damage to the device.

---

### Core Concepts

- **Absolute Boundaries:** These values are destruction thresholds, not normal operating ranges.
- **No Functional Guarantee:** Device operation is not guaranteed at or near these maximum limits.
- **Reliability Risk:** Exposure to AMR conditions even briefly can degrade device lifespan and reliability.

---

### Common AMR Parameters

#### 1. Voltage Limits

- **`VDD` / `VCC`**：Maximum allowable DC power supply voltage.
- **`VIN`**：Maximum voltage allowed on input pins (often capped at `VDD` + 0.3V).

#### 2. Current Limits

- **`I_OUT`:** Maximum continuous current a single output pin can source or sink.

#### 3. Temperature Limits

- **`T_J`:** Maximum allowable junction temperature inside the silicon die (typically 150°C).
- **`T_STG`:** Storage temperature range when the device is unpowered.

#### 4. ESD Ratings

- **Electrostatic Discharge:** Maximum static voltage the device can withstand (e.g., Human Body Model / HBM).

## General Operating Conditions

General Operating Conditions define the safe and verified environmental and electrical boundaries for a device.

---

### Core Concepts

- **Guaranteed Performance:** The manufacturer guarantees that the device/microcontroller will function properly and meet all datasheet specifications within this range.
- **Design Baseline:** Engineers must design the circuit to ensure the system operates strictly inside these parameters.
- **Dependent Metrics:** All electrical characteristics (e.g., speed, power consumption) are measured and validated under these conditions.
