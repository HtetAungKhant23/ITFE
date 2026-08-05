# ITFE (ITPEC FE) — 12-Week Study Plan

**Exam date: Sunday, October 25, 2026 | Today: Tuesday, August 4, 2026 | ~12 weeks**

---

## 0. Exam Snapshot (know your enemy)

|                | Subject A (AM)                                                           | Subject B (PM)                                                    |
| -------------- | ------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Length         | 90 min                                                                   | 100 min                                                           |
| Questions      | 60, all compulsory MCQ                                                   | 20 scenario questions                                             |
| Content        | Technology, Management, Strategy — basically everything in Vol.1 + Vol.2 | Algorithms, program tracing, debugging, pseudocode/flowcharts     |
| Weighting      | Equal per question                                                       | Weighted by difficulty                                            |
| Language style | Concept + short calculation questions                                    | No language-specific code (pseudocode only), no network questions |

**Key implication for you:** Subject B is your natural strength as a backend engineer — algorithmic thinking and tracing logic is your day job. The main new skill is learning **IPA/ITPEC's specific pseudocode notation** (not any real language). Subject A is where the real work is, especially **Volume 2** (accounting, legal, management, strategy) — this is genuinely new material for a backend engineer, not something day-job experience covers.

**Your overall time budget assumption:** ~1–1.5 hrs on weekdays, ~2.5–3.5 hrs on weekend days (~11–13 hrs/week). Tell me if your real bandwidth is different and I'll rebalance the plan — it's easy to compress or stretch.

---

## 1. How We'll Work Day-to-Day

1. Each day, tell me something like "Day 12" or "today's topic" or just "let's continue" — I'll teach that day's topic conversationally, connect it to backend engineering where possible, then quiz you on it.
2. After each **chapter**, I'll give you a chapter-level quiz + a short "weak spot" diagnosis.
3. After each **major section (Vol.1, Vol.2, Data Structures/Algorithms)**, you'll get a timed mini mock exam.
4. Weeks 10–12 are dedicated to full timed mock exams (Subject A and B) under real time constraints, plus spaced revision of everything already covered.
5. Tell me your quiz/mock scores honestly — I'll adjust the schedule and add extra revision days for whatever's weak. This plan is a **living document**, not fixed in stone.

---

## 2. The 12-Week Roadmap (Overview)

| Week | Dates        | Focus                                              | Source                           |
| ---- | ------------ | -------------------------------------------------- | -------------------------------- |
| 1    | Aug 4–10     | Hardware I: history, 5 units, data representation  | Vol.1 Ch.1 (p.13–70)             |
| 2    | Aug 11–17    | Hardware II + Information Processing Systems       | Vol.1 Ch.1 end + Ch.2 (p.78–183) |
| 3    | Aug 18–24    | Software + Database                                | Vol.1 Ch.3–4 (p.184–286)         |
| 4    | Aug 25–31    | Network + Security                                 | Vol.1 Ch.5–6 (p.289–402)         |
| 5    | Sep 1–7      | Data Structures & Algorithms + pseudocode notation | Vol.1 Ch.7 (p.403–449)           |
| 6    | Sep 8–14     | Corporate & Legal Affairs (accounting, OR, QC)     | Vol.2 Ch.1 (p.11–93)             |
| 7    | Sep 15–21    | Finish Legal + Business Strategy                   | Vol.2 Ch.1 end + Ch.2 (p.93–197) |
| 8    | Sep 22–28    | Info Systems Strategy + Development Technology     | Vol.2 Ch.3–4 (p.198–309)         |
| 9    | Sep 29–Oct 5 | Project Mgmt + Service Mgmt + Audit                | Vol.2 Ch.5–7 (p.310–391)         |
| 10   | Oct 6–12     | Review weak areas + Mock Exam Set #1               | Full syllabus                    |
| 11   | Oct 13–19    | Mock Exam Set #2 + deep drilling                   | Full syllabus                    |
| 12   | Oct 20–25    | Taper, light review, exam day                      | Full syllabus                    |

---

## 3. Daily Breakdown

### Week 1 — Hardware I (Vol.1 Ch.1, p.13–78)

| Day        | Topic                                                                                                | Pages |
| ---------- | ---------------------------------------------------------------------------------------------------- | ----- |
| Mon Aug 4  | Orientation, diagnostic quiz, exam strategy overview; 1-1 History of Computers, 1-2 Five Major Units | 13–20 |
| Tue Aug 5  | 2-1 Data Representation, 2-2 Radix & Radix Conversion                                                | 20–28 |
| Wed Aug 6  | 2-3 Representation Form of Data (numbers, part 1)                                                    | 28–37 |
| Thu Aug 7  | 2-3 Representation Form of Data (floating point, character codes, part 2)                            | 37–46 |
| Fri Aug 8  | 3-1 CPU Configuration, 3-2 Main Memory Configuration                                                 | 46–52 |
| Sat Aug 9  | 3-3 Instruction & Addressing, 3-4 ALU Circuit Configuration + practice problems                      | 52–70 |
| Sun Aug 10 | 3-5 High Speed Technologies + **Week 1 quiz** (radix conversion, floating point, CPU)                | 70–77 |

### Week 2 — Hardware II + Information Processing Systems

| Day        | Topic                                                                                           | Pages   |
| ---------- | ----------------------------------------------------------------------------------------------- | ------- |
| Mon Aug 11 | 4 Auxiliary Storage (disk, optical, semiconductor)                                              | 78–91   |
| Tue Aug 12 | 5 Input/Output Unit + Ch.1 Exercises                                                            | 92–119  |
| Wed Aug 13 | **Chapter 1 full review quiz** + log weak spots                                                 | —       |
| Thu Aug 14 | Ch.2: 1 Processing Types (batch/real-time/distributed)                                          | 121–131 |
| Fri Aug 15 | Ch.2: 2 High-reliability Config (series/parallel/multiplex)                                     | 131–137 |
| Sat Aug 16 | Ch.2: 3 Evaluation of Processing Power, Reliability (MTBF/MTTR — memorize these formulas), Cost | 137–153 |
| Sun Aug 17 | Ch.2: 4 Human Interface, 5 Multimedia + Exercises + **Week 2 quiz**                             | 153–183 |

### Week 3 — Software + Database

| Day        | Topic                                                                                              | Pages   |
| ---------- | -------------------------------------------------------------------------------------------------- | ------- |
| Mon Aug 18 | Ch.3: 1 Classification of Software                                                                 | 184–196 |
| Tue Aug 19 | Ch.3: 2 OS Functions & Management (scheduling, memory mgmt)                                        | 196–211 |
| Wed Aug 20 | Ch.3: 3 Programming Languages & Language Processors                                                | 211–227 |
| Thu Aug 21 | Ch.3: 4 Files + Exercises                                                                          | 227–246 |
| Fri Aug 22 | Ch.4: 1 Outline of Database (DB design, DBMS) — should feel familiar                               | 247–262 |
| Sat Aug 23 | Ch.4: 2 SQL, 3 Various Databases + Exercises — fast review, then focus on IPA-specific terminology | 262–286 |
| Sun Aug 24 | **Week 3 quiz** (Software + DB) + spaced review: Ch.1–2 flashcards                                 | —       |

### Week 4 — Network + Security

| Day        | Topic                                                                                          | Pages   |
| ---------- | ---------------------------------------------------------------------------------------------- | ------- |
| Mon Aug 25 | Ch.5: 1 Network Mechanism                                                                      | 289–314 |
| Tue Aug 26 | Ch.5: 2 Network Architecture (OSI, TCP/IP), 3 LAN                                              | 314–328 |
| Wed Aug 27 | Ch.5: 4 The Internet                                                                           | 328–341 |
| Thu Aug 28 | Ch.5: 5 Network Management + Exercises                                                         | 341–351 |
| Fri Aug 29 | Ch.6: 1 Overview of Information Security                                                       | 352–383 |
| Sat Aug 30 | Ch.6: 2 Information Security Measures + Exercises                                              | 383–402 |
| Sun Aug 31 | **Week 4 quiz** (Network + Security — should be a strong week for you) + spaced review: Ch.3–4 | —       |

### Week 5 — Data Structures & Algorithms + Pseudocode

| Day       | Topic                                                                                                                         | Pages   |
| --------- | ----------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mon Sep 1 | Ch.7: 1-1 Array, 1-2 List                                                                                                     | 403–407 |
| Tue Sep 2 | Ch.7: 1-3 Stack & Queue, 1-4 Tree Structure                                                                                   | 407–416 |
| Wed Sep 3 | Ch.7: 2-1 Flowchart, 2-2 Data Search                                                                                          | 416–426 |
| Thu Sep 4 | Ch.7: 2-3 Data Sorting                                                                                                        | 426–438 |
| Fri Sep 5 | Ch.7: 2-4 Other Algorithms, 2-5 Algorithm Design + Exercises                                                                  | 438–449 |
| Sat Sep 6 | **Learn ITPEC's pseudocode notation** (I'll pull sample Subject B questions and walk you through the syntax) + trace practice | —       |
| Sun Sep 7 | **Vol.1 mini mock exam** (30 mixed questions, timed) — this closes out all of Volume 1                                        | —       |

### Week 6 — Corporate & Legal Affairs (heaviest week — new territory)

| Day        | Topic                                                                                                  | Pages |
| ---------- | ------------------------------------------------------------------------------------------------------ | ----- |
| Mon Sep 8  | Ch.1: 1 Corporate Activities                                                                           | 11–22 |
| Tue Sep 9  | Ch.1: 2-1 Financial Accounting                                                                         | 22–30 |
| Wed Sep 10 | Ch.1: 2-2 Management Accounting                                                                        | 30–36 |
| Thu Sep 11 | Ch.1: 3-1 Applied Mathematics (sets, matrices, probability)                                            | 36–57 |
| Fri Sep 12 | Ch.1: 3-2 OR — Operations Research (PERT/CPM, linear programming)                                      | 57–72 |
| Sat Sep 13 | Ch.1: 3-3 IE Analysis, 3-4 QC Techniques (control charts, QC 7 tools)                                  | 72–83 |
| Sun Sep 14 | Ch.1: 3-5 Business Analysis + **Week 6 quiz** (accounting/OR/QC — expect this to be your hardest quiz) | 83–93 |

### Week 7 — Finish Legal Affairs + Business Strategy

| Day        | Topic                                                              | Pages   |
| ---------- | ------------------------------------------------------------------ | ------- |
| Mon Sep 15 | Ch.1: 4-1 Intellectual Property Rights, 4-2 Security-related Laws  | 93–106  |
| Tue Sep 16 | Ch.1: 4-3 Labor/Transaction Laws, 4-4 Other Laws                   | 106–120 |
| Wed Sep 17 | Ch.1: 4-5 Compliance, 4-6 Standardization + Exercises              | 120–143 |
| Thu Sep 18 | Ch.2: 1 Business Strategy Management                               | 144–163 |
| Fri Sep 19 | Ch.2: 1-4 Business Mgmt System, 2 Technological Strategy Mgmt      | 163–171 |
| Sat Sep 20 | Ch.2: 3 Business Industry + Exercises                              | 171–197 |
| Sun Sep 21 | **Week 7 quiz** + spaced review: Vol.2 Ch.1 accounting/OR formulas | —       |

### Week 8 — Information Systems Strategy + Development Technology

| Day        | Topic                                                                                                   | Pages   |
| ---------- | ------------------------------------------------------------------------------------------------------- | ------- |
| Mon Sep 22 | Ch.3: 1 Overview of Information Systems Strategy                                                        | 198–214 |
| Tue Sep 23 | Ch.3: 2 Information System Planning + Exercises                                                         | 214–227 |
| Wed Sep 24 | Ch.4: 1 System Development Process (SLCP)                                                               | 233–245 |
| Thu Sep 25 | Ch.4: 1 Software Implementation Process — SDLC phases, testing types (this is bread-and-butter for you) | 245–270 |
| Fri Sep 26 | Ch.4: 1 Maintenance/Disposal, 2 Software Development Method                                             | 270–282 |
| Sat Sep 27 | Ch.4: 2 Software Design Technique, 3 Development Environment                                            | 282–301 |
| Sun Sep 28 | Ch.4: 4 Web Application Development + Exercises + **Week 8 quiz**                                       | 301–309 |

### Week 9 — Project Management, Service Management, Audit

| Day        | Topic                                                                         | Pages   |
| ---------- | ----------------------------------------------------------------------------- | ------- |
| Mon Sep 29 | Ch.5: 1 Project Management Overview                                           | 310–316 |
| Tue Sep 30 | Ch.5: 2 Integration/Scope/Time Management                                     | 316–325 |
| Wed Oct 1  | Ch.5: 2 Cost/Quality/Risk Management + Exercises                              | 325–339 |
| Thu Oct 2  | Ch.6: 1 Service Management Overview + ITIL                                    | 344–350 |
| Fri Oct 3  | Ch.6: 2 Service Management Method + Exercises                                 | 350–372 |
| Sat Oct 4  | Ch.7: System Audit & Internal Control + Exercises                             | 378–391 |
| Sun Oct 5  | **Vol.2 mini mock exam** (30 mixed questions, timed) — closes out all content | —       |

### Week 10 — Review + Mock Exam Set #1

| Day        | Focus                                                             |
| ---------- | ----------------------------------------------------------------- |
| Mon Oct 6  | Targeted review of your weakest chapter (based on quiz history)   |
| Tue Oct 7  | Targeted review, second weakest chapter                           |
| Wed Oct 8  | Targeted review, third weakest chapter                            |
| Thu Oct 9  | **Full Subject A mock exam** (60 Q, 90 min, timed)                |
| Fri Oct 10 | Review mock mistakes in detail; re-study those exact sub-topics   |
| Sat Oct 11 | **Full Subject B mock exam** (pseudocode tracing, 100 min, timed) |
| Sun Oct 12 | Review Subject B mistakes; light rest                             |

### Week 11 — Mock Exam Set #2 + Deep Drilling

| Day        | Focus                                   |
| ---------- | --------------------------------------- |
| Mon Oct 13 | Drill weak topics surfaced by mock #1   |
| Tue Oct 14 | Drill weak topics, continued            |
| Wed Oct 15 | **Full Subject A mock exam #2** (timed) |
| Thu Oct 16 | Review mistakes + drill                 |
| Fri Oct 17 | **Full Subject B mock exam #2** (timed) |
| Sat Oct 18 | Review mistakes + drill                 |
| Sun Oct 19 | Light review, rest                      |

### Week 12 — Taper & Exam Day

| Day                                                                                                   | Focus                                                                                                            |
| ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Mon Oct 20                                                                                            | Review your personal formula/definition cheat sheet (built over the past 11 weeks)                               |
| Tue Oct 21                                                                                            | Mixed practice quiz (30 Q) — confidence-building only, no new material                                           |
| Wed Oct 22                                                                                            | Review your 2 weakest chapters only                                                                              |
| Thu Oct 23                                                                                            | **Half mock** (Subject A, 30 Q) to stay sharp; confirm exam logistics (ID, venue, start time, materials allowed) |
| Fri Oct 24                                                                                            | Very light review, early night, no cramming                                                                      |
| Sat Oct 25                                                                                            | **Rest.** Sleep well.                                                                                            |
| Sun Oct 25 → _(adjust if exam is actually a different weekday — confirm your exact appointment time)_ | **EXAM DAY**                                                                                                     |

_(Note: I've placed Oct 25 as both Sat and exam day above as a placeholder for weekday alignment — confirm your actual test date's day-of-week and I'll fix the calendar. The content sequencing above doesn't depend on it.)_

---

## 4. What to Actively Memorize (build a running cheat sheet as we go)

- Number systems: binary/octal/hex conversion, two's complement, floating-point representation
- Reliability math: MTBF, MTTR, availability formulas, series vs. parallel system reliability
- OSI 7 layers + TCP/IP 4 layers, common protocols per layer
- Normalization (1NF/2NF/3NF), primary/foreign keys, SQL syntax
- Sorting algorithm time complexities (bubble, selection, quick, merge)
- PERT/CPM critical path calculations, financial accounting basics (B/S, P/L), OR linear programming
- SDLC phases, testing types (unit/integration/system/acceptance), development methodologies (waterfall/agile)
- Project management knowledge areas (scope, time, cost, quality, risk)
- ITIL basics, service management processes
- Security: CIA triad, encryption types (symmetric/asymmetric), common attacks and countermeasures

---

## 5. Exam-Day Strategy Notes (we'll revisit and expand this in Week 12)

- **Subject A (60 Q, 90 min = 1.5 min/question):** skip and flag anything taking >90 seconds; come back at the end. Every question is worth the same — don't burn 5 minutes on one hard calculation while easy questions wait.
- **Subject B (20 Q, weighted, 100 min = 5 min/question avg):** these are scenario-based and reward careful tracing over speed. Read the pseudocode twice before touching a table/trace. Build the habit now of tracing variable states in a table as you go — exactly like debugging with print statements, just on paper.
- Common failure modes to watch for: mixing up which system reliability formula applies (series vs. parallel), sign errors in two's complement, forgetting units when converting binary fractions, rushing PERT/CPM critical path identification.
- We'll do a full "time-management dry run" during the Week 10–11 mocks so results on exam day aren't a surprise.

---
