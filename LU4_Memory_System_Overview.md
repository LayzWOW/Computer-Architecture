---
id: LU4_Memory_System_Overview
aliases: []
tags: []
---

# LU4 — Computer Memory System Overview

<!--toc:start-->
- [LU4 — Computer Memory System Overview](#lu4-computer-memory-system-overview)
  - [Characteristics of Memory Systems](#characteristics-of-memory-systems)
  - [Memory Hierarchy](#memory-hierarchy)
    - [Locality of Reference](#locality-of-reference)
  - [Cache Memory](#cache-memory)
  - [Elements of Cache Design](#elements-of-cache-design)
    - [1. Cache Size](#1-cache-size)
    - [2. Mapping Functions](#2-mapping-functions)
    - [3. Replacement Algorithms](#3-replacement-algorithms)
    - [4. Write Policies](#4-write-policies)
    - [5. Line Size](#5-line-size)
    - [6. Number of Caches](#6-number-of-caches)
<!--toc:end-->

*TMF1214 Computer Architecture | Chapter 4 — Stallings 11th Ed.*

---

## Characteristics of Memory Systems

Memory systems are described across eight characteristics:

1. **Location** — CPU/internal (main memory: fast, expensive, close to CPU) or external/secondary (slow, cheap, remote).
2. **Capacity** — Measured in word size (typically bytes) and number of words. Common word lengths: 8, 16, 32 bits.
3. **Unit of Transfer** — Internal: governed by data bus width. External: larger blocks. The *addressable unit* is the smallest uniquely addressable location.
4. **Access Methods**:
   - *Sequential* — read in order from start (e.g. tape)
   - *Direct* — jump to vicinity + sequential search (e.g. disk)
   - *Random* — exact address, access time independent of location (e.g. RAM)
   - *Associative* — located by content comparison, access time independent of location (e.g. cache)
5. **Performance**:
   - *Access time* — time from address to valid data
   - *Memory cycle time* — access + recovery time
   - *Transfer rate* — for direct access: `TN = TA + N/R`
6. **Physical Types** — Semiconductor (RAM), Magnetic (disk/tape), Optical (CD/DVD), others (bubble, hologram).
7. **Physical Characteristics** — *Volatile* (loses data on power off) vs *non-volatile*; *erasable* vs *non-erasable*.
   - ROM → PROM → EPROM → EEPROM → Flash (each progressively more flexible to erase/reprogram)
8. **Organisation** — Physical arrangement of bits into words (e.g. interleaved).

---

## Memory Hierarchy

Fast memory is expensive, so designers *tier* memory — small amounts of fast memory backed by larger, slower memory.

```
CPU Registers    ← fastest, most expensive, smallest
Cache (L1, L2)
RAM (Physical + Virtual)
Storage (ROM/BIOS, HDD, Removable, Network)
Input Sources    ← slowest, cheapest, largest
```

Going **down** the hierarchy: increasing capacity, increasing access time, decreasing cost/bit.

### Locality of Reference

Cache effectiveness relies on the fact that programs tend to cluster memory references:

- **Temporal locality** — recently accessed items are likely to be accessed again soon.
- **Spatial locality** — items near a recently accessed address are likely to be accessed soon.

---

## Cache Memory

A small, fast memory placed between the CPU and main memory. Its operation is **transparent to the CPU**.

- CPU–Cache: word transfers
- Cache–Main Memory: block transfers
- A **cache hit** = required word is found in cache
- A **cache miss** = word not in cache; main memory is accessed and the relevant block is loaded into cache

Typical access times:
| Level | Speed | Size |
|---|---|---|
| L1 Cache | ~10 ns | 4–16 KB |
| L2 Cache (SRAM) | ~20–30 ns | 128–512 KB |
| Main Memory (RAM) | ~60 ns | 32–128 MB |
| Hard Disk | ~12 ms | 1–10 GB |

Both cache and main memory are divided into equal-sized **lines** (blocks). Each cache line has a **tag** identifying which main memory block it holds.

---

## Elements of Cache Design

### 1. Cache Size
- Small → low cost; Large → high hit ratio
- Common range: 1 KB – 256 KB

### 2. Mapping Functions

Determines which cache line(s) a main memory block can occupy.

| Scheme | Description | Address Fields |
|---|---|---|
| **Direct** | Each memory block maps to exactly one cache line | tag + block + word |
| **Associative** | Any memory block can go in any cache line | tag + word |
| **Set Associative** | Each block maps to a *set* of cache lines | tag + set + word |

**Direct Mapping**
- Simple, fast, short tag field
- Disadvantage: two blocks mapping to the same line cause constant replacement (thrashing), even if most of the cache is empty

**Associative Mapping**
- Maximum flexibility, highest hit ratio potential
- Disadvantage: complex, long tags, expensive associative memory required

**Set Associative Mapping**
- Compromise between the two; most commonly used in practice
- Requires a replacement algorithm

### 3. Replacement Algorithms

Needed for associative and set-associative caches (direct mapping has no choice).

| Algorithm | Strategy | Notes |
|---|---|---|
| **LRU** | Replace least recently used line | Most effective; hardware order tracking required |
| **FIFO** | Replace oldest line | Simple to implement |
| **LFU** | Replace least frequently used | Uses access bits, cleared periodically |
| **Random** | Replace random line | Simplest; surprisingly competitive results |

Must be implemented in **hardware** to be effective.

### 4. Write Policies

The problem: how to keep cache and main memory consistent after a write?

| Policy | Behaviour | Drawback |
|---|---|---|
| **Write Through** | Write to both cache and main memory simultaneously | High bus traffic / bottleneck |
| **Write Back** | Write only to cache; set *dirty bit*; flush to memory only on eviction | Main memory temporarily invalid; complex circuitry |
| **Write Once** | First write updates both (write-through); subsequent writes invalidate other caches | Reduces bus traffic vs pure write-through |

### 5. Line Size
- Larger lines: better spatial locality exploitation
- Smaller lines: more lines fit in cache, faster load time, avoids loading irrelevant data
- Common: **4–16 words**

### 6. Number of Caches

**Single vs Multi-level:**
- Modern CPUs typically have L1, L2, (and L3) caches
- Each level trades off speed vs size vs cost
- Cache coherence is the main complexity introduced by multiple levels

**Unified vs Split:**
| Type | Description | Advantage |
|---|---|---|
| Unified | One cache for both instructions and data | Flexible allocation |
| Split | Separate instruction cache (read-only) and data cache | Supports pipeline processing; parallel access |
