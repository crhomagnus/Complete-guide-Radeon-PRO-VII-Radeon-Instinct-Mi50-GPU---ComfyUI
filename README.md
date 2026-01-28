# 🎨 COMPLETE GUIDE: ComfyUI + ROCm on AMD Radeon Pro VII


![ComfyUI + ROCm on AMD Radeon Pro VII](assets_v2/cover.png)


<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════╗
║                                                                          ║
║          🚀 COMPLETE AND DETAILED COMFYUI INSTALLATION                   ║
║              WITH ROCm SUPPORT FOR AMD RADEON PRO VII                    ║
║                                                                          ║
║                    ⚡ Tested and Approved Versions                        ║
║                       ✅ January 2026                                     ║
║                                                                          ║
╚══════════════════════════════════════════════════════════════════════════╝
```

</div>

---

## 📋 TABLE OF CONTENTS

1. [🔍 Overview](#-overview)
2. [⚙️ Technical Specifications](#️-technical-specifications)
3. [🛠️ System Prerequisites](#️-system-prerequisites)
4. [📦 Step-by-Step Installation](#-step-by-step-installation)
5. [🎯 Critical Configuration](#-critical-configuration)
6. [🚀 Starting ComfyUI](#-starting-comfyui)
7. [⚠️ Common Issues and Fixes](#️-common-issues-and-fixes)
8. [📊 Benchmark and Performance](#-benchmark-and-performance)
9. [🔧 Maintenance and Updates](#-maintenance-and-updates)

---

## 🔍 OVERVIEW

This guide documents a **COMPLETE** and **WORKING** installation of **ComfyUI v0.11.0** with GPU acceleration using **ROCm 6.16.6** on **AMD Radeon Pro VII (gfx906 / Vega 20)**.

### ✅ Tested and Approved Setup

This installation was tested and is **100% WORKING** with the following exact setup:

| Component | Exact Version | Status |
|----------|---------------|--------|
| Operating System | Ubuntu 24.04.3 LTS (Noble) | ✅ Approved |
| Linux Kernel | 6.14.0-37-generic | ✅ Approved |
| GPU | AMD Radeon Pro VII | ✅ Approved |
| VBIOS | 113-D1640600-104 | ✅ **CRITICAL** |
| ROCm | 6.16.6 | ✅ Approved |
| Python | 3.12.3 | ✅ Approved |
| ComfyUI | 0.11.0 | ✅ Approved |
| PyTorch | 2.5.1+rocm6.2 | ✅ Approved |
| torchvision | 0.20.1+rocm6.2 | ✅ Approved |
| torchaudio | 2.5.1+rocm6.2 | ✅ Approved |

![Installation flow](assets_v2/installation_flow.png)

---

## ⚙️ TECHNICAL SPECIFICATIONS

### 🖥️ Required Hardware

#### GPU: AMD Radeon Pro VII

```
┌─────────────────────────────────────────────────────────────┐
│  📊 AMD RADEON PRO VII SPECIFICATIONS                        │
├─────────────────────────────────────────────────────────────┤
│  Architecture:        GCN 5.0 (Vega 20)                      │
│  GPU Code:            gfx906                                  │
│  Compute Units:       60 CUs                                  │
│  Stream Processors:   3840                                    │
│  Memory:              16 GB HBM2                              │
│  Bandwidth:           1 TB/s                                  │
│  TDP:                 300W                                    │
│  Frequency:           1700 MHz (boost)                        │
│  Required VBIOS:      113-D1640600-104 or newer               │
└─────────────────────────────────────────────────────────────┘
```

#### ⚠️ **IMPORTANT: VBIOS VERSION**

```
╔═══════════════════════════════════════════════════════════════╗
║  ⚠️  CRITICAL: VBIOS VERSION                                  ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  Your Radeon Pro VII VBIOS MUST be up to date!                ║
║                                                               ║
║  ✅ MIN VERSION:  113-D1640600-100                             ║
║  ✅ TESTED:       113-D1640600-104                             ║
║                                                               ║
║  📌 How to check:                                             ║
║     rocm-smi --showvbios                                      ║
║                                                               ║
║  ❌ An older VBIOS may cause:                                 ║
║     • Instability                                             ║
║     • Freezes / lockups                                       ║
║     • Memory errors                                           ║
║     • GPU not detected                                        ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 💻 Minimum System Requirements

| Item | Minimum | Recommended |
|------|---------|-------------|
| CPU | 4 cores | 6+ cores |
| RAM | 16 GB | 32 GB or more |
| Storage | 50 GB free | 200 GB+ SSD |
| Network | Stable internet | High-speed internet |

---

## 🛠️ SYSTEM PREREQUISITES

### 1️⃣ Operating System

```
╔═══════════════════════════════════════════════════════════════╗
║  💿 OPERATING SYSTEM                                          ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  ✅ SUPPORTED:                                                ║
║     • Ubuntu 24.04 LTS (Noble) - RECOMMENDED                  ║
║     • Ubuntu 22.04 LTS (Jammy)                                ║
║     • Ubuntu 20.04 LTS (Focal)                                ║
║                                                               ║
║  ⚠️  WITH LIMITATIONS:                                        ║
║     • Fedora 39/40                                            ║
║     • RHEL 9.x / Rocky Linux 9.x                              ║
║                                                               ║
║  ❌ NOT SUPPORTED:                                            ║
║     • Windows 10/11 (ROCm limited for gfx906)                 ║
║     • macOS (no AMD ROCm)                                     ║
║     • Non Debian/RPM-based distributions                      ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

**📌 Check your versions:**

![Terminal](assets_v2/term_001.png)
```bash
lsb_release -a
uname -r
```

**Expected output:**

![Output](assets_v2/term_002.png)
```
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.3 LTS
Release:        24.04
Codename:       noble
6.14.0-37-generic
```

---

### 2️⃣ Installing ROCm

ROCm (Radeon Open Compute) is AMD’s heterogeneous computing platform, similar in purpose to NVIDIA CUDA.

```
╔═══════════════════════════════════════════════════════════════╗
║  🔥 ROCm INSTALLATION                                         ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  ✅ TESTED:      ROCm 6.16.6                                  ║
║  ✅ COMPATIBLE:  ROCm 6.0.x - 6.2.x                           ║
║                                                               ║
║  ⚠️  NOTE: ROCm 7.x has LIMITED support for gfx906!           ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 📥 **STEP 1: Download the ROCm Installer**

![Terminal](assets_v2/term_003.png)
```bash
# Create a temporary directory
cd ~/Downloads

# Download ROCm 6.2 installer (compatible with 6.16.6)
wget https://repo.radeon.com/amdgpu-install/6.2.4/ubuntu/noble/amdgpu-install_6.2.60204-1_all.deb
```

#### 📦 **STEP 2: Add ROCm Repository**

![Terminal](assets_v2/term_004.png)
```bash
# Install the installer package
sudo dpkg -i amdgpu-install_6.2.60204-1_all.deb

# Update repositories
sudo apt update
```

#### ⚡ **STEP 3: Install Full ROCm Stack**

![Terminal](assets_v2/term_005.png)
```bash
# Install ROCm with drivers and tools
sudo amdgpu-install --usecase=graphics,rocm,hip,hiplibsdk

# This installs:
# ✅ AMDGPU drivers
# ✅ ROCm runtime
# ✅ HIP (CUDA-like interface)
# ✅ Development libraries
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⏰ ESTIMATED TIME: 10–20 minutes                             ║
║  📦 REQUIRED SPACE: ~5 GB                                     ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 👥 **STEP 4: Configure User Permissions**

![Terminal](assets_v2/term_006.png)
```bash
# Add your user to required groups
sudo usermod -a -G render,video $USER

# IMPORTANT: log out and log back in to apply changes
# or reboot the system
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⚠️  CRITICAL: LOGOUT/LOGIN REQUIRED                          ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  After adding your user to 'render' and 'video',              ║
║  you MUST log out and log back in (or reboot).                ║
║                                                               ║
║  ❌ Without this: the GPU will not be accessible!             ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### ✅ **STEP 5: Verify ROCm Installation**

After reboot / logout-login:

![Terminal](assets_v2/term_007.png)
```bash
# Check ROCm version
rocminfo --version

# Check GPU detection
rocminfo | grep -A 5 "Marketing Name"

# Check GPU status
rocm-smi

# Check VBIOS
rocm-smi --showvbios
```

**Expected `rocm-smi` output:**

![Output](assets_v2/term_008.png)
```
======================== ROCm System Management Interface ========================
================================== Concise Info ==================================
Device  [Model : Revision]    Temp    Power    Partitions      SCLK     MCLK
        Name (20 chars)       (Edge)  (Socket)  (Mem, Compute)
===================================================================================
0       [0x66a1 : 0x00]       32.0°C  35.0W     N/A, N/A        852Mhz   800Mhz
        AMD Radeon (TM)
        Pro VII
===================================================================================
```

**Expected VBIOS output:**

![Output](assets_v2/term_009.png)
```
========================================= VBIOS ==========================================
GPU[0]		: VBIOS version: 113-D1640600-104
```

---

### 3️⃣ Installing Python and Tools

```
╔═══════════════════════════════════════════════════════════════╗
║  🐍 PYTHON AND DEVELOPMENT TOOLS                              ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  ✅ PYTHON VERSION: 3.12.3                                    ║
║  ✅ COMPATIBLE:     Python 3.10, 3.11, 3.12                    ║
║  ❌ AVOID:          Python 3.9 or older                        ║
║  ❌ AVOID:          Python 3.13 (not fully supported yet)      ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 📦 **Install System Dependencies**

![Terminal](assets_v2/term_010.png)
```bash
# Update the system
sudo apt update && sudo apt upgrade -y

# Install Python and essential tools
sudo apt install -y \
    python3 \
    python3-pip \
    python3-venv \
    python3-dev \
    python3-full \
    build-essential \
    git \
    wget \
    curl \
    libgl1-mesa-glx \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender-dev \
    libgomp1 \
    ffmpeg \
    libavcodec-dev \
    libavformat-dev \
    libswscale-dev
```

```
╔═══════════════════════════════════════════════════════════════╗
║  📝 PACKAGE NOTES                                             ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  python3-venv      → Create virtual environments              ║
║  python3-dev       → Headers for compilation                  ║
║  build-essential   → C/C++ compilers                          ║
║  git               → Version control                          ║
║  libgl1-mesa-glx   → OpenGL (for OpenCV)                      ║
║  libgomp1          → OpenMP (parallelization)                 ║
║  ffmpeg            → Video processing                         ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### ✅ **Verify Python**

![Terminal](assets_v2/term_011.png)
```bash
python3 --version
pip3 --version
```

**Expected output:**

![Output](assets_v2/term_012.png)
```
Python 3.12.3
pip 24.0 from /usr/lib/python3/dist-packages/pip (python 3.12)
```

---

## 📦 STEP-BY-STEP INSTALLATION

### STAGE 1: Create a Virtual Environment

```
╔═══════════════════════════════════════════════════════════════╗
║  🔒 WHY A VIRTUAL ENVIRONMENT MATTERS                         ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  ✅ BENEFITS:                                                 ║
║     • Isolates project dependencies                           ║
║     • Avoids package conflicts                                ║
║     • Easier updates and maintenance                           ║
║     • Multiple package versions possible                      ║
║                                                               ║
║  ❌ DO NOT INSTALL PACKAGES GLOBALLY!                         ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 📁 **STEP 1.1: Create a Directory for Virtual Envs**

![Terminal](assets_v2/term_013.png)
```bash
# Create a folder for virtual envs (if it doesn't exist)
mkdir -p ~/.venvs

# Go to your home directory
cd ~
```

#### 🔨 **STEP 1.2: Create a Virtual Env for ComfyUI**

![Terminal](assets_v2/term_014.png)
```bash
# Create a virtual environment called "comfyui-env"
python3 -m venv ~/.venvs/comfyui-env
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⏰ This takes ~30 seconds                                     ║
╚═══════════════════════════════════════════════════════════════╝
```

#### ⚡ **STEP 1.3: Activate the Virtual Env**

![Terminal](assets_v2/term_015.png)
```bash
# Activate the virtual environment
source ~/.venvs/comfyui-env/bin/activate
```

**How to confirm it is active?**

Your terminal prompt should change to something like:

![Output](assets_v2/term_016.png)
```
(comfyui-env) user@machine:~$
```

#### 📦 **STEP 1.4: Update pip**

![Terminal](assets_v2/term_017.png)
```bash
# Update pip to the latest version
pip install --upgrade pip

# Check version
pip --version
```

**Expected output:**

![Output](assets_v2/term_018.png)
```
pip 25.3 from /home/user/.venvs/comfyui-env/lib/python3.12/site-packages/pip (python 3.12)
```

---

### STAGE 2: Install PyTorch with ROCm

This is the **MOST CRITICAL** step of the entire installation.

```
╔═══════════════════════════════════════════════════════════════╗
║  🔥 PYTORCH INSTALLATION - CRITICAL                           ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  ⚠️  PAY CLOSE ATTENTION HERE!                                ║
║                                                               ║
║  ✅ CORRECT VERSIONS:                                         ║
║     • PyTorch:      2.5.1+rocm6.2                             ║
║     • torchvision:  0.20.1+rocm6.2                            ║
║     • torchaudio:   2.5.1+rocm6.2                             ║
║                                                               ║
║  ❌ DO NOT INSTALL:                                           ║
║     • Non-ROCm PyTorch (CUDA build)                           ║
║     • CPU-only PyTorch                                        ║
║     • Old versions                                            ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 🔥 **STEP 2.1: Install PyTorch for ROCm 6.2**

Make sure your virtual environment is active (you should see `(comfyui-env)` in the prompt):

![Terminal](assets_v2/term_019.png)
```bash
# Install PyTorch, torchvision and torchaudio with ROCm 6.2 support
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm6.2
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⏰ ESTIMATED TIME: 15–30 minutes                             ║
║  📦 DOWNLOAD SIZE: ~4.5 GB                                    ║
║  💾 REQUIRED SPACE: ~8 GB                                     ║
║                                                               ║
║  ☕ This download is LARGE.                                   ║
╚═══════════════════════════════════════════════════════════════╝
```

#### ✅ **STEP 2.2: Verify PyTorch Install**

![Terminal](assets_v2/term_020.png)
```bash
python3 -c "import torch; print(f'PyTorch: {torch.__version__}'); print(f'ROCm: {torch.version.hip}')"
```

**Expected output:**

![Output](assets_v2/term_021.png)
```
PyTorch: 2.5.1+rocm6.2
ROCm: 6.2.41133-dd7f95766
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⚠️  IF YOUR OUTPUT IS DIFFERENT:                             ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  If you do not see “+rocm6.2”, you installed the WRONG build  ║
║  (typically CPU-only or CUDA).                                ║
║                                                               ║
║  🔄 FIX:                                                      ║
║                                                               ║
║  pip uninstall torch torchvision torchaudio -y                ║
║  pip install torch torchvision torchaudio \                   ║
║       --index-url https://download.pytorch.org/whl/rocm6.2    ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

### STAGE 3: Clone and Prepare ComfyUI

#### 📥 **STEP 3.1: Clone the ComfyUI Repository**

![Terminal](assets_v2/term_022.png)
```bash
# Go to your home directory
cd ~

# Clone ComfyUI from GitHub
git clone https://github.com/comfyanonymous/ComfyUI.git

# Enter the directory
cd ComfyUI
```

#### 📌 **STEP 3.2: Check Version (Optional but Recommended)**

![Terminal](assets_v2/term_023.png)
```bash
# Show current commit
git log --oneline -1

# Show available tags
git tag -l | tail -10
```

```
╔═══════════════════════════════════════════════════════════════╗
║  💡 TIP: USE A SPECIFIC VERSION                               ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  To use the tested version (v0.11.0):                         ║
║                                                               ║
║  git checkout v0.11.0                                         ║
║                                                               ║
║  To go back to latest:                                        ║
║                                                               ║
║  git checkout master                                          ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 📦 **STEP 3.3: Install ComfyUI Dependencies**

![Terminal](assets_v2/term_024.png)
```bash
# Make sure you're in ~/ComfyUI and the venv is active
cd ~/ComfyUI
source ~/.venvs/comfyui-env/bin/activate

# Install all dependencies
pip install -r requirements.txt
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⏰ ESTIMATED TIME: 5–10 minutes                              ║
║  📦 MAIN DEPENDENCIES INSTALLED:                              ║
║     • safetensors (0.7.0)                                     ║
║     • transformers (4.57.6)                                   ║
║     • diffusers (0.36.0)                                      ║
║     • accelerate (1.12.0)                                     ║
║     • opencv-python (4.11.0)                                  ║
║     • scipy (1.17.0)                                          ║
║     • einops (0.8.1)                                          ║
║     • Pillow (12.1.0)                                         ║
║     • numpy (2.3.5)                                           ║
╚═══════════════════════════════════════════════════════════════╝
```

#### ✅ **STEP 3.4: Verify Installed Dependencies**

![Terminal](assets_v2/term_025.png)
```bash
pip list | grep -E "(torch|safetensors|transformers|diffusers|opencv)"
```

**Expected output:**

![Output](assets_v2/term_026.png)
```
diffusers                 0.36.0
opencv-python             4.11.0
safetensors               0.7.0
torch                     2.5.1+rocm6.2
torchaudio                2.5.1+rocm6.2
torchvision               0.20.1+rocm6.2
transformers              4.57.6
```

---

## 🎯 CRITICAL CONFIGURATION

### Configure HSA_OVERRIDE_GFX_VERSION

This is the **MOST IMPORTANT** configuration to make ComfyUI work on Radeon Pro VII.

```
╔═══════════════════════════════════════════════════════════════╗
║  🚨 CRITICAL CONFIGURATION - READ CAREFULLY                   ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  Radeon Pro VII (gfx906) is not a primary ROCm target in      ║
║  ROCm 6.x / 7.x. To make PyTorch recognize the GPU, we        ║
║  “spoof” the architecture as gfx900.                          ║
║                                                               ║
║  ⚡ MAGIC VARIABLE: HSA_OVERRIDE_GFX_VERSION=9.0.0            ║
║                                                               ║
║  ❌ WITHOUT IT:                                               ║
║     • torch.cuda.is_available() returns False                 ║
║     • GPU is not detected                                     ║
║     • ComfyUI runs on CPU only (VERY SLOW)                    ║
║                                                               ║
║  ✅ WITH IT:                                                  ║
║     • GPU detected correctly ✓                                ║
║     • Hardware acceleration ✓                                 ║
║     • Maximum performance ✓                                   ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### 🔧 **STEP 4.1: Set the Variable Permanently**

**METHOD 1: Add to `.bashrc` (Recommended)**

![Terminal](assets_v2/term_027.png)
```bash
# Open .bashrc
nano ~/.bashrc
```

Scroll to the bottom and add:

![Terminal](assets_v2/term_028.png)
```bash
# ══════════════════════════════════════════════════════════════
# ROCm - AMD Radeon Pro VII (gfx906) configuration
# ══════════════════════════════════════════════════════════════

# Critical compatibility variable for gfx906
export HSA_OVERRIDE_GFX_VERSION=9.0.0

# Auto-activate the ComfyUI venv (OPTIONAL)
# Uncomment the line below if you want that:
# source ~/.venvs/comfyui-env/bin/activate
```

Save and exit:
- `Ctrl + O` (save)
- `Enter` (confirm)
- `Ctrl + X` (exit)

Apply changes:

![Terminal](assets_v2/term_029.png)
```bash
# Reload .bashrc
source ~/.bashrc

# Confirm variable
echo $HSA_OVERRIDE_GFX_VERSION
```

Expected output:

![Output](assets_v2/term_030.png)
```
9.0.0
```

---

**METHOD 2: Add to `/etc/environment` (System-wide)**

![Terminal](assets_v2/term_031.png)
```bash
# Edit global environment file (requires sudo)
sudo nano /etc/environment
```

Add:

![Output](assets_v2/term_032.png)
```
HSA_OVERRIDE_GFX_VERSION="9.0.0"
```

Save and reboot.

```
╔═══════════════════════════════════════════════════════════════╗
║  💡 WHICH METHOD SHOULD YOU USE?                              ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  📌 Use .bashrc if:                                           ║
║     • You are the only ROCm user                              ║
║     • You want per-user configuration                          ║
║                                                               ║
║  📌 Use /etc/environment if:                                  ║
║     • Multiple users need ROCm                                ║
║     • You want system-wide configuration                      ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### ✅ **STEP 4.2: Test GPU Detection**

![Terminal](assets_v2/term_033.png)
```bash
# Activate venv
source ~/.venvs/comfyui-env/bin/activate

# Set the variable (if not in .bashrc yet)
export HSA_OVERRIDE_GFX_VERSION=9.0.0

# Test GPU detection
python3 -c "import torch; print(f'GPU available: {torch.cuda.is_available()}'); print(f'Name: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else \"N/A\"}'); print(f'Memory: {torch.cuda.get_device_properties(0).total_memory / (1024**3):.2f} GB' if torch.cuda.is_available() else '')"
```

**Expected output:**

![Output](assets_v2/term_034.png)
```
GPU available: True
Name: AMD Radeon (TM) Pro VII
Memory: 15.98 GB
```

```
╔═══════════════════════════════════════════════════════════════╗
║  🎉 If you saw "GPU available: True" — you're good to go!     ║
║                                                               ║
║  ❌ If you saw "False", check:                                ║
║     1) HSA_OVERRIDE_GFX_VERSION is set                         ║
║     2) ROCm is installed (rocminfo)                            ║
║     3) Your user is in 'render' and 'video' groups            ║
║     4) Reboot or log out/in                                   ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## 🚀 STARTING COMFYUI

### 🎬 First Run

#### 📝 **STEP 5.1: Startup Script (Recommended)**

Create a helper script to launch ComfyUI consistently:

![Terminal](assets_v2/term_035.png)
```bash
# Create startup script
nano ~/start_comfyui.sh
```

Paste:

![Terminal](assets_v2/term_036.png)
```bash
#!/bin/bash

# ══════════════════════════════════════════════════════════════
# ComfyUI Startup Script
# AMD Radeon Pro VII (gfx906) + ROCm
# ══════════════════════════════════════════════════════════════

clear

echo "╔══════════════════════════════════════════════════════════════╗"
echo "║                                                              ║"
echo "║           🎨 STARTING COMFYUI WITH ROCm                       ║"
echo "║              AMD Radeon Pro VII                               ║"
echo "║                                                              ║"
echo "╚══════════════════════════════════════════════════════════════╝"
echo ""

# Environment variable for gfx906 compatibility
export HSA_OVERRIDE_GFX_VERSION=9.0.0

# Activate venv
echo "📦 Activating virtual environment..."
source ~/.venvs/comfyui-env/bin/activate

# Verify GPU
echo "🔍 Checking GPU..."
python3 -c "import torch; print(f'✅ GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else \"❌ Not detected\"}')"

# Go to ComfyUI directory
cd ~/ComfyUI

echo ""
echo "🚀 Starting ComfyUI server..."
echo "📡 URL: http://localhost:8188"
echo ""
echo "⚠️  Press Ctrl+C to stop the server"
echo ""
echo "══════════════════════════════════════════════════════════════"
echo ""

# Launch ComfyUI
python3 main.py --listen 0.0.0.0 --port 8188
```

Save and set executable permission:

![Terminal](assets_v2/term_037.png)
```bash
chmod +x ~/start_comfyui.sh
```

#### 🚀 **STEP 5.2: Launch ComfyUI**

**OPTION A: Using the script (Recommended)**

![Terminal](assets_v2/term_038.png)
```bash
~/start_comfyui.sh
```

**OPTION B: Manually**

![Terminal](assets_v2/term_039.png)
```bash
# Activate venv
source ~/.venvs/comfyui-env/bin/activate

# Set variable
export HSA_OVERRIDE_GFX_VERSION=9.0.0

# Go to ComfyUI
cd ~/ComfyUI

# Start server
python3 main.py --listen 0.0.0.0 --port 8188
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⏰ FIRST START - BE PATIENT                                  ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  The first time you run ComfyUI, it may:                      ║
║                                                               ║
║  1) ⏳ Load models (1–3 minutes)                               ║
║  2) 🔍 Scan directories                                        ║
║  3) 🔨 Initialize ROCm/HIP                                     ║
║  4) 📡 Start the web server                                    ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

#### ✅ **STEP 5.3: Confirm It’s Working**

When you see these lines in the logs, the server is ready:

![Output](assets_v2/term_040.png)
```
Starting server
To see the GUI go to: http://0.0.0.0:8188
```

Test locally:

![Terminal](assets_v2/term_041.png)
```bash
# In another terminal
curl http://localhost:8188
```

If you get HTML back, it’s working.

#### 🌐 **STEP 5.4: Open the Web UI**

**Option 1: From the terminal**

![Terminal](assets_v2/term_042.png)
```bash
chromium http://localhost:8188 &
```

**Option 2: Manually**

Open:

![Output](assets_v2/term_043.png)
```
http://localhost:8188
```

```
╔═══════════════════════════════════════════════════════════════╗
║  🎉 CONGRATS! COMFYUI IS RUNNING                               ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  You should see the ComfyUI interface with:                   ║
║                                                               ║
║  ✅ An example workflow loaded                                ║
║  ✅ Nodes in the side menu                                    ║
║  ✅ “Queue Prompt” button working                             ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

### 🎯 Command-line Parameters

ComfyUI supports many useful arguments:

| Parameter | Description | Example |
|----------|-------------|---------|
| `--listen` | Bind address | `--listen 0.0.0.0` (all interfaces) |
| `--port` | Server port | `--port 8188` |
| `--cpu` | Force CPU | `--cpu` |
| `--preview-method` | Preview strategy | `--preview-method auto` |
| `--use-split-cross-attention` | Reduce VRAM | `--use-split-cross-attention` |
| `--normalvram` | Normal VRAM | `--normalvram` |
| `--lowvram` | Low VRAM | `--lowvram` |
| `--novram` | No VRAM | `--novram` |

**Examples:**

![Terminal](assets_v2/term_044.png)
```bash
# Remote access (allow other PCs on the LAN)
python3 main.py --listen 0.0.0.0 --port 8188

# Save VRAM (for large models)
python3 main.py --use-split-cross-attention

# Low VRAM mode (if you hit OOM)
python3 main.py --lowvram

# Combined (maximum saving)
python3 main.py --lowvram --use-split-cross-attention
```

```
╔═══════════════════════════════════════════════════════════════╗
║  💡 WHICH MODE SHOULD YOU USE?                                ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  --normalvram (DEFAULT)                                       ║
║  • Use for small/medium models                                 ║
║  • Best performance                                            ║
║  • Recommended for Radeon Pro VII (16GB)                       ║
║                                                               ║
║  --use-split-cross-attention                                   ║
║  • Use for large models (SDXL, etc)                            ║
║  • Small performance hit                                       ║
║  • Saves ~20–30% VRAM                                          ║
║                                                               ║
║  --lowvram                                                     ║
║  • Use ONLY if you get OOM                                     ║
║  • Significant speed loss                                      ║
║  • Last resort                                                  ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## ⚠️ COMMON ISSUES AND FIXES

### 🔴 Issue 1: GPU Not Detected

**Symptom:**
```python
>>> import torch
>>> torch.cuda.is_available()
False
```

```
╔═══════════════════════════════════════════════════════════════╗
║  🔧 FIXES FOR “GPU NOT DETECTED”                              ║
╚═══════════════════════════════════════════════════════════════╝
```

**✅ Fix 1: Verify HSA_OVERRIDE_GFX_VERSION**

![Terminal](assets_v2/term_045.png)
```bash
# Show current value
echo $HSA_OVERRIDE_GFX_VERSION

# If it's not "9.0.0", set it:
export HSA_OVERRIDE_GFX_VERSION=9.0.0

# Persist in .bashrc
echo 'export HSA_OVERRIDE_GFX_VERSION=9.0.0' >> ~/.bashrc
source ~/.bashrc
```

**✅ Fix 2: Verify User Groups**

![Terminal](assets_v2/term_046.png)
```bash
# List your groups
groups

# You must see 'render' and 'video'
# If you don't, add yourself:
sudo usermod -a -G render,video $USER

# IMPORTANT: log out/in or reboot
```

**✅ Fix 3: Verify ROCm**

![Terminal](assets_v2/term_047.png)
```bash
# Check if ROCm sees the GPU
rocminfo | grep -A 5 "AMD Radeon"

# If it shows nothing, reinstall ROCm:
sudo amdgpu-install --usecase=rocm,hip
```

**✅ Fix 4: Verify PyTorch Build**

![Terminal](assets_v2/term_048.png)
```bash
# Check torch build
pip show torch

# If it does not contain "+rocm6.2", reinstall:
pip uninstall torch torchvision torchaudio -y
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm6.2
```

---

### 🔴 Issue 2: “HIP error: invalid device function”

**Symptom:**

![Output](assets_v2/term_049.png)
```
RuntimeError: HIP error: invalid device function
```

```
╔═══════════════════════════════════════════════════════════════╗
║  🔧 CAUSE: HSA_OVERRIDE_GFX_VERSION not set                   ║
╚═══════════════════════════════════════════════════════════════╝
```

**✅ Fix:**

![Terminal](assets_v2/term_050.png)
```bash
export HSA_OVERRIDE_GFX_VERSION=9.0.0

cd ~/ComfyUI
source ~/.venvs/comfyui-env/bin/activate
python3 main.py --listen 0.0.0.0 --port 8188
```

---

### 🔴 Issue 3: Out of Memory (OOM)

**Symptom:**

![Output](assets_v2/term_051.png)
```
RuntimeError: HIP out of memory
```

```
╔═══════════════════════════════════════════════════════════════╗
║  🔧 FIXES FOR OOM                                             ║
╚═══════════════════════════════════════════════════════════════╝
```

**✅ Fix 1: Use Low VRAM Mode**

![Terminal](assets_v2/term_052.png)
```bash
python3 main.py --lowvram
```

**✅ Fix 2: Use Split Attention**

![Terminal](assets_v2/term_053.png)
```bash
python3 main.py --use-split-cross-attention
```

**✅ Fix 3: Clear PyTorch Cache**

```python
import torch
torch.cuda.empty_cache()
```

**✅ Fix 4: Use Smaller Models**

```
• Stable Diffusion 1.5  → ~2 GB VRAM
• Stable Diffusion 2.1  → ~3 GB VRAM
• SDXL                  → ~6–8 GB VRAM
• SDXL + refiner        → ~10–12 GB VRAM
```

---

### 🔴 Issue 4: ComfyUI Is Very Slow

**Symptom:**
Image generation takes minutes

```
╔═══════════════════════════════════════════════════════════════╗
║  🔧 DIAGNOSIS AND OPTIMIZATION                                ║
╚═══════════════════════════════════════════════════════════════╝
```

**🔍 Step 1: Confirm it’s using the GPU**

![Terminal](assets_v2/term_054.png)
```bash
# During generation, in another terminal:
rocm-smi

# You should see GPU usage > 0% and temps increasing
```

**✅ Fix 1: GPU is not being used**

![Terminal](assets_v2/term_055.png)
```bash
python3 -c "import torch; print(torch.cuda.is_available())"
```

If it returns `False`, follow Issue 1.

**✅ Fix 2: Disable Real-time Preview**

In ComfyUI: Settings → Preview Method = `none` or `latent2rgb`

**✅ Fix 3: Use optimized launch flags**

![Terminal](assets_v2/term_056.png)
```bash
python3 main.py \
  --normalvram \
  --preview-method latent2rgb \
  --listen 0.0.0.0 \
  --port 8188
```

---

### 🔴 Issue 5: Import Error “No module named 'torch'”

**Symptom:**

![Output](assets_v2/term_057.png)
```
ModuleNotFoundError: No module named 'torch'
```

```
╔═══════════════════════════════════════════════════════════════╗
║  🔧 CAUSE: Virtual env not activated                          ║
╚═══════════════════════════════════════════════════════════════╝
```

**✅ Fix:**

![Terminal](assets_v2/term_058.png)
```bash
source ~/.venvs/comfyui-env/bin/activate

which python3
```

Expected:

![Output](assets_v2/term_059.png)
```
/home/user/.venvs/comfyui-env/bin/python3
```

---

### 🔴 Issue 6: “Permission Denied” When Accessing the GPU

**Symptom:**

![Output](assets_v2/term_060.png)
```
RuntimeError: hipErrorNoBinaryForGpu: Unable to find code object for all current devices
```

```
╔═══════════════════════════════════════════════════════════════╗
║  🔧 CAUSE: User not in required groups                        ║
╚═══════════════════════════════════════════════════════════════╝
```

**✅ Fix:**

![Terminal](assets_v2/term_061.png)
```bash
sudo usermod -a -G render,video $USER
groups
sudo reboot
```

---

## 📊 BENCHMARK AND PERFORMANCE

### 🎯 Expected Performance

With the correct setup, Radeon Pro VII should deliver approximately:

```
╔═══════════════════════════════════════════════════════════════╗
║  📊 EXPECTED PERFORMANCE - RADEON PRO VII                      ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  Stable Diffusion 1.5 (512x512, 20 steps):                    ║
║  • Time: ~3–5 seconds                                         ║
║  • it/s: ~4–6 it/s                                            ║
║                                                               ║
║  SDXL Base (1024x1024, 25 steps):                             ║
║  • Time: ~15–25 seconds                                       ║
║  • it/s: ~1–2 it/s                                            ║
║                                                               ║
║  SDXL + Refiner (1024x1024, 25+10 steps):                     ║
║  • Time: ~30–40 seconds                                       ║
║  • it/s: ~0.8–1.5 it/s                                        ║
║                                                               ║
║  VRAM usage:                                                  ║
║  • SD 1.5: ~2–3 GB                                            ║
║  • SDXL:  ~6–8 GB                                             ║
║  • SDXL + Refiner: ~10–12 GB                                  ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

### 🔬 Performance Test Script

Create a small script to validate performance:

![Terminal](assets_v2/term_062.png)
```bash
nano ~/test_comfyui_performance.py
```

Paste:

```python
#!/usr/bin/env python3
"""
Performance Test - PyTorch + ROCm
AMD Radeon Pro VII (gfx906)
"""

import torch
import time

print("="*70)
print("  PERFORMANCE TEST - PYTORCH + ROCM")
print("  AMD Radeon Pro VII")
print("="*70)
print()

print("1. HARDWARE DETECTION")
print("-"*70)
print(f"PyTorch version: {torch.__version__}")
print(f"ROCm available: {torch.cuda.is_available()}")
print(f"GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'N/A'}")
print(f"VRAM: {torch.cuda.get_device_properties(0).total_memory / (1024**3):.2f} GB" if torch.cuda.is_available() else "")
print()

if not torch.cuda.is_available():
    print("❌ GPU not detected! Set HSA_OVERRIDE_GFX_VERSION=9.0.0")
    exit(1)

print("2. OPERATIONS TEST")
print("-"*70)

sizes = [1024, 2048, 4096]
for size in sizes:
    a = torch.randn(size, size, device='cuda')
    b = torch.randn(size, size, device='cuda')

    # Warm-up
    for _ in range(3):
        _ = torch.matmul(a, b)
    torch.cuda.synchronize()

    # Benchmark
    start = time.time()
    iterations = 10
    for _ in range(iterations):
        c = torch.matmul(a, b)
    torch.cuda.synchronize()
    end = time.time()

    elapsed = (end - start) / iterations
    gflops = (2 * size**3) / (elapsed * 1e9)

    print(f"Matrix {size}x{size}: {elapsed*1000:.2f} ms | {gflops:.2f} GFLOPS")

    del a, b, c
    torch.cuda.empty_cache()

print()
print("✅ Test completed!")
print("="*70)
```

Save and run:

![Terminal](assets_v2/term_063.png)
```bash
chmod +x ~/test_comfyui_performance.py
source ~/.venvs/comfyui-env/bin/activate
export HSA_OVERRIDE_GFX_VERSION=9.0.0
python3 ~/test_comfyui_performance.py
```

---

## 🔧 MAINTENANCE AND UPDATES

### 🔄 Update ComfyUI

![Terminal](assets_v2/term_064.png)
```bash
cd ~/ComfyUI

# Optional backups
cp -r custom_nodes custom_nodes.backup
cp -r models models.backup

git pull

source ~/.venvs/comfyui-env/bin/activate
pip install -r requirements.txt --upgrade
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⚠️  BEFORE UPDATING                                          ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  • Backup your custom_nodes                                   ║
║  • Backup your models (if stored inside ComfyUI)              ║
║  • Keep your important workflows saved                        ║
║  • Check the GitHub changelog                                 ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

### 🔄 Update PyTorch

![Terminal](assets_v2/term_065.png)
```bash
source ~/.venvs/comfyui-env/bin/activate
pip install --upgrade torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm6.2
```

```
╔═══════════════════════════════════════════════════════════════╗
║  ⚠️  WARNING ABOUT UPDATING PYTORCH                           ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  ❌ DO NOT UPDATE if:                                         ║
║     • Everything is working perfectly                         ║
║     • There are no critical bugs you need to fix              ║
║     • You don’t need new features                             ║
║                                                               ║
║  “If it ain't broke, don't fix it!”                           ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

### 🧹 Cleanup and Optimization

![Terminal](assets_v2/term_066.png)
```bash
pip cache purge
python3 -c "import torch; torch.cuda.empty_cache()"

cd ~/ComfyUI
rm -rf temp/*
rm -rf output/.tmp/*
```

---

## 📚 ADDITIONAL RESOURCES

### 🔗 Useful Links

| Resource | Link |
|---------|------|
| ComfyUI GitHub | https://github.com/comfyanonymous/ComfyUI |
| ROCm Documentation | https://rocm.docs.amd.com/ |
| PyTorch ROCm | https://pytorch.org/get-started/locally/ |
| AMD GPU Support | https://github.com/RadeonOpenCompute/ROCm |
| ComfyUI Wiki | https://github.com/comfyanonymous/ComfyUI/wiki |

### 📂 ComfyUI Directory Structure

```
ComfyUI/
├── main.py                    # Main script
├── requirements.txt           # Python dependencies
├── comfy/                     # ComfyUI core
├── comfy_extras/              # Extra features
├── custom_nodes/              # Custom nodes
│   └── ComfyUI-Manager/       # Node manager (optional)
├── models/                    # AI models
│   ├── checkpoints/           # SD checkpoints (safetensors/ckpt)
│   ├── loras/                 # LoRAs
│   ├── vae/                   # VAEs
│   ├── clip/                  # CLIP models
│   ├── controlnet/            # ControlNet models
│   └── upscale_models/        # Upscalers
├── input/                     # Input images
├── output/                    # Generated images
└── user/                      # User config
```

### 📥 Where to Download Models

```
╔═══════════════════════════════════════════════════════════════╗
║  📦 MODEL SOURCES                                             ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  🤗 Hugging Face                                              ║
║     https://huggingface.co/models                             ║
║     • Open-source models                                      ║
║     • Free                                                     ║
║                                                               ║
║  🎨 Civitai                                                   ║
║     https://civitai.com/                                      ║
║     • Community models                                        ║
║     • LoRAs, checkpoints, embeddings                          ║
║                                                               ║
║  ⚠️  NOTE: Check licenses before using models                 ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

### 🎯 Recommended Custom Nodes

To install custom nodes, use **ComfyUI Manager**:

```bash
cd ~/ComfyUI/custom_nodes
git clone https://github.com/ltdrdata/ComfyUI-Manager.git
```

Then restart ComfyUI and open the Manager from the web UI.

Popular custom nodes:
- **ComfyUI-Manager**: node manager
- **rgthree's nodes**: utility nodes
- **ComfyUI-Impact-Pack**: advanced detection/processing
- **was-node-suite-comfyui**: large node suite

---

## ✅ FINAL CHECKLIST

Before calling your setup “done”, confirm:

```
╔═══════════════════════════════════════════════════════════════╗
║  ✅ FINAL VERIFICATION CHECKLIST                              ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  [ ] ROCm installed and working (rocminfo)                    ║
║  [ ] GPU visible in rocm-smi                                  ║
║  [ ] VBIOS 113-D1640600-104 or newer                          ║
║  [ ] User in 'render' and 'video' groups                      ║
║  [ ] Python 3.12.3 installed                                  ║
║  [ ] Virtual environment created                              ║
║  [ ] PyTorch 2.5.1+rocm6.2 installed in venv                  ║
║  [ ] torch.cuda.is_available() returns True                   ║
║  [ ] HSA_OVERRIDE_GFX_VERSION=9.0.0 set                        ║
║  [ ] ComfyUI cloned from GitHub                               ║
║  [ ] ComfyUI dependencies installed                           ║
║  [ ] ComfyUI starts with no errors                            ║
║  [ ] Web UI reachable at localhost:8188                       ║
║  [ ] GPU used during generation                               ║
║  [ ] Startup script created                                   ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## 🎉 CONCLUSION

```
╔══════════════════════════════════════════════════════════════════╗
║                                                                  ║
║              🎊 CONGRATS! INSTALLATION COMPLETE! 🎊              ║
║                                                                  ║
║  You now have a complete and working ComfyUI setup with          ║
║  ROCm acceleration on AMD Radeon Pro VII!                        ║
║                                                                  ║
║  ⚡ Your GPU is ready for:                                       ║
║     • Image generation with Stable Diffusion                     ║
║     • Complex AI workflows                                      ║
║     • Custom model experimentation                               ║
║     • Professional-grade performance                             ║
║                                                                  ║
║  🚀 Next steps:                                                  ║
║     1. Download Stable Diffusion models                          ║
║     2. Explore example workflows                                 ║
║     3. Install useful custom nodes                               ║
║     4. Join the ComfyUI community                                ║
║                                                                  ║
║  💡 Remember:                                                    ║
║     • Always activate your venv before use                       ║
║     • Keep HSA_OVERRIDE_GFX_VERSION=9.0.0 set                     ║
║     • Backup your workflows                                      ║
║                                                                  ║
║              ✨ Good luck and have fun creating! ✨               ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## 📝 QUICK COMMAND REFERENCE

### Start ComfyUI

```bash
source ~/.venvs/comfyui-env/bin/activate
export HSA_OVERRIDE_GFX_VERSION=9.0.0
cd ~/ComfyUI
python3 main.py --listen 0.0.0.0 --port 8188
```

### Check GPU Status

```bash
rocm-smi
rocminfo | grep "Marketing Name"
```

### Check PyTorch

```bash
source ~/.venvs/comfyui-env/bin/activate
python3 -c "import torch; print(torch.__version__); print(torch.cuda.is_available())"
```

### Update ComfyUI

```bash
cd ~/ComfyUI
git pull
source ~/.venvs/comfyui-env/bin/activate
pip install -r requirements.txt --upgrade
```

---

<div align="center">

**📅 Guide updated: January 2026**

**💻 Tested on: Ubuntu 24.04.3 LTS + ROCm 6.16.6 + PyTorch 2.5.1**

**🎨 Built for the AMD + ComfyUI community**

</div>
