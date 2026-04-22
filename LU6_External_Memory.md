---
id: LU6_External_Memory
aliases: []
tags: []
---

# LU6 - External Memory

<!--toc:start-->
- [LU6 - External Memory](#lu6-external-memory)
  - [Types of External Memory](#types-of-external-memory)
  - [Magnetic Disk (HDD)](#magnetic-disk-hdd)
    - [Basics](#basics)
    - [Key Components](#key-components)
    - [Read/Write Mechanisms](#readwrite-mechanisms)
    - [Data Organisation](#data-organisation)
    - [Disk Velocity](#disk-velocity)
    - [Performance Metrics](#performance-metrics)
    - [Physical Characteristics](#physical-characteristics)
    - [Multiple Platters](#multiple-platters)
    - [Winchester Hard Disk](#winchester-hard-disk)
  - [Removable Storage](#removable-storage)
    - [Floppy Disk](#floppy-disk)
    - [Zip Disk (Magnetic)](#zip-disk-magnetic)
    - [Jaz Cartridge (Magnetic)](#jaz-cartridge-magnetic)
    - [Portable Drives](#portable-drives)
  - [RAID (Redundant Array of Independent Disks)](#raid-redundant-array-of-independent-disks)
    - [Overview](#overview)
    - [Why Use RAID?](#why-use-raid)
    - [Key Concepts](#key-concepts)
    - [RAID Levels Summary](#raid-levels-summary)
  - [Optical Storage](#optical-storage)
    - [CD-ROM](#cd-rom)
    - [CD-R](#cd-r)
    - [CD-RW](#cd-rw)
    - [DVD](#dvd)
  - [Magnetic Tape](#magnetic-tape)
    - [Overview](#overview-1)
    - [Standards](#standards)
<!--toc:end-->

**TMC1214 Computer Architecture | Semester 2 2024/2025**
*Reference: Chapter 6 — William Stallings, Computer Organization and Architecture, 11th Edition*

---

## Types of External Memory

1. **Magnetic Disk** — RAID, Removable
2. **Optical** — CD-ROM, CD-R, CD-R/W, DVD
3. **Magnetic Tape**

---

## Magnetic Disk (HDD)

### Basics
- Hard disks invented in the 1950s; originally called "fixed disks" or "Winchesters"
- Uses magnetic recording techniques (similar to cassette tape)
- Magnetic medium can be erased/rewritten and retains data for many years
- Substrate (formerly aluminium, now **glass**) coated with magnetizable material
  - Glass improves surface uniformity, reduces defects, allows lower fly heights, and better shock resistance

### Key Components
| Component | Description |
|-----------|-------------|
| **Platters** | Actual disks coated with magnetic material that store data |
| **Spindle/Motor** | Rotates all platters in unison at 3,600–7,200 RPM |
| **Read/Write Heads** | One per platter side; all mounted on a single actuator shaft |
| **Head Actuator** | Moves heads across platters; modern drives use a **voice coil actuator** (servo-based) |

> Heads float on a boundary layer of air when spinning. The gap between head and platter is extremely small — even dust can cause failure. Assembly must be done in a **clean room**.

### Read/Write Mechanisms
- **Write:** Current through coil → magnetic field → pattern recorded on platter surface
- **Read (traditional):** Moving magnetic field induces current in the same coil
- **Read (contemporary — MR):** Separate magneto-resistive (MR) sensor; resistance varies with magnetic field direction → higher density and speed

### Data Organisation
- Data stored as concentric **tracks**, divided into **sectors** (pie-shaped wedges)
- Tracks separated by **inter-track gaps**; sectors by **inter-sector gaps**
- Minimum block size = one sector; sectors often grouped into **clusters**
- **Low-level formatting** establishes tracks and sectors
- **High-level formatting** writes file-storage structures (e.g. FAT)

### Disk Velocity
- **Constant Angular Velocity (CAV):** Same RPM throughout; pie-shaped sectors; wastes space on outer tracks
- **Multiple Zoned Recording (MZR):** Outer zones have more sectors per track; increases capacity but adds complexity

### Performance Metrics
- **Seek time** — time to move head to correct track
- **Rotational latency** — waiting for data to rotate under head
- **Access time** = Seek + Latency
- **Transfer rate** — bytes per second delivered to CPU (typically 5–40 MB/s)
- Seek times: typically 10–20 ms

### Physical Characteristics
| Characteristic | Options |
|----------------|---------|
| Head motion | Fixed (rare) or Movable |
| Portability | Removable or Fixed |
| Sides | Single or Double |
| Platters | Single or Multiple |
| Head mechanism | Contact (Floppy), Fixed gap, Flying (Winchester) |

### Multiple Platters
- One head per side; heads aligned → form **cylinders**
- Data striped by cylinder → reduces head movement, increases transfer rate

### Winchester Hard Disk
- Developed by IBM (1973); sealed unit with one or more platters
- Heads fly on boundary layer of air; very small head-to-disk gap
- Fastest type of external storage

---

## Removable Storage

### Floppy Disk
- Sizes: 8", 5.25", 3.5" — capacity up to 1.44 MB
- Invented by IBM (Alan Shugart, 1967)
- Slow, cheap, universal — largely obsolete

### Zip Disk (Magnetic)
- Higher-quality magnetic coating → smaller read/write head (×10 smaller than floppy)
- Thousands of tracks per inch → up to 750 MB capacity

### Jaz Cartridge (Magnetic)
- Essentially a hard disk in a removable plastic case
- Platters inside cartridge; heads and motor stay in the drive unit

### Portable Drives
- External USB hard drives; drive mechanism and media in one sealed case

---

## RAID (Redundant Array of Independent Disks)

### Overview
- Multiple physical disks viewed as a **single logical drive** by the OS
- Data distributed across physical drives; redundant capacity stores parity info
- Coined at UC Berkeley (1987); originally "Inexpensive", now often "Independent"

### Why Use RAID?
1. **Higher Data Security** — redundancy protects against single-disk failure
2. **Fault Tolerance** — more reliable than a single disk
3. **Improved Availability** — fault tolerance + hot-swap recovery
4. **Improved Performance** — parallel use of multiple drives

### Key Concepts
| Concept | Description |
|---------|-------------|
| **Mirroring** | All data written to two disks simultaneously; 100% redundancy; used in RAID 1 |
| **Striping** | Files split into blocks/bytes distributed across drives; improves performance |
| **Parity** | Extra redundancy data computed from N data blocks; stored on N+1 drives; allows recovery if one drive fails |

### RAID Levels Summary

| Level | Technique | Redundancy | Notes |
|-------|-----------|------------|-------|
| **RAID 0** | Block striping, no parity | ❌ None | Best performance, no fault tolerance |
| **RAID 1** | Mirroring / Duplexing | ✅ Full | 50% overhead; fast recovery; expensive |
| **RAID 2** | Bit-level striping + Hamming ECC | ✅ | Multiple parity disks; expensive; **not used** |
| **RAID 3** | Byte-level striping + dedicated parity | ✅ | One parity disk; high transfer rates |
| **RAID 4** | Block-level striping + dedicated parity | ✅ | Better random access than RAID 3; parity disk is bottleneck |
| **RAID 5** | Block-level striping + distributed parity | ✅ | Parity spread across all disks; eliminates bottleneck; most common |
| **RAID 6** | Block-level striping + dual distributed parity | ✅✅ | Tolerates **2 simultaneous disk failures**; needs N+2 disks |

---

## Optical Storage

### CD-ROM
- Polycarbonate plastic disc (~1.2 mm thick) with microscopic **pits** and **lands** on a spiral track
- Layers: Polycarbonate → Aluminium (reflective) → Acrylic (protective) → Label
- Laser reads reflectivity changes: pit/land transitions = data bits
- Capacity: ~650–682 MB; ~70 min audio
- **Constant Linear Velocity (CLV):** disc spins faster for inner tracks
- Speed quoted as multiples of 1× (1.2 m/s); e.g. 24× drive
- **Pros:** large capacity, easy mass production, removable, robust
- **Cons:** slow, read-only, expensive for small runs

### CD-R
- **WORM** (Write Once, Read Many)
- Compatible with standard CD-ROM drives

### CD-RW
- Erasable using **phase-change** material
  - *Amorphous state* → reflects light poorly
  - *Crystalline state* → reflects light well
- Mostly compatible with CD-ROM drives

### DVD
- Same physical size as CD but ~7× more data capacity (4.7 GB per layer)
- Smaller pits and tracks; less error-correction overhead
- Up to **4 layers** (2 per side); laser can focus through layers
- Capacities: 4.7 GB (SS/SL) → 8.5 GB (SS/DL) → 17 GB (DS/DL)
- Uses **MPEG compression** for full-length movies
- Carries **regional coding**

---

## Magnetic Tape

### Overview
- **Sequential access** (must read all preceding data to reach target)
- Very cheap; used for **backup and archiving**
- Slow access speed

### Standards
| Standard | Description |
|----------|-------------|
| **8mm** | Similar to camcorder tape; up to 6 MB/s transfer |
| **DAT** (Digital Audio Tape) | Developed by HP & Sony; rotating head; 4 GB uncompressed / 8 GB compressed |
| **DLT** (Digital Linear Tape) | Parallel horizontal tracks; single stationary head; high reliability; used for network backups |
