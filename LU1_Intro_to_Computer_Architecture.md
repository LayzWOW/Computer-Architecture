---
id: LU1_Intro_to_Computer_Architecture
aliases: []
tags: []
---

# TMF1214 Computer Architecture — LU1 Summary

<!--toc:start-->
- [TMF1214 Computer Architecture — LU1 Summary](#tmf1214-computer-architecture-lu1-summary)
  - [1. Architecture vs. Organisation](#1-architecture-vs-organisation)
  - [2. Structure & Function](#2-structure-function)
    - [The Four Core Computer Functions](#the-four-core-computer-functions)
  - [3. Top-Level Computer Structure](#3-top-level-computer-structure)
    - [Component Roles](#component-roles)
  - [4. CPU Internal Structure](#4-cpu-internal-structure)
    - [Control Unit Internals](#control-unit-internals)
  - [5. Computer Systems Hierarchy (Software Layers)](#5-computer-systems-hierarchy-software-layers)
  - [6. Brief History of Computers](#6-brief-history-of-computers)
    - [Generation 1 — Vacuum Tubes (1945–1955)](#generation-1-vacuum-tubes-19451955)
    - [Generation 2 — Transistors (1955–1965)](#generation-2-transistors-19551965)
    - [Generation 3 — Integrated Circuits (1965–1980)](#generation-3-integrated-circuits-19651980)
    - [Generation 4 — VLSI (1980–present)](#generation-4-vlsi-1980present)
    - [IC Density Milestones](#ic-density-milestones)
  - [7. Moore's Law](#7-moores-law)
  - [8. The IAS (von Neumann) Machine](#8-the-ias-von-neumann-machine)
    - [Key Registers](#key-registers)
    - [Instruction Cycle (two sub-cycles)](#instruction-cycle-two-sub-cycles)
  - [9. Evolution of Intel Microprocessors](#9-evolution-of-intel-microprocessors)
    - [1970s](#1970s)
    - [Key 1980s–1990s Milestones](#key-1980s1990s-milestones)
  - [10. Performance Challenges & Solutions](#10-performance-challenges-solutions)
    - [Problem 1: Processor–Memory Speed Gap](#problem-1-processormemory-speed-gap)
    - [Problem 2: I/O Device Bandwidth Demands](#problem-2-io-device-bandwidth-demands)
    - [Techniques for Speeding Up Processors](#techniques-for-speeding-up-processors)
  - [11. Physical Limits & the Shift to Multi-Core](#11-physical-limits-the-shift-to-multi-core)
    - [Challenges with Increasing Clock Speed](#challenges-with-increasing-clock-speed)
    - [Multi-Core Solution](#multi-core-solution)
  - [12. Key Balance Principle](#12-key-balance-principle)
<!--toc:end-->

> Reference: Chapters 1 & 2, William Stallings *Computer Organization and Architecture* (10th/11th Ed.)

---

## 1. Architecture vs. Organisation

| Concept | Definition | Example |
|---|---|---|
| **Architecture** | Attributes visible to the programmer | Is there a multiply instruction? |
| **Organisation** | How those features are implemented in hardware | Is there a dedicated multiply unit, or is it done via repeated addition? |

- All Intel x86 family members share the **same basic architecture** → enables **code compatibility** (at least backwards)
- Organisation differs between versions of the same family

---

## 2. Structure & Function

- **Structure** — how components *relate* to each other
- **Function** — the *operation* of each individual component within that structure

### The Four Core Computer Functions
1. **Data Processing** — performing computations on data
2. **Data Storage** — temporary (registers/RAM) or permanent (disk); even "on-the-fly" processing requires at least temporary storage
3. **Data Movement** — I/O (between computer and external devices) vs. data communication (between systems)
4. **Control** — coordinating all of the above via the Control Unit (CU)

---

## 3. Top-Level Computer Structure

```
Computer
├── CPU (Central Processing Unit)
├── Main Memory
├── I/O
└── System Interconnection / Bus
```

### Component Roles
- **CPU** — controls operation and performs data processing; also called the *processor*
- **Main Memory** — stores data and instructions
- **I/O** — moves data between the computer and the outside world
- **System Bus** — a set of conducting wires providing communication among CPU, memory, and I/O

---

## 4. CPU Internal Structure

```
CPU
├── ALU (Arithmetic and Logic Unit)
├── Control Unit (CU)
├── Registers
└── Internal CPU Interconnection (Internal Bus)
```

- **ALU** — performs arithmetic and logical operations
- **Control Unit** — controls the operation of the CPU (and therefore the entire computer)
- **Registers** — fast internal storage within the CPU
- **Internal Bus** — connects ALU, CU, and registers

### Control Unit Internals
- Sequencing Logic
- Control Unit Registers & Decoders
- Control Memory

---

## 5. Computer Systems Hierarchy (Software Layers)

From highest to lowest abstraction:

1. High-level languages (C++, Java, VB) — *programmers*
2. Assembly language
3. Operating System (UNIX, Windows NT)
4. Instruction Sets (Pentium, PowerPC)
5. Micro-programs — *systems programmers*
6. Hardware

A **program** is a sequence of instructions describing how to perform a task. Human language must be **interpreted/translated** → machine-like/human-like language → machine language → computer.

---

## 6. Brief History of Computers

### Generation 1 — Vacuum Tubes (1945–1955)
- **ENIAC** (1943–1946): 18,000+ vacuum tubes, 1,500 sq ft, 30 tons, 140 kW; decimal machine; programmed via switches and jumper cables
- **John von Neumann** (1945–1952):
  - Introduced **binary arithmetic**
  - Proposed the **stored-program concept** — main memory holds *both* data and instructions
  - IAS architecture: Memory, ALU, Program Control Unit, Input, Output
  - Almost all modern computers are **von Neumann machines**

### Generation 2 — Transistors (1955–1965)
- Transistor invented 1948 at Bell Labs (Bardeen, Brattain, Shockley)
- **TX-0** — first transistor computer (MIT)
- **DEC PDP-1** — first affordable microcomputer ($120,000)
- **PDP-8** — $16,000; first to use a **single bus**

### Generation 3 — Integrated Circuits (1965–1980)
- **IBM System/360** — shared assembly language across a family; first to allow microprogramming; both scientific and commercial use

### Generation 4 — VLSI (1980–present)
- Very Large Scale Integration → thousands of transistors on one chip
- Led to the PC revolution; high performance at low cost

### IC Density Milestones

| Era | Technology | Devices per Chip |
|---|---|---|
| 1946–1957 | Vacuum tube | — |
| 1958–1964 | Transistor | — |
| 1965+ | SSI (Small Scale Integration) | up to 100 |
| to 1971 | MSI (Medium Scale) | 100–3,000 |
| 1971–1977 | LSI (Large Scale) | 3,000–100,000 |
| 1978–1991 | VLSI | 100,000–100,000,000 |
| 1991– | ULSI (Ultra Large Scale) | >100,000,000 |

---

## 7. Moore's Law

> The number of transistors on a chip doubles roughly every **18 months**, while cost remains nearly unchanged.

- Proposed by **Gordon Moore**, co-founder of Intel
- Consequences:
  - Higher packing density → shorter electrical paths → **higher performance**
  - Smaller size → increased flexibility
  - Reduced power and cooling requirements
  - Fewer interconnections → **improved reliability**

---

## 8. The IAS (von Neumann) Machine

**Stored-program concept** underpins all modern computers.

### Key Registers
| Register | Full Name | Purpose |
|---|---|---|
| MBR | Memory Buffer Register | Holds data being transferred to/from memory |
| MAR | Memory Address Register | Holds address of memory location to access |
| IR | Instruction Register | Holds opcode of current instruction |
| IBR | Instruction Buffer Register | Temporarily holds the next instruction |
| PC | Program Counter | Holds address of next instruction |
| AC | Accumulator | Holds intermediate ALU results |
| MQ | Multiplier Quotient | Used during multiplication/division |

### Instruction Cycle (two sub-cycles)
1. **Fetch Cycle** — opcode of next instruction loaded into IR; address portion into MAR
2. **Execute Cycle** — control circuitry interprets the opcode; sends control signals to move data or trigger ALU operations

Memory: 1000 × 40-bit words; each word holds **two 20-bit instructions**

---

## 9. Evolution of Intel Microprocessors

### 1970s
| Processor | Year | Clock | Bus Width | Transistors | Max RAM |
|---|---|---|---|---|---|
| 4004 | 1971 | 108 kHz | 4-bit | 2,300 | 640 B |
| 8008 | 1972 | 200 kHz | 8-bit | 3,500 | 16 KB |
| 8080 | 1974 | 2 MHz | 8-bit | 6,000 | 64 KB |
| 8086 | 1978 | 5–10 MHz | 16-bit | 29,000 | 1 MB |
| 8088 | 1979 | 5–8 MHz | 8-bit (external) | 29,000 | 1 MB |

### Key 1980s–1990s Milestones
- **80286** — 16 MB addressable memory (up from 1 MB)
- **80386** — first 32-bit x86; multitasking support
- **80486** — first L1 cache **on-chip** (1.2M transistors)
- **Pentium** — 64-bit bus; superscalar (5× the 486 DX at 33 MHz)
- **Pentium Pro** — dynamic execution architecture
- **Pentium II** — MMX technology (graphics/video/audio)
- **Pentium III** — additional floating-point instructions for 3D graphics
- **Pentium 4** — further FP and multimedia; Arabic numeral naming
- **Itanium** — 64-bit architecture

---

## 10. Performance Challenges & Solutions

### Problem 1: Processor–Memory Speed Gap
- Processor speed has outpaced memory speed significantly
- **Solutions:**
  - Widen DRAM (retrieve more bits at once)
  - Change DRAM interface → **cache**
  - Reduce memory access frequency → **larger, on-chip caches**
  - Increase interconnection bandwidth (high-speed / hierarchical buses)

### Problem 2: I/O Device Bandwidth Demands
- Peripherals demand large, fast data throughput
- **Solutions:**
  - Caching & buffering
  - Higher-speed interconnection buses
  - More elaborate bus hierarchies
  - Multiple-processor configurations

### Techniques for Speeding Up Processors
- **Pipelining** — instruction-level parallelism (assembly-line style)
- **On-board cache** (L1 & L2)
- **Branch prediction**
- **Data flow analysis**
- **Speculative execution**
- **Superscalar** — multiple pipelines; independent instructions execute in parallel

---

## 11. Physical Limits & the Shift to Multi-Core

### Challenges with Increasing Clock Speed
- **Power/heat** — power density grows with logic density and clock speed
- **RC delay** — resistance and capacitance of wires increase as they get thinner/closer
- **Memory latency** — DRAM speeds continue to lag far behind processor speeds

### Multi-Core Solution
- Put **multiple simpler processors** on one chip with a **large shared cache**
- Performance scales almost linearly with core count *if* software can exploit parallelism
- Cache power consumption is lower than processing logic power consumption
- Pentium used ~10% of die area for cache; Pentium 4 used ~50%

---

## 12. Key Balance Principle

> Effective system performance requires balance across: **processor**, **main memory**, **I/O devices**, and **interconnection structures**.

No single component should be a bottleneck; improvements must be coordinated across all layers.
