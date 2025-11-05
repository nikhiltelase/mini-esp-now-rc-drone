# ESP-NOW Remote Control for Betaflight FC

A custom RC transmitter built with ESP32 that uses ESP-NOW protocol to communicate with a Betaflight flight controller via ESP-NOW RC Link.

## Features

- 4-channel control (Roll, Pitch, Throttle, Yaw)
- 1 auxiliary channel (toggle switch)
- Auto-calibrating stick centers
- Adjustable trim values
- Low-pass filtering for smooth control
- Configurable deadzone
- 50 Hz update rate

## Hardware Requirements

- ESP32 development board (e.g., LOLIN32)
- 2x analog joysticks (dual-axis)
- 1x toggle switch
- Jumper wires
- USB cable for programming

See `circuit-connections.txt` for detailed wiring instructions.

## Software Requirements

- [PlatformIO](https://platformio.org/) (recommended) or Arduino IDE
- Python 3.x (for PlatformIO)
- USB drivers for ESP32 (CP210x or CH340)

## Installation

### Method 1: Using PlatformIO (Recommended)

1. **Install PlatformIO**
   - Install [Visual Studio Code](https://code.visualstudio.com/)
   - Install the PlatformIO IDE extension from VS Code marketplace

2. **Clone or Download Project**
   ```bash
   cd "d:\My Code Base\IOT&ROBOTICS-projects\32_DRONE-delevlopment\mini-rc-drone"
   # Clone or extract the project here
   ```

3. **Open Project**
   - Open VS Code
   - File → Open Folder → Select `espnow-remote-betafilght-fc` folder
   - PlatformIO will automatically detect the project

4. **Connect ESP32**
   - Connect your ESP32 board via USB
   - Wait for drivers to install (if first time)

5. **Upload Code**
   - Click the **Upload** button (→) in the PlatformIO toolbar, OR
   - Open PlatformIO Terminal and run:
     ```bash
     pio run --target upload
     ```
   - Wait for compilation and upload to complete

6. **Monitor Serial Output**
   - Click the **Serial Monitor** button (🔌) in PlatformIO toolbar, OR
   - Run in terminal:
     ```bash
     pio device monitor
     ```
   - You should see RC channel values being printed

### Method 2: Using PlatformIO CLI

1. **Install PlatformIO Core**
   ```bash
   pip install platformio
   ```

2. **Navigate to Project**
   ```bash
   cd "d:\My Code Base\IOT&ROBOTICS-projects\32_DRONE-delevlopment\mini-rc-drone\espnow-remote-betafilght-fc"
   ```

3. **Build and Upload**
   ```bash
   # Build only
   pio run

   # Build and upload
   pio run --target upload

   # Upload and open serial monitor
   pio run --target upload && pio device monitor
   ```

## Configuration

### Pin Assignments
Edit `src/tx.cpp` to change pin assignments:
```cpp
const int JOY1_X_PIN = 34; // throttle (left stick up/down)
const int JOY1_Y_PIN = 35; // yaw (left stick left/right)
const int JOY2_X_PIN = 33; // roll (right stick left/right)
const int JOY2_Y_PIN = 32; // pitch (right stick up/down)
const int SWITCH_PIN = 25; // toggle switch
```

### Tuning Parameters
Adjust these in `src/tx.cpp`:
```cpp
const uint32_t SEND_INTERVAL_MS = 20;    // Update rate (50 Hz)
const float FILTER_ALPHA = 0.15f;        // Filter smoothing (0.01-1.0)
const float DEADZONE_FRACTION = 0.05f;   // Stick deadzone (5%)
```

### Trim Values
Adjust stick trims in `src/tx.cpp`:
```cpp
static StickTrims trims = {
  0,    // yaw trim (µs)
  0,    // roll trim (µs)
  -30   // pitch trim (µs)
};
```

## Troubleshooting

### Upload Fails
- **Check COM port**: Device Manager → Ports (verify ESP32 is detected)
- **Hold BOOT button**: Hold during upload if auto-reset fails
- **Check USB cable**: Use a data cable, not charging-only
- **Driver issues**: Install [CP210x](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) or [CH340](http://www.wch-ic.com/downloads/CH341SER_ZIP.html) drivers

### No Serial Output
- Check baud rate is set to **115200**
- Press RESET button on ESP32 after upload
- Try a different USB port

### Compilation Errors
- Clean build: `pio run --target clean`
- Update dependencies: `pio pkg update`
- Check PlatformIO version: `pio upgrade`

### Joystick Not Responding
- Verify wiring matches `circuit-connections.txt`
- Check joystick power (should be 3.3V, not 5V)
- Test analog readings with simple sketch first

## Project Structure

```
espnow-remote-betafilght-fc/
├── src/
│   ├── tx.cpp              # Main transmitter code (active)
│   └── working.txt         # Reference/backup code
├── platformio.ini          # PlatformIO configuration
├── merge_firmware.py       # Firmware merge script
├── circuit-connections.txt # Wiring guide
└── README.md              # This file
```

## Channel Mapping

| Channel | Control    | Stick/Switch    | Range      |
|---------|------------|-----------------|------------|
| CH1     | Roll       | Right X-axis    | 1000-2000  |
| CH2     | Pitch      | Right Y-axis    | 1000-2000  |
| CH3     | Throttle   | Left Y-axis     | 1000-2000  |
| CH4     | Yaw        | Left X-axis     | 1000-2000  |
| CH5     | Aux1       | Toggle Switch   | 1000/2000  |

## License

This project uses the ESP-NOW RC Link library. Check individual library licenses for details.

## Support

For issues or questions:
1. Check troubleshooting section above
2. Review `circuit-connections.txt` for wiring
3. Check serial monitor output for debugging info

## Additional Resources

- [PlatformIO Documentation](https://docs.platformio.org/)
- [ESP-NOW RC Link](https://github.com/rtlopez/espnow-rclink)
- [ESP32 Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
