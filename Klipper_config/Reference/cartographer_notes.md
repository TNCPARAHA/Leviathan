# Cartographer Scanner Configuration Notes

## 📡 Hardware Information

**Model**: Cartographer 3D Scanner  
**Serial ID**: `usb-Cartographer_614e_230029000443565633393720-if00`  
**Connection**: USB to RPi  
**Firmware Version**: _(add version here)_

## ⚙️ Current Configuration

### Scanner Settings
```
x_offset: 0
y_offset: 0
calibration_method: touch
sensor: cartographer
sensor_alt: carto
scanner_touch_threshold: 3500
```

### Bed Mesh Settings
```
speed: 300
horizontal_move_z: 10
mesh_min: 40, 40
mesh_max: 310, 310
probe_count: 5,5
algorithm: bicubic
zero_reference_position: 175,175
```

## 🎯 Touch Threshold Testing

| Threshold | Result | Notes |
|-----------|--------|-------|
| 3500 | Current | _(Add results here)_ |
| | | |
| | | |

**Recommended Range**: 2500-4500  
**Best Value**: 3500 _(adjust based on testing)_

## 📊 Calibration History

### Latest Calibration
- **Date**: _(add date)_
- **Method**: touch
- **Result**: _(success/issues)_
- **Z Offset**: _(add value)_
- **Notes**: _(any observations)_

### Previous Calibrations
| Date | Method | Z Offset | Notes |
|------|--------|----------|-------|
| | | | |
| | | | |

## 🗺️ Bed Mesh Notes

### Mesh Quality
- **Current mesh name**: default
- **Probe points**: 5x5 (25 points)
- **Scan time**: ~_(add time)_
- **Max deviation**: _(add value)_
- **Notes**: _(flat areas, warping, etc.)_

### Mesh Optimization
- Fast mesh: 5x5 points (current)
- Detailed mesh: Can go up to 50x50+ for high resolution
- Recommended for prints: _(your preference)_

## 🔧 Common Commands

```bash
# Touch calibration
CARTOGRAPHER_TOUCH

# Bed mesh
BED_MESH_CALIBRATE
BED_MESH_PROFILE SAVE=default
BED_MESH_PROFILE LOAD=default

# Scanner info
SCANNER_QUERY_STATUS
```

## ⚠️ Troubleshooting

### Issue Log
| Date | Issue | Solution | Preventive Action |
|------|-------|----------|-------------------|
| | | | |
| | | | |

### Common Issues
- **Touch threshold too low**: Increase `scanner_touch_threshold`
- **Touch threshold too high**: Decrease value
- **Inconsistent probing**: Check mounting, clean sensor surface
- **Connection issues**: Verify USB cable, check serial ID

## 📌 Tips & Tricks

- _(Add your discoveries here)_
- Clean scanner surface regularly
- Verify touch threshold after bed changes
- Save multiple mesh profiles for different temps

## 🔗 Resources

- [Cartographer Docs](https://docs.cartographer3d.com)
- [Cartographer GitHub](https://github.com/Cartographer3D/cartographer-klipper)
- [Discord Support](link)

---

*Last Updated: _(date)_*
