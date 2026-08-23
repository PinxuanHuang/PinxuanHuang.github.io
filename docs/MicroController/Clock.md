---
sidebar_position: 1
title: Clock
sidebar_label: Clock
---

# Introduction

An introduction to the clock. Use STM32 Microcontroller for example.

## Understanding SYSCLK and Domain Clocks

The heartbeat of the microcontroller is the **SYSCLK (System Clock)**. This is the primary clock from which all other peripheral clocks are derived, including:

- **HCLK:** Clock to the AHB domain (High-performance bus).
- **PCLK1 / PCLK2:** Clocks to the APB domains (Peripheral buses).
- Specific clocks for the processor, USB, Ethernet, Timers, etc.

## Primary System Clock Sources

The microcontroller provides three primary sources to drive the SYSCLK:

### 1. HSI (High-Speed Internal Oscillator)

The HSI is an internal RC oscillator (typically 16 MHz).

- **Advantages:** It provides a low-cost clock source since it requires no external components. It also boasts a very fast startup time (around 2 µs) and serves as the automatic backup if the HSE fails.
- **Disadvantages:** It is less accurate than an external crystal. Its accuracy is highly dependent on temperature; while it may have only a 1% variation at 25°C, the frequency can drift significantly (e.g., -8% to +4.5%) as the temperature increases.

### 2. HSE (High-Speed External Oscillator)

The HSE relies on an external crystal oscillator connected to the microcontroller (e.g., an 8 MHz signal from the ST-Link circuitry on a NUCLEO board).

- **Advantages:** It is highly accurate and stable across varying temperatures compared to the HSI.
- **Disadvantages:** It requires external hardware (crystal and capacitors) and has a slower startup time.

### 3. PLL (Phase-Locked Loop)

The PLL is an internal engine that takes a base clock (from either HSI or HSE) and multiplies it to achieve much higher frequencies.

## Secondary Clock Sources

The microcontroller also features two secondary clock sources. These operate at much lower frequencies (~32 kHz) and are not used to drive the main SYSCLK. Instead, they are dedicated to specific low-power peripherals:

- **LSI (Low-Speed Internal):** An internal ~32 kHz RC oscillator used primarily to drive the independent Watchdog Timer or the RTC (Real-Time Clock). It is less accurate but always available.
- **LSE (Low-Speed External):** An external 32.768 kHz crystal oscillator. It provides a highly accurate and stable clock specifically for the RTC.

## Default Clock State and Power Management

- **HSI:** **ON**
- **HSE:** **OFF**
- **PLL:** **OFF**
- **LSI / LSE:** **OFF**
