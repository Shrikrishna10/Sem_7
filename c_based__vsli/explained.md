
---
#  C-Based VLSI Design — Weeks 1 → 6 (Concept + Takeaway Summary)

---

##  Week 1 – Introduction to C-Based VLSI Design

| # | Concept Explanation                                                                                          | Takeaway                                                     |
| - | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| 1 | Correct VLSI flow: **Spec → Arch → HLS → Logic → Physical → Fab → Test**.                                    | Spec → Arch → HLS → Logic → Phys → Fab → Test.               |
| 2 | Moore = transistor count doubles ≈ 2 yrs; Dennard = constant power density (so given statements were wrong). | Moore → count↑ every 2 yrs; Dennard → power density ≈ const. |
| 3 | HLS flow order: Preprocess → Schedule → Allocate/Bind → Datapath + Controller.                               | Clean → Time → Hardware → Connect.                           |
| 4 | “nm” node = distance between source and drain of MOSFET.                                                     | nm = gate length.                                            |
| 5 | Scheduling assigns timestamps to operations on CDFG; can’t be random.                                        | Scheduling = time-stamping CDFG.                             |
| 6 | C → RTL map: Fn→Module, Var→Reg, Op→FU, Arg→I/O, Array→Mem.                                                  | Fn-Mod, Var-Reg, Op-FU, Arg-IO, Arr-Mem.                     |
| 7 | ASAP/ALAP define earliest and latest start times.                                                            | ASAP early, ALAP late.                                       |
| 8 | Mobility = ALAP – ASAP; 0 means fixed time.                                                                  | Mobility = timing flexibility.                               |

---

##  Week 2 – Basic Scheduling

| #      | Concept Explanation                                                   | Takeaway                         |
| ------ | --------------------------------------------------------------------- | -------------------------------- |
| 1      | ILP gives **optimal** solution if solved fully.                       | ILP = optimal.                   |
| 2      | MLRC = Min Latency Res Constraint; MRLC = Min Res Latency Constraint. | MLRC → min time; MRLC → min hw.  |
| 3 – 4  | ASAP/ALAP find critical path and slack.                               | ASAP = earliest, ALAP = latest.  |
| 5 – 6  | Mobility drives priority (low mobility → schedule first).             | Less mobility → higher priority. |
| 7      | ASAP schedule length = critical path latency.                         | Critical path = ASAP length.     |
| 8      | MLRC needs unique start + data dep + resource constraints.            | Who + when + resource.           |
| 9 – 10 | ILP eqns: Σxᵢₜ = 1 (start); Σxᵢₜ ≤ avail (res).                       | Binary ILP → run & limit.        |

---

## ⚙️ Week 3 – List-Based Scheduling

| #    | Concept Explanation                                  | Takeaway                       |
| ---- | ---------------------------------------------------- | ------------------------------ |
| 1    | Edges in DFG → precedence constraints.               | Edge = “exec before”.          |
| 2    | Resource constraint: Σx ≤ available units.           | Sum ≤ resource count.          |
| 3    | Hu’s algorithm labels nodes by level from sink.      | Label = distance to sink.      |
| 4    | Lower labels schedule first; ties → small ID.        | Low label = early.             |
| 5    | Hu’s Theorem → min processors needed ≈ max Σ p(l)/λ. | Hu = proc bound.               |
| 6–7  | Candidate = ready; unfinished = waiting.             | Ready vs waiting sets.         |
| 8–10 | Priority = longest path distance.                    | Longer path → higher priority. |

---

##  Week 4 – Advanced Scheduling (Force-Directed etc.)

| #    | Concept Explanation                                                               | Takeaway                     |
| ---- | --------------------------------------------------------------------------------- | ---------------------------- |
| 1–3  | FDS computes self + pred + succ forces; choose least total force.                 | Least force wins.            |
| 4    | Scheduling one node changes probabilities of others.                              | Forces shift on schedule.    |
| 5–6  | MLRC → least force; MRLC → highest force first.                                   | MLRC min time, MRLC min hw.  |
| 7    | FDS ≈ O(n²) complexity.                                                           | FDS = quadratic.             |
| 8–10 | AFAP state edges added when last op of Si → first of Sj and succ relation exists. | State edge follows succ ops. |

---

## Week 5 – Allocation & Binding

| # | Concept Explanation                                                | Takeaway                                    |
| - | ------------------------------------------------------------------ | ------------------------------------------- |
| 1 | Chordal graph test → every cycle ≥ 4 has chord.                    | Chordal = no long hole.                     |
| 2 | Compatible ops can share FU.                                       | Non-conflict → share FU.                    |
| 3 | Chromatic no. = # FUs; Clique cover = # regs.                      | Color → alloc, Clique → bind.               |
| 4 | Ops 1 & 4 same timestamp ⇒ conflict edge exists.                   | Conflict ↔ same time.                       |
| 5 | Interval graph iff chordal & complement comparability.             | Interval = Chordal ∧ Comp( ) comparability. |
| 6 | Hierarchical FU binding → conflict not interval → Left Edge fails. | Left Edge only for intervals.               |
| 7 | Share FU if intervals don’t overlap.                               | Non-overlap → safe share.                   |
| 8 | Left Edge = O(n log n), not O(log n).                              | Left Edge ≈ n log n.                        |

---

##  Week 6 – Allocation, Binding, Datapath & Controller Generation

| #   | Concept Explanation                                                       | Takeaway                       |
| --- | ------------------------------------------------------------------------- | ------------------------------ |
| 1   | Hierarchical register binding: conflict graph non-interval ⇒ NP-complete. | Non-interval → hard.           |
| 2–3 | Control pattern bits specify which multipliers & regs enabled each cycle. | Controller = enable pattern.   |
| 4   | Given pattern → which ops execute (parallel activation).                  | Control vector = active ops.   |
| 5   | Mux-based datapath interconnect is standard for sharing resources.        | Mux = shared wiring.           |
| 6   | Reg alloc/bind: Conflict → graph coloring; Compat → clique covering.      | Color conflict; Clique compat. |
| 7   | Single-port memory binding → Σ bᵢ ≤ ports (a).                            | ≤ ports per timestamp.         |
| 8   | Register sharing in single function hierarchy is polynomial.              | Flat hierarchy → easy.         |

---

## Overall Themes

| Concept                   | Key Idea                                   | Memory Hook            |
| ------------------------- | ------------------------------------------ | ---------------------- |
| Scheduling                | Assign time steps                          | “Who runs when.”       |
| Allocation & Binding      | Assign hardware                            | “Who runs where.”      |
| Conflict vs Compatibility | Conflict → can’t share; Compat → can share | “Color vs Clique.”     |
| Left Edge Algo            | Resource allocation on interval graphs     | “Sort by start time.”  |
| FDS                       | Balance load by forces                     | “Least force wins.”    |
| Hierarchy                 | Adds complexity (NP-hard)                  | “Hierarchy hurts.”     |
| Mux-Datapath              | Shared connections                         | “One path → many ops.” |

---

---

##  WEEK 1 — Introduction to C-Based VLSI Design

**Q1. Correct VLSI design flow**
→ *System Specification → Architectural Design → High-Level Synthesis → Logic Synthesis → Physical Synthesis → Fabrication → Packaging & Testing*
**Why:** You start from behavior → architecture → RTL → gates → layout → chip.
**Takeaway:** “Spec → Arch → HLS → Logic → Physical → Fab → Test.”

**Q2. Moore vs Dennard laws**
Both given statements were wrong. Moore’s doubling happens ~every 2 years, not 5; Dennard scaling keeps power density *constant*, not increasing.
**Takeaway:** Moore = transistor count ↑ ~2 years; Dennard = power density ≈ constant.

**Q3. Correct HLS flow**
→ *Preprocessing → Scheduling → Allocation & Binding → Datapath & Controller Generation*
**Why:** You first clean/parse code, then assign time (schedule), then assign hardware (allocate/bind).
**Takeaway:** “Clean → Time → Hardware → Connect.”

**Q4. What does ‘nanometer’ mean?**
→ Distance between source and drain.
**Why:** Technology node represents transistor gate length.
**Takeaway:** “nm = gate length.”

**Q5. Scheduling statements**
→ S1 and S3 correct. Scheduling assigns timestamps on a CDFG, under constraints.
**Takeaway:** “Scheduling = timestamp assignment on CDFG.”

**Q6. C/RTL mapping**
Functions→Modules; Variables→Registers; Operators→Functional Units; Arguments→I/O ports; Arrays→Memory.
**Takeaway:** “Fn→Mod; Var→Reg; Op→FU; Arg→IO; Arr→Mem.”

**Q7. Data-dependency graph scheduling**
Uses ASAP (start as soon as possible) & ALAP (start as late as possible) times to find operation windows.
**Takeaway:** ASAP→earliest start, ALAP→latest start.

**Q8–Q10. Mobility and Constraints**
Mobility = ALAP – ASAP. Zero mobility means fixed time. MLRC needs unique start + data dep + resource constraints.
**Takeaway:** “Mobility = timing flexibility.”

---

##  WEEK 2 — Basic Scheduling

**Q1. ILP output**
→ Always optimal.
**Takeaway:** “ILP = guaranteed optimum (if solved fully).”

**Q2. Full form MLRC/MRLC**
→ *Minimum Latency Resource Constraint* / *Minimum Resource Latency Constraint.*
**Takeaway:** “MLRC min time, MRLC min hardware.”

**Q3–Q4. ASAP/ALAP timing**
ASAP/ALAP help find critical path and slack.
**Takeaway:** ASAP = fastest; ALAP = latest without violation.

**Q5–Q6. Mobility**
Mobility = ALAP – ASAP. Used to pick flexible operations.
**Takeaway:** Low mobility → higher scheduling priority.

**Q7. Cycles in ASAP**
→ Sum of longest path latencies.
**Takeaway:** “ASAP cycles = critical path length.”

**Q8. MLRC constraints**
→ Unique start, data dep, resource.
**Takeaway:** “MLRC = when + dependency + resource.”

**Q9–Q10. ILP equations**
Unique start = sum(xᵢₜ)=1; Resource = Σ x ≤ no. of units.
**Takeaway:** Binary ILP models ‘who runs when’ and ‘resource use ≤ limit.’

---

## WEEK 3 — List-Based Scheduling

**Q1. Precedence constraints**
Derived from edges in DFG — each edge → execution order.
**Takeaway:** “Edge = must-execute-before rule.”

**Q2. Resource constraint formulas**
Sum of xᵢₜ for same type ≤ a (resources).
**Takeaway:** “Each type ≤ resource count.”

**Q3. Hu’s algorithm labels**
α = max label; p(j) = #nodes per label.
**Takeaway:** “Hu labels = levels from sink.”

**Q4. Scheduling via Hu’s algorithm**
Smallest label nodes first; ties by ID.
**Takeaway:** “Lower label = earlier slot.”

**Q5. Lower bound on processors**
Use Hu’s Theorem: max over l (p(l )+ p(l+1) + …)/λ.
**Takeaway:** “Hu → min processors needed.”

**Q6–Q7. Candidate vs unfinished ops**
Used to define ready operations for MLRC.
**Takeaway:** “Candidate = ready; unfinished = waiting.”

**Q8–Q10. List scheduling examples**
Use priority = longest path distance → schedule until resources full.
**Takeaway:** “Longer path → higher priority in list scheduling.”

---

##  WEEK 4 — Advanced Scheduling

**Q1–Q3. Forced-Directed Scheduling (FDS)**
Compute self force, predecessor force, successor force → pick least total force.
**Takeaway:** “Least force → best time slot for balance.”

**Q4. Force variation**
Change in probabilities when a node is scheduled.
**Takeaway:** “Scheduling one op shifts others’ probability.”

**Q5–Q6. MLRC/MRLC force rules**
MLRC → least force; MRLC → highest force first.
**Takeaway:** “MLRC min latency → least force; MRLC min resources → most force.”

**Q7. Force-directed complexity**
Typically O(n²).
**Takeaway:** “FDS = quadratic.”

**Q8–Q10. AFAP & state transitions**
In AFAP, edges between states based on successor relations; condition = TRUE if unconditional.
**Takeaway:** “State edges follow successor ops logic.”

---

## WEEK 5 — Allocation & Binding

**Q1. Chordal graph property**
False statement: Graph was chordal. Chordal → every cycle ≥ 4 has chord.
**Takeaway:** “Chordal = no long cycle without shortcut.”

**Q2. FU binding**
Compatible ops share FU.
**Takeaway:** “Bind non-conflicting ops to same unit.”

**Q3. Chromatic/clique numbers**
Chromatic = # colors (conflict graph) = min FUs. Clique cover = # registers.
**Takeaway:** “Coloring ↔ allocation.”

**Q4. Operations 1 and 4 same timestamp** → True.
**Takeaway:** “Conflict edge ↔ same timestamp.”

**Q5. Interval graph iff** chordal + comparability of complement.
**Takeaway:** “Interval = chordal ∧ complement comparability.”

**Q6. Hierarchical FU binding**
Conflict graph not interval → Left Edge fails.
**Takeaway:** “Left Edge works only for interval graphs.”

**Q7. Mapping ops in interval graph**
Some pairs share FUs if intervals non-overlapping.
**Takeaway:** “Non-overlap = safe to share hardware.”

**Q8. False statement**
Left Edge complexity is O(n log n), not O(log n).
**Takeaway:** “Left Edge = O(n log n).”

---

##  WEEK 6 — Allocation, Binding, Datapath & Controller Generation

**Q1. Register binding problem (hierarchical)**
Conflict graph not interval → NP-complete. Hence S1 true, S2 true, S1 explains S2.
**Takeaway:** “Non-interval → register binding hard.”

**Q2–Q3. Control assertion patterns**
Bit pattern shows which multiplier enabled and which registers written.
**Takeaway:** “Controller = pattern of enables per cycle.”

**Q4. Control signals mapping**
Pattern → which ops execute (activate corresponding mult/regs).
**Takeaway:** “Control vector decides active datapath ops.”

**Q5. Mux-based architecture**
True — datapaths connect via multiplexers.
**Takeaway:** “Mux = reconfigurable interconnect.”

**Q6. Register allocation and binding**
Conflict graph → graph coloring; compatibility graph → clique covering.
**Takeaway:** “Conflict = color; compatibility = clique.”

**Q7–Q8. Memory/register mapping**
No two simultaneous accesses → same port constraint: Σ bᵢ ≤ a.
**Takeaway:** “Each timestamp ≤ ports available.”

**Q9. Register sharing complexity**
Single function calls → polynomial time.
**Takeaway:** “Flat hierarchy → easy sharing.”

---

##  Overall Revision Themes

| Concept                              | Key Idea                                   | Quick Memory Hook             |
| ------------------------------------ | ------------------------------------------ | ----------------------------- |
| **Scheduling**                       | Assigning time steps                       | “Who runs when”               |
| **Allocation & Binding**             | Mapping ops to hardware                    | “Who runs where”              |
| **Conflict vs Compatibility Graphs** | Conflict → can’t share; Compat → can share | “Color vs Clique”             |
| **Left Edge Algo**                   | Interval-graph resource allocation         | “Sorted by start times.”      |
| **FDS**                              | Balances load using forces                 | “Least force wins.”           |
| **Hierarchical HLS**                 | More constraints → NP-hard                 | “Hierarchy hurts complexity.” |
| **Mux-based Datapath**               | Shared resources via selectors             | “One path many ops.”          |

---


