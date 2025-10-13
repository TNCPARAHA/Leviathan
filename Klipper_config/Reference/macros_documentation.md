# Macros Documentation & Usage Guide

## 📖 Overview

This document explains all custom macros available in your Klipper configuration and how to use them effectively.

## 🔧 Diagnostic Macros

### TOOLHEAD_DIAG
**Purpose:** Complete toolhead health check  
**Usage:** `TOOLHEAD_DIAG`  
**What it does:**
- Blinks activity LED
- Queries part cooling fan status
- Queries hotend fan status
- Checks probe status
- Reads NHK MCU temperature

**When to use:**
- Before starting a print
- Troubleshooting toolhead issues
- After maintenance work

---

### CHECK_UART_HEALTH
**Purpose:** Verify TMC driver communication  
**Usage:** `CHECK_UART_HEALTH`  
**What it does:**
- Dumps status of all TMC drivers
- Validates UART communication
- Shows current settings

**When to use:**
- After config changes
- Diagnosing stepper issues
- Verifying driver configuration

---

## 🌀 Fan Control Macros

### FAN_EB_ON / FAN_EB_OFF / FAN_EB_HALF
**Purpose:** Manual control of electronics bay fans  
**Usage:** 
```bash
FAN_EB_ON      # Full speed
FAN_EB_HALF    # 50% speed
FAN_EB_OFF     # Turn off
```

**Note:** Temperature-controlled fans will override these commands when temps rise

---

## 🔥 PID Calibration Macros

### PID_HOTEND
**Purpose:** Calibrate hotend PID values  
**Usage:** `PID_HOTEND TARGET=245`  
**Default temp:** 245°C  
**Duration:** ~5 minutes  

**When to use:**
- After changing nozzle
- After changing hotend
- If temperature fluctuates
- Seasonal temperature changes

**Remember:** Run `SAVE_CONFIG` after completion

---

### PID_BED
**Purpose:** Calibrate bed PID values  
**Usage:** `PID_BED TARGET=110`  
**Default temp:** 110°C  
**Duration:** ~10-15 minutes  

**When to use:**
- After bed replacement
- If bed temp unstable
- Changing build plate material

**Remember:** Run `SAVE_CONFIG` after completion

---

### PID_ALL
**Purpose:** Calibrate both hotend and bed  
**Usage:** `PID_ALL HOTEND_TEMP=245 BED_TEMP=110`  
**Duration:** ~20 minutes total  

**Best practice:** Run during initial setup or major maintenance

---

## 💡 Chamber Lighting Macros

### CHAMBER_LIGHTS_ON
**Usage:** `CHAMBER_LIGHTS_ON`  
**Result:** Full brightness white (80%)

### CHAMBER_LIGHTS_OFF
**Usage:** `CHAMBER_LIGHTS_OFF`  
**Result:** All LEDs off

### CHAMBER_LIGHTS_DIM
**Usage:** `CHAMBER_LIGHTS_DIM`  
**Result:** Low ambient blue lighting

### CHAMBER_LIGHTS_PRINT
**Usage:** `CHAMBER_LIGHTS_PRINT`  
**Result:** Moderate white lighting for printing

### CHAMBER_RAINBOW
**Usage:** `CHAMBER_RAINBOW`  
**Result:** Animated rainbow effect across 25 LEDs  
**Note:** Fun for demos!

---

## 🖨️ Print Control Macros

### PRINT_START
**Purpose:** Complete print start sequence  
**Usage:** `PRINT_START BED_TEMP=60 EXTRUDER_TEMP=210`  
**Required parameters:**
- `BED_TEMP` - Target bed temperature
- `EXTRUDER_TEMP` - Target extruder temperature

**What it does:**
1. Sets status LEDs to heating
2. Turns on chamber lights
3. Starts bed heating
4. Homes all axes
5. Waits for bed temp
6. Runs QGL (Quad Gantry Level)
7. Re-homes Z after QGL
8. Creates bed mesh
9. Moves to purge position
10. Heats extruder and waits
11. Purges filament
12. Sets status to printing

**Slicer integration:**
```gcode
; In your slicer start G-code:
PRINT_START BED_TEMP=[bed_temperature] EXTRUDER_TEMP=[nozzle_temperature]
```

---

### PRINT_END
**Purpose:** Safe print completion  
**Usage:** `PRINT_END` (typically called by slicer)  

**What it does:**
1. Retracts filament
2. Turns off all heaters
3. Moves nozzle away from print
4. Parks toolhead at rear
5. Turns off part cooling fan
6. Clears bed mesh
7. Sets status to part ready
8. Turns on chamber lights

**Slicer integration:**
```gcode
; In your slicer end G-code:
PRINT_END
```

---

### SCAN_BED
**Purpose:** Cartographer bed scanning  
**Usage:** `SCAN_BED`  
**What it does:**
1. Starts scanner
2. Waits for scanner ready
3. Calibrates bed mesh

**When to use:**
- Alternative to standard bed mesh
- High-resolution scanning

---

## 🔧 Maintenance Macros

### PARK_TOOLHEAD
**Purpose:** Park toolhead at rear for maintenance  
**Usage:** `PARK_TOOLHEAD`  
**Position:** X175, Y350, Z100  

**When to use:**
- Cleaning nozzle
- Changing filament
- Bed maintenance

---

### CENTER_TOOLHEAD
**Purpose:** Center toolhead for bed access  
**Usage:** `CENTER_TOOLHEAD`  
**Position:** X175, Y175, Z100  

**When to use:**
- Bed mesh calibration
- Z offset calibration
- Manual leveling checks

---

### LOAD_FILAMENT
**Purpose:** Load filament with heating  
**Usage:** `LOAD_FILAMENT EXTRUDER_TEMP=210`  
**Default temp:** 210°C  

**What it does:**
1. Heats extruder
2. Waits for temperature
3. Extrudes 50mm slowly
4. Extrudes 30mm to prime

**Best practice:** Insert filament before running macro

---

### UNLOAD_FILAMENT
**Purpose:** Unload filament with heating  
**Usage:** `UNLOAD_FILAMENT EXTRUDER_TEMP=210`  
**Default temp:** 210°C  

**What it does:**
1. Heats extruder
2. Waits for temperature
3. Extrudes 10mm to soften tip
4. Retracts 80mm

---

## 🌡️ Preheat Macros

### PREHEAT_PLA
**Usage:** `PREHEAT_PLA`  
**Settings:** Bed: 60°C, Extruder: 210°C (no wait)

### PREHEAT_ABS
**Usage:** `PREHEAT_ABS`  
**Settings:** Bed: 100°C, Extruder: 250°C (no wait)

### PREHEAT_PETG
**Usage:** `PREHEAT_PETG`  
**Settings:** Bed: 80°C, Extruder: 230°C (no wait)

**Note:** These start heating but don't wait. Useful for prep while doing other tasks.

---

## 📊 Calibration Macros

### CALIBRATE_Z
**Purpose:** Z offset calibration  
**Usage:** `CALIBRATE_Z`  

**What it does:**
1. Homes all axes
2. Runs Z calibration routine
3. Sets status when complete

---

### BED_MESH_LOAD
**Purpose:** Load saved bed mesh  
**Usage:** `BED_MESH_LOAD`  
**Loads:** "default" profile  

**When to use:**
- Before each print (if mesh is good)
- Skip recalibration to save time

---

### BED_MESH_SAVE
**Purpose:** Save current bed mesh  
**Usage:** `BED_MESH_SAVE`  
**Saves as:** "default" profile  
**Runs:** SAVE_CONFIG automatically  

**When to use:**
- After creating a good mesh
- Before testing prints

---

## 🚨 Emergency Macros

### EMERGENCY_STOP
**Purpose:** Full emergency shutdown  
**Usage:** `EMERGENCY_STOP`  
**What it does:**
1. Triggers M112 (emergency stop)
2. Turns off status LEDs
3. Turns off chamber lights

**Note:** Requires Klipper restart after use

---

### MOTORS_OFF
**Purpose:** Disable all stepper motors  
**Usage:** `MOTORS_OFF`  

**When to use:**
- Moving toolhead manually
- After print completion
- Emergency situations

---

## 🎉 Fun Macros

### LIGHT_SHOW
**Purpose:** Demo light show  
**Usage:** `LIGHT_SHOW`  
**Duration:** ~10 seconds  

**What it does:**
1. Runs rainbow effect
2. Alternates on/off
3. Ends with dim lights

**When to use:**
- Showing off your printer
- Testing LEDs
- Having fun!

---

### PRINTER_STATUS
**Purpose:** Display current printer state  
**Usage:** `PRINTER_STATUS`  

**What it shows:**
- "PRINTING" if actively printing
- "HOT - Cooling Down" if above 50°C
- "READY" if idle and cool

---

## 🎨 Status LED Macros

These are called automatically by other macros:

- `STATUS_HEATING` - Orange breathing
- `STATUS_HOMING` - Green breathing
- `STATUS_LEVELING` - Purple breathing
- `STATUS_MESHING` - Lime green breathing
- `STATUS_PRINTING` - Red/orange gradient
- `STATUS_COOLING` - Blue breathing
- `STATUS_BUSY` - Red breathing
- `STATUS_READY` - Rainbow gradient
- `STATUS_PART_READY` - Green breathing
- `STATUS_OFF` - All LEDs off

---

## 💡 Tips & Best Practices

### Print Start Sequence
Always use `PRINT_START` instead of manual commands. It ensures:
- Proper homing sequence
- QGL is performed
- Fresh bed mesh
- Correct temperatures
- Proper purging

### Filament Changes
Use `LOAD_FILAMENT` and `UNLOAD_FILAMENT` for consistent results.

### Regular Diagnostics
Run `TOOLHEAD_DIAG` before important prints.

### Save After Calibration
Always `SAVE_CONFIG` after PID tuning or calibration macros.

## 📋 Macro Customization Notes

Add your custom macros or modifications here:

---

*Last Updated: _(date)_*
