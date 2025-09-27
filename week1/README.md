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
before:

<img width="417" height="171" alt="opt_check_before_rohn" src="https://github.com/user-attachments/assets/c4b4e49f-1b1e-4b30-8288-948f36c2b312" />
after:

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

# 📅 Day 4: Gate-Level Simulation & Coding Practices

## **Key Concepts**
- **Gate-Level Simulation (GLS)** flow
- **Blocking vs Non-blocking** assignments  
- **Synthesis-Simulation mismatch** prevention

## **Essential Execution Steps**

### 1. **GLS Execution:**
```bash
# Synthesize to netlist
yosys -p "read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; read_verilog good_mux.v; synth -top good_mux; abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; write_verilog good_mux_netlist.v;"

# Gate-Level Simulation
iverilog ../lib/primitives.v ../lib/sky130_fd_sc_hd.v good_mux_netlist.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```

### 2. **Blocking vs Non-blocking Analysis:**
```bash
# Blocking assignment (Combinational)
iverilog blocking_example.v tb_blocking.v
./a.out
gtkwave tb_blocking.vcd

# Non-blocking assignment (Sequential)  
iverilog non_blocking_example.v tb_non_blocking.v
./a.out
gtkwave tb_non_blocking.vcd
```

### 3. **Good vs Bad Code Comparison:**
```bash
# Good Mux (Correct)
iverilog good_mux.v tb_good_mux.v
./a.out

# Bad Mux (Incomplete sensitivity list)
iverilog bad_mux.v tb_good_mux.v  # Expect warnings
./a.out
```

## **Screenshots**

<img width="710" height="233" alt="good_mux_synth_rohn" src="https://github.com/user-attachments/assets/4aa44849-f6d9-4f57-b4f4-5b5d39fb872e" />

**Good Mux Synthesis - rohn** 👆  
*Properly synthesized multiplexer with complete sensitivity list*

<img width="1005" height="677" alt="Screenshot from 2025-09-27 17-19-51" src="https://github.com/user-attachments/assets/a38c9d13-ae27-4bfd-bb8e-c1c96b9acb1c" />
<img width="994" height="679" alt="Screenshot from 2025-09-27 17-14-50" src="https://github.com/user-attachments/assets/179b42c6-9ce2-4163-a279-d1e1bfa3843e" />
<img width="704" height="233" alt="blocking_synth_rohn" src="https://github.com/user-attachments/assets/c73e900a-8b61-46fd-849a-eb4a0aa5182b" />


**Blocking vs Non-blocking Simulation - rohn** 👆  
*Comparison of assignment styles in simulation*

<img width="973" height="286" alt="Screenshot from 2025-09-27 17-23-01" src="https://github.com/user-attachments/assets/6ede6881-e537-4a7b-bb1b-e342ae4dd3a9" />

analysis:


<img width="421" height="233" alt="bad_mux_analysis_rohn" src="https://github.com/user-attachments/assets/98eadf94-4910-43eb-b13a-044da315bf2d" />

synthesis:

<img width="710" height="233" alt="bad_mux_synth_rohn" src="https://github.com/user-attachments/assets/7deb46aa-94a0-4785-9697-2cb378aaf9be" />
<img width="710" height="233" alt="good_mux_synth_rohn" src="https://github.com/user-attachments/assets/b0b36f45-0d6c-4e8f-a70a-bfa36d9b03be" />


**Good vs Bad Mux Analysis - rohn** 👆  
*Synthesis results showing proper vs problematic coding*

## **Code Examples**

### ✅ Good Mux (Correct):
```verilog
module good_mux (input i0, input i1, input sel, output reg y);
    always @(*) begin  // Complete sensitivity list
        if(sel) y = i1;
        else y = i0;
    end
endmodule
```

### ❌ Bad Mux (Problematic):
```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
    always @(sel) begin  // Incomplete sensitivity list
        if(sel) y = i1;
        else y = i0;
    end
endmodule
```

## **Quick Verification Commands**
```bash
# Generate all schematics with PNG outputs
yosys -p "read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; read_verilog good_mux.v; synth -top good_mux; show -format dot -prefix good_mux_rohn;"
dot -Tpng good_mux_rohn.dot -o good_mux_rohn.png

# Test simulation matching
iverilog good_mux.v tb_good_mux.v && ./a.out
echo "GLS verification completed successfully!"
```

## **Expected Results**
- ✅ **Good Mux**: Clean synthesis, proper simulation behavior
- ⚠️ **Bad Mux**: Tool warnings, potential simulation mismatch  
- 🔄 **Blocking**: Immediate assignment (combinational logic)
- ⏰ **Non-blocking**: Scheduled assignment (sequential logic)

# 📅 Day 5: Advanced Optimization & Scalable Coding

## **Key Concepts**
- **Latch inference prevention**
- **For loops and generate blocks**  
- **Scalable hardware design**

## **Essential Execution Steps**

### **Latch Detection Test:**
```bash
iverilog incomplete_if.v tb_incomplete_if.v  # Shows latch warnings
gtkwave tb_incomplete_if.vcd
```

### **Generate Block Synthesis:**
```bash
yosys -p "read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; 
          read_verilog rca_generate.v; 
          synth -top rca; 
          show -format dot -prefix rca_generate_rohn;"
dot -Tpng rca_generate_rohn.dot -o rca_generate_rohn.png
```

### **Loop-based Design Synthesis:**
```bash
yosys -p "read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; 
          read_verilog mux_generate.v; 
          synth -top mux_generate; 
          abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; 
          show -format dot -prefix mux_loop_rohn;"
dot -Tpng mux_loop_rohn.dot -o mux_loop_rohn.png
```

### **Complete vs Incomplete Case Analysis:**
```bash
# Complete case (no latches)
iverilog comp_case.v tb_comp_case.v
./a.out

# Incomplete case (latches inferred)  
iverilog incomplete_if.v tb_incomplete_if.v
./a.out
```

## **Screenshots**
<img width="998" height="685" alt="Screenshot from 2025-09-27 18-07-17" src="https://github.com/user-attachments/assets/03513f39-adf0-4939-8d41-40ebb9335d43" />

<img width="480" height="233" alt="latch_inference_rohn" src="https://github.com/user-attachments/assets/19b6147a-709b-4f54-b752-fa83fd3ac3a1" />


**Latch Inference in Incomplete If Statement - rohn** 👆  
*Shows latch cell generated due to missing else clause*
<img width="1007" height="685" alt="Screenshot from 2025-09-27 18-10-52" src="https://github.com/user-attachments/assets/b4e43252-90aa-4643-89ea-10fae4b9c47f" />

<img width="1009" height="236" alt="rca_generate_rohn" src="https://github.com/user-attachments/assets/3cd1152d-29c2-4e11-8ba3-9ac78247687c" />


**8-bit RCA with Generate Blocks - rohn** 👆  
*Scalable adder implementation using generate for loops*
<img width="1007" height="685" alt="Screenshot from 2025-09-27 18-09-40" src="https://github.com/user-attachments/assets/686c9949-5945-4dd5-b4f6-cb86548b60c1" />

<img width="1086" height="584" alt="mux_loop_rohn" src="https://github.com/user-attachments/assets/685feb12-55c2-4f8b-a673-be54dac97325" />


**For-loop Based MUX Implementation - rohn** 👆  
*Parameterized multiplexer using procedural for loop*

## **Code Examples**

### ✅ **Complete Case (No Latch):**
```verilog
module comp_case(input i0, i1, i2, input [1:0] sel, output reg y);
    always @(*) begin
        case(sel)
            2'b00: y = i0;
            2'b01: y = i1;
            default: y = i2;  // Prevents latch
        endcase
    end
endmodule
```

### ❌ **Incomplete If (Latch Inferred):**
```verilog
module incomplete_if(input i0, i1, i2, output reg y);
    always @(*) begin
        if (i0) y = i1;  // Missing else → LATCH!
    end
endmodule
```

### 🔄 **Scalable RCA with Generate:**
```verilog
module rca(input [7:0] num1, num2, output [8:0] sum);
    wire [7:0] int_co, int_sum;
    genvar i;
    generate for (i=0; i<8; i=i+1) begin
        if (i==0) fa u_fa(num1[0], num2[0], 1'b0, int_co[0], int_sum[0]);
        else fa u_fa(num1[i], num2[i], int_co[i-1], int_co[i], int_sum[i]);
    end endgenerate
    assign sum = {int_co[7], int_sum};
endmodule
```

## **Quick Verification Commands**
```bash
# Generate all schematics
yosys -p "read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib; 
          read_verilog incomplete_if.v; 
          synth -top incomplete_if; 
          show -format dot -prefix day5_rohn;"

# Test latch behavior
iverilog incomplete_if.v tb_incomplete_if.v && ./a.out
echo "Latch detection test completed!"
```

## **Expected Results**
- ⚠️ **Incomplete If**: Latch cell in synthesis, simulation shows memory behavior
- ✅ **Complete Case**: Pure combinational logic, no latches  
- 🔄 **Generate Blocks**: Scalable hardware, easy to modify parameters
- 🎯 **For-loop MUX**: Efficient parameterized design

**Day 5 Complete:** Mastered latch prevention and scalable coding techniques! ✅
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
