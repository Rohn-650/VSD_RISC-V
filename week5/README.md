# 📋 OpenROAD Flow Setup: Floorplan & Placement - Week 5 Implementation

> **🎯 Objective:** Install OpenROAD Flow Scripts and execute Floorplan + Placement stages of the physical design flow

---

## 📋 Task Overview

**Week 5 Focus:** Transition from SPICE-level transistor design (Week 4) to backend physical implementation by setting up OpenROAD and running floorplanning and placement stages.

### **Deliverables Completed:**
- ✅ OpenROAD Flow Scripts installation
- ✅ Floorplan stage execution
- ✅ Placement stage execution  
- ✅ Visual evidence of both stages
- ✅ Challenge documentation

---

## 🚀 Implementation Steps

### **Step 1: Environment Setup**
```bash
# Clone OpenROAD Flow Scripts
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts

# Install dependencies and setup environment
./setup.sh
source env.sh
```

### **Step 2: Run Floorplan & Placement**
```bash
cd flow
make DESIGN_CONFIG=designs/nangate45/gcd/config.mk
```

### **Step 3: Verify Installation**
```bash
openroad -version
yosys -version
```

---

## 📊 Results & Evidence

### **Floorplan Stage Completed**
![floorplan_layout png](https://github.com/user-attachments/assets/8eda4bc6-b806-4370-9410-cea7a8d0675c)

*GCD design after floorplanning showing:*
- Core area boundaries
- I/O placement
- Power distribution network
- Standard cell rows created

### **Placement Stage Completed**  
![placement_layout png](https://github.com/user-attachments/assets/b86785ed-e74f-44f9-a204-0d2a8b5865f2)

*GCD design after placement showing:*
- All standard cells placed in rows
- Optimal cell distribution
- Routing channels prepared
- Placement density optimized

---

## 🔧 Technical Details

### **Design Specifications:**
- **Design:** GCD (Greatest Common Divisor)
- **Technology:** Nangate45 (45nm PDK)
- **Total Standard Cells:** 527
- **Die Area:** ~2000 µm²
- **Core Utilization:** ~35%

### **Flow Stages Executed:**
1. **Synthesis** - RTL to gate-level netlist
2. **Floorplan** - Chip dimensions and core area
3. **Placement** - Standard cell positioning


---

## ⚠️ Challenges & Solutions

### **Challenge 1: Build Dependencies**
**Issue:** Google Test linking errors during OpenROAD build  
**Solution:** Configured CMake with test disabling:
```bash
cmake .. -DBUILD_TESTING=OFF -DENABLE_TESTS=OFF
make openroad -j$(nproc)
```

### **Challenge 2: Tool Integration**
**Issue:** Flow expected tools in specific directories  
**Solution:** Created symbolic links to redirect to actual installations

### **Challenge 3: GUI Launch**
**Issue:** `make gui_final` target dependencies missing  
**Solution:** Manual OpenROAD launch with direct file loading

---

## 🎯 Key Learnings

### **Physical Design Insights:**
- Floorplanning determines chip dimensions and macro placement
- Placement optimizes standard cell positions for timing and routability
- Core utilization affects both performance and manufacturability
- Congestion analysis predicts routing challenges

### **Toolchain Experience:**
- OpenROAD provides complete RTL-to-GDSII automation
- Yosys handles RTL synthesis and technology mapping
- Makefile-driven flows enable reproducible builds
- GUI visualization essential for design debugging


## ✅ Completion Status

**Week 5 Requirements:** ✅ **FULLY COMPLETED**

- [x] OpenROAD Flow Scripts installed and verified
- [x] Floorplan stage executed - core area generated
- [x] Placement stage executed - all cells placed  
- [x] Visual evidence captured and documented
- [x] Challenges documented with solutions
- [x] Personal setup verified (terminal evidence)


**Tool Versions:** OpenROAD v2.0, Yosys 0.9, Nangate45 PDK  
**Platform:** Ubuntu Linux  
**Flow Stage:** Floorplan + Placement ✅ Complete
