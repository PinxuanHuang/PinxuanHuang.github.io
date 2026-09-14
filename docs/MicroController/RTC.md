---
sidebar_position: 1
title: RTC
sidebar_label: RTC
---

# Introduction

An introduction to RTC(Real-Time Clock) with MCU(STM32)

## What is an RTC?

An **RTC (Real-Time Clock)** is a hardware peripheral commonly integrated into microcontrollers.

Its main purpose is to:

- Keep track of the current time.
- Maintain calendar information.
- Generate alarms at specific times.
- Wake the MCU from low-power modes.
- Preserve small amounts of important application data.
- Continue operating independently from the main CPU.

The RTC is designed to continue keeping time even when the main processor is not actively executing application code.

## RTC Calendar Unit

The **Calendar Unit** is a programmable block inside the RTC peripheral. After the software loads the initial time and date, the RTC automatically keeps updating the calendar information.

The calendar can track:

- **Time:** sub-seconds, seconds, minutes, and hours.
- **Hour format:** 12-hour or 24-hour format.
- **Date:** day of week, day of month, month, and year.
- **Month length:** automatic handling for 28, 29, 30, and 31-day months.
- **Leap year:** automatic compensation for leap-year February.

### Calendar Register Overview

| Calendar Information       | Register  | Notes                                                                                   |
| :------------------------- | :-------- | :-------------------------------------------------------------------------------------- |
| Date, month, weekday, year | `RTC_DR`  | Used to configure and read date fields.                                                 |
| Hours, minutes, seconds    | `RTC_TR`  | Used to configure and read time fields.                                                 |
| Sub-second counter         | `RTC_SSR` | Handled automatically by RTC hardware; not directly programmable like date/time fields. |

### Important Calendar Rules

- STM32 RTC calendar fields are programmed in **BCD (Binary-Coded Decimal)** format.
- The RTC year field starts from **2000**. Values earlier than 2000 are not represented by the calendar year field.
- Date and time fields must be written into the correct unit/tens bit fields in the RTC registers.
- After initialization, the RTC maintains the date and time by itself.

## RTC Clock Source

The RTC requires a clock source before the calendar, alarm, or wake-up timer can operate.

STM32 RTC can typically be driven by one of three clock sources:

| Clock Source | Typical Frequency                                    | Accuracy          | Notes                                                                                              |
| :----------- | :--------------------------------------------------- | :---------------- | :------------------------------------------------------------------------------------------------- |
| `LSE`        | `32.768 kHz`                                         | High              | External low-speed crystal oscillator. Best choice for accurate calendar applications.             |
| `LSI`        | Around `32 kHz` or `37 kHz`, depending on MCU series | Low               | Internal low-speed RC oscillator. Convenient, but not accurate enough for production calendar use. |
| `HSE`        | Board-dependent, then divided down                   | Depends on source | External high-speed oscillator. Usually divided before being used by RTC.                          |

The selected source becomes the **RTC clock**. The clock source is selected through the `RTCSEL` bits in the `RCC_BDCR` register.

### RTC Clock Path

The RTC clock is divided by prescalers before it reaches the calendar unit.

```text
LSE / LSI / HSE
      |
      v
 RTC clock source mux
      |
      v
 Asynchronous prescaler
      |
      v
 Synchronous prescaler
      |
      v
 1 Hz calendar clock
      |
      v
 Calendar unit
```

The calendar unit must receive a **1 Hz** clock because calendar time advances once per second. If the prescalers are configured incorrectly, the calendar may run too fast, too slow, or not behave correctly.

### Prescaler Examples

The RTC prescalers divide the selected clock source down to 1 Hz.

| RTC Source | Input to RTC Prescalers      | Asynchronous Division | Synchronous Division | Result                        |
| :--------- | :--------------------------- | :-------------------- | :------------------- | :---------------------------- |
| `LSE`      | `32768 Hz`                   | `128`                 | `256`                | `32768 / 128 / 256 = 1 Hz`    |
| `LSI`      | `32000 Hz`                   | `128`                 | `250`                | `32000 / 128 / 250 = 1 Hz`    |
| `HSE`      | `1 MHz` after HSE prescaling | `125`                 | `8000`               | `1000000 / 125 / 8000 = 1 Hz` |

Register values are commonly programmed as division value minus one:

| Desired Division | Register Value Example |
| :--------------- | :--------------------- |
| Divide by `128`  | Program `127`          |
| Divide by `256`  | Program `255`          |
| Divide by `250`  | Program `249`          |
| Divide by `125`  | Program `124`          |
| Divide by `8000` | Program `7999`         |

### Clock Source Selection Guideline

- Use **LSE** when accurate date and time keeping is important.
- Use **LSI** only when approximate timing is acceptable.
- Avoid LSI for production calendar applications because RC oscillator accuracy is much worse than a crystal oscillator.
- When using **HSE**, first divide it to the required RTC input range, then configure the RTC asynchronous and synchronous prescalers to generate 1 Hz.

### BCD vs Binary in RTC

BCD stores each decimal digit separately in 4-bit groups. This is different from normal binary encoding.

| Decimal Value | Binary Representation | BCD Representation | Hex BCD Value |
| :------------ | :-------------------- | :----------------- | :------------ |
| 2             | `0010`                | `0010`             | `0x02`        |
| 10            | `1010`                | `0001 0000`        | `0x10`        |
| 12            | `1100`                | `0001 0010`        | `0x12`        |
| 40            | `101000`              | `0100 0000`        | `0x40`        |
| 58            | `111010`              | `0101 1000`        | `0x58`        |

For values `0` to `9`, binary and BCD look the same. For values `10` and above, BCD uses one 4-bit group for each decimal digit.

- RTC Time Register Example

To program **02:40:58 AM** into `RTC_TR`, split every decimal field into tens and units.

| Field   | Decimal Value | Tens Digit | Units Digit | BCD Value |
| :------ | :------------ | :--------- | :---------- | :-------- |
| Hours   | `02`          | `0`        | `2`         | `0x02`    |
| Minutes | `40`          | `4`        | `0`         | `0x40`    |
| Seconds | `58`          | `5`        | `8`         | `0x58`    |

- RTC Date Register Example

To program **12 June 2018** into `RTC_DR`, write each date field in BCD format.

| Field        | Decimal Value | BCD Value | Register Usage                                      |
| :----------- | :------------ | :-------- | :-------------------------------------------------- |
| Day of month | `12`          | `0x12`    | Date tens and date units fields.                    |
| Month        | `06`          | `0x06`    | Month tens and month units fields.                  |
| Year         | `18`          | `0x18`    | Year tens and year units fields, representing 2018. |

- Summary:
  - Use `RTC_DR` to configure or read the current date.
  - Use `RTC_TR` to configure or read the current time.
  - Use `RTC_SSR` to read sub-second information.
  - Always convert decimal calendar values into BCD before writing RTC calendar registers.
