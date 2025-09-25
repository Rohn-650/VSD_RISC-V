# RTL Workshop - 5-Day Combined Summary

## 🛠️ Essential Setup & Execution Steps

### **Prerequisites Installation**
```bash
# Install required tools
sudo apt update
sudo apt install iverilog gtkwave yosys gedit

# Clone workshop repository
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop
```

### **Standard Simulation Flow**
```bash
# Compile design and testbench
iverilog design.v tb_design.v

# Run simulation
./a.out

# View waveforms
gtkwave tb_design.vcd
```

### **Standard Synthesis Flow**
```bash
# Start Yosys
yosys

# Execute synthesis commands
read_liberty -lib path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog design.v
synth -top module_name
abc -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

## 📅 Day 1: Introduction to Verilog RTL Design & Synthesis

### **Key Concepts**
- Simulator vs Design vs Testbench fundamentals
- Icarus Verilog simulation workflow
- Yosys synthesis flow introduction

### **Essential Execution Steps**
1. **Simulate 2-to-1 Multiplexer:**
   ```bash
   cd verilog_files
   iverilog good_mux.v tb_good_mux.v
   ./a.out
   gtkwave tb_good_mux.vcd
   ```

2. **Synthesize with Yosys:**
   ```bash
   yosys
   read_liberty -lib path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog good_mux.v
   synth -top good_mux
   abc -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   show
   ```

**[INSERT DAY 1 SCREENSHOT: good_mux.v simulation waveform]**

**[INSERT DAY 1 SCREENSHOT: Yosys synthesis schematic]**

---

## 📅 Day 2: Timing Libraries & Synthesis Approaches

### **Key Concepts**
- SKY130 timing library analysis (tt_025C_1v80)
- Hierarchical vs Flattened synthesis
- Flip-flop coding styles

### **Essential Execution Steps**
1. **Explore .lib File:**
   ```bash
   gedit sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

2. **Hierarchical Synthesis:**
   ```bash
   read_verilog hierarchical_design.v
   hierarchy -check -top module_name
   synth -top module_name
   ```

3. **Flattened Synthesis:**
   ```bash
   read_verilog design.v
   flatten
   synth -top module_name
   ```

4. **Flip-Flop Synthesis:**
   ```bash
   read_verilog dff_asyncres.v
   synth -top dff_asyncres
   dfflibmap -liberty path/to/sky130.lib
   abc -liberty path/to/sky130.lib
   show
   ```

**[INSERT DAY 2 SCREENSHOT: .lib file content view]**

**[INSERT DAY 2 SCREENSHOT: Hierarchical vs Flattened synthesis comparison]**

**[INSERT DAY 2 SCREENSHOT: D flip-flop synthesis result]**

---

## 📅 Day 3: Combinational & Sequential Optimization

### **Key Concepts**
- Constant propagation optimization
- State optimization techniques
- Cloning and retiming methods

### **Essential Execution Steps**
1. **Optimization Commands in Yosys:**
   ```bash
   read_verilog opt_check.v
   synth -top opt_check
   opt_clean -purge  # Critical optimization step
   abc -liberty path/to/sky130.lib
   show
   ```

2. **Sequential Circuit Optimization:**
   ```bash
   read_verilog dff_const1.v
   synth -top dff_const1
   opt -full
   abc -liberty path/to/sky130.lib
   show
   ```

**[INSERT DAY 3 SCREENSHOT: opt_check.v before and after optimization]**

**[INSERT DAY 3 SCREENSHOT: Constant propagation example]**

**[INSERT DAY 3 SCREENSHOT: D flip-flop constant optimization]**

---

## 📅 Day 4: Gate-Level Simulation & Coding Practices

### **Key Concepts**
- Gate-Level Simulation (GLS) flow
- Blocking vs Non-blocking assignments
- Synthesis-Simulation mismatch prevention

### **Essential Execution Steps**
1. **GLS Execution:**
   ```bash
   iverilog primitives.v sky130_fd_sc_hd.v synthesized_netlist.v tb_design.v
   ./a.out
   gtkwave tb_design.vcd
   ```

2. **Test Different Coding Styles:**
   ```bash
   # Test blocking assignment version
   iverilog blocking_example.v tb_blocking.v
   
   # Test non-blocking assignment version  
   iverilog non_blocking_example.v tb_non_blocking.v
   ```

3. **Bad Code Example Analysis:**
   ```bash
   iverilog bad_mux.v tb_bad_mux.v  # Expect warnings/mismatches
   ```

**[INSERT DAY 4 SCREENSHOT: GLS waveform vs RTL simulation]**

**[INSERT DAY 4 SCREENSHOT: Blocking vs Non-blocking simulation results]**

**[INSERT DAY 4 SCREENSHOT: Bad mux example warnings]**

---

## 📅 Day 5: Advanced Optimization & Scalable Coding

### **Key Concepts**
- Latch inference prevention
- For loops and generate blocks
- Scalable hardware design

### **Essential Execution Steps**
1. **Latch Detection Test:**
   ```bash
   iverilog incomplete_if.v tb_incomplete_if.v  # Should show latch warnings
   ```

2. **Generate Block Synthesis:**
   ```bash
   read_verilog rca_generate.v
   synth -top rca
   show
   ```

3. **Loop-based Design Synthesis:**
   ```bash
   read_verilog mux_generate.v
   synth -top mux_generate
   abc -liberty path/to/sky130.lib
   show
   ```

4. **Complete vs Incomplete Case Analysis:**
   ```bash
   # Complete case (no latches)
   iverilog comp_case.v tb_comp_case.v
   
   # Incomplete case (latches inferred)  
   iverilog bad_case.v tb_bad_case.v
   ```

**[INSERT DAY 5 SCREENSHOT: Latch inference in incomplete if statement]**

**[INSERT DAY 5 SCREENSHOT: 8-bit RCA with generate blocks]**

**[INSERT DAY 5 SCREENSHOT: For-loop based MUX implementation]**

---

## 📊 Complete Workflow Summary

### **Daily Execution Pattern:**
1. **Design Analysis** → Understand the Verilog module
2. **Simulation** → Verify functionality with testbench
3. **Synthesis** → Convert to gate-level netlist
4. **Optimization** → Apply appropriate optimization techniques
5. **GLS** → Validate post-synthesis functionality
6. **Comparison** → Check for synthesis-simulation mismatch

### **Critical Checks Each Day:**
- ✅ Sensitivity lists complete for combinational logic
- ✅ All outputs assigned in all paths (no latches)
- ✅ Proper assignment style (blocking/non-blocking)
- ✅ Simulation matches synthesis results
- ✅ Optimization commands applied correctly

### **Common Debugging Commands:**
```bash
# Check for warnings
iverilog -Wall design.v tb_design.v

# Detailed synthesis statistics
yosys -p "read_verilog design.v; synth -top module; stat"

# Optimization verification
yosys -p "read_verilog design.v; synth -top module; opt -full; stat"
```

---

## 🎯 Key Success Metrics
- Zero latch inferences in synthesis reports
- No synthesis-simulation mismatches
- Optimal gate count after optimization
- Clean GLS with proper timing
- Scalable, reusable code structure

*Note: Replace `path/to/sky130_fd_sc_hd__tt_025C_1v80.lib` with actual library file path on your system.*
