# Installation Guide

Welcome! This guide will walk you through setting up the PiModal CNC Tap Tester on your Raspberry Pi.

## Table of Contents

- [Quick Start (5 minutes)](#quick-start-5-minutes)
- [Detailed Setup](#detailed-setup)
- [Hardware Setup](#hardware-setup)
- [Troubleshooting](#troubleshooting)

---

## Quick Start (5 minutes)

### 1. Clone the Repository

```bash
git clone https://github.com/rzega02/PiModal-CNC---Raspberry-Pi-CNC-Tap-Tester.git
cd PiModal-CNC---Raspberry-Pi-CNC-Tap-Tester
```

### 2. Enable I2C Interface

I2C must be enabled on your Raspberry Pi:

```bash
sudo raspi-config
```

Navigate to:
- **Interface Options** → **I2C** → **Yes** → **Finish**

Reboot if prompted:

```bash
sudo reboot
```

### 3. Create Virtual Environment & Install

```bash
python3 -m venv env
source env/bin/activate
pip install -r requirements.txt
```

### 4. Verify Hardware Connection

```bash
i2cdetect -y 1
```

You should see `68` in the address table. If not, check your wiring (see [Hardware Setup](#hardware-setup) below).

### 5. Run the Program

```bash
python tap_test.py
```

Press **Enter** when ready to capture a tap test.

---

## Detailed Setup

### Prerequisites

- Raspberry Pi 4B (or compatible)
- Debian/Raspberry Pi OS (Bullseye or newer)
- Python 3.7+
- Internet connection (for initial setup only)

### Step 1: Enable I2C Interface

**GUI Method (Desktop):**

1. Click the Raspberry Pi menu → Preferences → Raspberry Pi Configuration
2. Go to the **Interfaces** tab
3. Enable **I2C**
4. Click **OK** and reboot

**CLI Method:**

```bash
sudo raspi-config
```

Navigate to **Interface Options** → **I2C** and select **Yes**.

Verify I2C is enabled:

```bash
sudo grep -E "^dtparam=i2c" /boot/config.txt
```

Should output: `dtparam=i2c_arm=on`

### Step 2: Install Python Dependencies

Create a virtual environment (recommended):

```bash
python3 -m venv ~/venvs/mpu6050-env
source ~/venvs/mpu6050-env/bin/activate
```

Clone the repository:

```bash
git clone https://github.com/rzega02/PiModal-CNC---Raspberry-Pi-CNC-Tap-Tester.git
cd PiModal-CNC---Raspberry-Pi-CNC-Tap-Tester
```

Install dependencies:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

Verify installation:

```bash
python -c "import mpu6050; import numpy; import matplotlib; print('All dependencies installed!')"
```

### Step 3: Verify I2C Device Detection

Connect your MPU-6050 and run:

```bash
i2cdetect -y 1
```

Expected output:

```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- 68 -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

The **68** indicates the MPU-6050 is detected at I2C address 0x68 ✓

---

## Hardware Setup

### Wiring Diagram

Connect your MPU-6050 to the Raspberry Pi as follows:

```
MPU-6050 Pin  →  Raspberry Pi GPIO Pin  →  Function
─────────────────────────────────────────────────────
VCC (Pin 1)   →  Pin 1 or 17            →  3.3V Power
GND (Pin 2)   →  Pin 6, 9, 14, 20, 25   →  Ground
SDA (Pin 3)   →  Pin 3 (GPIO 2)         →  I2C Data
SCL (Pin 4)   →  Pin 5 (GPIO 3)         →  I2C Clock
INT (Pin 8)   →  Pin 7 (GPIO 4)         →  Interrupt (Optional)
```

### Pinout Reference

Raspberry Pi GPIO Header (looking at the board):

```
3V3  1 ○|○ 2  5V
 SDA 3 ○|○ 4  5V
 SCL 5 ○|○ 6  GND
 G7  7 ○|○ 8  TXD
GND  9 ○|○ 10 RXD
G17 11 ○|○ 12 G18
G27 13 ○|○ 14 GND
G22 15 ○|○ 16 G23
3V3 17 ○|○ 18 G24
MOSI 19 ○|○ 20 GND
MISO 21 ○|○ 22 G25
SCLK 23 ○|○ 24 CE0
GND 25 ○|○ 26 CE1
```

### MPU-6050 Pinout

```
Pin  Name        Description
───  ────────    ───────────────────────
1    VCC         +3.3V Power Supply
2    GND         Ground
3    SDA         I2C Serial Data
4    SCL         I2C Serial Clock
5    XDA         External I2C Data (leave unconnected)
6    XCL         External I2C Clock (leave unconnected)
7    AD0         Address Select (leave unconnected for 0x68)
8    INT         Interrupt Pin (optional)
```

### Best Practices

✓ **Use short, shielded wires** for I2C connections (keep < 50 cm)
✓ **Keep SDA/SCL wires together** to minimize noise
✓ **Mount the accelerometer rigidly** on the tool holder or spindle nose
✓ **Keep wires away from rotating parts** to prevent damage
✓ **Use hot glue or mounting tape** for secure placement
✓ **Ensure consistent orientation** between tests

---

## Troubleshooting

### "No module named mpu6050"

```bash
# Ensure virtual environment is activated
source env/bin/activate

# Reinstall mpu6050 package
pip install --upgrade mpu6050-raspberrypi
```

### "I2C device not found (Address: 0x68)"

1. **Verify wiring:**
   ```bash
   i2cdetect -y 1
   ```
   If you don't see `68`, check your SDA/SCL connections.

2. **Check I2C is enabled:**
   ```bash
   grep dtparam=i2c_arm /boot/config.txt
   ```
   Should output: `dtparam=i2c_arm=on`

3. **Check GPIO permissions:**
   ```bash
   ls -la /dev/i2c-1
   ```
   Should show: `crw-rw---- 1 root i2c`

   If you get permission denied, add your user to the i2c group:
   ```bash
   sudo usermod -aG i2c $USER
   sudo usermod -aG gpio $USER
   newgrp i2c
   ```

4. **Test with known-working device:**
   Try your I2C connection with another device to rule out hardware issues.

### "Permission denied" when running tap_test.py

**Option 1:** Add user to GPIO group (recommended)
```bash
sudo usermod -aG gpio $USER
sudo usermod -aG i2c $USER
newgrp gpio
```

**Option 2:** Run with sudo (not recommended)
```bash
sudo python tap_test.py
```

### ImportError: matplotlib cannot find display

This is normal on headless systems. The program will save plots as PNG files instead.

To fix if you have a display:
```bash
sudo apt-get install python3-tk
export DISPLAY=:0
python tap_test.py
```

### "ValueError: invalid literal for int()"

This occurs if matplotlib backend cannot be initialized. Try:
```bash
sudo apt-get install python3-matplotlib
```

### Data capture has low sample rate

- **Check system load:** `top` should show CPU <50% during capture
- **Disable unnecessary services:** `sudo systemctl status`
- **Close other programs**
- **Use Raspberry Pi OS Lite** (headless) for best performance

### Inconsistent results between tests

- **Ensure rigid mounting** of the accelerometer
- **Use consistent tap force** and location
- **Let spindle ring down** for full 3-5 seconds before ending capture
- **Check for loose tooling** or worn bearings
- **Verify accelerometer orientation** is consistent

### Still having issues?

1. Check the [FAQ](README.md#troubleshooting) in the main README
2. Open an issue on GitHub with:
   - Your Raspberry Pi model and OS version
   - Output of `i2cdetect -y 1`
   - The error message or symptoms
   - Steps you've taken to troubleshoot

---

## Next Steps

Once installation is complete:

1. Read the [Testing Guide](TESTING_GUIDE.md) for proper tap testing procedure
2. Check the [README.md](README.md) for theory and features
3. Run your first test with `python tap_test.py`
4. Check [Sample Output](./Sample%20Output/) for expected results

Happy testing! 🚀
