# ITFE Study Pack — Week 1 (Aug 4–10)

## Hardware Part I — Vol.1 Chapter 1, p.13–78

Each day below is self-contained: read the explanation, work the examples yourself before looking at the answers, then do the quiz. Answer keys are at the end of each day so you can't accidentally see them early. Budget roughly 60–90 min on weekdays, 2–3 hrs Sat/Sun.

---

---

# DAY 1 (Mon Aug 4) — Computer History & The Five Major Units

### Why this matters for you

This is the "warm-up" topic — mostly memorization, low difficulty, guaranteed points if you know the generations and the five-unit model cold. As a backend engineer you already _use_ all five units every day; today is about learning the exam's vocabulary for what you already do intuitively.

### 1. Generations of computers (memorize the trigger word → generation)

| Generation | Era     | Core switching technology           | Trigger keyword                                    |
| ---------- | ------- | ----------------------------------- | -------------------------------------------------- |
| 1st        | 1940s   | Vacuum tubes                        | ENIAC, EDSAC, stored-program concept (von Neumann) |
| 2nd        | 1950s   | Transistors                         | UNIVAC I, first _commercial_ computer              |
| 3rd        | 1960s   | IC (Integrated Circuit)             | IBM/360, general-purpose computer                  |
| 3.5th      | 1970s   | LSI (Large Scale Integration)       | microcomputers, supercomputers                     |
| 4th        | 1980s   | VLSI (Very Large Scale Integration) | "one computer per person," PCs                     |
| 5th        | ongoing | FPGA and beyond                     | programmable-after-manufacture logic               |

The exam loves asking "which technology defines generation X?" — so the safest way to lock this in is the pattern: **vacuum tube → transistor → IC → LSI → VLSI**, each step meaning "smaller, cooler, faster, cheaper, more reliable."

Two names worth pinning down because they get quoted directly:

- **ENIAC (1946)** — first computer, but _rewired by hand_ for each new job (no stored program).
- **EDSAC (1949)** — introduced the **stored-program concept**: instructions live in memory alongside data, so you don't rewire the machine, you just load a different program. This is literally why you can `deploy` new code to a server without an electrician showing up. Computers named on this principle are called **von Neumann machines**, after John von Neumann.

**Backend connection:** every server you've ever SSH'd into is a von Neumann machine. "Stored program" is the entire reason software exists as a separate, deployable artifact from hardware.

### 2. The Five Major Units

Think of a computer as modeling how a person solves "3 + 6 = ?":

1. Read the problem (**input**)
2. Understand "+" means add (**control**)
3. Do the math (**arithmetic & logic**)
4. Remember the numbers and the result (**storage**)
5. Write down the answer (**output**)

| Unit                              | Role                                                                                                                           | Backend analogy                                             |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------- |
| **Input unit**                    | Reads data into the system                                                                                                     | `stdin`, an incoming HTTP request, a keyboard               |
| **Output unit**                   | Presents results to a human                                                                                                    | `stdout`, an HTTP response, a monitor                       |
| **Storage unit**                  | Holds data — split into **main memory** (volatile, directly wired to CPU) and **auxiliary storage** (non-volatile, e.g., disk) | RAM vs. your Postgres data directory on disk                |
| **Arithmetic & Logic Unit (ALU)** | Performs the actual math/comparisons                                                                                           | The CPU core doing `x + y`                                  |
| **Control unit**                  | Decodes instructions, tells the other 4 units what to do                                                                       | The CPU's instruction decoder / your program's control flow |

**Critical distinction the exam tests directly:**

- **Main memory**: volatile (loses contents on power-off), directly addressable by the CPU.
- **Auxiliary storage**: non-volatile (survives power-off), used when data won't fit in main memory.

- **CPU (= Central Processing Unit = "the processor")** is the combination of **control unit + ALU**.
- Everything _outside_ the processor — input, output, and auxiliary storage — is collectively called **peripheral devices**.
- A **microprocessor / MPU**: a CPU condensed onto a single LSI chip. This is what's inside your laptop and your phone.
- **SoC (System on a Chip)**: not just the CPU, but memory and I/O functions _also_ integrated onto one chip. Trade-off: fast and power-efficient, but expensive/risky to develop, so it's often paired with **SiP (System in a Package)**, which packages multiple separate chips together instead of merging them onto one die.
- A **one-chip microcomputer**: goes even further — CPU + memory + I/O all on one chip, common in appliances (a washing machine controller, not your laptop).
- **Co-processor**: assists the main CPU with a specific job (e.g., an old-school math co-processor for floating point).
- **Dedicated processor**: built for one job only (e.g., a GPU is closer to this than a general-purpose CPU).

### Key Points to memorize

- Order of generations and their switching tech: tube → transistor → IC → LSI → VLSI.
- EDSAC = stored-program concept = why we call them "von Neumann machines."
- Five units: Input, Output, Storage (main + auxiliary), ALU, Control.
- CPU = Control unit + ALU. Peripherals = everything else.
- Main memory = volatile; Auxiliary storage = non-volatile.
- SoC (all-in-one chip) vs. SiP (multiple chips, one package) vs. one-chip microcomputer.

### Practice Questions

1. Which generation of computers first used transistors as logic gates?
2. What specific innovation did EDSAC introduce that ENIAC lacked?
3. Name the two units that together make up the CPU.
4. Which storage type loses its contents when power is turned off?
5. A chip that integrates CPU + memory + I/O functions, aimed at things like home appliances, is called a ___________.
6. True or False: peripheral devices include the ALU.

### Day 1 Quiz (5 questions, multiple choice)

**Q1.** The stored-program concept, which allows a computer to run different programs without rewiring, was introduced by:
A) ENIAC B) EDSAC C) UNIVAC I D) IBM/360

**Q2.** Which of the following is NOT one of the five major units of a computer?
A) Control unit B) Compiler unit C) Arithmetic and logic unit D) Storage unit

**Q3.** SoC (System on a Chip) differs from SiP (System in a Package) in that SoC:
A) uses multiple separate chips in one package
B) integrates all functions, including memory, onto a single chip
C) is only used in supercomputers
D) has no memory integration at all

**Q4.** Which unit interprets commands and directs the other four units?
A) ALU B) Control unit C) Storage unit D) Output unit

**Q5.** 3rd-generation computers (1960s) primarily used which technology as logic gates?
A) Vacuum tubes B) Transistors C) IC (Integrated Circuit) D) VLSI

**Day 1 Answers:** Q1: B | Q2: B | Q3: B | Q4: B | Q5: C
**Practice Answers:** 1) 2nd generation 2) stored-program concept (no rewiring needed) 3) Control unit + ALU 4) Main memory 5) One-chip microcomputer (single-chip microcomputer) 6) False — ALU is part of the CPU, not a peripheral.

---

---

# DAY 2 (Tue Aug 5) — Data Representation & Radix Conversion

### Why this matters for you

This is pure "speak computer" fluency — bits, bytes, words, and converting between binary/octal/decimal/hex. You already do hex-reading (memory addresses, color codes, git hashes) but the exam wants you to do it _by hand_, methodically, including fractional (post-decimal-point) conversions, which most engineers never practice manually.

### 1. Bits, Bytes, Words

- **Bit**: smallest unit, holds 0 or 1 (a signal is either "on" or "off," "high voltage" or "low voltage").
- **Byte**: 8 bits grouped together.
- **Word**: the CPU's native processing chunk size — 16, 32, or 64 bits depending on the architecture. Modern CPUs are mostly 64-bit words. (This is literally why you choose `int32` vs `int64` types.)

**Information amount rule:** n bits can represent **2ⁿ distinct values**.

- 1 byte (8 bits) → 2⁸ = 256 values (0–255)
- 1 word (16 bits) → 2¹⁶ = 65,536 values

This is exactly why an unsigned byte maxes at 255, and why a 16-bit integer maxes at 65,535 — you've hit this in code before, now you know the exam's way of deriving it.

### 2. Prefixes (know both the power-of-10 and power-of-2 version)

| Symbol | Name | Decimal (10ⁿ) | Binary (2ⁿ) |
| ------ | ---- | ------------- | ----------- |
| k      | kilo | 10³           | 2¹⁰         |
| M      | mega | 10⁶           | 2²⁰         |
| G      | giga | 10⁹           | 2³⁰         |
| T      | tera | 10¹²          | 2⁴⁰         |
| P      | peta | 10¹⁵          | 2⁵⁰         |

For small numbers: m (milli, 10⁻³), μ (micro, 10⁻⁶), n (nano, 10⁻⁹), p (pico, 10⁻¹²).

_Note the trap:_ marketers/hard-drive vendors use decimal kilo (1000), but memory and OS tools often use binary kilo (1024). The exam may test whether you know these aren't the same number — "1 KB" can mean 1000 bytes or 1024 bytes depending on context.

### 3. Radix Conversion — the method you must be able to do by hand

**Binary → Decimal:** multiply each bit by its positional weight (power of 2) and sum.

Example: convert `1101.01₂` to decimal.

- Integer part: 1×2³ + 1×2² + 0×2¹ + 1×2⁰ = 8 + 4 + 0 + 1 = 13
- Fractional part: 0×2⁻¹ + 1×2⁻² = 0 + 0.25 = 0.25
- Result: **13.25₁₀**

**Decimal → Binary:** integer part and fractional part are handled _separately_, with opposite techniques.

- **Integer part:** repeatedly divide by 2, keep the remainders, read them **bottom to top**.
- **Fractional part:** repeatedly multiply by 2, keep the integer part of each result, read them **top to bottom**.

Worked example: convert `45.375₁₀` to binary.

Integer part (45):

```
45 ÷ 2 = 22 remainder 1   ← read bottom to top
22 ÷ 2 = 11 remainder 0
11 ÷ 2 = 5  remainder 1
5  ÷ 2 = 2  remainder 1
2  ÷ 2 = 1  remainder 0
1  ÷ 2 = 0  remainder 1
```

Reading remainders bottom-to-top: `101101`

Fractional part (0.375):

```
0.375 × 2 = 0.75  → integer part 0   ← read top to bottom
0.75  × 2 = 1.5   → integer part 1
0.5   × 2 = 1.0   → integer part 1
(fractional part reached 0 — stop)
```

Reading top-to-bottom: `011`

**Result: 45.375₁₀ = 101101.011₂**

**Binary ↔ Octal / Hex (the shortcut):** since 8 = 2³ and 16 = 2⁴, you don't need the divide/multiply method — just group bits.

- For **octal**, group binary digits in **3s** from the radix point outward.
- For **hex**, group binary digits in **4s** from the radix point outward. Pad with zeros where a group is incomplete.

Example: `10110.11₂` → hex

- Group in 4s from the decimal point: `0001 0110 . 1100` (padded with 0s)
- = `1 6 . C` → **16.C₁₆**

**Important trap the exam sets:** not every decimal fraction converts to a _finite_ binary fraction. E.g., 0.2₁₀ becomes an infinitely repeating binary fraction — computers approximate it (this is the same reason `0.1 + 0.2 !== 0.3` in floating point in your day job).

### Key Points

- n bits → 2ⁿ values.
- Binary→decimal: sum of (bit × positional power of 2).
- Decimal→binary: integer part uses repeated division (read bottom-up); fractional part uses repeated multiplication (read top-down).
- Binary↔octal: group by 3. Binary↔hex: group by 4.
- Some decimal fractions never terminate in binary — this is a real source of floating-point rounding error.

### Practice Questions

1. Convert `1011.101₂` to decimal.
2. Convert `29.5₁₀` to binary.
3. Convert `10111001₂` to hexadecimal.
4. How many distinct values can 5 bits represent?
5. Convert `3A₁₆` to binary, then to decimal.
6. Why can't 0.1₁₀ be represented exactly in binary floating point?

### Day 2 Quiz

**Q1.** What is `1010₂` in decimal?
A) 8 B) 10 C) 12 D) 16

**Q2.** To convert a decimal integer to binary, you repeatedly:
A) multiply by 2, keep integer parts, read top to bottom
B) divide by 2, keep remainders, read bottom to top
C) divide by 16, keep remainders, read top to bottom
D) multiply by 8, keep integer parts, read bottom to top

**Q3.** To convert binary to hexadecimal, you group binary digits in sets of:
A) 2 B) 3 C) 4 D) 8

**Q4.** 6 bits can represent how many distinct values?
A) 32 B) 64 C) 12 D) 6

**Q5.** `0.75₁₀` converted to binary is:
A) 0.11 B) 0.101 C) 0.110 D) 0.010

---

**Day 2 Answers:** Q1: B | Q2: B | Q3: C | Q4: B | Q5: A
**Practice Answers:** 1) 11.625 2) 11101.1 3) B9 4) 32 5) 0011 1010 = 58 6) its fractional expansion in binary never terminates (it's a repeating fraction), so it must be stored as an approximation.

---

---

# DAY 3 (Wed Aug 6) — Number Representation Inside Computers, Part 1

### Why this matters for you

This is where "how does a computer actually store a signed integer" gets formalized. You've used `int8`/`int16` types for years; today you learn _why_ two's complement is the near-universal choice, which is a favorite exam topic because it's easy to test with a clean calculation.

### 1. Decimal-coded formats (less critical, know they exist)

Computers sometimes store decimal digits directly rather than converting to pure binary — useful for exact decimal arithmetic (e.g., financial calculations where binary rounding is unacceptable).

- **Binary-Coded Decimal (BCD):** each decimal digit stored as its own 4-bit binary pattern (e.g., 27 → `0010 0111`).
- **Zoned decimal:** 1 byte per digit (4 "zone" bits + 4 digit bits); easy to read in/out but can't be used directly in arithmetic.
- **Packed decimal:** 2 digits per byte (no wasted zone bits) — compact and usable in arithmetic directly. Converting packed→zoned is called "unpacking."

You'll rarely touch this day-to-day (most databases use binary integers or a dedicated `DECIMAL` type under the hood), but expect at least one exam question asking you to identify which format is used for input/output vs. internal calculation.

### 2. Fixed-point representation & signed numbers

A **fixed-point number** has the radix (binary) point locked at a specific bit position — usually the far right, i.e., the number is treated as a plain integer.

There are two competing ways to represent _negative_ numbers:

**A. Signed absolute value (sign-magnitude):** the leftmost bit is a pure sign flag (0 = positive, 1 = negative); the rest of the bits are the magnitude, unchanged.

- +89 (8-bit) = `01011001`
- −89 (8-bit) = `11011001` (just flip the sign bit)
- **Problem:** you get two zeros, `+0` (`00000000`) and `−0` (`10000000`) — wasteful and awkward for comparisons.

**B. Two's complement — the one every real computer uses.**

To get the complement of a binary number: flip every bit (this gives you the **1's complement**), then add 1 (this gives you the **2's complement**).

Worked example: represent −89 as an 8-bit two's complement number.

```
+89 in binary:        01011001
Flip every bit (1's complement): 10100110
Add 1 (2's complement):          10100111
```

So −89 = `10100111`.

**Why the industry standardized on two's complement (this is a favorite exam question — memorize both reasons):**

1. **Subtraction becomes addition.** With sign-magnitude, the ALU needs separate add/subtract logic depending on signs. With two's complement, "a − b" is just "a + (−b)" using ordinary binary addition, and any carry-out past the highest bit is simply discarded. This means one adder circuit handles both addition and subtraction — simpler hardware.
2. **Only one representation of zero**, and as a result the representable range is asymmetric but _fully utilized_: an n-bit two's complement number ranges from **−2ⁿ⁻¹ to +2ⁿ⁻¹−1**. (For 8 bits: −128 to +127 — which is exactly the range of a signed `int8` in every language you've used. This is not a coincidence.)

Worked subtraction example using two's complement: compute `12 − 5` as `12 + (−5)`.

```
12 in binary (8-bit):        00001100
5  in binary:                 00000101
1's complement of 5:          11111010
2's complement of 5 (−5):     11111011

  00001100   (+12)
+ 11111011   (−5)
-----------
1 00000111   → discard the carry-out bit → 00000111 = 7  ✓
```

### Key Points

- BCD / zoned / packed decimal exist for exact decimal arithmetic — packed is compact and calculation-ready, zoned matches human-readable I/O.
- 1's complement = flip all bits. 2's complement = flip all bits, then add 1.
- Two's complement is standard because: (1) subtraction reduces to addition, (2) only one zero, giving a clean, fully-used range of −2ⁿ⁻¹ to +2ⁿ⁻¹−1.
- This is exactly the range of a signed integer type in any programming language — e.g., `int8` = −128 to 127.

### Practice Questions

1. What is the 1's complement of `00110101`?
2. What is the 2's complement of `00110101`?
3. Represent −45 as an 8-bit two's complement number (show your work).
4. What is the representable range of a 16-bit signed two's complement integer?
5. Compute `20 − 7` using 8-bit two's complement addition (show the binary steps).
6. Name the two problems with sign-magnitude representation that two's complement solves.

### Day 3 Quiz

**Q1.** The 2's complement of a binary number is obtained by:
A) reversing the bit order B) flipping all bits then adding 1 C) adding 1 then flipping all bits D) multiplying by −1

**Q2.** An 8-bit signed two's complement number can represent values in the range:
A) −127 to +127 B) −128 to +127 C) −128 to +128 D) 0 to 255

**Q3.** Which decimal number format stores two decimal digits per byte and can be used directly in arithmetic?
A) Zoned decimal B) Packed decimal C) BCD-extended D) Floating-point decimal

**Q4.** The main hardware benefit of two's complement is that:
A) it needs a dedicated subtraction circuit
B) subtraction can be performed using the same circuit as addition
C) it doubles the representable range compared to sign-magnitude
D) it eliminates the need for a sign bit entirely

**Q5.** Sign-magnitude representation has which drawback compared to two's complement?
A) It cannot represent negative numbers at all
B) It has two representations of zero (+0 and −0)
C) It requires more bits per number
D) It cannot be converted to hexadecimal

---

**Day 3 Answers:** Q1: B | Q2: B | Q3: B | Q4: B | Q5: B
**Practice Answers:** 1) `11001010` 2) `11001011` 3) 45 = `00101101`, flip = `11010010`, +1 = `11010011` 4) −32,768 to +32,767 5) 20=`00010100`, −7 = flip(`00000111`)=`11111000`+1=`11111001`; `00010100`+`11111001` = `1 00001101` → discard carry → `00001101` = 13 ✓ 6) two zeros exist, and mixed-sign addition/subtraction needs separate logic paths.

---

---

# DAY 4 (Thu Aug 7) — Floating Point (IEEE 754) & Character Codes

### Why this matters for you

Floating point is the single highest-value topic in this section — it explains real bugs you've hit (`0.1 + 0.2` weirdness) and is a guaranteed exam favorite because it's easy to turn into a clean calculation question.

### 1. Floating point — the concept

A fixed number of bits can only represent a limited _range_ of integers. To represent very large or very small numbers with the same bit budget, we instead store a number as:

**value = (−1)^sign × mantissa × radix^exponent**

- **Sign:** 0 = positive, 1 = negative.
- **Mantissa (fraction):** the significant digits.
- **Exponent:** how far to shift the radix point.

This is precisely scientific notation, just in binary: `456,000,000,000 = 0.456 × 10¹²` — you record the digits `456` and the exponent `12` instead of every zero.

### 2. IEEE 754 — the real-world standard

| Format           | Total bits | Sign | Exponent | Mantissa |
| ---------------- | ---------- | ---- | -------- | -------- |
| Single precision | 32         | 1    | 8        | 23       |
| Double precision | 64         | 1    | 11       | 52       |

This is exactly `float` (32-bit) and `double` (64-bit) in every mainstream language. Double precision has a wider exponent range _and_ more mantissa bits — more range AND more precision, which is why `double` is the safer default for financial-adjacent math (though for actual currency, you should use a decimal/fixed-point type instead — floating point can never exactly represent most decimal fractions, as you saw in Day 2's binary-conversion trap).

**Why floating-point rounding errors happen, in one sentence:** the mantissa has a fixed, finite number of bits, and many decimal fractions (like 0.1) require infinitely repeating binary digits, so they get silently rounded to the nearest representable value — which is why comparing floats with `==` is dangerous and why you compare with an epsilon/tolerance instead.

### 3. Character codes

| Code         | Bits                                                         | Scope                                | Key fact                                                                                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| **ASCII**    | 7 data bits + 1 parity bit (8 total)                         | English letters, digits, punctuation | Defined by ANSI, 1962. No support for non-Latin scripts.                                                                         |
| **ISO code** | 7 bits                                                       | International baseline               | Built on ASCII, standardized by ISO in 1967; forms the basis most national character sets built on top of.                       |
| **JIS code** | varies                                                       | Japanese-specific extensions         | Adds kana/kanji support on top of the ISO base. (Lower priority for you outside Japan — know it exists, don't over-invest here.) |
| **Unicode**  | 16/32-bit (not in older textbook editions but worth knowing) | Universal                            | What virtually all modern software actually uses (UTF-8 encoding, etc.)                                                          |

**Key exam concept — "character corruption" (garbled text / mojibake):** happens when data encoded with one character code is _interpreted_ using a different one on the receiving end. As a backend engineer you've absolutely seen this — a UTF-8 file opened as Latin-1 turns into gibberish. Same root cause the textbook describes, just with modern encodings instead of the older ones listed above.

### Key Points

- Floating point = sign + mantissa + exponent, base 2 (or occasionally base 16) inside real computers.
- IEEE 754 single = 32 bits (1/8/23); double = 64 bits (1/11/52).
- More exponent bits = wider range; more mantissa bits = more precision.
- Floating point cannot exactly store most decimal fractions → rounding error is inherent, not a bug.
- ASCII (7-bit + parity) → ISO (7-bit international) → JIS (adds Japanese characters) — a chain of extensions.
- Mismatched character codes between sender and receiver = corrupted/garbled text.

### Practice Questions

1. Write the general formula for a floating-point number in terms of sign, mantissa, and exponent.
2. How many total bits, and how are they split, in an IEEE 754 single-precision float?
3. Why does double precision offer both more range and more precision than single precision?
4. In one sentence, explain why `0.1 + 0.2 == 0.3` is often `false` in code.
5. What was the historical purpose of the 8th bit in the original ASCII byte?
6. Give a real-world example (from your own work) of character corruption and what caused it.

### Day 4 Quiz

**Q1.** In IEEE 754 single precision (32-bit), how many bits are allocated to the mantissa (fraction)?
A) 8 B) 11 C) 23 D) 52

**Q2.** The general form of a floating-point number is:
A) sign × exponent × mantissa² B) (−1)^sign × mantissa × radix^exponent C) mantissa + exponent D) (−1)^exponent × mantissa × radix^sign

**Q3.** ASCII code, as originally defined, uses how many bits for the character code itself (excluding parity)?
A) 6 B) 7 C) 8 D) 16

**Q4.** Compared to single precision, double precision floating point has:
A) fewer exponent bits, more mantissa bits B) more exponent bits, fewer mantissa bits C) more bits for both exponent and mantissa D) the same total bit layout, different byte order

**Q5.** Character corruption ("garbled characters") most directly results from:
A) using too many bits per character B) a mismatch between the character code used to encode and to decode data C) using ASCII instead of Unicode D) storing characters in packed decimal format

---

**Day 4 Answers:** Q1: C | Q2: B | Q3: B | Q4: C | Q5: B
**Practice Answers:** 1) value = (−1)^sign × mantissa × radix^exponent 2) 32 bits: 1 sign + 8 exponent + 23 mantissa 3) it has more exponent bits (wider range) AND more mantissa bits (more precision) than single precision 4) both 0.1 and 0.2 are non-terminating in binary, so each is stored as a rounded approximation, and the sum of the two approximations doesn't exactly equal the stored approximation of 0.3 5) originally a parity bit for basic error detection 6) e.g., a UTF-8-encoded file displayed as Latin-1/Windows-1252, corrupting multi-byte characters into unrelated symbols.

---

---

# DAY 5 (Fri Aug 8) — CPU Configuration & Main Memory

### Why this matters for you

This section formalizes the fetch-decode-execute cycle and memory hierarchy — concepts you've absorbed intuitively from performance tuning and caching, now with exam-precise vocabulary.

### 1. CPU internal configuration

The CPU (control unit + ALU) contains a set of small, extremely fast storage locations called **registers**. Key ones to know:

| Register                           | Role                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------- |
| **Program counter (PC)**           | Holds the address of the _next_ instruction to execute                            |
| **Instruction register (IR)**      | Holds the instruction currently being decoded/executed                            |
| **General-purpose registers**      | Hold operands/intermediate results for the ALU                                    |
| **Accumulator**                    | A specific general-purpose register traditionally used to hold arithmetic results |
| **Memory address register (MAR)**  | Holds the memory address about to be accessed                                     |
| **Memory data register (MDR/MBR)** | Holds the data being transferred to/from memory                                   |

**The fetch–decode–execute cycle** (the CPU's core loop, running billions of times per second):

1. **Fetch:** read the instruction at the address in the Program Counter, load it into the Instruction Register; increment the Program Counter.
2. **Decode:** the control unit interprets what the instruction means.
3. **Execute:** the ALU (or other unit) actually performs the operation.
4. Repeat.

This is the literal hardware version of what an interpreter's "eval loop" does in software — you've built the software analog of this even if you've never thought of it that way.

### 2. Memory hierarchy — speed vs. capacity trade-off

Closer to the CPU = faster but smaller and more expensive; farther away = slower but larger and cheaper.

```
Registers  →  Cache (L1/L2/L3)  →  Main memory (RAM)  →  Auxiliary storage (SSD/HDD)
[fastest, tiniest]                                        [slowest, largest]
```

- **Cache memory** sits between the CPU and main memory to hide the speed gap. It works because of **locality of reference**: programs tend to reuse the same data/instructions repeatedly (temporal locality) and access nearby memory addresses together (spatial locality) — exactly why cache-friendly data structures speed up real code you write.
- **Cache hit ratio:** the fraction of memory accesses satisfied directly from cache. A common exam calculation:

  **Average access time = (hit ratio × cache access time) + ((1 − hit ratio) × main memory access time)**

  Worked example: cache access = 10 ns, main memory access = 100 ns, hit ratio = 90%.
  Average = (0.9 × 10) + (0.1 × 100) = 9 + 10 = **19 ns**

- **RAM types:**
  - **DRAM (Dynamic RAM):** needs periodic refreshing (charge leaks), slower, cheaper, used for main memory.
  - **SRAM (Static RAM):** no refresh needed, faster, more expensive, used for cache.

### Key Points

- Registers: PC (next instruction address), IR (current instruction), accumulator (arithmetic result holder), MAR/MDR (memory address/data interface).
- Fetch → Decode → Execute is the CPU's core loop.
- Memory hierarchy trades speed for capacity: registers > cache > main memory > auxiliary storage.
- Cache works because of locality of reference (temporal + spatial).
- Average access time formula: (hit ratio × cache time) + ((1 − hit ratio) × memory time).
- DRAM = needs refresh, cheap, main memory. SRAM = no refresh, fast, expensive, cache.

### Practice Questions

1. What does the Program Counter hold?
2. Name the three stages of the CPU's basic execution cycle.
3. Why is cache memory effective, in terms of program behavior?
4. A system has cache access time 5 ns, main memory access time 80 ns, and a hit ratio of 95%. Calculate the average access time.
5. Which RAM type needs periodic refreshing: SRAM or DRAM?
6. Why is SRAM used for cache rather than main memory, despite being faster?

### Day 5 Quiz

**Q1.** Which register holds the address of the next instruction to be executed?
A) Accumulator B) Program Counter C) Instruction Register D) MDR

**Q2.** The three core stages of CPU instruction processing, in order, are:
A) Decode, Fetch, Execute B) Fetch, Execute, Decode C) Fetch, Decode, Execute D) Execute, Fetch, Decode

**Q3.** Cache memory improves performance mainly because of:
A) higher clock speed B) locality of reference in program behavior C) larger storage capacity D) non-volatility

**Q4.** Which memory type requires periodic refresh to retain data?
A) SRAM B) DRAM C) ROM D) Flash memory

**Q5.** Given cache access time = 8 ns, main memory access time = 60 ns, and hit ratio = 80%, what is the average access time?
A) 14.8 ns B) 17.6 ns C) 20.0 ns D) 34.0 ns

---

**Day 5 Answers:** Q1: B | Q2: C | Q3: B | Q4: B | Q5: B — (0.8×8) + (0.2×60) = 6.4 + 12 = 17.6 ns
**Practice Answers:** 1) the memory address of the next instruction to fetch 2) fetch, decode, execute 3) programs tend to reuse recently-accessed data/instructions (temporal locality) and access nearby addresses together (spatial locality) 4) (0.95×5)+(0.05×80) = 4.75+4 = 8.75 ns 5) DRAM 6) cache needs to keep up with CPU speed, and SRAM's higher cost is acceptable because cache is small; main memory needs to be large and cheap, which SRAM can't deliver at scale.

---

---

# DAY 6 (Sat Aug 9) — Instructions, Addressing Modes & ALU Circuits

### Why this matters for you

This is the most "assembly-language" feeling day of the week. You won't write assembly for this exam, but understanding addressing modes explains _why_ pointers, arrays, and indirection work the way they do at the hardware level.

### 1. Instruction format

A machine instruction typically has two parts:

- **Operation code (opcode):** what to do (add, load, jump, compare, etc.)
- **Operand (address part):** what data to do it to / where to find it

### 2. Addressing modes — how the operand's address is actually determined

| Mode                           | How it works                                                                                               | Analogy                                                     |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Immediate addressing**       | The operand value is embedded directly in the instruction                                                  | `x = 5` — the literal 5 is right there                      |
| **Direct addressing**          | The instruction holds the actual memory address of the operand                                             | `x = memory[100]`                                           |
| **Indirect addressing**        | The instruction holds an address, and _that address_ holds the address of the real operand (one extra hop) | A pointer to a pointer                                      |
| **Register addressing**        | The operand is in a CPU register, not memory                                                               | Using a local variable that the compiler kept in a register |
| **Index addressing**           | A base address + an index register's value = final address                                                 | `array[i]` — base address of the array + offset `i`         |
| **Base addressing (relative)** | A base register's value + a fixed displacement = final address                                             | Used heavily for relocatable code and stack frames          |

**Backend connection:** index addressing is _literally_ how array indexing (`arr[i]`) compiles down to hardware instructions — base address + (index × element size). Indirect addressing is the hardware ancestor of the pointer-dereference (`*ptr`) concept in C-like languages.

### 3. ALU circuit basics — how "1 + 1 = 10" happens electronically

The ALU is built from **logic gates**, which implement Boolean operations:

| Gate           | Behavior                                                                                        |
| -------------- | ----------------------------------------------------------------------------------------------- |
| **AND**        | Output 1 only if both inputs are 1                                                              |
| **OR**         | Output 1 if at least one input is 1                                                             |
| **NOT**        | Inverts the input                                                                               |
| **XOR**        | Output 1 if inputs differ                                                                       |
| **NAND / NOR** | AND/OR followed by NOT (these are "universal gates" — any circuit can be built from NAND alone) |

A **half adder** adds two single bits and produces a sum bit and a carry bit:

- Sum = A XOR B
- Carry = A AND B

A **full adder** extends this to also accept a carry-in from the previous bit position (needed to chain adders together for multi-bit addition) — this is literally how your CPU adds two 64-bit numbers: 64 full adders chained together, carry rippling from the lowest bit to the highest.

### Key Points

- Instruction = opcode + operand (address part).
- Addressing modes: immediate (value in instruction), direct (address in instruction), indirect (address of an address), register (value in a register), index (base + index register), base/relative (base register + displacement).
- Logic gates: AND, OR, NOT, XOR, NAND, NOR.
- Half adder: Sum = A XOR B, Carry = A AND B. Full adder chains these with carry-in/out to build multi-bit adders.

### Practice Questions

1. What is stored in the "operand" part of an instruction under immediate addressing?
2. Explain the difference between direct and indirect addressing in one sentence.
3. Which addressing mode most directly explains how `array[i]` is computed in hardware?
4. Write the Boolean formulas for the Sum and Carry outputs of a half adder.
5. Why are NAND gates called "universal gates"?
6. A full adder differs from a half adder by accepting an extra input — what is it?

### Day 6 Quiz

**Q1.** In immediate addressing, the operand is:
A) an address pointing to the data B) the actual data value, embedded in the instruction C) held in a register D) computed from a base + index

**Q2.** Index addressing computes the final address as:
A) opcode + operand B) base address + index register value C) address of an address D) a fixed value in the instruction

**Q3.** A half adder's Sum output is computed as:
A) A AND B B) A OR B C) A XOR B D) NOT A

**Q4.** Which gate's output is 1 only when both inputs are 1?
A) OR B) XOR C) AND D) NOT

**Q5.** Indirect addressing requires how many memory accesses (beyond the instruction fetch) to reach the actual data, compared to direct addressing?
A) The same number B) One fewer C) One more D) Indirect addressing doesn't use memory

---

**Day 6 Answers:** Q1: B | Q2: B | Q3: C | Q4: C | Q5: C
**Practice Answers:** 1) the actual value to be used, not an address 2) direct addressing stores the operand's real memory address in the instruction; indirect addressing stores the address of _another_ memory location that itself holds the real address 3) index addressing (base address of the array + index register offset) 4) Sum = A XOR B, Carry = A AND B 5) because any logic function can be constructed using only NAND gates 6) a carry-in bit from the previous (lower-order) bit position, allowing adders to be chained for multi-bit numbers.

---

---

# DAY 7 (Sun Aug 10) — High-Speed CPU Techniques + Week 1 Review

### Why this matters for you

This closes out the "why is my CPU fast" story — pipelining and parallelism are concepts you've likely heard of from performance discussions; today formalizes them into exam-ready definitions.

### 1. High-speed processing techniques

- **Pipelining:** overlaps the stages of multiple instructions, like an assembly line. While instruction 1 is being _executed_, instruction 2 can already be _decoded_, and instruction 3 can already be _fetched_. This increases throughput without increasing clock speed — analogous to how you'd overlap I/O-bound and CPU-bound work in an async pipeline instead of doing everything strictly sequentially.
  - A **pipeline hazard** (e.g., a branch instruction whose outcome isn't known yet) can stall the pipeline — the hardware equivalent of your async pipeline blocking on an unresolved dependency.
- **Superscalar architecture:** has multiple execution units so it can dispatch _more than one instruction per clock cycle_ — true parallel execution within a single core, not just overlapped stages.
- **Multi-core / multiprocessing:** entire additional CPU cores, each capable of running an independent instruction stream — this is the hardware basis for the concurrent/parallel programming you already do (multi-threading benefits from this directly).
- **VLIW / other advanced techniques:** the textbook may mention Very Long Instruction Word designs where the compiler (not the hardware) decides which operations run in parallel — lower priority for exam purposes, know the name.

### 2. Week 1 Full Review Quiz (10 questions, mixed topics)

**Q1.** The stored-program concept was introduced by which machine?
A) ENIAC B) EDSAC C) IBM/360 D) UNIVAC I

**Q2.** CPU = which two units combined?
A) Input + Output B) Control unit + ALU C) Main memory + Cache D) Register + Cache

**Q3.** Convert `101110₂` to decimal.
A) 44 B) 46 C) 48 D) 45

**Q4.** To convert binary to octal, group bits in sets of:
A) 2 B) 3 C) 4 D) 8

**Q5.** The 2's complement of `01001000` is:
A) 10110111 B) 10111000 C) 10110000 D) 11000111

**Q6.** An 8-bit two's complement number's range is:
A) 0 to 255 B) −127 to 127 C) −128 to 127 D) −256 to 255

**Q7.** In IEEE 754 double precision, how many bits are used for the exponent?
A) 8 B) 11 C) 23 D) 52

**Q8.** Which addressing mode uses a base address plus an index register value?
A) Immediate B) Direct C) Index D) Indirect

**Q9.** Cache memory is effective mainly due to:
A) larger capacity than RAM B) locality of reference C) non-volatility D) lower cost per bit

**Q10.** Pipelining improves CPU throughput by:
A) increasing clock speed B) overlapping the stages of multiple instructions C) adding more registers D) using two's complement arithmetic

<br>

**Week 1 Review Answers:** Q1: B | Q2: B | Q3: B (1·32+0·16+1·8+1·4+1·2+0·1 = 32+8+4+2 = 46) | Q4: B | Q5: A | Q6: C | Q7: B | Q8: C | Q9: B | Q10: B

### Self-check before moving to Week 2

Rate yourself 1–5 (1 = shaky, 5 = solid) on:

- [ ] Radix conversion (binary/decimal/hex/octal), including fractions
- [ ] Two's complement arithmetic
- [ ] IEEE 754 floating point layout and why rounding errors happen
- [ ] Fetch-decode-execute cycle and register roles
- [ ] Addressing modes
- [ ] Cache/memory hierarchy and hit-ratio calculations

Anything you rated 3 or below, note it — we'll fold it into Week 10's targeted-review days. If most of your ratings are 4–5, you're exactly on pace.

**Next up: Week 2 — Auxiliary Storage, I/O, and Information Processing Systems.**
