# ITFE Study Pack — Week 2 (Aug 11–17)

## Hardware Part II (Auxiliary Storage, I/O) + Chapter 2: Information Processing Systems

---

---

# DAY 8 (Mon Aug 11) — Auxiliary Storage & Disk Access Time Calculations

### Why this matters for you

This is the "storage math" day — the exam loves turning disk geometry (cylinders/tracks/sectors) and access-time formulas into calculation questions. As a backend engineer you already know SSDs have replaced most of this physically, but the _calculation method_ (and the vocabulary: seek time, rotational latency, transfer time) is exactly the mental model behind why database index design and "sequential vs. random I/O" performance differences exist.

### 1. Magnetic disk anatomy

- **Track:** one concentric ring on the disk surface.
- **Sector:** a fixed-length slice of a track — the smallest addressable chunk.
- **Cylinder:** the same track position across _all_ stacked platters, accessible without moving the read/write arm.
- Hierarchy: **drive > cylinder > track > sector**.

### 2. Storage capacity calculation — work from smallest unit up

```
Capacity of 1 track   = bytes/sector × sectors/track
Capacity of 1 cylinder = capacity of 1 track × tracks/cylinder
Capacity of drive      = capacity of 1 cylinder × cylinders/drive
```

**Worked example** (different numbers than any textbook example, so work it yourself first):
A drive has 400 cylinders, 15 tracks/cylinder, 40 sectors/track, 512 bytes/sector.

- 1 track = 512 × 40 = 20,480 bytes
- 1 cylinder = 20,480 × 15 = 307,200 bytes
- Whole drive = 307,200 × 400 = 122,880,000 bytes ≈ 122.88 MB

### 3. Access time — the three-part formula (memorize this)

**Access time = seek time + rotational latency + data transfer time**

| Component              | What it means                                                                                                                                                       | Backend analogy                                                     |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Seek time**          | Time to move the read/write arm to the correct track                                                                                                                | Like `seek()`-ing to a file offset                                  |
| **Rotational latency** | Time waiting for the disk to spin the right sector under the head. Since this varies, you use the **average rotational latency = (time for one full rotation) ÷ 2** | The "worst case is a full lap, best case is zero, average it" logic |
| **Data transfer time** | Time to actually read/write once the head is in position                                                                                                            | Bandwidth-bound copy time                                           |

**Worked example** — disk spins at 6,000 RPM, has 12,000 bytes per track, average seek time 25 ms. Find the average access time for reading a 3,000-byte block.

1. Time for one full rotation = 60,000 ms ÷ 6,000 rotations = **10 ms/rotation**
2. Average rotational latency = 10 ms ÷ 2 = **5 ms**
3. Data transfer rate = 12,000 bytes/track ÷ 10 ms = **1,200 bytes/ms**
4. Data transfer time = 3,000 bytes ÷ 1,200 bytes/ms = **2.5 ms**
5. Average access time = 25 + 5 + 2.5 = **32.5 ms**

**Why this matters practically (and shows up as a concept question, not just a calculation):** the textbook notes that shortening seek time (by keeping data in contiguous areas — i.e., avoiding fragmentation) is one of the most effective ways to reduce total access time, since seek time tends to dominate. This is the _literal hardware reason_ database engineers care about sequential vs. random disk access, and why B-tree indexes are designed to minimize the number of separate disk seeks.

### 3a. The four-term version, and a unit-conversion trap (newly added — matches the real exam's exact style)

Real exam questions often add a fourth component: **controller overhead** — fixed processing delay from the disk controller itself, separate from the physical seek/rotation/transfer.

**Access time = average seek time + controller overhead + rotational latency + data transfer time**

**Worked example (exam-style, with a real unit-conversion trap built in):** an HDD has average seek time 5 ms, rotation speed 6,000 RPM, transfer rate 1 MB/s, controller overhead 0.1 ms. What is the average access time to transfer 16 kB of data? _(Use 1 MB = 1,024 kB — this exact conversion factor is stated explicitly because 1,000 vs. 1,024 changes the answer, and the exam wants to see if you use the one it gave you.)_

1. **Seek time** = 5 ms (given directly).
2. **Controller overhead** = 0.1 ms (given directly).
3. **Rotational latency:** time for 1 full rotation = 60,000 ms ÷ 6,000 rotations = 10 ms/rotation → average rotational latency = 10 ÷ 2 = **5 ms**.
4. **Data transfer time:** transfer rate is given in MB/s, but the data is in kB — convert to consistent units first. 1 MB/s = 1,024 kB/s = 1.024 kB/ms. Transfer time = 16 kB ÷ 1.024 kB/ms = **15.625 ms**.
5. **Total access time** = 5 + 0.1 + 5 + 15.625 = **25.725 ms**

**The two traps this question is designed to catch:**

- Forgetting the controller overhead term entirely (a common oversight if you've only memorized the classic 3-term formula).
- Using 1 MB = 1,000 kB instead of the stated 1,024 kB, which silently shifts your transfer-time answer and therefore your final total — always use whatever conversion factor the question explicitly states, even if it conflicts with a "cleaner" real-world assumption.

**Backend connection:** this multi-term overhead stacking is exactly why real-world I/O latency numbers (e.g., in a `EXPLAIN ANALYZE` query plan or an S3 request latency breakdown) are never just "one number" — they're always the sum of several distinct overhead sources, each with a different root cause and a different lever to pull if you want to reduce it.

### 4. Optical discs (lower priority, but appears occasionally)

- **CD:** ~700 MB. **DVD single-layer:** 4.7 GB. **DVD dual-layer:** 8.5 GB.
- **Read-only** (CD-ROM/DVD-ROM): data pressed at the factory as physical pits — cheap to mass-produce, can't be written by the user.
- **Write-once** (CD-R/DVD-R/DVD+R): user can burn data once by scorching an organic dye layer with a laser; cannot be rewritten once burned.
- Optical seek time tends to be _longer_ than magnetic disk (heavier optical head), so average access time is typically worse.

### Key Points

- Capacity: build up sector → track → cylinder → drive.
- Access time = seek time + rotational latency + transfer time (add controller overhead as a 4th term when the question gives one).
- Average rotational latency = (time per full rotation) ÷ 2.
- Data transfer rate = track capacity ÷ time per rotation; watch for MB vs. kB (1,024 vs. 1,000) unit-conversion traps.
- Reducing fragmentation shortens seek time, which usually dominates access time.

### Practice Questions

1. A drive has 250 cylinders, 10 tracks/cylinder, 50 sectors/track, 256 bytes/sector. What's the total capacity?
2. Write the 3-part access time formula from memory.
3. A disk spins at 7,200 RPM. What is the time for one full rotation, in milliseconds?
4. Using the answer from Q3, what is the average rotational latency?
5. Why does reducing fragmentation improve average access time?
6. Which typically has a longer average access time: magnetic disk or optical disc? Why?
7. An HDD has average seek time 4 ms, controller overhead 0.2 ms, rotation speed 7,200 RPM, and transfer rate 2 MB/s. Using 1 MB = 1,024 kB, what is the average access time to transfer 8 kB? Show every step.

### Official-Exam-Style Practice (matching real ITFE Subject A format/difficulty)

**EP1.** An HDD has the specifications: average seek time 4 ms, rotation speed 7,200 RPM, transfer rate 2 MB/s, controller overhead 0.15 ms. What is the average access time to transfer 20 kB of data, in milliseconds? Here, 1 MB = 1,024 kB, and other overheads can be ignored.
a) 9.30 b) 13.65 c) 18.08 d) 24.5

**EP2.** Which of the following is an appropriate description concerning the storage capacity of a magnetic disk drive with multiple platters?
a) A cylinder refers to the same-numbered track across all platter surfaces, accessible without moving the read/write arm.
b) A sector is always larger than a track in terms of storage capacity.
c) Increasing the number of platters always decreases total seek time.
d) A track's capacity is calculated by multiplying the number of cylinders by the number of sectors.

_(Answers: EP1 → c. seek=4ms; overhead=0.15ms; rotation time=60,000÷7,200=8.33ms→avg latency=4.17ms; transfer rate=2MB/s=2×1,024=2,048 kB/s=2.048 kB/ms→transfer time=20÷2.048=9.77ms; total=4+0.15+4.17+9.77≈18.08ms. EP2 → a.)_

### Day 8 Quiz

**Q1.** The correct storage hierarchy (largest to smallest) for a magnetic disk drive is:
A) sector > track > cylinder > drive B) drive > cylinder > track > sector C) drive > track > cylinder > sector D) cylinder > drive > sector > track

**Q2.** Average rotational latency is calculated as:
A) time for one full rotation × 2 B) time for one full rotation ÷ 2 C) seek time ÷ 2 D) data transfer rate ÷ 2

**Q3.** A disk spins at 5,000 RPM. Time for one full rotation is:
A) 6 ms B) 8 ms C) 10 ms D) 12 ms

**Q4.** Given: rotation time = 10 ms, track capacity = 8,000 bytes. The data transfer rate is:
A) 400 bytes/ms B) 800 bytes/ms C) 8,000 bytes/ms D) 80 bytes/ms

**Q5.** DVD-R is best classified as:
A) read-only B) write-once C) rewritable D) magnetic storage

**Q6 (exam-style, 4-term calculation).** An HDD has average seek time 5 ms, rotation speed 6,000 RPM, transfer rate 1 MB/s, controller overhead 0.1 ms. Using 1 MB = 1,024 kB, what is the average access time to transfer 16 kB?
A) 20.1 ms B) 25.725 ms C) 30.725 ms D) 74.1 ms

---

**Day 8 Answers:** Q1: B | Q2: B | Q3: D (60,000 ms ÷ 5,000 rotations = 12 ms) | Q4: B | Q5: B | Q6: B (worked in section 3a above: 5 + 0.1 + 5 + 15.625 = 25.725 ms)

**Practice Answers:** 1) 256×50=12,800/track; ×10=128,000/cylinder; ×250=32,000,000 bytes ≈ 32 MB 2) seek time + rotational latency + data transfer time 3) 60,000÷7,200 = 8.33 ms 4) 8.33÷2 = 4.17 ms 5) less arm movement is needed when data sits in contiguous areas, so seek time (which tends to dominate access time) shrinks 6) optical disc, because the optical head is heavier than a magnetic head, making seek slower 7) seek=4ms; overhead=0.2ms; rotation time=60,000÷7,200=8.33ms→avg latency=4.17ms; transfer rate=2MB/s=2×1,024=2,048 kB/s=2.048 kB/ms→transfer time=8÷2.048=3.91ms; total=4+0.2+4.17+3.91=**≈12.28 ms**.

---

---

# DAY 9 (Tue Aug 12) — Input/Output Units & Interfaces

### Why this matters for you

This day is mostly vocabulary and classification — the exam likes matching devices/interfaces to their defining characteristic. Skim for recognition rather than deep memorization; you'll never need to derive anything here, just recall facts.

### 1. Output devices — quick reference

| Device       | Mechanism                                                   | Key trait                                                                                                  |
| ------------ | ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| CRT          | Electron beam on phosphor screen                            | Fast, bulky, high power                                                                                    |
| LCD          | Liquid crystal blocks/passes light                          | Thin, low power; **TFT** (transistor per pixel) has faster response and wider viewing angle than older STN |
| PDP (plasma) | Ionized gas between glass panels                            | High luminance, wide viewing angle, high power draw                                                        |
| OLED         | Organic compound emits light directly (no backlight needed) | Fast response, high contrast, low power, shorter lifespan                                                  |

**Printers:**

- **Impact** (dot-matrix, line printer): physically strikes ribbon onto paper — noisy, can print carbon-copy multiples.
- **Non-impact** (thermal, inkjet, laser): no physical striking — quieter, no carbon copies.
- **Laser printer** performance is measured by **dpi** (dots per inch, resolution) and **ppm** (pages per minute, speed) — this pairing is a common exam matching question.
- Color printing uses **CMY** (Cyan/Magenta/Yellow); real printers add **K** (black) → **CMYK**, because mixing CMY alone can't produce a true black.

**1a. Modern/emerging output devices (newly added — these show up in recent exam sittings as distractor-heavy questions):**

- **3D printer:** builds a genuinely three-dimensional physical object, typically layer by layer, using methods like **FFF/FDM (Fused Filament/Deposition Modeling)** — melting and extruding plastic filament layer by layer — or resin-curing/sintering methods. The key exam distinction: a 3D printer _makes_ physical 3D objects; it does not scan or detect them (that's a 3D scanner) and it doesn't project images onto surfaces (that's projection mapping, next).
- **Projection mapping:** projects computer graphics onto irregular/uneven 3D surfaces (buildings, furniture, product displays) so the imagery appears to conform to the object's shape — this is a display/projection technique, not a fabrication technique, and it's a favorite wrong-answer pairing against 3D printers on the exam.
- **3D scanner:** the reverse of a 3D printer — an _input_ device that detects the shape of a real-world three-dimensional object and produces 3D data output from it. If a question describes "detecting a shape and producing 3D data," that's a scanner (input), not a printer (output) — a very common exam trap.

### 2. I/O control methods — how data actually moves between a device and memory

| Method                               | How it works                                                                                                                                                   | CPU involvement                      |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| **Program control (direct control)** | CPU itself interprets the I/O program and directs the device, moving every byte through itself                                                                 | Highest — CPU is the bottleneck      |
| **DMA (Direct Memory Access)**       | A dedicated DMA controller moves data directly between device and main memory; CPU just issues the request and is free to do other work while transfer happens | Low — CPU only involved at start/end |
| **Channel control**                  | A dedicated I/O processor (channel) runs its own program to control I/O entirely independently of the CPU (used in mainframes)                                 | Lowest — fully offloaded             |

**Backend connection:** this is the hardware ancestor of async I/O in your applications. Program control is like blocking, synchronous I/O (your thread waits). DMA is like non-blocking I/O with a callback — the CPU "fires and forgets," then gets notified (interrupt) when done. You've been unconsciously relying on this hardware pattern every time you use `async/await` for a network call.

### 3. I/O interfaces

| Interface   | Type                  | Notable facts                                                                                   |
| ----------- | --------------------- | ----------------------------------------------------------------------------------------------- |
| **RS-232C** | Serial                | Classic modem/serial-mouse standard, two-way transmission                                       |
| **SCSI**    | Parallel (originally) | Daisy-chains up to 7 devices, each with a unique SCSI ID (0–7); Ultra Wide SCSI reaches 40 Mbps |
| **USB**     | Serial                | Standardized 1996; tree topology via hubs, up to 127 devices                                    |

**Connection topologies:**

- **Star:** devices connect through a central hub.
- **Cascade:** hubs chained to other hubs (multi-level star) — star + cascade are jointly called **tree connection**.
- **Daisy chain:** devices connected one after another in a line, usually terminated at the far end.

### Key Points

- OLED needs no backlight (self-emitting); LCD does.
- Laser printer = dpi (quality) + ppm (speed).
- CMYK, not just CMY, because CMY alone can't make true black.
- Program control = CPU does everything (bottleneck). DMA = dedicated controller frees CPU. Channel control = fully independent I/O processor (mainframes).
- SCSI: up to 7 daisy-chained devices, unique ID each. USB: up to 127 devices, tree topology via hubs.

### Practice Questions

1. Which display technology requires no backlight because it emits light directly?
2. What two metrics are used to evaluate laser printer performance?
3. Why is CMYK used instead of just CMY?
4. Rank program control, DMA, and channel control from _most_ to _least_ CPU involvement.
5. How many devices can a single SCSI chain support, and how are they identified?
6. Name the three device connection topologies described above.
7. What's the precise difference between a 3D printer, a 3D scanner, and projection mapping? (This is a very common exam mix-up.)

### Official-Exam-Style Practice (matching real ITFE Subject A format/difficulty)

**EP1.** Which of the following is an appropriate description of DMA (Direct Memory Access) control?
a) The CPU directly executes every byte-level transfer between the I/O device and main memory.
b) A dedicated DMA controller transfers data directly between the I/O device and main memory, and the CPU is only involved at the start and end of the transfer.
c) An independent I/O processor runs its own program entirely separately from the CPU, used mainly in mainframes.
d) The I/O device communicates only through a channel program, bypassing main memory entirely.

**EP2.** Which of the following is the most appropriate explanation of a 3D printer's function?
a) It detects the shape of three-dimensional objects and produces output of 3D data.
b) It functions by pushing the pins of a high-temperature printing head onto heat-sensitive paper.
c) It makes three-dimensional objects using methods such as fused filament fabrication.
d) It projects computer graphics onto uneven three-dimensional objects such as buildings and furniture.

_(Answers: EP1 → b — this is the definition of DMA specifically; (a) describes program control, (c) describes channel control. EP2 → c — (a) describes a 3D scanner, (d) describes projection mapping; this exact question format appeared on a real ITPEC sitting.)_

### Day 9 Quiz

**Q1.** Which of these does NOT require a backlight to produce an image?
A) TFT LCD B) STN LCD C) OLED D) All of the above require a backlight

**Q2.** In the DMA method, data transfer between an I/O device and main memory:
A) always passes through the CPU B) is handled by a dedicated controller, freeing the CPU C) requires the channel program D) is impossible without channel control

**Q3.** A laser printer's speed is typically measured in:
A) dpi B) fps C) ppm D) Mbps

**Q4.** SCSI can connect up to how many devices via daisy chain, using unique IDs 0–7?
A) 5 B) 7 C) 8 D) 127

**Q5.** The connection topology where devices connect one after another in a single line is called:
A) star B) cascade C) daisy chain D) mesh

**Q6 (exam-style).** Which of the following correctly describes a 3D printer's function?
A) It detects the shape of three-dimensional objects and outputs 3D data B) It projects computer graphics onto uneven three-dimensional surfaces C) It makes three-dimensional objects using methods such as fused filament fabrication D) It reads magnetic stripes to identify physical objects

---

**Day 9 Answers:** Q1: C | Q2: B | Q3: C | Q4: B | Q5: C | Q6: C
**Practice Answers:** 1) OLED 2) dpi (resolution) and ppm (speed) 3) mixing cyan, magenta, and yellow cannot produce a true, deep black 4) most CPU involvement: program control > DMA > channel control (least) 5) 7 devices, each assigned a unique SCSI ID 0–7 6) star, cascade (tree), and daisy chain 7) a 3D printer is an _output_ device that fabricates a physical 3D object (e.g., via FFF/FDM layer-by-layer extrusion); a 3D scanner is an _input_ device that detects an existing physical object's shape and produces 3D data from it; projection mapping _displays_ computer graphics onto the surface of an existing uneven 3D object rather than fabricating or scanning anything.

---

---

# DAY 10 (Wed Aug 13) — Chapter 1 Full Review

### Instructions

No new content today — this is a consolidation day. Re-read your Week 1 "Key Points" boxes and the Day 8–9 material above, then take this comprehensive 15-question quiz cold (no peeking). Time yourself: aim for under 20 minutes, since Subject A gives you roughly 1.5 minutes per question.

### Chapter 1 Comprehensive Quiz

**Q1.** Which generation of computers used IC (Integrated Circuit) as logic gates?
A) 2nd B) 3rd C) 3.5th D) 4th

**Q2.** Convert `11010₂` to decimal.
A) 24 B) 26 C) 25 D) 27

**Q3.** The 2's complement of `00010111` is:
A) 11101000 B) 11101001 C) 11100111 D) 11110001

**Q4.** IEEE 754 single precision uses how many bits for the exponent?
A) 5 B) 8 C) 11 D) 23

**Q5.** Cache memory improves performance primarily due to:
A) higher voltage B) locality of reference C) larger capacity than RAM D) lower latency by design alone, unrelated to access patterns

**Q6.** Which register holds the address of the next instruction?
A) MDR B) Accumulator C) Program Counter D) IR

**Q7.** Index addressing computes an address as:
A) opcode + operand B) base address + index register value C) a literal value D) address of an address

**Q8.** Pipelining improves throughput by:
A) raising clock speed B) overlapping instruction execution stages C) increasing word size D) using channel control

**Q9.** A drive has 100 cylinders, 8 tracks/cylinder, 20 sectors/track, 512 bytes/sector. Total capacity is:
A) 8,192,000 bytes B) 8,192 bytes C) 819,200 bytes D) 81,920,000 bytes

**Q10.** Access time is the sum of:
A) seek time + transfer rate B) seek time + rotational latency + data transfer time C) rotational latency × transfer rate D) cylinder count × sector size

**Q11.** Which I/O control method fully offloads I/O processing from the CPU using a dedicated processor?
A) Program control B) DMA C) Channel control D) Register control

**Q12.** SCSI can daisy-chain up to how many devices?
A) 5 B) 7 C) 127 D) 255

**Q13.** OLED displays are distinguished from LCD by:
A) needing a backlight B) emitting light on their own without a backlight C) using plasma gas D) being a printer technology

**Q14.** Two's complement is preferred over sign-magnitude because it:
A) needs more circuitry B) allows subtraction via addition and has only one zero C) can't represent negative numbers D) uses fewer bits

**Q15.** RISC architecture is generally more suitable than CISC for pipelining because:
A) RISC instructions have a more uniform, fixed length and execution time B) RISC uses microprograms C) CISC has fewer instructions D) RISC doesn't use registers

**Q16 (official-exam-style).** Let n be a binary integer in two's complement. Which operation computes `9 × n` using only bit shifting and one addition?
A) Shift n left 2 bits, then add n B) Shift n left 3 bits, then add n C) Shift n left 3 bits, then subtract n D) Shift n left 4 bits, then add n

**Q17 (official-exam-style).** An HDD has average seek time 3 ms, controller overhead 0.1 ms, rotation speed 6,000 RPM, and transfer rate 4 MB/s. Using 1 MB = 1,024 kB, what is the average access time to transfer 4 kB, in milliseconds?
A) 4.08 B) 6.08 C) 9.08 D) 13.08

---

**Answers:** Q1: B | Q2: B | Q3: A | Q4: B | Q5: B | Q6: C | Q7: B | Q8: B | Q9: A | Q10: B | Q11: C | Q12: B | Q13: B | Q14: B | Q15: A | Q16: B (9=8+1=2³+1, so shift left 3 bits then add n) | Q17: C (seek=3ms, overhead=0.1ms, rotation time=60,000÷6,000=10ms→avg latency=5ms, transfer rate=4MB/s=4×1,024=4,096 kB/s=4.096 kB/ms→transfer time=4÷4.096≈0.98ms; total=3+0.1+5+0.98≈9.08ms)

### Self-scoring

- 13–15 correct: You've got Chapter 1 solid. Move on confidently.
- 10–12 correct: Good, but skim back over whichever 3–5 topics you missed before Week 2 continues.
- Below 10: Spend an extra hour tonight re-reading the "Key Points" sections for Days 1–9 before continuing — don't let gaps compound, since Chapter 2 builds on some of these ideas (e.g., you'll reuse the access-time and reliability math again this week).

---

---

# DAY 11 (Thu Aug 14) — Processing Types (Batch, Real-Time, Distributed) & ACID

### Why this matters for you

This is one of the most _directly relevant_ days in the whole textbook for a backend engineer — ACID properties, client/server tiers, and batch vs. real-time processing are concepts you use professionally, just possibly without the formal exam vocabulary attached.

### 1. Non-interactive vs. interactive processing

- **Non-interactive:** a full set of instructions is submitted; no human intervenes once it starts (think: a submitted batch job).
- **Interactive:** the user and system exchange commands/responses step by step (think: a REPL, or any UI you click through).

### 2. Batch vs. real-time processing

**Batch processing:** data accumulates, then gets processed together at a scheduled time. Good for periodic tasks with no urgency (e.g., payroll calculation, nightly reports).

- **Center batch processing:** data physically brought to a central computer, processed periodically. Sub-variants describe _who_ does data entry vs. operation (open/closed/cafeteria batch — low exam priority, just recognize the names).
- **Remote batch processing:** requests submitted from a remote terminal (RJE — Remote Job Entry), accumulated centrally, then processed.

**Real-time processing:** the system starts processing the moment a request arrives — no waiting to accumulate a batch.

- **Hard real-time:** missing the deadline causes serious/fatal consequences (e.g., an airbag controller). No excuses — the response must land within the time bound.
- **Soft real-time:** missing the deadline is undesirable but not catastrophic (e.g., a booking system responding a bit slowly).
- **OLTP (On-Line Transaction Processing):** remote requests sent to a central system and processed immediately (e.g., an ATM). This is a form of transaction processing.
- **TSS (Time Sharing System):** many users share one computer, each getting a small time slice, giving the illusion each has exclusive access — the direct ancestor of how your OS scheduler timeshares a CPU core across your open applications right now.

### 3. ACID — you already know this, now know the exam's exact phrasing

Transaction processing (updating a master file or database from a stream of transaction requests) requires **ACID**:

| Property        | Meaning (exam phrasing)                       | What it means in your day job                                      |
| --------------- | --------------------------------------------- | ------------------------------------------------------------------ |
| **Atomicity**   | "All are executed" or "none is executed"      | A DB transaction either fully commits or fully rolls back          |
| **Consistency** | Processing doesn't create inconsistencies     | The DB moves from one valid state to another valid state           |
| **Isolation**   | Transactions don't interfere with one another | Concurrent transactions don't see each other's uncommitted changes |
| **Durability**  | The result stays recorded even after a fault  | Once committed, a crash right after doesn't undo it (WAL, etc.)    |

This is the exact ACID you know from `BEGIN; ... COMMIT;` in SQL — the exam just wants you to recite the textbook definition of each letter precisely, since it's an easy, guaranteed point if memorized.

### 4. Centralized vs. distributed processing

|                      | Centralized                         | Distributed                        |
| -------------------- | ----------------------------------- | ---------------------------------- |
| Setup/operation      | Easy                                | Difficult                          |
| Load on one computer | High (everything on one machine)    | Low (spread across machines)       |
| Reliability          | Low (one failure = everything down) | High (other machines keep running) |
| Security             | Easy to control                     | Harder to control                  |

**Distributed processing sub-types:**

- **Horizontal distributed (P2P — Peer to Peer):** all connected computers are equals; each must be able to both request and serve. Cheap and simple to set up, but doesn't scale well to large systems because every peer needs full capability and shared-resource management gets messy.
- **Vertical distributed (Client/Server):** a clear hierarchy — clients request, servers provide. This is your entire career. Common server roles: **print server, file server, database server, communication/gateway server, proxy server** (the last one specifically stands in for the client when reaching an external network).

**Two-tier vs. three-tier client/server (an exam favorite because it maps exactly onto real architecture you've built):**

- **Two-tier:** client handles UI + business logic; server just handles the database. Downside: updating business logic means redeploying every client.
- **Three-tier:** splits into **presentation layer** (UI), **function/application layer** (business logic — processes SQL statements, does calculations), and **database access layer** (data). This is precisely your typical web app: frontend / backend API / database. The exam is essentially describing the architecture pattern you build every week, just using its own formal layer names.

### Key Points

- Batch = accumulate then process together; real-time = process immediately on request.
- Hard real-time = deadline miss is catastrophic; soft real-time = deadline miss is tolerable but undesirable.
- ACID: Atomicity, Consistency, Isolation, Durability — memorize the exact wording.
- Centralized = simple but fragile and high load; distributed = complex but resilient and load-spread.
- P2P = horizontal, all equal. Client/server = vertical, hierarchical.
- Three-tier = presentation / function (application) / database access layers — this is your web stack.

### Practice Questions

1. What's the key difference between hard and soft real-time systems?
2. List the four ACID properties and give a one-line definition for each, from memory.
3. Why does distributed processing offer higher reliability than centralized processing?
4. What's the main drawback of a two-tier client/server system that three-tier solves?
5. Name three types of servers found in a client/server system.
6. Which processing type — P2P or client/server — is "vertical distributed"?

### Official-Exam-Style Practice (matching real ITFE Subject A format/difficulty)

**EP1.** In the context of transaction management, which of the following is a condition that the Isolation property ensures?
a) Data is written permanently after a transaction commits.
b) Only valid data is written to the database, with no contradictions.
c) Transactions are either fully completed or fully revoked.
d) Transactions are executed without affecting each other, even when running concurrently.

**EP2.** Which of the following is an appropriate description of a distributed database system?
a) Access to a single central database server is shared among a globally distributed userbase.
b) A database's data is made freely available to researchers worldwide.
c) A NoSQL database is used instead of a relational DBMS.
d) Different parts of a database are stored in different physical locations, and processing is distributed across those parts.

_(Answers: EP1 → d — this is the precise definition of Isolation; (a) describes Durability, (b) describes Consistency, (c) describes Atomicity. EP2 → d — a distributed database's defining trait is that the data itself, not just access to it, is physically spread across multiple locations, with query processing spread across those same locations.)_

### Day 11 Quiz

**Q1.** A system where missing a deadline could cause loss of life is an example of:
A) Soft real-time B) Hard real-time C) Batch processing D) TSS

**Q2.** Which ACID property ensures a transaction is either fully completed or not executed at all?
A) Consistency B) Isolation C) Atomicity D) Durability

**Q3.** TSS (Time Sharing System) works by:
A) processing all requests in a nightly batch B) dividing CPU time into slices shared among multiple users C) using only one user at a time exclusively D) requiring a dedicated channel processor

**Q4.** In a three-tier client/server architecture, which layer handles business logic and processes SQL statements?
A) Presentation layer B) Function (application) layer C) Database access layer D) Network layer

**Q5.** Compared to a centralized processing system, a distributed processing system generally has:
A) lower reliability, easier security B) higher reliability, harder security C) lower reliability, harder security D) higher reliability, easier security

---

**Day 11 Answers:** Q1: B | Q2: C | Q3: B | Q4: B | Q5: B
**Practice Answers:** 1) hard real-time failure causes fatal/serious consequences; soft real-time failure is undesirable but tolerable 2) Atomicity (all-or-nothing), Consistency (no inconsistencies result), Isolation (transactions don't interfere with each other), Durability (results persist even after a fault) 3) if one machine fails, the others can continue operating; there's no single point of failure 4) updating business logic requires redeploying it to every client; three-tier centralizes that logic on the server 5) any three of: print server, file server, database server, communication/gateway server, proxy server 6) client/server is vertical distributed; P2P is horizontal distributed.

---

---

# DAY 12 (Fri Aug 15) — High-Reliability System Configurations

### Why this matters for you

This day formalizes redundancy patterns you've likely designed around intuitively (active-passive failover, replica sets) — now with the exam's precise names and the RAID levels you should have memorized cold as a backend engineer, but from the exam's specific angle.

### 1. Series vs. parallel systems (the conceptual foundation for tomorrow's math)

- **Series system:** every device must work for the system to work. One failure = total failure. (Think: a single unreplicated service in a request chain.)
- **Parallel system:** the system can survive if at least one component (or some threshold) is still working. This is redundancy. (Think: a load-balanced pool of servers.)

### 2. Duplex vs. dual systems — don't mix these up, the exam tests the distinction directly

|                  | Duplex system                                                               | Dual system                                                                                        |
| ---------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Setup            | One **primary** (active) system + one **secondary** (standby/backup) system | Two (or more) systems running **the exact same processing simultaneously**, cross-checking results |
| Normal operation | Only the primary handles live traffic                                       | Both systems process everything in parallel                                                        |
| Cost             | Lower                                                                       | Higher (double the compute for every transaction)                                                  |
| Use case         | General high-availability needs                                             | Life-critical systems (e.g., medical systems) where results must be verified by cross-checking     |

**Duplex has two standby styles:**

- **Cold standby:** the backup is idle (or doing unrelated local work) until a failure happens, then it's activated and takes over — slower recovery.
- **Hot standby:** the backup runs the _same_ system continuously, monitoring the primary, and takes over immediately on failure — faster recovery, closer to what you'd call "active-passive failover with health checks" today.

### 3. Fault tolerant systems & multiprocessors

- **Fault tolerant system:** designed to keep functioning even if part of it fails.
  - Hardware approach: duplicate the actual components (servers, disks).
  - Software approach: **N-version programming** — run several independently-written programs with the same spec simultaneously, then take the majority-agreeing result.
  - **Fail-soft / degraded operation:** keep core functions running while sacrificing non-essential ones under failure (e.g., a hospital running only life-support equipment during a power cut).
- **Multiprocessor system:** multiple processors share the workload.
  - **Tightly coupled:** processors share one main memory, run under one OS — fine-grained load distribution, but needs synchronization mechanisms between tasks.
  - **Loosely coupled:** each processor has its own memory and OS, communicating over high-speed I/O — this is essentially a **cluster**, a term you already use.
  - **Amdahl's Law** (memorize the idea, not necessarily the formula): adding more processors doesn't give proportional speedup, because the _sequential_ (non-parallelizable) portion of a program caps the maximum possible speedup. This is exactly why "just add more workers" has diminishing returns on real workloads — you've felt this in practice.

### 4. RAID — you know this from work, here's the exam's exact framing

| Level  | What it does                                                                             | Redundancy?                                | Notes                                                 |
| ------ | ---------------------------------------------------------------------------------------- | ------------------------------------------ | ----------------------------------------------------- |
| RAID 0 | Striping (splits data across disks for speed)                                            | **None**                                   | Fastest, but any single disk failure loses everything |
| RAID 1 | Mirroring (identical copy on 2 disks)                                                    | Yes                                        | 50% usable capacity, simple and robust                |
| RAID 2 | Striping + Hamming code for error correction                                             | Yes                                        | Rare in practice                                      |
| RAID 3 | Striping + dedicated parity disk, byte-level                                             | Yes                                        | Rare in practice                                      |
| RAID 4 | Like RAID 3 but block-level striping                                                     | Yes                                        | Parity disk becomes a bottleneck                      |
| RAID 5 | Striping with **parity distributed across all disks** (no single parity disk bottleneck) | Yes                                        | The most common general-purpose choice                |
| RAID 6 | Like RAID 5 but with **two** independent parity blocks                                   | Yes, survives 2 simultaneous disk failures | Higher safety margin than RAID 5                      |

**NAS vs. SAN** (both often built on RAID):

- **NAS (Network Attached Storage):** a file-server-like device on the network, speaks file-level protocols (CIFS for Windows, NFS for Unix) — easy to share files across different OSes.
- **SAN (Storage Area Network):** a dedicated storage network, lower network load than NAS, but harder to share data between systems that use different file systems, because it operates at the block level rather than the file level.

### Key Points

- Series = all must work; Parallel = redundancy, some can fail.
- Duplex = one active + one standby (cold or hot). Dual = both run simultaneously, results cross-checked (used for life-critical systems).
- Fault tolerant: hardware duplication, or N-version programming (software) with majority voting.
- Tightly coupled multiprocessor = shared memory/OS. Loosely coupled = separate memory/OS = essentially a cluster.
- Amdahl's Law: sequential portions of a program cap the speedup from adding processors.
- RAID 0 = striping, no redundancy. RAID 1 = mirroring. RAID 5 = distributed parity (most common). RAID 6 = double parity (survives 2 failures).
- NAS = file-level, multi-OS friendly. SAN = block-level, dedicated network, less flexible across OSes.

### Practice Questions

1. What's the core difference between a duplex system and a dual system?
2. Distinguish cold standby from hot standby.
3. In N-version programming, how is the final result determined?
4. What's the difference between tightly coupled and loosely coupled multiprocessing? Which corresponds to a "cluster"?
5. Why does RAID 5 avoid the parity-disk bottleneck that RAID 4 has?
6. Which RAID level can survive two simultaneous disk failures?

### Official-Exam-Style Practice (matching real ITFE Subject A format/difficulty)

**EP1.** Which of the following is the computer system where one computer is in the standby state while the other computer operates normally?
a) Dual system b) Duplex system c) Multiprocessor system d) Load sharing system

**EP2.** Which of the following is a component of a fault tolerant system?
a) RAID 0 b) Duplexing of a hard disk c) Scheduled backup d) Data encryption

_(Answers: EP1 → b — this is the textbook definition of a duplex system, worded exactly as it commonly appears on the real exam; a dual system runs both computers simultaneously on the same processing, not one active/one standby. EP2 → b — mirroring/duplexing a hard disk provides hardware redundancy so the system keeps functioning through a single disk failure; RAID 0 (striping only) has *no* redundancy, and scheduled backup/encryption serve different purposes (recovery and confidentiality, not fault tolerance during live operation).)_

### Day 12 Quiz

**Q1.** A system with a primary active unit and a passive backup unit that takes over on failure is called:
A) Dual system B) Duplex system C) RAID 1 D) Tightly coupled system

**Q2.** In a dual system, the two systems:
A) run different programs on different data B) run the same processing simultaneously and cross-check results C) alternate active/standby roles D) only exist for cost savings

**Q3.** Which RAID level uses mirroring, giving 50% storage efficiency?
A) RAID 0 B) RAID 1 C) RAID 5 D) RAID 6

**Q4.** Amdahl's Law states that the speedup from adding more processors is fundamentally limited by:
A) the amount of available memory B) the sequential (non-parallelizable) portion of the program C) the number of disks in the RAID array D) the clock frequency of each core

**Q5.** A loosely coupled multiprocessor system, where each processor has its own memory and OS, is best described as:
A) a single powerful CPU B) a cluster C) a RAID array D) a duplex system

---

**Day 12 Answers:** Q1: B | Q2: B | Q3: B | Q4: B | Q5: B
**Practice Answers:** 1) duplex has one active + one idle/standby system; dual runs two systems simultaneously on the same processing and cross-checks results 2) cold standby: backup is idle/doing other work until activated after a failure (slower); hot standby: backup runs continuously alongside primary and takes over immediately (faster) 3) several independently-written programs with the same spec run in parallel, and the majority-agreeing result is adopted 4) tightly coupled shares memory and OS across processors (needs task synchronization); loosely coupled has separate memory/OS per processor, communicating via high-speed I/O — this is essentially a cluster 5) RAID 5 spreads parity data across all disks instead of dedicating one disk to it, so no single disk becomes an I/O bottleneck 6) RAID 6.

---

---

# DAY 13 (Sat Aug 16) — Performance & Reliability Metrics: MTBF, MTTR, Availability, MIPS

### Why this matters for you

This is the single highest-yield calculation-heavy day in Chapter 2. These formulas appear across multiple exam sessions almost every cycle. Take your time, and re-derive every worked example yourself with a calculator before checking answers.

### 1. Time-based performance indicators

- **Turnaround time:** from submitting a job to receiving the _complete_ result. Used for **batch** systems.
  `Turnaround time = CPU processing time + I/O time + overhead (wait time)`
- **Response time:** from submitting a request to when result output _begins_. Used for **online/interactive** systems.
  `Response time = CPU processing time + overhead (transmission + terminal processing time)`
- **Throughput:** amount of work completed per unit time. Common unit: **TPS (Transactions Per Second)**.

  Worked example: a web server takes 4 ms of CPU time per transaction, and you cap CPU utilization at 75% to leave headroom.
  `Throughput = 1 second ÷ 4 ms/transaction × 0.75 = 187.5 TPS`

  **Important system-level nuance:** if several servers work together, the overall system throughput is capped by the _slowest_ one in the chain — the same "bottleneck" logic you already apply when profiling a pipeline.

- **Capacity planning** (4 steps, know the order): 1) collect workload data (especially peak load) → 2) determine performance requirements → 3) sizing (estimate needed hardware/software) → 4) evaluate and tune, repeating even after go-live.

**1a. Average turnaround time under FCFS scheduling (newly added — a real, guaranteed-style exam calculation).** When multiple jobs/processes arrive at (essentially) the same time and are run strictly one after another under **FCFS (First-Come-First-Served)**, each job's _individual_ turnaround time = the sum of every job's running time _up to and including its own_, since it has to wait for everyone ahead of it to finish first. The **average** turnaround time is the mean of all those individual turnaround times.

_Worked example:_ 5 processes A, B, C, D, E arrive together in that order, with running times 10, 6, 2, 8, 4 ms respectively. Find the average turnaround time under FCFS.

Since FCFS runs them strictly in arrival order (A, then B, then C, then D, then E), each process's turnaround time is the cumulative sum of running times up to that point:

| Process | Running time | Turnaround time (cumulative) |
| ------- | ------------ | ---------------------------- |
| A       | 10           | 10                           |
| B       | 6            | 10+6 = 16                    |
| C       | 2            | 16+2 = 18                    |
| D       | 8            | 18+8 = 26                    |
| E       | 4            | 26+4 = 30                    |

Average turnaround time = (10+16+18+26+30) ÷ 5 = 100 ÷ 5 = **20 ms**

**Why this matters conceptually (not just the arithmetic):** notice that C, which only needed 2 ms of work, still had to wait 18 ms total simply because it arrived third — this is exactly the head-of-line-blocking weakness of FCFS mentioned back on your OS scheduling material: a short job stuck behind long ones suffers a disproportionately bad turnaround time, purely due to arrival order rather than its own size. This is precisely why real schedulers (and real-world queueing systems you might design) often favor shorter jobs first, or at least avoid pure FCFS when job sizes vary widely.

### 2. CPU performance indicators

- **Clock frequency** alone doesn't tell the full story, because **CPI (Cycles Per Instruction)** varies by CPU — you can't compare two different CPU models by clock speed alone.
- **MIPS (Million Instructions Per Second):**
  `Average instruction execution time = CPI × clock cycle time`
  `MIPS = 1 ÷ average instruction execution time (in microseconds)`

  Worked example: a CPU runs at 800 MHz and averages 4 CPI per instruction.
  1. Clock cycle time = 1 ÷ 800,000,000 = 0.00000000125 s
  2. Average instruction time = 4 × 0.00000000125 = 0.000000005 s = 0.005 microseconds
  3. MIPS = 1 ÷ 0.005 = **200 MIPS**

  When instruction mixes vary in execution time (a weighted-average question):
  `Average instruction time = Σ(instruction time × occurrence rate)`, then `MIPS = 1 ÷ that average` (in microseconds).

- **FLOPS (Floating-point Operations Per Second):** used for scientific/supercomputer benchmarking (TFLOPS = tera scale).
- **Benchmarks:** TPC (for OLTP systems, cost-per-transaction focus), SPEC (SPECint for integer ops, SPECfp for floating point), and older named benchmarks (Dhrystone = fixed point, Whetstone = floating point, Linpack = linear equations).

### 3. Reliability metrics — MTBF, MTTR, Availability (near-guaranteed exam appearance)

- **MTBF (Mean Time Between Failures):** average _uptime_ between one recovery and the next failure. Represents **Reliability**.
- **MTTR (Mean Time To Repair):** average _downtime_ to fix a failure. Represents **Serviceability**.
- **Availability:** probability the system is working when you need it.

  **Availability = MTBF ÷ (MTBF + MTTR)**
  **Non-availability = 1 − Availability = MTTR ÷ (MTBF + MTTR)**

**Worked example** (different numbers from any textbook example): a server ran for 500 hours total, with 2 failures. Uptime segments: 180h, 220h. Downtime (repair) segments: 15h, 25h.

- MTBF = (180+220) ÷ 2 = 400 ÷ 2 = **200 hours**
- MTTR = (15+25) ÷ 2 = 40 ÷ 2 = **20 hours**
- Availability = 200 ÷ (200+20) = 200 ÷ 220 = **0.909 (≈90.9%)**

**RASIS** (memorize the acronym — it's a guaranteed vocabulary question): **R**eliability, **A**vailability, **S**erviceability, **I**ntegrity, **S**ecurity. (RAS = just the first three.)

### 4. System-level availability: series and parallel combinations

**Series system** (all devices must work): `Availability = A₁ × A₂ × ... × Aₙ`

Worked example: two devices in series, availability 0.95 and 0.90.
`Series availability = 0.95 × 0.90 = 0.855`

**Parallel system, 2 devices** (system works if _at least one_ works): `Availability = 1 − (1−A₁)(1−A₂)`

Worked example: two devices in parallel, availability 0.9 and 0.85.
`Parallel availability = 1 − (1−0.9)(1−0.85) = 1 − (0.1 × 0.15) = 1 − 0.015 = 0.985`

**Parallel with 3+ devices, "at least 2 of 3 must work"** — sum the probabilities of every combination that satisfies the requirement (all three working, or exactly two working). This is the one calculation worth practicing more than once until the combinatorics feel natural — walk through the Week 2 worked example in the textbook-style table method: enumerate every operating/failed combination, compute each probability as a product, then sum the qualifying rows.

**Series/parallel mixed systems:** solve piece by piece — compute the availability of each parallel sub-block first, then multiply those sub-block availabilities together as if they were a series chain.

**4a. Worked example: a real multi-group system (newly added — this exact pattern is a proven exam favorite).** A system has 1 server (availability _a_), 3 clients (each availability _b_), and 2 printers (each availability _c_), all connected via one LAN. The system is considered "up" if the server works AND at least 1 of the 3 clients works AND at least 1 of the 2 printers works. What is the overall system availability?

Break it into three independent blocks, then chain them as a series system (since _all three conditions_ — server up, at least one client up, at least one printer up — must simultaneously hold):

1. **Server block:** a single component, not redundant → availability = **a**
2. **Client block:** 3 identical devices in parallel, "at least 1 of 3 must work" → using the parallel-availability idea from rule 17, generalized to n identical devices: `availability = 1 − (1 − b)ⁿ` → for 3 clients: **1 − (1−b)³**
3. **Printer block:** 2 identical devices in parallel, "at least 1 of 2 must work" → **1 − (1−c)²**

Since the whole system needs _all three blocks_ to be up simultaneously (server AND at-least-one-client AND at-least-one-printer), multiply the three block availabilities together as a series chain:

**System availability = a × (1 − (1−b)³) × (1 − (1−c)²)**

_Plug in numbers to sanity-check the shape of the answer:_ if a=0.99, b=0.9, c=0.8:

- Client block: 1 − (0.1)³ = 1 − 0.001 = 0.999
- Printer block: 1 − (0.2)² = 1 − 0.04 = 0.96
- System = 0.99 × 0.999 × 0.96 ≈ **0.9505**

**The general pattern to internalize:** "at least 1 of n identical parallel devices must work" always generalizes to `1 − (1 − individual availability)ⁿ` — this is just the 2-device parallel formula (rule 17) extended to n devices, since "at least one works" is the complement of "every single one fails simultaneously," and "every one fails" has probability `(1−A)ⁿ` when all n devices are independent and identical. Once you can build that one sub-formula, any system diagram — no matter how many device _groups_ it has — reduces to: identify each independent redundant group, apply `1−(1−p)ⁿ` to each, then multiply every group's result together in series.

**Failure rate:** `Failure rate = 1 ÷ MTBF`. For a system built of many components, `System failure rate = Σ (failure rate of each component)`, and you then invert the sum to get system MTBF.

**Bathtub curve** (shape of failure rate over a device's life, memorize the three phases in order): **early failure period** (manufacturing defects surface early) → **random failure period** (low, roughly constant failure rate — normal operating life) → **wear-out failure period** (rising failure rate as components age past their useful life).

### 5. Cost efficiency: TCO

**TCO (Total Cost of Ownership)** = **initial cost** (hardware purchase, software purchase/development — one-time) + **operational/running cost** (hardware lease, operator salaries, facility maintenance — recurring). Also know: **direct cost** relates clearly to the specific target system; **indirect cost** is shared/harder to attribute (e.g., a shared IT operator's salary split across many systems) — this classification is relative, not absolute.

### Key Points

- Turnaround (batch) vs. response time (online); throughput in TPS, capped by the slowest component.
- MIPS = 1 ÷ average instruction execution time (µs); average time = CPI × clock cycle time, or Σ(time × rate) for a mix.
- MTBF = uptime ÷ failures (Reliability). MTTR = downtime ÷ failures (Serviceability).
- Availability = MTBF/(MTBF+MTTR). RASIS = Reliability, Availability, Serviceability, Integrity, Security.
- Series availability = product of all. Parallel (2 devices) = 1 − product of non-availabilities.
- Failure rate = 1/MTBF; system failure rate = sum of component failure rates.
- Bathtub curve: early failure → random failure → wear-out failure.
- TCO = initial cost + operational cost.
- FCFS average turnaround time: each job's turnaround = cumulative running time of every job up to and including itself, in arrival order; average those.
- "At least 1 of n identical parallel devices" generalizes to `1 − (1 − p)ⁿ`; chain multiple independent device groups (server, client pool, printer pool, etc.) together as a series system by multiplying each group's availability.

### Practice Questions

1. Write the formula for availability in terms of MTBF and MTTR.
2. A device ran with 3 failures over a period where total uptime was 540 hours and total repair time was 60 hours. Find MTBF, MTTR, and availability.
3. Two devices are in series with availabilities 0.98 and 0.92. What's the overall availability?
4. Two devices are in parallel with availabilities 0.8 and 0.7. What's the overall availability?
5. What does RASIS stand for?
6. Name the three phases of the bathtub curve, in order.
7. A CPU runs at 1 GHz with an average CPI of 2. What is its MIPS rating?
8. Four processes arrive together in order P1, P2, P3, P4 with running times 5, 3, 9, 2 ms. Find the average turnaround time under FCFS.
9. A system needs its 1 server (availability 0.95) up, AND at least 1 of 4 identical routers (each availability 0.8) up, to be considered available. Write the expression for system availability and compute it.

### Official-Exam-Style Practice (matching real ITFE Subject A format/difficulty)

**EP1.** In a processor with a clock cycle time of 2 nanoseconds, the number of clocks needed for each instruction type and its occurrence rate are shown below. What is the approximate performance of this processor in MIPS?

| Type of instruction               | Clocks needed | Occurrence rate |
| --------------------------------- | ------------- | --------------- |
| Register-to-register operation    | 3             | 40%             |
| Register-to/from-memory operation | 5             | 40%             |
| Unconditional branch              | 9             | 20%             |

a) 50 b) 100 c) 200 d) 250

**EP2.** One server, three clients, and two printers are connected via a LAN. The system prints data located on the server in response to client instructions. The system is considered available if the server is up, at least 1 of the 3 clients is up, and at least 1 of the 2 printers is up. Given availability values: server = a, client = b, printer = c, which of the following is the expression for overall system availability?
a) a·b³·c² b) a·(1−b³)·(1−c²) c) a·(1−(1−b)³)·(1−(1−c)²) d) a·(1−(1−b))³·(1−(1−c))²

_(Answers: EP1 → b) 100. Average clocks/instruction = (3×0.4)+(5×0.4)+(9×0.2) = 1.2+2.0+1.8 = 5.0 clocks exactly. Average instruction time = 5.0 × 2ns = 10ns = 0.01µs. MIPS = 1÷0.01 = 100. EP2 → c, using the same "1 − (1−p)ⁿ per group, then multiply groups in series" pattern from section 4a.)_

### Day 13 Quiz

**Q1.** MTBF represents which RASIS component?
A) Availability B) Reliability C) Serviceability D) Integrity

**Q2.** Availability is calculated as:
A) MTTR ÷ (MTBF + MTTR) B) MTBF ÷ (MTBF + MTTR) C) MTBF × MTTR D) MTBF − MTTR

**Q3.** Two devices in series have availability 0.9 and 0.85. Overall availability is:
A) 0.9 B) 0.765 C) 0.85 D) 1.75

**Q4.** Two devices in parallel have availability 0.6 and 0.5. Overall availability is:
A) 0.30 B) 0.70 C) 0.80 D) 1.10

**Q5.** The bathtub curve's middle phase, with a low and roughly constant failure rate, is called:
A) Early failure period B) Random failure period C) Wear-out failure period D) Peak failure period

**Q6.** A CPU has a clock frequency of 500 MHz and average CPI of 5. Its MIPS rating is:
A) 50 MIPS B) 100 MIPS C) 200 MIPS D) 500 MIPS

**Q7.** TCO includes:
A) only hardware purchase cost B) only operational cost C) both initial cost and operational cost D) only software development cost

**Q8 (exam-style, FCFS turnaround).** Processes A, B, C arrive together in that order with running times 6, 3, 9 ms. The average turnaround time under FCFS is:
A) 6 ms B) 9 ms C) 11 ms D) 18 ms

**Q9 (exam-style, multi-group availability).** A system requires its server (availability a) AND at least 1 of 3 identical clients (each availability b) AND at least 1 of 2 identical printers (each availability c) to be up. The correct expression for system availability is:
A) a·b³·c² B) a·(1−b³)·(1−c²) C) a·(1−(1−b))³·(1−(1−c))² D) a·(1−(1−b)³)·(1−(1−c)²)

---

**Day 13 Answers:** Q1: B | Q2: B | Q3: B (0.9×0.85=0.765) | Q4: C (1−(0.4×0.5)=1−0.2=0.8) | Q5: B | Q6: B (clock cycle=1/500,000,000=2ns; avg instr time=5×2=10ns=0.01µs; MIPS=1/0.01=100) | Q7: C | Q8: C (turnarounds: A=6, B=6+3=9, C=9+9=18; average=(6+9+18)/3=33/3=11 ms) | Q9: D
**Practice Answers:** 1) Availability = MTBF ÷ (MTBF + MTTR) 2) MTBF=540/3=180h, MTTR=60/3=20h, Availability=180/(180+20)=0.9 3) 0.98×0.92=0.9016 4) 1−(0.2×0.3)=1−0.06=0.94 5) Reliability, Availability, Serviceability, Integrity, Security 6) early failure period → random failure period → wear-out failure period 7) clock cycle time=1/1,000,000,000=1ns; avg instruction time=2×1=2ns=0.002µs; MIPS=1/0.002=500 MIPS 8) turnarounds: P1=5, P2=5+3=8, P3=8+9=17, P4=17+2=19; average=(5+8+17+19)/4=49/4=12.25 ms 9) availability = a × (1−(1−b)⁴); with a=0.95, b=0.8: 1−(0.2)⁴=1−0.0016=0.9984; system=0.95×0.9984≈**0.9485**.

---

---

# DAY 14 (Sun Aug 17) — Human Interface, Multimedia & Week 2 Review

### Why this matters for you

Lighter, mostly-recall content today — good pacing after yesterday's calculation-heavy session. Then a full Week 2 review to lock in the material before Week 3.

### 1. Human interface essentials

- **Human interface:** the contact point between a person and a system/service; **user interface (UI)** specifically means the human-computer contact point, emphasized heavily in interactive systems.
- **Information architecture:** organizing information so it's easy to find and understand — starts with structuring/organizing the information to be presented (the textbook frames this as the first step of good UI design, something you already practice when designing API responses or admin dashboards, just without the formal name attached).
- Common UI elements: windows, icons, menus, pointers (the classic **WIMP** paradigm, though the textbook may not name it explicitly) — the graphical building blocks of any GUI you've ever built or used.

_This subsection is intentionally light — the textbook treats it more conceptually than mathematically, and the exam tends to test it as straightforward recall/matching rather than calculation. Don't over-invest study time here relative to Day 13's formulas._

### 2. Multimedia — file formats and compression (matching-style questions, memorize the table)

**Still image formats:**

| Format | Compression                     | Colors     | Notes                                                        |
| ------ | ------------------------------- | ---------- | ------------------------------------------------------------ |
| JPEG   | Usually lossy (can be lossless) | Full color | Very high compression ratio; most common photo format        |
| GIF    | Lossless                        | 256 colors | Small file size, historically common on the web              |
| PNG    | Lossless                        | Full color | Larger than JPEG, no quality loss; GIF's spiritual successor |

**Moving image formats:**

| Format    | Notes                                                                                                                           |
| --------- | ------------------------------------------------------------------------------------------------------------------------------- |
| MPEG      | ISO standard; MPEG-1 (video-level quality), MPEG-2 (TV/high-vision), MPEG-4 (mobile/cellular use), MPEG-7 (metadata for search) |
| QuickTime | Apple's format, uses Motion JPEG (continuous JPEG playback)                                                                     |
| AVI       | Windows-native, built on the RIFF container format                                                                              |

**Frame rate:** frames displayed per second, measured in **fps**.

**Compression concepts:**

- **Lossless:** original data fully recoverable (e.g., ZIP, GZIP, PNG, GIF).
- **Lossy:** original data not fully recoverable, but achieves much higher compression — acceptable for images/audio/video because humans rarely notice the missing detail (e.g., JPEG, MPEG).
- File archivers: **ZIP** (de facto global standard, can archive multiple files), **GZIP** (common on Unix, name aside it is _not_ ZIP-compatible and has no archiving function by itself), **LZH** (Japan-specific, via LHA software).

**CG (Computer Graphics) techniques — recognize the names:**

- **Anti-aliasing:** smooths jagged edges on diagonal/curved lines.
- **Texture mapping:** applies an image/pattern onto a 3D surface.
- **Ray-tracing:** traces light rays backward from the eye to render realistic images.
- **Shading:** adds shadow/depth cues for a sense of solidity.
- **Morphing:** smoothly transforms one image into another.

### Official-Exam-Style Practice (matching real ITFE Subject A format/difficulty)

**EP1.** Which of the following is a method used to improve the appearance of a jagged edge of a slanted line so that it appears smooth on a screen, such as that of an LCD?
a) Anti-aliasing b) Bump mapping c) Shading d) Texture mapping

**EP2.** Which of the following is an appropriate description of the GUI component commonly known as a "radio button"?
a) A component that allows the user to select exactly one option from a mutually exclusive set of choices.
b) A component that allows the user to select any number of options independently, including zero or all of them.
c) A component that opens a list of choices only when clicked, collapsing again after selection.
d) A component used exclusively to trigger an immediate action, such as submitting a form.

_(Answers: EP1 → a, matching the real exam's exact phrasing on this topic. EP2 → a — a radio button group enforces mutual exclusivity (only one selected at a time); (b) describes a checkbox, (c) describes a dropdown/combo box, (d) describes a push button. Mixing these GUI-component definitions up is a common exam trap worth double-checking.)_

### 3. Week 2 Full Review Quiz (12 questions)

**Q1.** A disk drive has 200 cylinders, 12 tracks/cylinder, 25 sectors/track, 512 bytes/sector. Total capacity is:
A) 30,720,000 bytes B) 3,072,000 bytes C) 307,200 bytes D) 6,144,000 bytes

**Q2.** Access time is the sum of:
A) clock time + CPI B) seek time + rotational latency + data transfer time C) MTBF + MTTR D) throughput + latency

**Q3.** In the DMA method, who controls the actual data transfer between device and memory?
A) The CPU directly B) A dedicated DMA controller C) The channel program D) The operating system's scheduler exclusively

**Q4.** Which ACID property guarantees a transaction is all-or-nothing?
A) Consistency B) Atomicity C) Isolation D) Durability

**Q5.** In a three-tier client/server system, the layer that processes SQL statements and business logic is:
A) presentation layer B) function/application layer C) database access layer D) network layer

**Q6.** A duplex system with a backup that's continuously running and monitoring the primary, ready for instant takeover, uses:
A) cold standby B) hot standby C) N-version programming D) RAID 1

**Q7.** RAID 5 improves on RAID 4 by:
A) removing redundancy entirely B) distributing parity across all disks instead of one dedicated disk C) using tape backup D) requiring fewer disks

**Q8.** MTBF ÷ (MTBF + MTTR) calculates:
A) Failure rate B) Throughput C) Availability D) TCO

**Q9.** Two devices in parallel have availabilities 0.9 and 0.6. Overall availability is:
A) 0.54 B) 0.96 C) 1.50 D) 0.6

**Q10.** The bathtub curve phase where failure rate rises due to aging components is:
A) Early failure B) Random failure C) Wear-out failure D) Peak failure

**Q11.** JPEG typically uses which type of compression?
A) Lossless only B) Lossy (though lossless is possible) C) No compression D) Archival compression only

**Q12.** TCO stands for:
A) Total Capacity Output B) Total Cost of Ownership C) Time-based Cost Optimization D) Transfer Cost Overhead

---

**Week 2 Review Answers:** Q1: A (512×25=12,800/track; ×12=153,600/cylinder; ×200=30,720,000 bytes) | Q2: B | Q3: B | Q4: B | Q5: B | Q6: B | Q7: B | Q8: C | Q9: B (1−(0.1×0.4)=1−0.04=0.96) | Q10: C | Q11: B | Q12: B

### Self-check before moving to Week 3

Rate yourself 1–5 on:

- [ ] Disk capacity & access time calculations
- [ ] I/O control methods (program control vs. DMA vs. channel) and interfaces
- [ ] ACID properties (can you recite all four unprompted?)
- [ ] Client/server tiers and distributed processing types
- [ ] Duplex vs. dual systems, RAID levels
- [ ] MTBF/MTTR/Availability calculations (series and parallel)
- [ ] MIPS calculation

Anything at 3 or below — flag it now. We'll build these into the Week 10 targeted-review days, and I'd also recommend redoing just that day's quiz once more midweek during Week 3 as a spaced-repetition check.

**Next up: Week 3 — Software (OS, programming languages, files) and Database (SQL, DB design) — this should be one of your faster weeks given your backend background.**
