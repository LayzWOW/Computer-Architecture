---
id: LU2_Number_Systems
aliases: []
tags: []
---

# TMF1214 Computer Architecture — LU2 Summary

<!--toc:start-->
- [TMF1214 Computer Architecture — LU2 Summary](#tmf1214-computer-architecture-lu2-summary)
  - [PART 1 — Number Systems (Chapter 19)](#part-1-number-systems-chapter-19)
  - [1. The Three Number Systems](#1-the-three-number-systems)
  - [2. Decimal System (Base 10)](#2-decimal-system-base-10)
  - [3. Binary System (Base 2)](#3-binary-system-base-2)
  - [4. Converting Decimal → Binary](#4-converting-decimal-binary)
    - [4.1 Integer Part — Repeated Division by 2](#41-integer-part-repeated-division-by-2)
    - [4.2 Fractional Part — Repeated Multiplication by 2](#42-fractional-part-repeated-multiplication-by-2)
  - [5. Hexadecimal Notation (Base 16)](#5-hexadecimal-notation-base-16)
    - [5.1 Hex Digit Table](#51-hex-digit-table)
    - [5.2 Hex → Decimal Examples](#52-hex-decimal-examples)
  - [PART 2 — Digital Logic (Chapter 20)](#part-2-digital-logic-chapter-20)
  - [6. Boolean Algebra](#6-boolean-algebra)
    - [Basic Operations](#basic-operations)
    - [Boolean Identities](#boolean-identities)
  - [7. Logic Gates](#7-logic-gates)
    - [Basic Gate Summary](#basic-gate-summary)
    - [Truth Table — All 2-Input Operators](#truth-table-all-2-input-operators)
  - [8. Functionally Complete Sets](#8-functionally-complete-sets)
    - [Synthesizing with {AND, NOT}](#synthesizing-with-and-not)
    - [Synthesizing with {OR, NOT}](#synthesizing-with-or-not)
    - [Synthesizing with {NAND} alone](#synthesizing-with-nand-alone)
    - [Synthesizing with {NOR} alone](#synthesizing-with-nor-alone)
  - [9. Combinational Circuits](#9-combinational-circuits)
    - [Implementing Boolean Functions from a Truth Table](#implementing-boolean-functions-from-a-truth-table)
  - [10. Sum of Products (SOP) vs Product of Sums (POS)](#10-sum-of-products-sop-vs-product-of-sums-pos)
    - [Sum of Products (SOP)](#sum-of-products-sop)
    - [Product of Sums (POS)](#product-of-sums-pos)
  - [11. Sequential Circuits](#11-sequential-circuits)
    - [Combinational vs Sequential](#combinational-vs-sequential)
  - [12. Flip-Flops](#12-flip-flops)
    - [S-R Latch (Set-Reset Latch)](#s-r-latch-set-reset-latch)
<!--toc:end-->

> Reference: Chapters 19 & 20, William Stallings *Computer Organization and Architecture* 11th Ed.

---

## PART 1 — Number Systems (Chapter 19)

---

## 1. The Three Number Systems

| System | Base (Radix) | Digits Used |
|---|---|---|
| Decimal | 10 | 0–9 |
| Binary | 2 | 0, 1 |
| Hexadecimal | 16 | 0–9, A–F |

Quick equivalents: `10₂ = 2₁₀ = 2₁₆` and `10₁₆ = 16₁₀ = 10000₂`

---

## 2. Decimal System (Base 10)

Each digit is multiplied by 10 raised to the power of its position, counting from 0 at the rightmost digit.

- **Integer example:** `4728 = (4×10³) + (7×10²) + (2×10¹) + (8×10⁰)`
- **Fraction example:** `0.256 = (2×10⁻¹) + (5×10⁻²) + (6×10⁻³)`
- **Mixed example:** `472.256 = (4×10²) + (7×10¹) + (2×10⁰) + (2×10⁻¹) + (5×10⁻²) + (6×10⁻³)`

Position values: `... 10⁴, 10³, 10², 10¹, 10⁰ | 10⁻¹, 10⁻², 10⁻³ ...`

---

## 3. Binary System (Base 2)

Only two digits: **0** and **1**. Each digit's value depends on its position as a power of 2.

Position values: `... 2⁴(16), 2³(8), 2²(4), 2¹(2), 2⁰(1) | 2⁻¹(0.5), 2⁻²(0.25), 2⁻³(0.125) ...`

**Examples (binary → decimal):**

| Binary | Calculation | Decimal |
|---|---|---|
| `10₂` | 1×2¹ + 0×2⁰ | `2₁₀` |
| `11₂` | 1×2¹ + 1×2⁰ | `3₁₀` |
| `100₂` | 1×2² + 0×2¹ + 0×2⁰ | `4₁₀` |
| `1001.101₂` | 2³ + 2⁰ + 2⁻¹ + 2⁻³ | `9.625₁₀` |

---

## 4. Converting Decimal → Binary

Integer and fractional parts must be **handled separately**.

### 4.1 Integer Part — Repeated Division by 2

Divide repeatedly by 2. Collect remainders **bottom to top** (LSB to MSB).

**Example: 11₁₀ → binary**
```
11 ÷ 2 = 5  remainder 1  ← LSB
 5 ÷ 2 = 2  remainder 1
 2 ÷ 2 = 1  remainder 0
 1 ÷ 2 = 0  remainder 1  ← MSB

Result: 1011₂
```

**Example: 21₁₀ → binary**
```
21 ÷ 2 = 10  remainder 1
10 ÷ 2 =  5  remainder 0
 5 ÷ 2 =  2  remainder 1
 2 ÷ 2 =  1  remainder 0
 1 ÷ 2 =  0  remainder 1

Result: 10101₂
```

### 4.2 Fractional Part — Repeated Multiplication by 2

Multiply by 2 repeatedly. The **integer part** (0 or 1) of each product is a binary digit, read **top to bottom** (MSB to LSB).

**Example: 0.25₁₀ → binary (exact)**
```
0.25 × 2 = 0.50  → integer part: 0
0.50 × 2 = 1.00  → integer part: 1

Result: 0.01₂  (exact)
```

**Example: 0.81₁₀ → binary (approximate)**
```
0.81 × 2 = 1.62  → 1
0.62 × 2 = 1.24  → 1
0.24 × 2 = 0.48  → 0
0.48 × 2 = 0.96  → 0
0.96 × 2 = 1.92  → 1
0.92 × 2 = 1.84  → 1

Result: 0.110011₂  (approximately)
```

> **Important:** A decimal fraction with a finite number of digits may need an infinite number of binary digits. In practice, conversion is stopped after a pre-specified number of steps based on desired accuracy.

---

## 5. Hexadecimal Notation (Base 16)

**Why hex?** Binary is natural for computers but cumbersome for humans. Decimal-to-binary conversion is tedious. Hex is compact and converts trivially to/from binary (4 bits = 1 hex digit).

### 5.1 Hex Digit Table

| Binary | Hex | Decimal | | Binary | Hex | Decimal |
|---|---|---|---|---|---|---|
| 0000 | 0 | 0 | | 1000 | 8 | 8 |
| 0001 | 1 | 1 | | 1001 | 9 | 9 |
| 0010 | 2 | 2 | | 1010 | A | 10 |
| 0011 | 3 | 3 | | 1011 | B | 11 |
| 0100 | 4 | 4 | | 1100 | C | 12 |
| 0101 | 5 | 5 | | 1101 | D | 13 |
| 0110 | 6 | 6 | | 1110 | E | 14 |
| 0111 | 7 | 7 | | 1111 | F | 15 |

### 5.2 Hex → Decimal Examples

Position values: `... 16³(4096), 16²(256), 16¹(16), 16⁰(1), 16⁻¹(0.0625) ...`

| Hex | Calculation | Decimal |
|---|---|---|
| `2C₁₆` | (2×16¹) + (12×16⁰) | `44₁₀` |
| `12₁₆` | (1×16¹) + (2×16⁰) | `18₁₀` |
| `115₁₆` | (1×16²) + (1×16¹) + (5×16⁰) | `277₁₀` |
| `9.A₁₆` | (9×16⁰) + (10×16⁻¹) | `9.625₁₀` |

---

## PART 2 — Digital Logic (Chapter 20)

---

## 6. Boolean Algebra

Digital circuits are designed and analyzed using **Boolean algebra**. Variables take values of **1 (TRUE)** or **0 (FALSE)**.

### Basic Operations

| Operation | Symbol | Expression |
|---|---|---|
| AND | · (dot) | A · B |
| OR | + (plus) | A + B |
| NOT | ‾ (overbar) | Ā |

### Boolean Identities

**Basic Postulates:**

| AND form | OR form | Law |
|---|---|---|
| A · B = B · A | A + B = B + A | Commutative |
| A · (B + C) = (A·B) + (A·C) | A + (B·C) = (A+B)·(A+C) | Distributive |
| 1 · A = A | 0 + A = A | Identity Elements |
| A · Ā = 0 | A + Ā = 1 | Inverse Elements |

**Other Identities:**

| AND form | OR form | Law |
|---|---|---|
| 0 · A = 0 | 1 + A = 1 | — |
| A · A = A | A + A = A | — |
| A · (B · C) = (A · B) · C | A + (B + C) = (A + B) + C | Associative |
| **Ā·B̄ = Ā + B̄** | **Ā+B̄ = Ā · B̄** | **DeMorgan's Theorem** |

> **DeMorgan's Theorem** is critical — it lets you convert between AND/OR forms with inversion.

---

## 7. Logic Gates

A **gate** is an electronic circuit that produces an output signal based on a simple Boolean operation on its inputs. Gates are the **fundamental building blocks** of all digital logic circuits.

### Basic Gate Summary

| Gate | Expression | Output = 1 when... |
|---|---|---|
| AND | F = A · B | **All** inputs are 1 |
| OR | F = A + B | **Any** input is 1 |
| NOT | F = Ā | Input is 0 |
| NAND | F = A̅B̅ | **Any** input is 0 |
| NOR | F = A̅+̅B̅ | **All** inputs are 0 |
| XOR | F = A ⊕ B | **Odd number** of inputs are 1 |

### Truth Table — All 2-Input Operators

| P | Q | NOT P (P̄) | P AND Q | P OR Q | P NAND Q | P NOR Q | P XOR Q |
|---|---|---|---|---|---|---|---|
| 0 | 0 | 1 | 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 | 1 | 0 | 1 |
| 1 | 1 | 0 | 1 | 1 | 0 | 0 | 0 |

---

## 8. Functionally Complete Sets

A **functionally complete set** is a set of gates with which **any** Boolean function can be implemented. This simplifies chip design since only 1–2 gate types are needed.

The complete sets are: `{AND, OR, NOT}`, `{AND, NOT}`, `{OR, NOT}`, `{NAND}`, `{NOR}`

### Synthesizing with {AND, NOT}
Use DeMorgan's theorem to replace OR:
```
A OR B = NOT((NOT A) AND (NOT B))
i.e.  A + B = (Ā · B̄)‾
```

### Synthesizing with {OR, NOT}
Use DeMorgan's theorem to replace AND:
```
A AND B = NOT((NOT A) OR (NOT B))
i.e.  A · B = (Ā + B̄)‾
```

### Synthesizing with {NAND} alone
| Target | NAND equivalent |
|---|---|
| NOT A | A NAND A |
| A AND B | (A NAND B) NAND (A NAND B) — i.e., NAND the result with itself |
| A OR B | (A NAND A) NAND (B NAND B) — NOT each input, then NAND them |

### Synthesizing with {NOR} alone
| Target | NOR equivalent |
|---|---|
| NOT A | A NOR A |
| A OR B | (A NOR B) NOR (A NOR B) |
| A AND B | (A NOR A) NOR (B NOR B) |

---

## 9. Combinational Circuits

A **combinational circuit** is a network of interconnected gates whose output depends **only on the current input** (no memory of past inputs).

- Has **n** binary inputs and **m** binary outputs
- Used in: multiplexers, decoders, ROM, adders

A combinational circuit can be described three ways:
1. **Truth table** — lists output for all 2ⁿ input combinations
2. **Boolean equation** — each output expressed as a Boolean function of inputs
3. **Graphical (gate diagram)** — interconnected layout of gates

### Implementing Boolean Functions from a Truth Table

**Example:** Given the truth table below, find the Boolean expression for F.

| A | B | C | F |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 0 |

---

## 10. Sum of Products (SOP) vs Product of Sums (POS)

These are the two standard forms for writing any Boolean expression derived from a truth table.

### Sum of Products (SOP)
- List all input combinations where **F = 1**
- AND the inputs together for each row (a **minterm**)
- OR all the minterms together
- Structure: **AND gates feeding into a final OR gate**

From the table above:
```
F = (Ā·B·C̄) + (Ā·B·C) + (A·B·C̄)
```
> Mnemonic: **multiply = AND**, **sum = OR**

### Product of Sums (POS)
- List all input combinations where **F = 0**
- Apply the generalization of DeMorgan's theorem to convert: `(X·Y·Z)‾ = X̄ + Ȳ + Z̄`
- This turns each 0-row into a **maxterm** (an OR expression)
- AND all the maxterms together
- Structure: **OR gates feeding into a final AND gate**

From the table above (5 rows where F = 0):
```
F = (A+B+C)·(A+B+C̄)·(Ā+B+C)·(Ā+B+C̄)·(Ā+B̄+C̄)
```

> Both SOP and POS represent the same function — just different gate implementations.

---

## 11. Sequential Circuits

A **sequential circuit** is more complex than a combinational circuit. Its output depends on both the **current input** AND its **current state** (i.e., previous inputs are remembered).

- Used in: **registers**, **counters**
- Requires memory elements (flip-flops)

### Combinational vs Sequential

| Feature | Combinational | Sequential |
|---|---|---|
| Output depends on | Current input only | Current input + current state |
| Has memory? | No | Yes |
| Examples | Adder, decoder, MUX | Register, counter |

---

## 12. Flip-Flops

The **simplest sequential circuit** — the fundamental memory element.

**Two key properties:**
1. **Bistable** — exists in one of two stable states (0 or 1); holds that state in the absence of input → acts as **1-bit memory**
2. **Complementary outputs** — has two outputs, **Q** and **Q̄**, which are always opposites of each other

### S-R Latch (Set-Reset Latch)
The most basic flip-flop, built from **two NOR gates** in a cross-coupled feedback arrangement.

- **Inputs:** S (Set) and R (Reset)
- **Outputs:** Q and Q̄

**Behaviour:**

| S | R | Q (next) | Q̄ (next) | Action |
|---|---|---|---|---|
| 0 | 0 | Q (unchanged) | Q̄ | **Hold** — remembers previous state |
| 0 | 1 | 0 | 1 | **Reset** — Q forced to 0 |
| 1 | 0 | 1 | 0 | **Set** — Q forced to 1 |
| 1 | 1 | Undefined | Undefined | **Forbidden** state |

**How it works (bistable states):**
- When R=0, S=0: Q=0, Q̄=1 is one stable state; Q=1, Q̄=0 is the other. Both are self-consistent due to feedback.
- Applying S=1 forces Q→1; applying R=1 forces Q→0. Removing the input leaves the latch in its new state.
