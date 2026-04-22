---
id: LU3_Function_&_Interconnection
aliases: []
tags: []
---

# LU3 — Computer Function and Interconnection

<!--toc:start-->
- [LU3 — Computer Function and Interconnection](#lu3-computer-function-and-interconnection)
  - [1. Von Neumann Architecture](#1-von-neumann-architecture)
  - [2. Program Concept](#2-program-concept)
  - [3. Computer Components (Top-Level View)](#3-computer-components-top-level-view)
  - [4. Instruction Cycle](#4-instruction-cycle)
    - [Fetch Cycle](#fetch-cycle)
    - [Execute Cycle](#execute-cycle)
    - [Program Execution Example](#program-execution-example)
    - [Instruction Cycle States (Detailed)](#instruction-cycle-states-detailed)
  - [5. Interrupts](#5-interrupts)
    - [Classes of Interrupts](#classes-of-interrupts)
    - [Interrupt Cycle (added to instruction cycle)](#interrupt-cycle-added-to-instruction-cycle)
    - [Program Flow Control Scenarios](#program-flow-control-scenarios)
    - [Multiple Interrupts](#multiple-interrupts)
  - [6. Connecting Computer Components](#6-connecting-computer-components)
  - [7. The Bus](#7-the-bus)
    - [Types of Bus in a Typical PC](#types-of-bus-in-a-typical-pc)
    - [Traditional vs. High-Performance Bus Architecture](#traditional-vs-high-performance-bus-architecture)
    - [The Three Bus Lines](#the-three-bus-lines)
    - [Bus Speed vs. Bandwidth](#bus-speed-vs-bandwidth)
    - [Single Bus Problems](#single-bus-problems)
  - [8. Elements of Bus Design](#8-elements-of-bus-design)
    - [Bus Type](#bus-type)
    - [Bus Arbitration](#bus-arbitration)
    - [Timing](#timing)
    - [Data Transfer Types](#data-transfer-types)
  - [9. PCI Bus](#9-pci-bus)
    - [PCI Bus Lines — Required](#pci-bus-lines-required)
    - [PCI Bus Lines — Optional](#pci-bus-lines-optional)
    - [PCI Transactions](#pci-transactions)
<!--toc:end-->

---

## 1. Von Neumann Architecture

Three key concepts:
1. Data and instructions share a single read-write memory.
2. Memory contents are addressable by location (not by data type).
3. Execution is **sequential** unless explicitly modified (e.g. a jump instruction).

The CPU is made up of:
- **ALU (Arithmetic Logic Unit)** — performs arithmetic and logic operations
- **Control Unit (CU)** — decodes instructions and directs other components
- **Registers** — temporary storage for intensively used data and intermediate results

The primary function of the CPU is to execute instructions fetched from main memory. Each instruction tells the CPU to perform one basic operation.

---

## 2. Program Concept

- **Hardwired systems** are inflexible — changing behaviour requires physical rewiring.
- **General-purpose hardware** can perform different tasks by supplying different control signals via software.
- A **program** is a sequence of steps; each step is an arithmetic or logical operation, and each operation requires a different set of control signals.
- Instead of rewiring, you simply supply a new program — this is the foundation of the stored-program computer.

---

## 3. Computer Components (Top-Level View)

| Register | Full Name | Purpose |
|---|---|---|
| PC | Program Counter | Holds address of the next instruction to fetch |
| IR | Instruction Register | Holds the instruction currently being executed |
| MAR | Memory Address Register | Specifies the memory address for the next read/write |
| MBR | Memory Buffer Register | Holds data to be written to memory, or data just read from memory |
| I/O AR | I/O Address Register | Identifies the I/O device/port address |
| I/O BR | I/O Buffer Register | Buffers data transferred between CPU and I/O devices |

The CPU, main memory, and I/O module are all connected via the **system bus**.

---

## 4. Instruction Cycle

The basic loop a processor runs continuously:

```
START → [Fetch Next Instruction] → [Execute Instruction] → (repeat) → HALT
```

### Fetch Cycle
1. PC holds the address of the next instruction
2. Instruction is read from that memory address into the **IR**
3. PC is incremented (unless a branch/jump modifies it)
4. CPU interprets (decodes) the instruction and performs the required action

### Execute Cycle
Possible types of operations during execution:
- **Processor-Memory** — data transfer between CPU and main memory
- **Processor-I/O** — data transfer between CPU and an I/O module
- **Data Processing** — arithmetic or logical operation on data
- **Control** — altering the sequence of operations (e.g. jump)
- **Combination** — any mix of the above

### Program Execution Example

**Instruction format:** 4-bit opcode + 12-bit address = 16-bit instruction

**Partial opcode reference:**

| Opcode | Operation |
|---|---|
| `0001` | Load AC from memory |
| `0010` | Store AC to memory |
| `0101` | Add to AC from memory |

**Internal CPU registers used:** PC, IR, AC (Accumulator — temporary storage)

**Worked trace (program adds two numbers stored at addresses 940 and 941):**

| Step | Action | PC | IR | AC |
|---|---|---|---|---|
| 1 | Fetch instruction at address 300 | 300 | `1940` | — |
| 2 | Execute: Load AC from mem[940] = 3 | 301 | `1940` | 3 |
| 3 | Fetch instruction at address 301 | 301 | `5941` | 3 |
| 4 | Execute: AC = AC + mem[941]=2 = **5** | 302 | `5941` | 5 |
| 5 | Fetch instruction at address 302 | 302 | `2941` | 5 |
| 6 | Execute: Store AC to mem[941] | 303 | `2941` | 5 |

Result: memory[941] now contains **5**.

### Instruction Cycle States (Detailed)

| Abbrev | State | Description |
|---|---|---|
| IAC | Instruction Address Calculation | Determine address of next instruction to execute |
| IF | Instruction Fetch | Read instruction from memory into processor |
| IOD | Instruction Operation Decoding | Analyse instruction: determine operation type and operands |
| OAC | Operand Address Calculation | If operation references memory/I/O, determine operand address |
| OF | Operand Fetch | Fetch operand from memory or read from I/O |
| DO | Data Operation | Perform the operation specified by the instruction |
| OS | Operand Store | Write result to memory or output to I/O |

---

## 5. Interrupts

An interrupt is a mechanism by which other modules (e.g. I/O, memory) can **interrupt the normal sequence of CPU processing** — a key way to improve processing efficiency.

### Classes of Interrupts

| Class | Cause |
|---|---|
| **Program** | Condition from instruction execution: arithmetic overflow, division by zero, illegal instruction, memory access violation |
| **Timer** | Generated by internal processor timer; allows OS to perform functions on a regular basis |
| **I/O** | Generated by I/O controller to signal normal completion or an error condition |
| **Hardware Failure** | Power failure or memory parity error |

### Interrupt Cycle (added to instruction cycle)

The full cycle becomes: **Fetch → Execute → Interrupt Check → (repeat)**

```
Execute Instruction → Check for Interrupt
  ├─ Interrupts disabled  → go back to Fetch (ignore pending interrupts)
  └─ Interrupts enabled + interrupt pending:
       1. Suspend execution of current program
       2. Save context (PC, registers)
       3. Set PC to start address of interrupt handler routine
       4. Process interrupt
       5. Restore context → resume interrupted program
```

### Program Flow Control Scenarios

| Scenario | Behaviour |
|---|---|
| **(a) No interrupts** | CPU waits idle throughout the entire I/O operation |
| **(b) Interrupts; short I/O wait** | CPU runs other instructions while I/O occurs; I/O finishes before CPU needs to return — no extra wait |
| **(c) Interrupts; long I/O wait** | CPU overlaps some work with I/O but eventually still has to wait for I/O to finish |

### Multiple Interrupts

| Strategy | Behaviour |
|---|---|
| **Sequential (Disable interrupts)** | Interrupts disabled while handling one; pending interrupts queued and handled in order after current one completes |
| **Nested (Priority-based)** | Higher-priority interrupts can preempt a lower-priority handler; processor returns to lower-priority handler when done |

**Nested time sequence example (from slides):**
- t=0: User program running
- t=10: Printer interrupt → jump to Printer ISR
- t=15: Communication interrupt (higher priority) → preempts Printer ISR → jump to Comm ISR
- t=25: Disk interrupt triggers; Comm ISR completes → jump to Disk ISR
- t=35: Disk ISR completes → return to Comm → return to Printer ISR → finishes at t=40 → return to User Program

---

## 6. Connecting Computer Components

All computer components must be interconnected. Connection type differs per component.

| Component | Sends | Receives |
|---|---|---|
| **Memory** | Data | Data, Address, Control signals (Read/Write/Timing) |
| **I/O Module** | Internal data, External data, Interrupt signals, Control signals to peripherals | Data (internal/external), Address (port number), Control from CPU |
| **CPU** | Address, Control signals, Data | Instructions, Data, Interrupt signals |

**I/O module specifics:**
- Receives control signals from CPU; sends control signals *to* peripherals (e.g. "spin disk")
- Receives port/device addresses from CPU to identify the target peripheral
- Sends interrupt signals back to CPU to signal completion or errors

---

## 7. The Bus

A **bus** is a communication pathway connecting two or more devices. Devices tap into it and can send/receive information from other connected devices.

### Types of Bus in a Typical PC

| Bus | Speed | Purpose |
|---|---|---|
| **System / Local Bus** | Fastest | Connects major components: CPU, memory, I/O |
| **High-Speed Bus** | Fast | High-bandwidth devices: graphics, video, SCSI, LAN, P1394 |
| **Expansion Bus** | Slower | Legacy/slower peripherals: modem, serial, FAX |

Slower buses connect to the system bus via a **bridge** (part of the chipset) — acts as a traffic controller integrating data from lower buses.

### Traditional vs. High-Performance Bus Architecture

| Architecture | Description |
|---|---|
| **Traditional** | Processor → Local Bus → System Bus → Expansion Bus. Simple but the system bus becomes a bottleneck for high-bandwidth devices. |
| **High-Performance** | Adds a **High-Speed Bus** tier between the system bus and expansion bus, directly hosting bandwidth-hungry devices (graphics, video, SCSI, LAN). Reduces system bus congestion. |

### The Three Bus Lines

| Line | Function | Details |
|---|---|---|
| **Data Bus** | Carries data (and instructions — no distinction at hardware level) | Width: 8, 16, 32, or 64 bits; each line carries 1 bit at a time |
| **Address Bus** | Identifies the source or destination of data | Width determines max memory capacity of the system |
| **Control Bus** | Carries timing and control signals | Full list below |

**Address bus capacity example:**
- 20 address lines (A0–A19) → 2²⁰ = **1,048,576 = 1 MB** addressable locations
- (1 K = 1024; 1 M = 1024 K)

**Full list of control bus signals:**
- Memory Read / Write signal
- I/O Read / Write signal
- Transfer ACK signal
- Bus Request / Grant signal
- Interrupt Request signal
- Interrupt ACK signal
- Clock signal
- Reset signal

### Bus Speed vs. Bandwidth

| Concept | Definition |
|---|---|
| **Bus Speed** | How many bits of information can be sent across each wire per second |
| **Bus Bandwidth (Throughput)** | Total amount of data that can theoretically be transferred on the bus per unit time |

### Single Bus Problems

Having too many devices on one bus causes:
- **Propagation delays** — long data paths mean coordination overhead adversely affects performance
- **Saturation** — if aggregate data transfer approaches bus capacity, performance degrades severely

Most systems use **multiple buses** to overcome these problems.

---

## 8. Elements of Bus Design

### Bus Type

| Type | Description | Trade-off |
|---|---|---|
| **Dedicated** | Separate physical lines for address and data at all times | Simpler, faster; requires more physical lines |
| **Multiplexed** | Shared lines for both address and data; a control line signals whether address or data is present | Fewer lines; more complex control logic, reduced performance |

### Bus Arbitration

Only one module may control (drive) the bus at a time (e.g. both CPU and DMA controller may want bus access simultaneously).

| Method | Description |
|---|---|
| **Centralised** | A single hardware arbiter receives requests and grants bus access |
| **Distributed** | Devices use distributed logic to negotiate access among themselves |

### Timing

| Type | Description |
|---|---|
| **Synchronous** | Includes a clock line in the control bus; fixed protocol relative to clock edges; fast and cheap; **all devices must run at the same clock rate** (limited by the slowest attached device) |
| **Asynchronous** | No clock; uses self-timed **handshaking** (request + acknowledge); flexible across different device speeds; higher reliability — transaction only complete when ACK received |

> I/O buses tend to be **asynchronous**; CPU-memory buses tend to be **synchronous**.

### Data Transfer Types

| Type | Description |
|---|---|
| **Read** (multiplexed) | Address sent → access time → data returned |
| **Write** (multiplexed) | Address in 1st cycle, data in 2nd cycle |
| **Write** (non-multiplexed) | Address and data sent simultaneously on separate lines in the same cycle |
| **Read** (non-multiplexed) | Address sent on address lines; data returned on separate data lines |
| **Read-Modify-Write** | Read data → modify it → write back to same address (address held throughout) |
| **Read-After-Write** | Write data, then immediately read back to verify correctness |
| **Block Transfer** | Single address → multiple sequential data words transferred back-to-back |

---

## 9. PCI Bus

- **Peripheral Component Interconnect**
- Development started **1990** by Intel; released to public domain; became standard **1995**
- **32-bit** or **64-bit**, approximately **50 lines**
- Used across: Sun Workstations, Apple Macintosh, Wintel PCs, Compaq Alpha Server
- Key advantage: the **same peripheral I/O cards** can be plugged into many different computers

### PCI Bus Lines — Required

| Category | Details |
|---|---|
| **System** | Clock and reset lines |
| **Address & Data** | 32 time-multiplexed lines for address/data; interrupt and validate lines |
| **Interface Control** | Handshaking/control signals between master (initiator) and target |
| **Arbitration** | Not shared — each device has a direct connection to the PCI bus arbiter |
| **Error** | Parity error and system error reporting lines |

### PCI Bus Lines — Optional

| Category | Details |
|---|---|
| **Interrupt lines** | Not shared between devices |
| **Cache support** | Supports cache coherency operations |
| **64-bit Bus Extension** | Additional 32 lines (time-multiplexed); 2 extra lines allow devices to negotiate 64-bit transfers |
| **JTAG / Boundary Scan** | For hardware testing and diagnostic procedures |

### PCI Transactions
- A **master (initiator)** claims the bus
- Determines and signals the transaction type (e.g. I/O read, I/O write, memory read, memory write)
- Communicates with a **target** device
- Only one master may control the bus at a time (arbitration required) Communicates with a **target** device
