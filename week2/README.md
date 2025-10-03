# BabySoC Fundamentals & Functional Modelling

## Week 2 Task

This repository contains the work for Week 2 of the BabySoC project, covering both theoretical understanding and hands-on functional modelling of a simplified System-on-Chip.




## Part 1: Theory (Conceptual Understanding)

The theoretical foundation covering SoC fundamentals has been completed and is available in the PDF document:
**`Week2_Part1.pdf`**

This write-up covers:
- System-on-Chip (SoC) fundamentals
- Components of typical SoCs (CPU, memory, peripherals, interconnect)
- BabySoC as a simplified learning model
- Role of functional modelling in SoC design flow

## Part 2: Labs (Hands-on Functional Modelling)

### Overview
This part involves functional simulation and verification of the BabySoC design using Icarus Verilog and GTKWave. The BabySoC integrates three main components:
- **RVMYTH**: RISC-V based processor core
- **PLL**: Phase-Locked Loop for clock generation
- **DAC**: 10-bit Digital-to-Analog Converter

### Tools Used
- **Icarus Verilog (iverilog)**: For compiling and simulating Verilog code
- **GTKWave**: For viewing and analyzing simulation waveforms

### Lab Steps Completed

1. **Project Setup**
   - Cloned the BabySoC repository
   - Verified project structure and dependencies

2. **Compilation**
   - Compiled all Verilog modules using Icarus Verilog
   - Resolved dependencies and include paths

3. **Simulation**
   - Ran pre-synthesis functional simulation
   - Generated VCD waveform files for analysis

4. **Waveform Analysis**
   - Analyzed reset operation and initialization sequence
   - Verified clock generation and distribution
   - Monitored data flow between RISC-V core and DAC

### Simulation Results

#### Screenshot 1: Reset Operation
<img width="1030" height="674" alt="reset waveform " src="https://github.com/user-attachments/assets/2f46be38-2d4c-4f36-879f-8840d909090a" />

**Description**: Shows the reset signal transitioning from high to low, initializing the BabySoC components to their known states.

#### Screenshot 2: Clock Generation
<img width="1014" height="674" alt="clock signals " src="https://github.com/user-attachments/assets/b9dc9d94-0403-4822-ab14-77917b12da12" />

**Description**: Demonstrates the PLL generating stable clock signals and the reference clock oscillation that synchronizes system operations.

#### Screenshot 3: Data Flow
<img width="1014" height="674" alt="data flow" src="https://github.com/user-attachments/assets/cc56c522-a7d6-4f83-9c86-732c15ec7f64" />

**Description**: Illustrates the digital data output from RISC-V core being converted to analog output by the DAC, showing the complete data path.
#### Screenshot 4: dump waveform
<img width="1214" height="770" alt="dump_vcd" src="https://github.com/user-attachments/assets/5ecfb7c4-5238-4be3-9d33-9f7fa9eab160" />


**Description**: completed dump.vcd waveform

### Files Generated
- `pre_synth_sim.out`: Compiled simulation executable
- `dump.vcd`: Value Change Dump file with waveform data
- Simulation logs and verification reports

### How to Reproduce

1. **Install Dependencies**:
   ```bash
   sudo apt-get install iverilog gtkwave
   ```

2. **Compile Design**:
   ```bash
   iverilog -o output/pre_synth_sim/pre_synth_sim.out -I src/include -I src/module src/module/testbench.v src/module/*.v
   ```

3. **Run Simulation**:
   ```bash
   cd output/pre_synth_sim
   ./pre_synth_sim.out
   ```

4. **View Waveforms**:
   ```bash
   gtkwave dump.vcd
   ```

## Conclusion

This project successfully demonstrates:
- Understanding of SoC architecture and components
- Functional modelling of digital systems
- Simulation and verification using industry-standard tools
- Integration of processor, clock management, and analog interfaces

The BabySoC serves as an excellent educational platform for learning embedded systems design and digital-analog interfacing concepts.

---
