---
id: LU7_Input_Output
aliases: []
tags: []
---

# LU7 – Input / Output

<!--toc:start-->
- [LU7 – Input / Output](#lu7-input-output)
  - [1. The I/O System](#1-the-io-system)
  - [2. I/O Devices](#2-io-devices)
  - [3. I/O Problems (Why Direct Connection Fails)](#3-io-problems-why-direct-connection-fails)
  - [4. The I/O Module](#4-the-io-module)
    - [Structure](#structure)
    - [Functions](#functions)
    - [I/O Steps](#io-steps)
  - [5. I/O Techniques / Strategies](#5-io-techniques-strategies)
    - [5.1 Programmed I/O](#51-programmed-io)
    - [5.2 Interrupt-Driven I/O](#52-interrupt-driven-io)
    - [5.3 Direct Memory Access (DMA)](#53-direct-memory-access-dma)
  - [6. I/O Channels](#6-io-channels)
  - [7. I/O Interfaces](#7-io-interfaces)
    - [Interface Types](#interface-types)
  - [8. I/O Mapping](#8-io-mapping)
  - [Summary Comparison of I/O Strategies](#summary-comparison-of-io-strategies)
<!--toc:end-->

**TMF1214 Computer Architecture | Semester 2 2024/2025**
*Reference: Chapter 7 – Stallings, Computer Organization and Architecture, 11th Ed.*

---

## 1. The I/O System

The I/O system has three main components:

1. **I/O devices** (peripherals)
2. **Device controllers / I/O modules** – mediate communication between devices and the CPU via a defined protocol
3. **I/O software** – device drivers, interrupt service routines

Three key realities about I/O:

- CPU and I/O devices cannot be natively synchronised → operations must be **coordinated**
- I/O devices are **orders of magnitude slower** than the CPU → they communicate **asynchronously**
- CPU works in machine language; devices deal with human-oriented data → **data must be encoded/decoded**

---

## 2. I/O Devices

| Category | Description | Examples |
|---|---|---|
| **Storage devices** | Secondary/auxiliary memory | Hard disk, optical disk, magnetic tape |
| **Source/sink devices** | Human ↔ computer communication | Keyboard, mouse, monitor, printer |
| **Communication devices** | Computer ↔ remote systems | Modem, NIC |

---

## 3. I/O Problems (Why Direct Connection Fails)

I/O devices cannot connect directly to the system bus because:

1. There are **many different device types**, each with its own method of operation — a CPU cannot be aware of every type
2. Most peripherals have **much lower data transfer rates** than the CPU — direct communication would slow the whole system
3. Peripherals often use **different data word sizes and formats** than the CPU

---

## 4. The I/O Module

### Structure
An I/O module sits between the system bus and peripheral devices and contains:

- Connection to the **system bus** (address, data, control lines)
- **Control logic**
- **Data buffer**
- **Interface to peripheral(s)**

### Functions
| Function | Description |
|---|---|
| **Control & Timing** | Coordinates traffic between internal resources and external devices |
| **CPU Communication** | Command decoding, data transfer, status reporting, address recognition |
| **Device Communication** | Issues commands, exchanges status and data with the device |
| **Data Buffering** | Absorbs speed mismatch between bus and peripheral |
| **Error Detection** | Detects transmission errors |

### I/O Steps
1. CPU checks I/O module device status
2. I/O module returns status
3. If ready, CPU requests data transfer
4. I/O module gets data from device
5. I/O module transfers data to CPU

---

## 5. I/O Techniques / Strategies

### 5.1 Programmed I/O
- CPU has **direct control** over all I/O operations
- CPU issues a command and then **busy-waits** (polls status bits) until the operation completes
- Simple but **wastes CPU time**
- Only practical in small microcontroller-based systems

### 5.2 Interrupt-Driven I/O
- Overcomes CPU waiting — CPU issues a command and **continues other work**
- When the device is ready, the I/O module **interrupts** the CPU via the control bus
- CPU saves context → jumps to an **interrupt service routine (ISR)** → transfers data → restores context

**Design issues:**
- **Identifying the interrupting module:**
  - Separate interrupt line per device *(limits device count)*
  - Software poll *(slow)*
  - Daisy chain / hardware poll — module places a vector on the bus
  - Bus mastering — module claims bus before raising interrupt (e.g. PCI, SCSI)
- **Multiple simultaneous interrupts:** handled via **priority levels** — higher-priority lines can interrupt lower-priority handlers

### 5.3 Direct Memory Access (DMA)
- All data in interrupt-driven I/O still flows **through the CPU** — inefficient for large transfers
- DMA adds a **DMA controller** that takes over the system bus and transfers data directly between an I/O module and main memory, **without CPU involvement**
- CPU provides the DMA controller with: direction, device address, memory address, transfer size — then continues other work
- DMA controller **interrupts CPU** when transfer is complete

**Bus sharing methods:**

| Method | Description |
|---|---|
| **Cycle stealing** | DMA takes the bus for one word at a time; CPU is briefly suspended but keeps context |
| **Burst mode** | DMA halts the CPU and controls the bus for the entire block transfer |

**DMA Configurations:**

| Config | Description | Bus uses per transfer | CPU suspensions |
|---|---|---|---|
| Single bus, detached DMA | DMA is separate on the bus | 2 (I/O→DMA, DMA→memory) | 2 |
| Single bus, integrated DMA | DMA integrated with I/O module | 1 (DMA→memory) | 1 |
| Separate I/O bus | Dedicated I/O bus; DMA on system bus | 1 (DMA→memory) | 1 |

---

## 6. I/O Channels

- An advanced I/O module with its **own dedicated processor**
- Takes on detailed I/O processing, presenting a **high-level interface** to the CPU
- CPU simply instructs the channel to perform a transfer; channel handles everything
- **Improves speed** by offloading work from the CPU (e.g. modern 3D graphics cards)

---

## 7. I/O Interfaces

The interface is the connection between an I/O module and its peripheral(s). It handles **synchronisation, control, and data transfer** via a process called **handshaking**:

1. I/O module requests permission to send data
2. Peripheral acknowledges
3. I/O module sends data
4. Peripheral acknowledges receipt

### Interface Types

| Type | Description | Use case |
|---|---|---|
| **Parallel** | Multiple wires; bits transferred simultaneously | High-speed devices (e.g. disk drives, ATA/PATA) |
| **Serial** | Single wire; bits transferred one at a time | Slower devices (e.g. keyboards, printers, SATA) |

---

## 8. I/O Mapping

| Method | Description |
|---|---|
| **Memory-mapped I/O** | Devices share the memory address space; I/O treated like memory read/write — no special instructions needed |
| **Isolated (port-mapped) I/O** | Separate address spaces for memory and I/O; requires special I/O instructions and select lines |

---

## Summary Comparison of I/O Strategies

| Strategy | CPU involvement | Efficiency | Notes |
|---|---|---|---|
| Programmed I/O | Full — CPU polls | Low | Simple; wastes CPU cycles |
| Interrupt-driven I/O | Partial — handles ISR | Medium | CPU free between transfers |
| DMA | Minimal — initiates & receives interrupt | High | Best for large data blocks |
| I/O Channel | Very low — high-level command only | Highest | Dedicated I/O processor |
