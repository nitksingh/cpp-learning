# 🎓 Data Representation

---

## 🤔 Why Study Data Representation?

Have you ever seen weird results like these?

```cpp
// Example 1: Adding two large positive numbers
int a = 1500000000;  // 1.5 billion
int b = 1500000000;  // 1.5 billion
int result = a + b;
std::cout << result;  // Output: -1294967296 (NEGATIVE?!)
```

```cpp
// Example 2: Adding decimals
double x = 0.15 + 0.15;
std::cout << x;  // Output: 0.29999999999... (not 0.30!)
```

These "wrong" results happen because of **how numbers are stored in memory**.

```
┌─────────────────────────────────────────────────────────────────┐
│                    WHY THIS MATTERS                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Memory stores everything as 1s and 0s (bits)                  │
│                                                                 │
│   Different data types interpret these bits differently:       │
│                                                                 │
│   ┌────────────┐                                                │
│   │ 0100 0001  │  → As int: 65                                  │
│   │            │  → As char: 'A'                                │
│   └────────────┘                                                │
│                                                                 │
│   Understanding this helps you:                                 │
│   • Debug "impossible" bugs                                     │
│   • Write more efficient code                                   │
│   • Avoid overflow and precision errors                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 💡 Binary Number System Basics

Computers only understand **two states**: ON (1) and OFF (0).

### Bit and Byte

| Term | Definition |
|------|------------|
| **Bit** | Single binary digit (0 or 1) |
| **Byte** | 8 bits grouped together |

```
1 byte = 8 bits

┌───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 1 │ 0 │ 0 │ 0 │ 0 │ 0 │ 1 │  = 65 (decimal)
└───┴───┴───┴───┴───┴───┴───┴───┘
  7   6   5   4   3   2   1   0   ← bit positions
```

### Positional Number Systems

Both decimal (base-10) and binary (base-2) are **positional systems** — the position of each digit determines its value.

```
DECIMAL (Base-10): Uses digits 0-9

   9   8   7   6
   ↓   ↓   ↓   ↓
   9×10³ + 8×10² + 7×10¹ + 6×10⁰
   = 9000 + 800  + 70    + 6
   = 9876


BINARY (Base-2): Uses digits 0-1

   1   0   1   1
   ↓   ↓   ↓   ↓
   1×2³ + 0×2² + 1×2¹ + 1×2⁰
   = 8   + 0   + 2    + 1
   = 11 (decimal)
```

### Powers of 2 Reference

| Position | 2^n | Value |
|----------|-----|-------|
| 0 | 2⁰ | 1 |
| 1 | 2¹ | 2 |
| 2 | 2² | 4 |
| 3 | 2³ | 8 |
| 4 | 2⁴ | 16 |
| 5 | 2⁵ | 32 |
| 6 | 2⁶ | 64 |
| 7 | 2⁷ | 128 |

---

## 🔄 Binary ↔ Decimal Conversions

### Binary to Decimal

**Method:** Expand each bit × its position value, then add.

```
Convert 1011 (binary) to decimal:

Position:     3    2    1    0
Binary:       1    0    1    1
              ↓    ↓    ↓    ↓
Value:       2³   2²   2¹   2⁰
            = 8  = 4  = 2  = 1

Calculation:
  1×8 + 0×4 + 1×2 + 1×1
= 8   + 0   + 2   + 1
= 11 (decimal)
```

### More Examples

```
Binary: 1 1 0 0
        ↓ ↓ ↓ ↓
        8+4+0+0 = 12

Binary: 1 1 1 1
        ↓ ↓ ↓ ↓
        8+4+2+1 = 15

Binary: 1 0 0 0 0
        ↓
        16+0+0+0+0 = 16
```

### Decimal to Binary

**Method:** Subtract the largest power of 2 that fits, mark it as 1, repeat until 0.

```
Convert 30 (decimal) to binary:

Step 1: What powers of 2 fit into 30?
        32 > 30 ❌
        16 ≤ 30 ✓  → 30 - 16 = 14 remaining

Step 2: What fits into 14?
        16 > 14 ❌
        8 ≤ 14 ✓   → 14 - 8 = 6 remaining

Step 3: What fits into 6?
        8 > 6 ❌
        4 ≤ 6 ✓    → 6 - 4 = 2 remaining

Step 4: What fits into 2?
        4 > 2 ❌
        2 ≤ 2 ✓    → 2 - 2 = 0 remaining ✓ Done!

Powers used: 16, 8, 4, 2 (positions 4, 3, 2, 1)

Position:  5   4   3   2   1   0
Used?:     ×   ✓   ✓   ✓   ✓   ×
Binary:    0   1   1   1   1   0

Answer: 30 = 011110 (binary) or simply 11110
```

---

## ➕ Binary Addition

### Addition Rules

| A | B | Sum | Carry |
|---|---|-----|-------|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

> 💡 **Key:** 1 + 1 = 10 (binary) — that's 0 with carry 1!

### Example: Add 1111 + 110

```
  1111   (15 in decimal)
+  110   (6 in decimal)
──────

Step-by-step (right to left):

     Carry: 1  1  1
            1  1  1  1
          +    1  1  0
          ───────────
         1  0  1  0  1

Position 0: 1 + 0 = 1
Position 1: 1 + 1 = 0, carry 1
Position 2: 1 + 1 + carry(1) = 1, carry 1
Position 3: 1 + 0 + carry(1) = 0, carry 1
Position 4: carry(1) = 1

Result: 10101 (binary) = 21 (decimal) ✓

Check: 15 + 6 = 21 ✓
```

---

## 🔢 Integer Representation

### Unsigned Integers (Positive Only)

With **n bits**, you can represent values from **0 to 2ⁿ - 1**.

```
4 bits (half byte):
┌───┬───┬───┬───┐
│ 0 │ 0 │ 0 │ 0 │  = 0 (minimum)
└───┴───┴───┴───┘

┌───┬───┬───┬───┐
│ 1 │ 1 │ 1 │ 1 │  = 15 (maximum)
└───┴───┴───┴───┘

Range: 0 to 15 (2⁴ - 1)
```

### The Problem: How to Store Negative Numbers?

If we only have 4 bits, how do we represent both positive AND negative numbers?

Two approaches:
1. ❌ **Sign Magnitude** — Simple but broken
2. ✅ **Two's Complement** — What computers actually use

---

## ❌ Sign Magnitude (The Bad Way)

**Idea:** Reserve the leftmost bit as a "sign bit."

```
┌───┬───┬───┬───┐
│ S │   │   │   │   S = 0 → positive
└───┴───┴───┴───┘   S = 1 → negative
  ↑
Sign bit
```

### Example

```
0 0 1 1 = +3   (sign bit = 0 → positive)
1 0 1 1 = -3   (sign bit = 1 → negative)
```

### Problems with Sign Magnitude

**Problem 1: Arithmetic Doesn't Work!**

```
Let's add +3 and -3 (should equal 0):

  0 0 1 1   (+3)
+ 1 0 1 1   (-3)
─────────
  1 1 1 0   = -6 ?!?!

WRONG! We expected 0, got -6!
```

**Problem 2: Two Zeros!**

```
0 0 0 0 = +0
1 0 0 0 = -0

Why do we need two different zeros?
```

> 💡 **Sign Magnitude is not used for integers in modern computers!**

---

## ✅ Two's Complement (The Right Way)

**Idea:** Encode negative numbers as the **additive inverse** — what you add to get zero.

### The Key Insight

With 4 bits, if we add 1 to 1111 (15):

```
    1 1 1 1   (15)
  +       1
  ─────────
  1 0 0 0 0   = 16, but...
  ↑
  This bit has nowhere to go! It's LOST.
  
Result stored: 0 0 0 0 = 0!
```

So 15 + 1 = 0 (with 4 bits). This means **1111 acts like -1**!

### Visualizing Two's Complement

```
┌─────────────────────────────────────────────────────────────────┐
│              TWO'S COMPLEMENT NUMBER CIRCLE                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                        0 (0000)                                 │
│                           │                                     │
│              -1 ←─────────┼───────────→ +1                      │
│            (1111)         │           (0001)                    │
│                           │                                     │
│         -2 ←──────────────┼──────────────→ +2                   │
│       (1110)              │              (0010)                 │
│                           │                                     │
│     -3 ←──────────────────┼──────────────────→ +3               │
│   (1101)                  │                  (0011)             │
│                           │                                     │
│  -4 ←─────────────────────┼─────────────────────→ +4            │
│(1100)                     │                     (0100)          │
│                           │                                     │
│   -5 ←────────────────────┼────────────────────→ +5             │
│   (1011)                  │                  (0101)             │
│                           │                                     │
│     -6 ←──────────────────┼──────────────────→ +6               │
│     (1010)                │                (0110)               │
│                           │                                     │
│       -7 ←────────────────┼────────────────→ +7                 │
│       (1001)              │              (0111)                 │
│                           │                                     │
│           -8 ←────────────┴────────────→                        │
│           (1000)      (no +8!)                                  │
│                                                                 │
│   Positive: 0xxx (0 to 7)                                       │
│   Negative: 1xxx (-8 to -1)                                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Two's Complement Range (4 bits)

| Binary | Unsigned | Two's Complement |
|--------|----------|------------------|
| 0000 | 0 | 0 |
| 0001 | 1 | +1 |
| 0010 | 2 | +2 |
| 0011 | 3 | +3 |
| 0100 | 4 | +4 |
| 0101 | 5 | +5 |
| 0110 | 6 | +6 |
| 0111 | 7 | +7 |
| 1000 | 8 | **-8** |
| 1001 | 9 | **-7** |
| 1010 | 10 | **-6** |
| 1011 | 11 | **-5** |
| 1100 | 12 | **-4** |
| 1101 | 13 | **-3** |
| 1110 | 14 | **-2** |
| 1111 | 15 | **-1** |

### How to Calculate Two's Complement

**To get the negative of a number: Flip all bits, then add 1.**

```
Find -3 from +3:

Step 1: Start with +3
        0 0 1 1

Step 2: Flip all bits
        1 1 0 0

Step 3: Add 1
        1 1 0 0
      +       1
        ───────
        1 1 0 1  = -3 in two's complement

Verify: 0011 + 1101 = 10000 → stored as 0000 = 0 ✓
```

### Two's Complement Arithmetic Works!

```
Add +3 and -3:

    0 0 1 1   (+3)
  + 1 1 0 1   (-3)
  ─────────
  1 0 0 0 0
  ↑
  Overflow bit is lost (no room!)

Result: 0 0 0 0 = 0 ✓ CORRECT!
```

### Range Formula

For **n bits** using two's complement:

| | Formula | 4-bit Example |
|---|---------|---------------|
| **Minimum** | -2^(n-1) | -8 |
| **Maximum** | 2^(n-1) - 1 | +7 |

```cpp
// In C++:
// int (typically 32 bits):
// Range: -2,147,483,648 to 2,147,483,647

// This explains our first "bug"!
int a = 1500000000;
int b = 1500000000;
int result = a + b;  // OVERFLOW! Wraps to negative!
```

---

## 🔣 Floating-Point Representation

How do we store decimal numbers like 3.14 using only 1s and 0s?

### Normalized Exponential Form

Any decimal number can be written as: **M × 10^e**

```
111.0 can be written as:
  0.111 × 10³   (M = 0.111, e = 3)
  1.11  × 10²   ← Normalized form
  11.1  × 10¹
  111.0 × 10⁰

Normalized form: decimal point after first non-zero digit
```

### Three Fields in Memory

```
┌─────────────────────────────────────────────────────────────────┐
│              FLOATING-POINT NUMBER LAYOUT                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌───┬───────────┬─────────────────────────────┐               │
│   │ S │ Exponent  │         Mantissa            │               │
│   └───┴───────────┴─────────────────────────────┘               │
│     ↑       ↑                    ↑                              │
│     │       │                    │                              │
│   Sign   Power of 2        Fractional part                      │
│  (1 bit)                   (assumes leading 1.)                 │
│                                                                 │
│   Value = (-1)^S × 1.mantissa × 2^exponent                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

| Field | Purpose |
|-------|---------|
| **Sign (S)** | 0 = positive, 1 = negative |
| **Exponent** | The power of 2 |
| **Mantissa** | Fractional digits (1. is assumed) |

### Example: Storing 3.5

```
3.5 in binary:
  3.5 = 3 + 0.5 = 11.1 (binary)
  
Normalized: 1.11 × 2¹

Store:
  Sign = 0 (positive)
  Exponent = 1 (with bias encoding)
  Mantissa = 11 (the .11 part, 1. is implied)
```

### Special Encodings

| Pattern | Meaning |
|---------|---------|
| All 1s in exponent, 0s in mantissa | ±Infinity |
| All 1s in exponent, non-zero mantissa | NaN (Not a Number) |
| All 0s | ±Zero |

```cpp
double x = 1.0 / 0.0;   // +Infinity
double y = -1.0 / 0.0;  // -Infinity
double z = 0.0 / 0.0;   // NaN
```

---

## ⚠️ Floating-Point Precision Issues

### Floating-Point Numbers are Approximations!

There are **gaps** between representable values:

```
┌─────────────────────────────────────────────────────────────────┐
│              GAPS BETWEEN FLOATING-POINT VALUES                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Near 0: Small gaps                                            │
│   ──┬──┬──┬──┬──┬──┬──┬──                                       │
│     0.1  0.2  0.3  ...                                          │
│                                                                 │
│   Far from 0: LARGE gaps                                        │
│   ──────────┬──────────┬──────────┬──────────                   │
│          1000000   1000001   1000002                            │
│                                                                 │
│   Values between the marks CANNOT be stored exactly!            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Why 0.15 + 0.15 ≠ 0.30?

```
0.15 in binary = 0.00100110011001100110... (repeating!)

Since we have limited bits, it gets truncated:
  Stored as: 0.14999999999999999...

So:
  0.15 + 0.15 
= 0.14999... + 0.14999...
= 0.29999...  (not exactly 0.30!)
```

### Error Propagation

```
Representing 2/3 with 4 decimal digits:
  0.6666 (instead of 0.6666...)
  
Adding it 6 times:
  0.6666 + 0.6666 = 1.3332
  + 0.6666 = 1.9998
  + 0.6666 = 2.6664
  + 0.6666 = 3.3330
  + 0.6666 = 3.9996

Expected: 4.0000
Got: 3.9996

The error grows with each operation!
```

### Practical Implications

```cpp
// ❌ NEVER compare floats with ==
if (x == 0.3) { ... }  // Might fail!

// ✅ Use a tolerance (epsilon)
const double EPSILON = 0.0001;
if (std::abs(x - 0.3) < EPSILON) { ... }  // Better!
```

> ⚠️ **Would you trust a bank that used floating-point for money?**
> 
> This is why financial software uses special decimal types!

---

## 🔤 Character Representation

Characters are stored as **numbers** using encoding schemes.

### ASCII (American Standard Code for Information Interchange)

Each character = 1 byte (8 bits) = value from 0-127

```
┌─────────────────────────────────────────────────────────────────┐
│                    ASCII TABLE (partial)                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Character    Binary       Decimal                             │
│   ─────────    ────────     ───────                             │
│      'A'       0100 0001      65                                │
│      'B'       0100 0010      66                                │
│      'Z'       0101 1010      90                                │
│                                                                 │
│      'a'       0110 0001      97                                │
│      'b'       0110 0010      98                                │
│      'z'       0111 1010      122                               │
│                                                                 │
│      '0'       0011 0000      48                                │
│      '9'       0011 1001      57                                │
│                                                                 │
│      ' '       0010 0000      32    (space)                     │
│      '\n'      0000 1010      10    (newline)                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Character Arithmetic Works!

```cpp
char letter = 'A';           // Stored as 65
char next = letter + 1;      // 65 + 1 = 66 = 'B'

char digit = '5';            // Stored as 53
int value = digit - '0';     // 53 - 48 = 5 (actual number!)

// Convert lowercase to uppercase
char lower = 'a';            // 97
char upper = lower - 32;     // 97 - 32 = 65 = 'A'
```

### Same Bits, Different Interpretation

```cpp
char c = 65;
int i = 65;

// Both store the same bits: 0100 0001
// But they're interpreted differently:

std::cout << c;  // Outputs: A (as character)
std::cout << i;  // Outputs: 65 (as number)
```

---

## 🔑 Key Takeaways

### Memory Basics

| Concept | Description |
|---------|-------------|
| Bit | Single 0 or 1 |
| Byte | 8 bits |
| All data | Stored as sequences of bits |

### Number Systems

| Base | Digits | Example |
|------|--------|---------|
| Binary (2) | 0, 1 | 1011 = 11 |
| Decimal (10) | 0-9 | 11 |
| Hexadecimal (16) | 0-9, A-F | B = 11 |

### Integer Representation

| Type | Method | n-bit Range |
|------|--------|-------------|
| Unsigned | Straight binary | 0 to 2ⁿ - 1 |
| Signed | Two's complement | -2^(n-1) to 2^(n-1) - 1 |

### Two's Complement Quick Reference

```
To negate: Flip all bits, add 1

+3 = 0011
-3 = 1100 + 1 = 1101
```

### Floating-Point

| Issue | Cause |
|-------|-------|
| Imprecision | Finite bits, infinite decimals |
| 0.1 + 0.2 ≠ 0.3 | Binary can't represent some decimals exactly |
| Overflow/Underflow | Number too large/small for exponent |

### Golden Rules

1. **Integers can overflow** — adding to max value wraps to negative
2. **Floats are approximations** — never use `==` to compare
3. **Characters are numbers** — 'A' is just 65 in disguise
4. **Bits have no meaning alone** — interpretation depends on type

