# Printer Calibration Guide

## 📋 Initial Setup Checklist

- [ ] Firmware flashed on all MCUs (Leviathan, NHK, Cartographer)
- [ ] All config files loaded and syntax verified
- [ ] Endstops tested (X, Y, Z)
- [ ] Stepper directions verified
- [ ] Fans operational
- [ ] Temperature readings accurate
- [ ] LEDs working

## 🔧 Calibration Sequence

Follow this order for best results:

### 1️⃣ Basic Mechanical Setup

#### Check Stepper Directions
```bash
# Test each axis individually
# X axis - should move RIGHT with positive movement
G91           # Relative mode
G1 X10 F3000  # Move 10mm
# If moves left, invert direction pin in config

# Y axis - should move BACK with positive movement
G1 Y10 F3000  # Move 10mm
# If moves forward, invert direction pin

# Z axis - should move UP with positive movement
G1 Z10 F600   # Move 10mm
# If moves down, invert direction pins

# Extruder - should extrude (not retract)
M109 S200     # Heat to 200°C
G1 E10 F300   # Extrude 10mm
# If retracts, invert direction pin
```

#### Verify Endstops
```bash
# Check endstop status
QUERY_ENDSTOPS

# Should show:
# x: open (when not triggered)
# y: open (when not triggered)
# z: open (when not triggered)

# Manually trigger each and re-query
# Should change to "TRIGGERED"
```

### 2️⃣ Quad Gantry Level (QGL)

```bash
# Home first
G28

# Run QGL
QUAD_GANTRY_LEVEL

# Check results - should converge within tolerance
# Retry tolerance: 0.0075mm (from config)
```

**Notes:**
- QGL should complete within 5 retries
- If not converging, check:
  - Z belt tension
  - Frame squareness
  - Bearing condition
  
**Last QGL Results:**
- Date: _(add date)_
- Iterations: _(add number)_
- Final tolerance: _(add value)_

### 3️⃣ Z Offset Calibration (Cartographer)

```bash
# Home all axes
G28

# Run QGL
QUAD_GANTRY_LEVEL

# Home Z again
G28 Z

# Move to center
G90
G1 X175 Y175 F6000

# Start touch calibration
CARTOGRAPHER_TOUCH

# Follow prompts to set Z offset
# Use paper test for final verification
```

**Current Z Offset:** _(record value)_  
**Date Calibrated:** _(add date)_

### 4️⃣ Bed Mesh

```bash
# Home and QGL first
G28
QUAD_GANTRY_LEVEL
G28 Z

# Create bed mesh
BED_MESH_CALIBRATE

# Save mesh
BED_MESH_PROFILE SAVE=default

# Save to config
SAVE_CONFIG
```

**Mesh Quality Check:**
- Max deviation: _(record value)_
- Problem areas: _(note any)_

### 5️⃣ PID Tuning

#### Bed PID Tune
```bash
# Run bed PID calibration
PID_BED TARGET=110

# Wait for completion (~10-15 minutes)
# Results will be displayed

# Save results
SAVE_CONFIG
```

**Bed PID Results:**
- Kp: _(add value)_
- Ki: _(add value)_
- Kd: _(add value)_
- Date: _(add date)_

#### Hotend PID Tune
```bash
# Run hotend PID calibration
PID_HOTEND TARGET=245

# Wait for completion (~5 minutes)
# Results will be displayed

# Save results
SAVE_CONFIG
```

**Hotend PID Results:**
- Kp: 43.100 (current)
- Ki: 11.972 (current)
- Kd: 38.790 (current)
- Date: _(add date)_

### 6️⃣ Extruder E-Steps Calibration

```bash
# Heat hotend
M109 S200

# Mark filament 120mm from extruder entrance
# Command 100mm extrusion
G91
G1 E100 F100

# Measure remaining distance
# Calculate new rotation_distance:
# new = old * (commanded / actual)
```

**E-Steps Results:**
- Original rotation_distance: 35
- Commanded: 100mm
- Actual: _(measure)_
- New rotation_distance: _(calculate)_
- Date: _(add date)_

### 7️⃣ Pressure Advance Tuning

```bash
# Heat and prepare
M109 S200
G28

# Set test PA value
SET_PRESSURE_ADVANCE ADVANCE=0.04

# Print PA test pattern
# Measure and adjust
```

**PA Test Results:**
| Filament | Temp | PA Value | Quality |
|----------|------|----------|---------|
| _(type)_ | _(°C)_ | 0.04 | Current |
| | | | |

### 8️⃣ Input Shaper Calibration

**Note:** Requires accelerometer (ADXL345) on toolhead

```bash
# Home
G28

# Run resonance test - X axis
SHAPER_CALIBRATE AXIS=X

# Run resonance test - Y axis
SHAPER_CALIBRATE AXIS=Y

# Review results and save
SAVE_CONFIG
```

**Input Shaper Results:**
- X Shaper: _(type)_
- X Frequency: _(Hz)_
- Y Shaper: _(type)_
- Y Frequency: _(Hz)_
- Date: _(add date)_

### 9️⃣ First Layer Tuning

```bash
# Use a first layer test print
# Adjust Z offset in real-time
# Use "baby stepping" or adjust offset

# Save when satisfied
SAVE_CONFIG
```

**First Layer Notes:**
- Final Z offset: _(value)_
- Bed temp: _(°C)_
- Hotend temp: _(°C)_
- Filament: _(type)_

## 🔄 Periodic Recalibration

### Daily (before each print)
- [ ] Home printer: `G28`
- [ ] Run QGL: `QUAD_GANTRY_LEVEL`
- [ ] Load bed mesh: `BED_MESH_LOAD`

### Weekly
- [ ] Verify Z offset with paper test
- [ ] Check belt tension
- [ ] Clean Cartographer sensor

### Monthly
- [ ] Re-run bed mesh calibration
- [ ] Verify QGL convergence
- [ ] Check all bolts and fasteners

### As Needed
- [ ] Re-tune PID (after nozzle change, seasonal temp change)
- [ ] Re-calibrate E-steps (after extruder maintenance)
- [ ] Re-tune pressure advance (new filament type)

## ⚙️ Advanced Calibration

### Maximum Speeds & Accelerations
```bash
# Test max velocity
# Start conservative, increase gradually
# Watch for layer shifts, skipping, artifacts
```

**Current Limits:**
- Max velocity: _(mm/s)_
- Max acceleration: _(mm/s²)_
- Max Z velocity: _(mm/s)_
- Max Z acceleration: _(mm/s²)_

### Retraction Tuning
**Current Settings:**
- Retraction distance: _(mm)_
- Retraction speed: _(mm/s)_
- Unretract speed: _(mm/s)_

## 📊 Calibration Log

| Date | Calibration Type | Result | Notes |
|------|-----------------|--------|-------|
| | | | |
| | | | |

## 🔗 Resources

- [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide/)
- [Teaching Tech Calibration](https://teachingtechyt.github.io/calibration.html)
- [Klipper Resonance Compensation](https://www.klipper3d.org/Resonance_Compensation.html)

---

*Last Updated: _(date)_*
