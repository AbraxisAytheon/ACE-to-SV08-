# Sovol SV08 → Klipper + Anycubic Color Engine (ACE) Complete Upgrade Guide

**Document Date:** February 15, 2026  
**Target Printer:** Sovol SV08  
**Final Configuration:** Klipper + ACEPROSV08 (Multi-Material Support)  
**Status:** Stock Firmware → Phase 1 & Phase 2 Implementation

---

## Table of Contents

1. [Component Checklist](#component-checklist)
2. [Phase 1: Stock Firmware → Klipper](#phase-1-stock-firmware--klipper)
3. [Phase 2: Add Anycubic Color Engine (ACE) Support](#phase-2-add-anycubic-color-engine-ace-support)
4. [Troubleshooting](#troubleshooting)
5. [Resources & Links](#resources--links)

---

## Component Checklist

### Required for Phase 1 (Klipper Installation)

- [ ] **Raspberry Pi 4** (8GB recommended) OR **Raspberry Pi 3B+** (minimum)
  - Links: https://www.raspberrypi.com/products/
- [ ] **MicroSD Card** (32GB+ recommended, Class 10)
  - Kingston, SanDisk, or Lexar recommended
- [ ] **MicroSD Card Reader** (if not built into computer)
- [ ] **USB-A to Micro-USB Cable** (for Pi power supply - 2.5A minimum)
  - Alternative: USB-C power supply if using Pi 4
- [ ] **USB-A to USB-B Micro Cable** (to connect Pi to SV08 mainboard)
  - This connects Raspberry Pi to SV08's USB port
- [ ] **WiFi Connection** (or Ethernet adapter for Pi)
  - Pi 4 has built-in WiFi; Pi 3B+ requires USB WiFi dongle
- [ ] **Computer with Internet** (for flashing SD card and initial setup)

### Required for Phase 2 (ACE Integration)

- [ ] **Anycubic Color Engine (ACE) Unit**
  - (Already owned or to be purchased)
- [ ] **USB-A to USB-B Micro Cable** (for ACE to Raspberry Pi connection)
  - Can reuse if you have multiple cables
- [ ] **Filament Sensors** (2 units minimum)
  - 1x at ACE splitter exit
  - 1x before hotend
  - Recommended: BLTouch-compatible or simple microswitch sensors
- [ ] **Jumper Wires/Dupont Cables** (for sensor GPIO connections)
  - Female-to-female, 20+ pack recommended
- [ ] **GPIO Pin Headers** (if not already on board)
  - May need additional GPIO pin extensions

### Optional but Recommended

- [ ] **HDMI Cable + Monitor** (for initial Pi setup debugging)
- [ ] **Ethernet Cable + USB Adapter** (for Pi if WiFi unstable)
- [ ] **Cooling Fan** for Raspberry Pi (passive cooling acceptable for Pi 4)
- [ ] **Small USB Hub** (powered, 4+ ports for future expansions)

---

## Phase 1: Stock Firmware → Klipper

### Overview
**Goal:** Replace Sovol SV08 stock firmware with Klipper running on Raspberry Pi  
**Time Required:** 2-4 hours (including calibration)  
**Difficulty:** Intermediate

---

### 1.1 Prepare Raspberry Pi with Klipper OS

#### Option A: Use MainsailOS (Recommended - Easiest)

**Step 1: Download MainsailOS**
1. Visit: https://docs.mainsail.xyz/setup/mainsail-os
2. Download the latest `.img.xz` file for your Pi model (Pi 4 or Pi 3B+)

**Step 2: Flash to MicroSD Card**
1. Insert MicroSD into card reader
2. Download **Raspberry Pi Imager**: https://www.raspberrypi.com/software/
3. Open Raspberry Pi Imager
4. Select:
   - **Choose Device:** Raspberry Pi 4 (or 3B+)
   - **Choose OS:** Use Custom Image → Select downloaded `.img.xz`
   - **Choose Storage:** Your MicroSD card
5. Click **NEXT** → **EDIT SETTINGS:**
   - Set hostname: `mainsail` (or custom name)
   - Enable SSH: Toggle ON
   - Set username/password: `pi` / `raspberry` (default)
   - Configure WiFi: Enter SSID + Password
6. Click **SAVE** → **YES** to flash

**⏱️ Wait 5-10 minutes for flashing to complete**

**Step 3: First Boot**
1. Eject MicroSD from reader
2. Insert into Raspberry Pi
3. Power on Pi (connect USB power supply)
4. Wait 5 minutes for first boot
5. Open browser: `http://mainsailos.local` (or `http://<pi-ip-address>`)

**✅ MainsailOS is now running with Klipper pre-installed!**

#### Option B: Manual Klipper + Fluidd Setup (Advanced)
- Follow official Klipper installation: https://www.klipper3d.org/Installation.html
- More complex but full control over environment
- *Skip to Step 1.2 if using this method*

---

### 1.2 Identify Your SV08 Control Board

**Goal:** Know which MCU board is in your SV08 so we compile correct firmware

**Method 1: Visual Inspection**
1. Power off SV08
2. Locate the main control board (usually under printer or in control box)
3. Look for printed text on board identifying model:
   - "Sovol SV08 Stock Board"
   - "MKS" series
   - "BTT SKR" series
   - "Creality v4.2.2" or similar

**Method 2: Stock Firmware Boot Screen**
1. Power on SV08 (while still on stock firmware)
2. Watch LCD display for board name during boot
3. Document the name

**Method 3: Check SV08 Manual**
- Look in original documentation for "Control Board" or "Mainboard" section

**📝 Record Your Board Model:**