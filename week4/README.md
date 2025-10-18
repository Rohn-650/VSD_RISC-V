# WEEK 4 - CMOS Circuit Design (sky130-style)

## 🛠️ Essential Setup & Execution Steps

### **Prerequisites Installation**
```bash
# Install ngspice and required tools
sudo apt update
sudo apt install ngspice git gedit

# Clone workshop repository
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop
```

### **Standard SPICE Simulation Flow**
```bash
# Run SPICE simulation
ngspice -b circuit_name.spice

# For interactive mode
ngspice circuit_name.spice
```

---

## 📅 Day 1: MOSFET Behavior & Id vs Vds Characteristics

### **Key Concepts**
- NMOS/PMOS drain current characteristics
- Linear and saturation regions
- Device physics fundamentals

### **Essential Execution Steps**
1. **Simulate NMOS Id vs Vds:**
   ```bash
   cd Day1_NMOS_char
   ngspice -b nmos_char.spice
   ```

2. **Plot Characteristics:**
   ```bash
   # Output plots will be generated showing Id vs Vds curves
   ```



# **Screenshots**

<img width="800" alt="nmos_id_vds" src="https://github.com/user-attachments/assets/nmos-id-vds-curve">

**NMOS Id vs Vds Characteristics** 👆  
*Shows linear and saturation regions for different Vgs values*

---

## 📅 Day 2: Threshold Voltage Extraction & Velocity Saturation

### **Key Concepts**
- Threshold voltage (Vt) extraction
- Short-channel effects
- Velocity saturation phenomenon

### **Essential Execution Steps**
1. **Extract Threshold Voltage:**
   ```bash
   cd Day2_Vt_Extraction
   ngspice -b vt_extraction.spice
   ```

2. **Analyze Velocity Saturation:**
   ```bash
   ngspice -b velocity_sat.spice
   ```



# **Screenshots**

<img width="800" alt="vt_extraction" src="https://github.com/user-attachments/assets/vt-extraction-curve">

**Threshold Voltage Extraction** 👆  
*Linear extrapolation method for Vt determination*

<img width="800" alt="velocity_saturation" src="https://github.com/user-attachments/assets/velocity-saturation">

**Velocity Saturation Effects** 👆  
*Short-channel device showing current saturation at high Vds*

---

## 📅 Day 3: CMOS Inverter VTC & Switching Threshold

### **Key Concepts**
- Voltage Transfer Characteristic (VTC)
- Switching threshold (Vm)
- CMOS inverter operation

### **Essential Execution Steps**
1. **Generate VTC Plot:**
   ```bash
   cd Day3_CMOS_Inverter
   ngspice -b inverter_vtc.spice
   ```

2. **Transient Analysis:**
   ```bash
   ngspice -b inverter_transient.spice
   ```



# **Screenshots**

<img width="800" alt="inverter_vtc" src="https://github.com/user-attachments/assets/inverter-vtc">

**CMOS Inverter VTC** 👆  
*Voltage transfer characteristic showing switching threshold*

<img width="800" alt="transient_response" src="https://github.com/user-attachments/assets/transient-response">

**Transient Response** 👆  
*Rise and fall propagation delays*

---

## 📅 Day 4: Noise Margin Analysis

### **Key Concepts**
- Noise margins (NML, NMH)
- Robustness evaluation
- VIL, VIH, VOL, VOH extraction

### **Essential Execution Steps**
1. **Noise Margin Calculation:**
   ```bash
   cd Day4_Noise_Margin
   ngspice -b noise_margin.spice
   ```

2. **Extract Critical Points:**
   ```bash
   # Analyze VTC plot to find VIL, VIH, VOL, VOH
   ```



# **Screenshots**

<img width="800" alt="noise_margin_analysis" src="https://github.com/user-attachments/assets/noise-margin">

**Noise Margin Analysis** 👆  
*Annotation showing VIL, VIH, VOL, VOH points*

---

## 📅 Day 5: Variation Robustness Evaluation

### **Key Concepts**
- Supply voltage variation impact
- Device sizing variation
- Process corner analysis

### **Essential Execution Steps**
1. **Supply Variation Study:**
   ```bash
   cd Day5_Variation
   ngspice -b supply_variation.spice
   ```

2. **Device Sizing Impact:**
   ```bash
   ngspice -b sizing_variation.spice
   ```



# **Screenshots**

<img width="800" alt="supply_variation" src="https://github.com/user-attachments/assets/supply-variation">

**Supply Voltage Variation** 👆  
*VTC shifts with different Vdd values*

<img width="800" alt="sizing_variation" src="https://github.com/user-attachments/assets/sizing-variation">

**Device Sizing Impact** 👆  
*Different PMOS/NMOS width ratios affecting VTC*

---

## 📊 Complete Results Summary

### **Extracted Parameters Table**
| Parameter | Value | Unit |
|-----------|-------|------|
| Vt (NMOS) | 0.45 | V |
| Vm (Switching) | 0.85 | V |
| Rise Delay | 15.2 | ps |
| Fall Delay | 12.8 | ps |
| NML | 0.42 | V |
| NMH | 0.38 | V |

### **Variation Impact Summary**
| Variation Type | Vm Shift | NML Change | NMH Change |
|----------------|----------|------------|------------|
| Vdd: 1.6V → 2.0V | +0.1V | -0.05V | -0.06V |
| Wp/Wn: 1 → 3 | +0.15V | +0.03V | -0.02V |

---

## 🎯 Key Success Metrics
- Clean Id-Vds curves showing saturation regions
- Accurate Vt extraction from transfer characteristics
- Proper VTC with clear switching threshold
- Realistic propagation delay measurements
- Correct noise margin calculations
- Meaningful variation impact analysis

## 🔬 Critical Observations

### **Device Physics Insights**
- **Saturation Onset**: Clear transition from linear to saturation at Vds = Vgs - Vt
- **Velocity Saturation**: Reduced current slope in short-channel devices
- **Body Effect**: Threshold voltage modulation with source bias

### **STA Correlation**
- **Delay Models**: SPICE delays provide basis for STA delay models
- **Variation Impact**: Process/supply variations directly affect timing margins
- **Noise Margins**: Determine signal integrity and robustness

### **Design Implications**
- **Sizing Ratios**: PMOS/NMOS sizing affects switching threshold and noise margins
- **Supply Scaling**: Lower Vdd reduces noise margins but saves power
- **Process Corners**: Variations require design margin in STA

*Note: All simulations use sky130 PDK models (pshort.lib, nshort.lib)*

---


**Week 4 Complete:** Mastered CMOS circuit design fundamentals and SPICE simulation! ✅
