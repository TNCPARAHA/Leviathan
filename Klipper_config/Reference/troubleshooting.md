# Troubleshooting Guide

## 🚨 Emergency Procedures

### Immediate Stop
```bash
# Emergency stop
M112

# Or use macro
EMERGENCY_STOP
```

### Safe Shutdown
```bash
# Turn off heaters
TURN_OFF_HEATERS

# Turn off motors
MOTORS_OFF
```

## 🔍 Common Issues & Solutions

### Connection Issues

#### MCU Not Found
**Symptoms:** `mcu 'nhk': Unable to connect`

**Solutions:**
1. Check USB connection
2. Verify serial ID:
   ```bash
   ls /dev/serial/by-id/
   ```
3. Match ID in config file
4. Check USB cable quality
5. Try different USB port
6. Reflash firmware if necessary

#### Cartographer Not Connecting
**Symptoms:** `scanner: Unable to connect`

**Solutions:**
1. Verify Cartographer serial ID
2. Check USB connection
3. Ensure Cartographer firmware is up to date
4. Test with different USB cable
5. Check power to Cartographer board

### Homing Issues

#### Cannot Home X/Y
**Symptoms:** Timeout, no endstop trigger

**Solutions:**
1. Check endstop wiring (nhk:gpio13 for X, nhk:gpio12 for Y)
2. Query endstops: `QUERY_ENDSTOPS`
3. Manually trigger and verify state change
4. Check if pins are inverted correctly
5. Verify sensorless homing settings (if applicable)

#### Z Homing Fails
**Symptoms:** Cartographer not triggering, timeout

**Solutions:**
1. Clean Cartographer sensor surface
2. Check `scanner_touch_threshold` value
3. Verify probe pin: `nhk:gpio10`
4. Manually test with `CARTOGRAPHER_TOUCH`
5. Check Z movement direction
6. Ensure bed is within range

### Temperature Issues

#### Hotend Not Heating
**Symptoms:** Temperature not rising, or rising slowly

**Solutions:**
1. Check heater connection (nhk:gpio9)
2. Verify thermistor reading (should be room temp)
3. Check PID values
4. Inspect for loose wires
5. Test heater resistance with multimeter
6. Check for thermistor short/open circuit

#### Temperature Fluctuations
**Symptoms:** ±5°C or more variation

**Solutions:**
1. Re-run PID tune: `PID_HOTEND TARGET=245`
2. Check part cooling fan interference
3. Verify thermistor connection
4. Check for loose thermistor
5. Ensure proper hotend insulation

#### Thermal Runaway
**Symptoms:** Emergency stop due to thermal runaway

**Solutions:**
1. **DO NOT IGNORE** - This is a safety feature
2. Check thermistor is properly seated
3. Verify thermistor type in config
4. Check heater cartridge connection
5. Re-run PID tune
6. Inspect for damaged wiring

### Extrusion Issues

#### Under-Extrusion
**Symptoms:** Gaps in layers, weak prints

**Solutions:**
1. Check filament path for tangles
2. Verify extruder tension
3. Increase temperature by 5-10°C
4. Check for partial nozzle clog
5. Verify E-steps calibration
6. Reduce print speed

#### Over-Extrusion
**Symptoms:** Blobby prints, excess material

**Solutions:**
1. Reduce flow rate in slicer
2. Check E-steps calibration
3. Verify filament diameter (1.75mm)
4. Reduce temperature by 5-10°C

#### Extruder Skipping
**Symptoms:** Clicking sound, intermittent extrusion

**Solutions:**
1. Check TMC current (currently 0.45A)
2. Increase hotend temperature
3. Check for nozzle clog
4. Reduce retraction settings
5. Verify filament path is clear
6. Check for heat creep

### Probing/Leveling Issues

#### QGL Not Converging
**Symptoms:** Exceeds retry limit (5 retries)

**Solutions:**
1. Check Z belt tension (all 4)
2. Verify frame is square
3. Check Z motor couplers
4. Inspect linear rails for binding
5. Loosen QGL retry tolerance temporarily
6. Check for loose bed mounting

#### Bed Mesh Inconsistent
**Symptoms:** Different results each time

**Solutions:**
1. Ensure bed is at printing temperature
2. Run QGL before mesh
3. Clean Cartographer sensor
4. Check `scanner_touch_threshold`
5. Increase `samples` in probe config
6. Allow bed to heat soak longer

#### First Layer Issues
**Symptoms:** Poor adhesion, too high/low

**Solutions:**
1. Re-calibrate Z offset
2. Clean bed surface
3. Check bed temperature
4. Verify bed mesh is loaded
5. Check for warped bed
6. Adjust Z offset in real-time

### Motion Issues

#### Layer Shifts
**Symptoms:** Layers offset from previous layers

**Solutions:**
1. Check belt tension
2. Reduce acceleration/velocity
3. Verify motor currents (TMC drivers)
4. Check for mechanical binding
5. Ensure stepper drivers not overheating
6. Check for loose pulleys

#### Resonance/Ringing
**Symptoms:** Ripples/waves on printed surfaces

**Solutions:**
1. Run Input Shaper calibration
2. Reduce acceleration
3. Check belt tension
4. Tighten any loose bolts
5. Add damping to frame
6. Reduce print speed on perimeters

#### Grinding/Binding
**Symptoms:** Noise during movement, jerky motion

**Solutions:**
1. Check linear rail lubrication
2. Inspect for debris
3. Verify belt alignment
4. Check for over-tightened bearings
5. Ensure gantry is square

### LED Issues

#### LEDs Not Working
**Symptoms:** No light output

**Solutions:**
1. Verify `led_effect` plugin installed
2. Check pin: nhk:gpio7
3. Verify color order: GRBW
4. Test with: `SET_LED LED=jw_leds RED=1 GREEN=1 BLUE=1`
5. Check LED power connection
6. Test individual LEDs

#### LED Effects Not Working
**Symptoms:** Static works, effects don't

**Solutions:**
1. Install LED effects: https://github.com/julianschill/klipper-led_effect
2. Restart Klipper after install
3. Check for macro conflicts
4. Verify effect names in config

### Fan Issues

#### Part Cooling Fan Not Working
**Symptoms:** No airflow from part fan

**Solutions:**
1. Check pin: nhk:gpio6
2. Test manually: `M106 S255`
3. Verify fan connection
4. Check for damaged fan
5. Test with different fan

#### Hotend Fan Not Running
**Symptoms:** No airflow, hotend overheating

**Solutions:**
1. Check activation temperature (50°C)
2. Verify pin: nhk:gpio5
3. Check tachometer reading
4. Test fan separately
5. **DO NOT RUN WITHOUT HOTEND FAN**

#### Fan Speed Incorrect
**Symptoms:** Always full speed or no speed control

**Solutions:**
1. Verify PWM-capable pin
2. Check `max_power` setting
3. Test with different speeds
4. Check for faulty fan

## 📊 Diagnostic Commands

### System Status
```bash
# Check MCU status
STATUS

# Check all endstops
QUERY_ENDSTOPS

# Check all fans
QUERY_FAN fan
QUERY_FAN hotend_fan

# Check probe
QUERY_PROBE

# Check ADC (temp sensors)
QUERY_ADC

# Complete toolhead diagnostic
TOOLHEAD_DIAG

# Check UART health
CHECK_UART_HEALTH
```

### Temperature Monitoring
```bash
# Get all temperatures
TEMPERATURE_SENSORS

# Monitor in real-time (console)
# Watch current temps during operation
```

### TMC Driver Diagnostics
```bash
# Dump driver registers
DUMP_TMC STEPPER=stepper_x
DUMP_TMC STEPPER=stepper_y
DUMP_TMC STEPPER=extruder

# Check for errors in output
```

## 🔧 Firmware Recovery

### Reflash MCU Firmware

#### Leviathan Main Board
```bash
# Enter bootloader mode
# Flash with appropriate method for STM32
```

#### Nitehawk (NHK)
```bash
# Enter bootloader mode on RP2040
# Flash Klipper firmware
```

#### Cartographer
```bash
# Follow Cartographer firmware update guide
# https://docs.cartographer3d.com
```

## 📋 Issue Log Template

When logging issues, include:

**Date:** _(date)_  
**Issue:** _(brief description)_  
**Symptoms:** _(what you observed)_  
**Environment:** _(temps, what was printing, etc.)_  
**Solutions Tried:** _(list what you attempted)_  
**Resolution:** _(what fixed it)_  
**Prevention:** _(how to avoid in future)_

## 📊 Personal Issue Log

| Date | Issue | Solution | Prevention |
|------|-------|----------|------------|
| | | | |
| | | | |

## 🆘 Getting Help

### Information to Provide
1. **Klipper version**: `version` command
2. **Error messages**: Full text from console
3. **Config files**: Relevant sections
4. **Recent changes**: What changed before issue
5. **Klippy.log**: Last 100 lines often helpful

### Where to Ask
- Voron Discord: #Leviathan channel (if exists)
- Klipper Discourse: https://klipper.discourse.group
- Cartographer Discord: For scanner-specific issues

### Before Asking
- [ ] Check this troubleshooting guide
- [ ] Search Discord/Discourse for similar issues
- [ ] Review Klipper documentation
- [ ] Check klippy.log for errors

## 🔗 Resources

- [Klipper FAQ](https://www.klipper3d.org/FAQ.html)
- [Klipper Debugging](https://www.klipper3d.org/Debugging.html)
- [Voron Troubleshooting](https://docs.vorondesign.com/community/troubleshooting/)

---

*Last Updated: _(date)_*
