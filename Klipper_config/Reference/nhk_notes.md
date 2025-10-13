# Nitehawk (NHK) Toolhead Configuration Notes

## 📡 Hardware Information

**Model**: Nitehawk Jabberwocky Toolhead Board  
**MCU**: RP2040  
**Serial ID**: `usb-Klipper_rp2040_3033393834055A9D-if00`  
**Connection**: USB to RPi  
**Firmware Version**: _(add version here)_

## ⚙️ Current Configuration

### Extruder Settings
```
Rotation Distance: 35
Gear Ratio: 50:17 (BMG-style)
Microsteps: 16
Nozzle Diameter: 0.400mm
Max Temp: 315°C
Sensor: Generic 3950
Pullup Resistor: 2200Ω
```

### TMC2209 Driver
```
Run Current: 0.45A
Sense Resistor: 0.100Ω
Interpolate: false
```

## 🔥 PID Tuning Results

### Hotend PID
- **Kp**: 43.100
- **Ki**: 11.972
- **Kd**: 38.790
- **Target Temp**: _(temperature used for tuning)_
- **Date Tuned**: _(add date)_
- **Notes**: _(stability, overshoot, etc.)_

### PID Tuning History
| Date | Target Temp | Kp | Ki | Kd | Notes |
|------|-------------|----|----|----|----|
| | | 43.100 | 11.972 | 38.790 | Current |
| | | | | | |

## 🎯 Pressure Advance

**Current Value**: 0.04  
**Last Calibrated**: _(add date)_

### PA Testing Results
| Filament Type | Temperature | PA Value | Notes |
|--------------|-------------|----------|-------|
| | | 0.04 | Current |
| | | | |

### How to Test
```bash
# Set PA temporarily
SET_PRESSURE_ADVANCE ADVANCE=0.04

# Print PA test pattern
# Measure and adjust
```

## 📍 Pin Assignments

### Critical Pins
| Function | Pin | Notes |
|----------|-----|-------|
| Extruder Step | gpio23 | |
| Extruder Dir | gpio24 | Inverted |
| Extruder Enable | gpio25 | Inverted |
| Heater | gpio9 | |
| Thermistor | gpio29 | 2200Ω pullup |
| Part Fan | gpio6 | PWM |
| Hotend Fan | gpio5 | With tachometer |
| Tachometer | gpio16 | 2 PPR |
| Probe | gpio10 | Inverted |
| X Endstop | gpio13 | |
| Y Endstop | gpio12 | |
| ACT LED | gpio8 | Inverted |
| Neopixel LEDs | gpio7 | 3 LEDs (GRBW) |

## 💨 Fan Configuration

### Part Cooling Fan
- **Pin**: gpio6
- **Max Power**: 1.0
- **Kick Start**: 0.5s
- **Min Speed**: 10%

### Hotend Fan
- **Pin**: gpio5
- **Activation Temp**: 50°C
- **Tachometer**: Enabled (gpio16)
- **Expected RPM**: _(add typical RPM)_

## 🌡️ Temperature Monitoring

### NHK MCU Temperature
- **Normal Range**: _(add typical range)_
- **Warning Level**: 80°C
- **Max**: 100°C
- **Notes**: _(cooling performance)_

## 🔧 Calibration Notes

### Extruder Calibration
- **Steps/mm tested**: _(add if calibrated)_
- **Rotation distance**: 35 (current)
- **Measured**: _(add measured value)_
- **Date**: _(add date)_

### E-Steps Formula
```
New rotation_distance = old_rotation_distance * (requested_extrude / actual_extrude)
```

## ⚠️ Troubleshooting

### Issue Log
| Date | Issue | Solution | Preventive Action |
|------|-------|----------|-------------------|
| | | | |
| | | | |

### Common Issues
- **Hotend fan not starting**: Check tachometer readings, verify temp threshold
- **Extruder skipping**: Check current, verify temperature, inspect filament path
- **Temperature fluctuations**: Re-run PID tune, check thermistor connection
- **LED effects not working**: Verify led_effect plugin installed

## 🔌 UART Health Check

Run `CHECK_UART_HEALTH` macro to verify communication.

### Expected Output
- All TMC drivers responding
- No communication errors
- Current settings match config

## 📌 Tips & Tricks

- _(Add your discoveries here)_
- Monitor hotend fan RPM for early failure detection
- Re-tune PID when changing nozzles
- Keep firmware updated
- Run TOOLHEAD_DIAG macro for quick health check

## 🔗 Resources

- [LDO Nitehawk Docs](https://docs.ldomotors.com/en/voron/nitehawk-sb)
- [LDO GitHub](https://github.com/LDO-Motors/Nitehawk)
- [Klipper TMC Docs](https://www.klipper3d.org/TMC_Drivers.html)

---

*Last Updated: _(date)_*
