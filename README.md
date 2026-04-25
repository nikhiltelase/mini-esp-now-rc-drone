# 🛸 ESP-NOW Brushed RC Drone (ESP32 + MPU6050 + ESP-FC)

A lightweight, affordable micro-quad built using **ESP32**, **MPU6050**, and MOSFET motor drivers.
It uses low-latency **ESP-NOW** radio protocol for control, and is configured via **Betaflight** using the ESP-FC firmware.

This project demonstrates  using inexpensive components — suitable for hobbyists, students, and STEM projects.

---

## 📺 Project Demo

[![ESP-NOW Brushed RC Drone Demo](https://img.youtube.com/vi/2iOVJZ2RgE0/0.jpg)](https://www.youtube.com/watch?v=2iOVJZ2RgE0)

Watch the full build and flight here!

---

## 📷 Project Images
| ![Drone](https://media.licdn.com/dms/image/v2/D5622AQG1cbxqwStfFw/feedshare-shrink_1280/B56Zn7p.qsJ8As-/0/1760863718602?e=1766620800&v=beta&t=97TpDzHStCpCh86nzE3-WnqMsuksOIBlsManQuCuy9A) |

---

# 📦 Hardware List

## 🚁 **Drone Components**

| Component                | Qty |
| ------------------------ | --- |
| ESP32 DevKit V1          | 1   |
| MPU6050                  | 1   |
| AO3400A MOSFET           | 4   |
| SS14 Diode               | 4   |
| 10k resistors            | 4   |
| 8520 brushed motors      | 4   |
| 65mm propellers          | 4   |
| 3.7V → 5V booster        | 1   |
| Li-Po 3.7V 850mAh 25C    | 1   |
| JST 2-pin connector      | 1   |
| Frame (3D printed / DIY) | 1   |

---

## 🎮 **Remote Components**

| Component                   | Qty |
| --------------------------- | --- |
| ESP32 DevKit V1             | 1   |
| Dual Joystick module        | 2   |
| Push button (AUX1 → Arming) | 1   |
| Battery                     | 1   |
| Rocker ON/OFF switch        | 1   |

---
## 🔌 Circuit Connections

### Drone — ESP32 ↔ MPU6050
Hardware wiring between the ESP32 and the MPU6050 IMU:

```
ESP32 GPIO | MPU6050
3v3        | Vin
Gnd        | Gnd
21         | SDA
22         | SCL
23         | INT
```

### Drone — ESP32 ↔ ESC / Motors
Motor outputs from the ESP32 to the MOSFET/ESC driver pins:

```
ESP32 GPIO | ESC / Motor
27         | Mot_1
4          | Mot_2
26         | Mot_3
25         | Mot_4
```

---

### Remote (Transmitter) — Circuit Connections (ESP32)

```
ESP32 GPIO | Control Signal
34         | JOY1 VRx (Throttle)
35         | JOY1 VRy (Yaw)
33         | JOY2 VRx (Roll)
32         | JOY2 VRy (Pitch)
25         | Toggle Switch (AUX1)
3v3        | Joystick + Switch VCC
Gnd        | Joystick + Switch GND
```

> Note: Adjust pin numbers if your PCB/wiring or src/tx.cpp constants differ.

# ⚙️ Software & Firmware

We use:
🔹 **ESP-FC firmware**
🔹 Fully compatible with **Betaflight**

Repo →
[https://github.com/rtlopez/esp-fc](https://github.com/rtlopez/esp-fc)

### ✅ Flash ESP-FC & open Betaflight Configurator

---

# 👨‍💻 How to Upload Code

This project uses **PlatformIO** within Visual Studio Code for uploading firmware to the ESP32.

## 1. Prerequisites

- [Visual Studio Code](https://code.visualstudio.com/)
- [PlatformIO IDE Extension](https://platformio.org/install/ide?install=vscode) for VS Code

## 2. Uploading Drone Firmware (ESP-FC)

The drone uses the **ESP-FC** firmware. You'll need to flash this to the drone's ESP32.

1.  **Download ESP-FC**:
    -   Clone or download the [ESP-FC repository](https://github.com/rtlopez/esp-fc).
2.  **Open in PlatformIO**:
    -   Open the cloned `esp-fc` folder in VS Code.
3.  **Configure `platformio.ini`**:
    -   Ensure the `platformio.ini` is set for your board (e.g., `esp32dev`).
4.  **Build & Upload**:
    -   Connect the drone's ESP32 to your computer via USB.
    -   In PlatformIO, click the **Upload** button (right arrow icon) in the bottom status bar.

## 3. Uploading Remote Control Firmware

The remote control code is located in the `espnow-remote-betafilght-fc/` directory.

1.  **Open in PlatformIO**:
    -   Open the `mini-rc-drone` project folder in VS Code.
2.  **Navigate to the Remote Code**:
    -   The code for the transmitter is in `src/tx.cpp`.
3.  **Build & Upload**:
    -   Connect the remote's ESP32 to your computer via USB.
    -   Click the **Upload** button in the PlatformIO toolbar.

---

# 🛠 Betaflight Configuration

## ✅ Model Setup → Mixer

```
QUAD X
```


Motor layout:

```
   4     1
    \   /
     \ /
     / \ 
    /   \
   3     2
```

---

## ✅ Receiver

```
Receiver Mode:   SPI Rx (built-in)
SPI Provider:    NRF24_V202_250K
Channel Map:     AETR1234
```

---

## ✅ Modes

| Mode  | AUX  | Range     |
| ----- | ---- | --------- |
| ARM   | AUX1 | 1700–2100 |
| ANGLE | AUX1 | 1700–2100 |

---

## ✅ PID Tuning

| Axis  | P  | I  | Dmax | FF |
| ----- | -- | -- | ---- | -- |
| Roll  | 43 | 40 | 22   | 72 |
| Pitch | 58 | 52 | 22   | 72 |
| Yaw   | 72 | 45 | 0    | 72 |

> Not Works great for 8520 motors + 65mm props
> Tuning in procces

---

## ✅ ESC/Motor Settings

```
ESC Protocol:       BRUSHED
Motor PWM Speed:    separated
PWM Frequency:      480 Hz
Motor Stop:         OFF
Min Throttle:       1070
Max Throttle:       2000
Min Command:        1000
```

---

# 🧾 CLI Settings

### ✅ Motor Pin Mapping

```
set pin_output_0 = 4
set pin_output_1 = 27
set pin_output_2 = 26
set pin_output_3 = 25
save
```
> Change pins as per PCB/wiring layout

---

# ✅ Testing

✅ Check Receiver channels
✅ IMU calibration
✅ Motor test (NO PROPS)
✅ Arm → Hover

> ALWAYS test motor direction before attaching props

---

# 🚁 Flight Notes

✅ Angle mode recommended
✅ Indoors stable
✅ Lightweight (60–80g)
✅ Great for learning + STEM

---

# 🔋 Power

Recommended:
**3.7V 850mAh 25C Li-Po**
~4–6 minutes flight

---

---

# 📌 Future Upgrades

✅ FPV camera
✅ GPS
✅ Brushless version
✅ PCB frame

---

# ✅ Credits

ESP-FC firmware: [https://github.com/rtlopez/esp-fc](https://github.com/rtlopez/esp-fc)

---

# ❤️ Support

If you like this project —
⭐ Star the repo
📨 DM us for workshop kits

ConnectRobo youtube: https://www.youtube.com/channel/UCLttyI0VHBm-k5h8QX2aRWQ  
insta: https://www.instagram.com/connectrobo/

Instagram → [https://www.instagram.com/connectshiksha/](https://www.instagram.com/connectshiksha/)

---
