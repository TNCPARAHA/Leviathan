# Leviathan V1.3 Pin Mappings Reference

## 📋 Board Overview

**Board**: Leviathan V1.3  
**MCU**: STM32 _(add specific model)_  
**Firmware**: Klipper  
**Documentation**: https://github.com/MotorDynamicsLab/Leviathan

## 🔌 Current Pin Assignments

### Stepper Motors

#### X Axis
- **Step Pin**: _(add pin)_
- **Dir Pin**: _(add pin)_
- **Enable Pin**: _(add pin)_
- **Endstop**: nhk:gpio13 (on toolhead)
- **TMC Driver**: _(model)_
- **UART Pin**: _(add pin)_

#### Y Axis
- **Step Pin**: _(add pin)_
- **Dir Pin**: _(add pin)_
- **Enable Pin**: _(add pin)_
- **Endstop**: nhk:gpio12 (on toolhead)
- **TMC Driver**: _(model)_
- **UART Pin**: _(add pin)_

#### Z Axis (4 motors)
- **Z (Front Left)**
  - Step: _(add pin)_
  - Dir: _(add pin)_
  - Enable: _(add pin)_
  - UART: _(add pin)_

- **Z1 (Rear Left)**
  - Step: _(add pin)_
  - Dir: _(add pin)_
  - Enable: _(add pin)_
  - UART: _(add pin)_

- **Z2 (Rear Right)**
  - Step: _(add pin)_
  - Dir: _(add pin)_
  - Enable: _(add pin)_
  - UART: _(add pin)_

- **Z3 (Front Right)**
  - Step: _(add pin)_
  - Dir: _(add pin)_
  - Enable: _(add pin)_
  - UART: _(add pin)_

#### Extruder
- **Step Pin**: nhk:gpio23 (on toolhead)
- **Dir Pin**: nhk:gpio24 (inverted)
- **Enable Pin**: nhk:gpio25 (inverted)
- **UART Pins**: nhk:gpio0 (rx), nhk:gpio1 (tx)

### Heaters & Thermistors

#### Bed Heater
- **Heater Pin**: _(add pin)_
- **Sensor Pin**: _(add pin)_
- **Sensor Type**: _(add type)_
- **Max Power**: _(add limit if any)_

#### Hotend Heater
- **Heater Pin**: nhk:gpio9 (on toolhead)
- **Sensor Pin**: nhk:gpio29 (on toolhead)
- **Sensor Type**: Generic 3950
- **Pullup Resistor**: 2200Ω

### Fans

#### Electronics Bay (Auto)
- **Pin**: PB3
- **Type**: Temperature controlled
- **Sensor**: MCU temperature

#### Motherboard (Auto)
- **Pin**: PF7
- **Tachometer**: PF6
- **Type**: Temperature controlled
- **Sensor**: Host (RPi) temperature

#### Part Cooling Fan
- **Pin**: nhk:gpio6 (on toolhead)
- **Type**: PWM controlled

#### Hotend Fan
- **Pin**: nhk:gpio5 (on toolhead)
- **Tachometer Pin**: nhk:gpio16
- **Type**: Temperature controlled (50°C threshold)

### Probe & Sensors

#### Cartographer Scanner
- **Connection**: USB (separate MCU)
- **Serial**: /dev/serial/by-id/usb-Cartographer_614e_230029000443565633393720-if00

#### Probe (on NHK)
- **Pin**: nhk:gpio10 (inverted)

#### Temperature Sensors
- **NHK MCU**: temperature_mcu (sensor_mcu: nhk)
- **Cartographer MCU**: temperature_mcu (sensor_mcu: scanner)
- **Host (RPi)**: temperature_host

### LEDs & Indicators

#### Toolhead LEDs (Neopixel)
- **Pin**: nhk:gpio7
- **Count**: 3 LEDs
- **Color Order**: GRBW

#### Chamber LEDs
- **Pin**: _(add pin if configured)_
- **Count**: _(add count)_
- **Color Order**: _(add order)_

#### NHK Activity LED
- **Pin**: nhk:gpio8 (inverted)

## 🔧 Available Pins

### Unused Pins (for expansion)
- _(List any unused pins here)_
- _(Note their location and capabilities)_

## ⚠️ Important Notes

### Pin Inversion
Pins marked with `!` are inverted:
- `!nhk:gpio24` - Extruder direction (inverted)
- `!nhk:gpio25` - Extruder enable (inverted)
- `!nhk:gpio8` - Activity LED (inverted)
- `!nhk:gpio10` - Probe pin (inverted)

### Pullup Resistors
- **NHK Thermistor**: 2200Ω pullup
- **Bed Thermistor**: _(add if different)_

### PWM Capable Pins
- _(List PWM pins for future reference)_

## 🔌 Connector Reference

### Toolhead Connector
- _(Add pinout if applicable)_

### Probe Connector
- _(Add pinout if applicable)_

## 📸 Wiring Diagrams

_(Add photos or links to wiring diagrams here)_

## 🔗 Resources

- [Leviathan GitHub](https://github.com/MotorDynamicsLab/Leviathan)
- [Klipper Pin Names](https://www.klipper3d.org/Config_Reference.html#pin-names)
- Board schematic: _(add link if available)_

---

*Last Updated: _(date)_*
