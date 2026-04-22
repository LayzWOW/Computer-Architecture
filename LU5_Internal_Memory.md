---
id: LU5_Internal_Memory
aliases: []
tags: []
---

# LU5 — Internal Memory

<!--toc:start-->
- [LU5 — Internal Memory](#lu5-internal-memory)
  - [1. Introduction](#1-introduction)
  - [2. Memory Cell Basics](#2-memory-cell-basics)
    - [Functional Terminals](#functional-terminals)
  - [3. Types of Semiconductor Memory](#3-types-of-semiconductor-memory)
    - [Summary Table](#summary-table)
  - [4. Dynamic RAM (DRAM)](#4-dynamic-ram-dram)
    - [DRAM Operation](#dram-operation)
    - [DRAM Characteristics](#dram-characteristics)
  - [5. Static RAM (SRAM)](#5-static-ram-sram)
    - [SRAM Operation](#sram-operation)
    - [SRAM Characteristics](#sram-characteristics)
  - [6. DRAM vs SRAM Comparison](#6-dram-vs-sram-comparison)
  - [7. Read-Only Memory (ROM)](#7-read-only-memory-rom)
    - [ROM Variants](#rom-variants)
  - [8. Error Correction](#8-error-correction)
    - [Parity (Simple Error Detection)](#parity-simple-error-detection)
    - [Hamming Code (Single Error Correction — SEC)](#hamming-code-single-error-correction-sec)
      - [Encoding Steps](#encoding-steps)
      - [Decoding / Error Detection](#decoding-error-detection)
      - [Example (8-bit data: `10011010`)](#example-8-bit-data-10011010)
  - [9. Advanced DRAM Variants](#9-advanced-dram-variants)
<!--toc:end-->

**TMF1214 Computer Architecture | Chapter 5 — William Stallings (11th Ed.)**

---

## 1. Introduction

- **Main memory** stores programs and data currently in use by the CPU.
- All programs and data must be loaded from disk into main memory before the CPU can execute them.
- Made from **semiconductor devices** (volatile, temporary storage).
- **Secondary storage** (magnetic/optical) is used for permanent, long-term storage.

---

## 2. Memory Cell Basics

Every semiconductor memory chip is built from an array of **memory cells**. Each cell:
- Has **2 stable states** representing binary 1 and 0.
- Can be **written** to (at least once) to set its state.
- Can be **read** to sense its state.

### Functional Terminals
| Terminal | Function |
|----------|----------|
| **Select** | Selects the cell for a read or write operation |
| **Control** | Indicates whether the operation is read or write |
| **Data In** | Provides the electrical signal to write a 1 or 0 |
| **Sense** | Outputs the cell's state during a read |

---

## 3. Types of Semiconductor Memory

### Summary Table

| Memory Type | Category | Erasure | Write Mechanism | Volatility |
|---|---|---|---|---|
| RAM | Read-write | Electrically, byte-level | Electrically | Volatile |
| ROM | Read-only | Not possible | Masks | Nonvolatile |
| PROM | Read-only | Not possible | Electrically | Nonvolatile |
| EPROM | Read-mostly | UV light, chip-level | Electrically | Nonvolatile |
| EEPROM | Read-mostly | Electrically, byte-level | Electrically | Nonvolatile |
| Flash | Read-mostly | Electrically, block-level | Electrically | Nonvolatile |

---

## 4. Dynamic RAM (DRAM)

- Each bit is stored as a **charge in a capacitor** paired with a transistor.
- Capacitors **leak** charge — memory must be **periodically refreshed** (thousands of times per second) or data is lost.
- The need for constant refreshing is why it's called *dynamic* RAM.

### DRAM Operation
- **Write:** Apply voltage to the bit line (high = 1, low = 0), then signal the address line to transfer charge to the capacitor.
- **Read:** Select the address line → transistor turns on → charge flows via bit line to a **sense amplifier**, which compares it to a reference value (>50% charge = 1, otherwise 0). Capacitor must be **recharged after reading**.
- Memory is organised as a **row/column (RAS/CAS) grid**.
- Access time measured in **nanoseconds** (e.g. a 70ns chip takes 70ns to fully read and recharge each cell).

### DRAM Characteristics
- Simpler, smaller, cheaper per bit.
- Needs refresh circuitry → slower.
- Used for **main memory**.

---

## 5. Static RAM (SRAM)

- Each bit is stored in a **flip-flop** (4–6 transistors).
- No charge to leak → **no refresh needed**.
- Faster but physically larger and more expensive per bit than DRAM.

### SRAM Operation
- The transistor arrangement gives a **stable logic state** (digital, not analogue).
- **State 1:** C1 high, C2 low; T1/T4 off, T2/T3 on.
- **State 0:** C2 high, C1 low; T2/T3 off, T1/T4 on.
- Transistors T5/T6 act as the address line switch.
- **Write:** Apply value to bit line B and its complement to B̄.
- **Read:** Value is present on line B.

### SRAM Characteristics
- More complex construction, larger per bit, more expensive.
- No refresh circuits needed → faster.
- Used for **cache memory**.

---

## 6. DRAM vs SRAM Comparison

| Property | DRAM | SRAM |
|---|---|---|
| Storage mechanism | Capacitor charge | Flip-flop |
| Refresh needed | Yes | No |
| Size per bit | Smaller | Larger |
| Cost | Cheaper | More expensive |
| Speed | Slower | Faster |
| Use case | Main memory | Cache |
| Volatility | Volatile | Volatile |

---

## 7. Read-Only Memory (ROM)

- **Nonvolatile** — retains data without power.
- Used for: microprogramming, library subroutines, BIOS, function tables.

### ROM Variants
| Type | Erasure | Notes |
|---|---|---|
| **ROM** | Not possible | Written during manufacture; expensive for small runs |
| **PROM** | Not possible | Programmed once using special equipment (blowing fuses) |
| **EPROM** | UV light (chip-level) | Can be erased and reprogrammed |
| **EEPROM** | Electrically (byte-level) | Much slower to write than to read |
| **Flash** | Electrically (block-level) | Entire block erased in 1–2 seconds |

---

## 8. Error Correction

Memory errors fall into two categories:

| Type | Description |
|---|---|
| **Hard failure** | Permanent hardware fault — consistently returns wrong results. Caused by loose modules, blown chips, motherboard defects. |
| **Soft error** | A bit reads incorrectly once but then works fine again. Random, non-destructive, no permanent damage. |

### Parity (Simple Error Detection)
- An extra **parity bit** is appended to each byte.
- **Even parity:** parity bit set to 1 if the number of 1s in the byte is odd (so total count of 1s becomes even).
- On read, the parity is recalculated and compared. A mismatch indicates an error.
- **Limitation:** Detects odd numbers of bit errors; two simultaneous errors cancel out and go undetected.

### Hamming Code (Single Error Correction — SEC)
- Uses **multiple parity bits** placed at power-of-two positions (1, 2, 4, 8, …) to pinpoint the exact faulty bit.
- For 4-bit data → 3 parity bits → 7-bit code word.
- Each parity bit covers a specific subset of bit positions:
  - **p1** covers bits 1, 3, 5, 7
  - **p2** covers bits 2, 3, 6, 7
  - **p3** covers bits 4, 5, 6, 7

#### Encoding Steps
1. Place data bits at non-power-of-two positions (3, 5, 6, 7, …).
2. Place parity bits at power-of-two positions (1, 2, 4, 8, …).
3. Set each parity bit so that the total number of 1s in its covered group is **even**.

#### Decoding / Error Detection
- On read, recalculate check bits (c1, c2, c3) for each parity group.
- If a group has odd parity, its check bit = 1; otherwise 0.
- The **binary value of the check bits** (c3 c2 c1) gives the position of the bad bit.
- **c = 000** means no error.
- **Example:** c = 101 → error in bits covered by p1 and p3, but not p2 → bit position **5**.

#### Example (8-bit data: `10011010`)
- 4 parity bits needed (positions 1, 2, 4, 8) → 12-bit code word.
- Final code word: `011100101010`
- If received word is `011100101110`, parity bits 2 and 8 fail → 2 + 8 = **bit 10** is the bad bit.

---

## 9. Advanced DRAM Variants

| Type | Description |
|---|---|
| **Enhanced DRAM** | Contains a small SRAM to hold the last line read |
| **Cache DRAM** | Larger SRAM component; can act as cache or serial buffer |
| **SDRAM** | Synchronous — access synchronised with the system clock; CPU knows exactly when data will be ready; supports **burst mode** |
| **DDR-SDRAM** | Sends data on both the rising and falling clock edge (twice per cycle) |
| **RAMBUS (RDRAM)** | Competitor to SDRAM; 28-wire bus <12cm; up to 1.6 Gbps; used in Pentium/Itanium platforms |
