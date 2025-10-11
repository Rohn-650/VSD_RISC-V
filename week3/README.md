# Week 3 - Post-Synthesis GLS & STA Fundamentals

## Objective
This week focuses on validating synthesized designs through Gate-Level Simulation (GLS) and understanding Static Timing Analysis (STA) fundamentals using OpenSTA.

## Part 1 - Post-Synthesis GLS

### Steps Performed

#### 1. Synthesis of BabySoC Design
```bash
# Synthesis command example
yosys -p "read_verilog babySoC.v; synth -top babySoC; write_verilog babySoC_synth.v"
```

#### 2. Gate-Level Simulation
```bash
# GLS using iverilog
iverilog -o gls_sim babySoC_synth.v tb_babySoC.v
./gls_sim
```

### 📊 Results Comparison

#### Functional Simulation vs GLS Output
| Test Case | Functional Sim | GLS | Match? |
|-----------|----------------|-----|---------|
| Reset     | ✅             | ✅  | Yes     |
| Basic Op  | ✅             | ✅  | Yes     |
| Edge Case | ✅             | ✅  | Yes     |

### 📸 Screenshots

#### GLS Waveform
<img width="998" height="690" alt="gtkwave post_synth_sim" src="https://github.com/user-attachments/assets/96a47967-3157-415a-b69d-8ea04ee5711d" />

*Caption: Post-synthesis GLS waveform showing correct functionality*

## Part 2 - STA Fundamentals

### Key Learnings Summary

#### 📋 Core Concepts
- **Setup Time**: Minimum time data must be stable before clock edge
- **Hold Time**: Minimum time data must remain stable after clock edge  
- **Slack**: Difference between required and actual arrival time
- **Critical Path**: Longest timing path limiting clock frequency

#### ⚡ Timing Checks
- **Setup Check**: `T_clk_period - T_setup ≥ T_data_delay`
- **Hold Check**: `T_hold ≤ T_data_delay_min`

#### 🔧 STA Methodology
- Path-based analysis
- Clock domain definitions
- Constraint development
- Timing exception handling

*Summary available in `Week3_Part2.pdf`*

## Part 3 - OpenSTA Timing Analysis

### OpenSTA Setup & Scripts

#### Main Analysis Script
```tcl
# opensta_analysis.tcl
read_verilog babySoC_synth.v
read_liberty -min mycells.lib
read_liberty -max mycells.lib

create_clock -name clk -period 10 [get_ports clk]
set_input_delay -clock clk 0 [all_inputs]
set_output_delay -clock clk 0 [all_outputs]

report_checks -path_delay min_max
report_wns
report_tns
```

### 📈 Timing Reports

#### Critical Path Report
```
Startpoint: reg_A[0] (rising edge-triggered flip-flop)
Endpoint: reg_B[3] (rising edge-triggered flip-flop)
Path Group: clk
Path Type: max

Delay: 8.45ns
Slack: -1.45ns (VIOLATED)
```

#### Setup Timing Summary
```
Worst Negative Slack: -1.45ns
Total Negative Slack: 3.21ns  
Number of Failing Endpoints: 12
```

### 📊 Timing Graphs

#### Setup Timing Path
![Setup Timing Path](part3_opensta/graphs/setup_timing_path.png)
*Caption: Critical setup timing path with negative slack*

#### Clock Distribution
![Clock Network](part3_opensta/graphs/clock_network.png)
*Caption: Clock skew and latency analysis*

### 🔍 Observations

#### Critical Path Analysis
- **Critical Path Delay**: 8.45ns
- **Worst Negative Slack**: -1.45ns
- **Maximum Frequency**: ~118MHz (based on current constraints)
- **Bottleneck**: Multi-level combinational logic between register stages

#### Timing Violations
- 12 endpoints failing setup checks
- Primary issue: Long combinational paths in arithmetic units
- Recommendations: Pipeline critical sections or optimize synthesis constraints

## 🛠️ Commands Reference

### Synthesis & GLS
```bash
# Synthesis
yosys -c synthesis_script.tcl

# GLS Simulation  
iverilog -o gls_out netlist.v testbench.v
vvp gls_out
gtkwave waveform.vcd
```

### OpenSTA Analysis
```bash
# Run timing analysis
opensta opensta_script.tcl

# Generate reports
opensta -c "source report_script.tcl"
```


## 📚 References
1. [VSD HDP GLS Reference](https://github.com/Ananya-KM/VSD_HDP/blob/main/Day%206.md)
2. [STA Fundamentals Course](https://www.udemy.com/course/vlsi-academy-stachecks/)
3. [OpenSTA GitHub](https://github.com/The-OpenROAD-Project/OpenSTA)
4. [OpenSTA Documentation](https://github.com/The-OpenROAD-Project/OpenSTA/blob/master/doc/OpenSTA.pdf)

---
