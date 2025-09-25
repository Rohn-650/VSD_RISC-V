
# 🚀 Week 1 – RTL to GLS

---

## 📌 Task 1 – Yosys Optimization with `opt_clean -purge`

**Objective:** Remove redundant wires, cells, and dead logic.

**Flow:**

1. `read_liberty -lib …` → Load library
2. `read_verilog opt_check4.v` → Load RTL
3. `synth -top opt_check4` → Synthesize
4. `opt_clean -purge` → Remove unused logic
5. `abc -liberty …` → Map to tech cells
6. `show` → Netlist view

**Results:**

* **opt_check4.v** → Clean netlist, no redundant wires.
* **multiple_module_opt.v** → Simpler design after purge.

**Takeaway:**
`opt_clean -purge` = **vacuum cleaner** 🧹 → smaller, faster, cleaner netlist.

<!-- (Insert before vs after netlist snapshot) -->

---

## 📌 Task 2 – Constant DFF Mapping & GLS

**Objective:** See how Yosys handles constant-driven flops (`const4.v`, `const5.v`).

**Flow:**

1. `read_liberty` + `read_verilog`
2. `synth -top constX`
3. `dfflibmap` → map DFFs
4. `abc` → optimize
5. `write_verilog` → netlist
6. GLS: `iverilog constX.v tb_constX.v`

**Results:**

* **const4.v** → Outputs tied to constant 1, buffer used.
* **const5.v** → Reset handled, two proper flops generated.

**Takeaway:**
Yosys optimizes constants but preserves **reset logic**.

<!-- (Insert GLS vs RTL sim waveform) -->

---

## 📌 Task 3 – MUX Using for-generate

**Flow:**

* Synth with `Test_Synth.ys`
* GLS with primitives + cell models

**Fix:** Missing latch replaced with `sky130_fd_sc_hd__udp_dlatch$lP`.

**Results:**

* RTL = GLS ✅
* Scalable MUX instantiation works.

**Takeaway:**
for-generate = **clean, reusable hardware construction**.

<!-- (Insert MUX netlist + sim result) -->

---

## 📌 Task 4 – DEMUX Using generate

**Flow:** GLS sim requires primitives + std cells.

**Results:**

* RTL = GLS ✅
* Code modular & clean.

**Takeaway:**
`generate` scales DEMUX without errors.

<!-- (Insert DEMUX sim image) -->

---

## 📌 Task 5 – Ripple Carry Adder (RCA)

**Flow:** GLS with primitives + std cells.

**Results:**

* RTL = GLS ✅
* RCA mapped to std-cell adders & carry chain.

**Takeaway:**
Arithmetic circuits synthesize robustly with Yosys.

<!-- (Insert RCA sim & netlist image) -->

---

## 📘 Theory Notes (Mini)

* **Behavioral Synthesis** → Converts `always/if/case` → muxes, flops, FSMs.
* **Timing** → Setup, hold, critical path delay.
* **Liberty file (.lib)** → Timing, power, functionality.
* **Hierarchical vs Flat Synthesis** → Hierarchy = reuse; Flat = optimization.
* **Blocking vs Non-blocking** → `=` sequential, `<=` parallel.
* **Incomplete case/if** → Leads to latch → add `default`.
* **GLS Importance** → Confirms RTL = hardware netlist.

---

## 📝 Quick Command Cheatsheet

```tcl
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog design.v
synth -top design
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
opt_clean -purge
write_verilog design_GLS.v
```

---

Would you like me to **add 1–2 tiny Verilog code snippets** (before vs after `opt_clean`), so your notes feel even more concrete and quick to revise?
