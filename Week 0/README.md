
# RISC-V Reference SoC Tapeout Program VSD
## Participant: Rohn Eldho

This repository documents my progress through the VSD RISC-V SoC Tapeout Program, starting with the essential tools installation.

---

## Tools Installation
All instructions for installation of required tools are documented below.

### System Requirements
- **6 GB RAM**
- **50 GB HDD**
- **Ubuntu 20.04 LTS**
- **4 vCPU**

### Resizing the Ubuntu VM Window to Fit the Screen
```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
```
*Note: After installing the Guest Additions from the VirtualBox menu, run the installer from the virtual CD drive that appears.*

---

## TOOL CHECK

### 1. Yosys Installation
```bash
sudo apt-get update
git clone https://github.com/YosysHQ/yosys.git
cd yosys
sudo apt install make
sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
git submodule update --init --recursive
make
sudo make install
```

**Verification:**
```bash
yosys --version
```
<img width="745" height="601" alt="yosys" src="https://github.com/user-attachments/assets/cb4c0ac2-09a0-4f45-9d19-b90bfa9c1e85" />

*Figure 1: Successful installation and version check of Yosys.*

---

### 2. Icarus Verilog (Iverilog) Installation
```bash
sudo apt-get update
sudo apt-get install iverilog
```

**Verification:**
```bash
iverilog -v
```
<img width="744" height="568" alt="iverilog" src="https://github.com/user-attachments/assets/13a9c7c5-6c21-412a-92a1-697035f66b5d" />

*Figure 2: Successful installation and version check of Iverilog.*

---

### 3. GTKWave Installation
```bash
sudo apt-get update
sudo apt install gtkwave
```

**Verification:**
```bash
gtkwave --version
```
<img width="619" height="223" alt="gtkwave" src="https://github.com/user-attachments/assets/be225c67-cddc-4322-8f8d-f3caf51e3abc" />

*Figure 3: Successful installation and version check of GTKWave.*

---

### 4. Ngspice Installation
```bash
sudo apt-get install ngspice
```

**Verification:**
```bash
ngspice --version
```
<img width="619" height="223" alt="image" src="https://github.com/user-attachments/assets/92a3ffde-999c-4576-9aaa-89f4f1764917" />

*Figure 4: Successful installation and version check of Ngspice.*

---

### 5. Magic VLSI Layout Tool Installation
```bash
sudo apt-get install m4 tcsh csh libx11-dev tcl-dev tk-dev libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
make
sudo make install
```

**Verification:**
```bash
magic --version
```
<!-- INSERT SCREENSHOT OF MAGIC VERSION CHECK HERE -->
*Figure 5: Successful installation and version check of Magic.*

---

### 6. Docker & OpenLANE Installation
```bash
# Install Docker
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

# Add user to docker group
sudo groupadd docker
sudo usermod -aG docker $USER
# REBOOT or log out/in required here

# Install OpenLANE
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```

**Verification:**
```bash
docker --version
python3 --version
# Check OpenLANE test results
```
<!-- INSERT SCREENSHOT OF DOCKER VERSION CHECK HERE -->
*Figure 6: Successful installation of Docker.*

<!-- INSERT SCREENSHOT OF OPENLANE TEST PASS HERE -->
*Figure 7: Successful OpenLANE test run.*

---

## Summary
All tools required for the VSD RISC-V SoC Tapeout Program have been successfully installed and verified on an Ubuntu 20.04 LTS virtual machine. The environment is now ready for the RTL to GDSII flow.

**Status:** ✅ Complete
```

### How to use this template:
1.  Copy this entire text into a new file named `README.md` in your GitHub repository.
2.  As you install each tool, take a screenshot of the terminal showing the successful version check command (e.g., `yosys --version`).
3.  Save the screenshots with clear names (e.g., `yosys_ver.png`, `iverilog_ver.png`) in your repository.
4.  Replace the `<!-- INSERT SCREENSHOT... -->` comments in the README with the actual Markdown code to display your images:  
    `![Description of the image](./path/to/your/image.png)`
5.  Commit and push the updated `README.md` and your screenshot files to GitHub.

This will create a professional and comprehensive record of your setup process.
