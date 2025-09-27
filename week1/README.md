# WEEK1 - 5-Day Combined Summary

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
# **Screenshots**

<img width="999" height="702" alt="good_mux_gtkwave" src="https://github.com/user-attachments/assets/57962214-f05d-4d95-9ff3-95f73a0abddd" />

**good_mux.v simulation waveform👆**

<img width="999" height="702" alt="good_mux_yosys" src="https://github.com/user-attachments/assets/7cfef8c6-9880-4beb-94fd-c486d1567f82" />


**Yosys synthesis schematic👆**

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

# **Screenshots**

<img width="999" height="702" alt="Screenshot from 2025-09-26 22-24-51" src="https://github.com/user-attachments/assets/0ff46e7c-12e4-425f-bbb3-aff00fd79045" />

**.lib file content view👆**


<img width="999" height="702" alt="Screenshot from 2025-09-26 22-25-37" src="https://github.com/user-attachments/assets/64b25255-d679-4db2-92ca-a4815e64a00f" />

## **VS**
<img width="999" height="702" alt="flatted" src="https://github.com/user-attachments/assets/6611b757-7655-4576-99a6-fe6868c4d814" />

**Hierarchical vs Flattened synthesis comparison👆**

<img width="999" height="300" alt="Screenshot from 2025-09-26 22-43-50" src="https://github.com/user-attachments/assets/89961f8a-24e6-45f9-929c-4cd6a6329c13" />

**D flip-flop synthesis result👆**

---

# 📅 Day 3: Combinational & Sequential Optimization

## Key Concepts
- **Constant Propagation** - Replacing variables with constant values
- **Logic Simplification** - Reducing complex expressions  
- **Sequential Optimization** - Optimizing flip-flops and registers
- **Dead Code Elimination** - Removing unused logic

## Essential Execution Steps

### Constant Propagation Lab:
```bash
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
stat
```

### Complex Expression Optimization:
```bash
read_verilog opt_check4.v  
synth -top opt_check4
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
stat
```

### Sequential Constant Optimization:
```bash
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
stat
```

## Lab Code Examples

### Lab 1: Basic Constant Propagation
```verilog
module opt_check (input a, input b, output y);
    assign y = a ? b : 0;  // Optimizes to y = a & b
endmodule
```

### Lab 4: Complex Expression Simplification  
```verilog
module opt_check4 (input a, input b, input c, output y);
    assign y = a ? (b ? (a & c) : c) : (!c);  // Optimizes to y = a ? c : !c
endmodule
```

### Lab 5: Sequential Constant Optimization
```verilog
module dff_const1(input clk, input reset, output reg q);
    always @(posedge clk, posedge reset)
        if(reset) q <= 1'b0; else q <= 1'b1;  // DFF with constant input
endmodule
```

## Screenshots


<img width="417" height="171" alt="opt_check_before_rohn" src="https://github.com/user-attachments/assets/c4b4e49f-1b1e-4b30-8288-948f36c2b312" />

<img width="694" height="171" alt="opt_check_after_rohn" src="https://github.com/user-attachments/assets/75197e0c-f4b3-4924-8334-11446706705d" />


**opt_check.v before and after optimization**👆

<img width="702" height="243" alt="opt_check4_optimized_rohn" src="https://github.com/user-attachments/assets/0bcdcc0b-e437-4019-8623-1dd1c309134f" />

**Constant propagation example**👆


<img width="1259" height="223" alt="dff_const_optimized_rohn" src="https://github.com/user-attachments/assets/04c285bb-eba5-49d4-bec6-da94a72c8803" />


**D flip-flop constant optimization**👆

## Quick Run All Labs
```bash
# Single command for all optimizations
for design in opt_check opt_check4 dff_const1; do
    yosys -p "read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; 
              read_verilog $design.v; 
              synth -top $design; 
              opt_clean -purge; 
              stat;"
done
```

## Optimization Results Summary
- **Lab 1**: `y = a ? b : 0` → Simplified AND logic
- **Lab 4**: Complex ternary → Simplified `y = a ? c : !c`  
- **Lab 5**: DFF with constant input → Optimized sequential cell



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
