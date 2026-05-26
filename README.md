# 🤖 IoT-Based Hand Gesture Controlled Car

> Control a Wi-Fi-connected car in real time using just your hand gestures — no physical controller needed. Also supports mobile remote control via the ESP Car Controller app.

---

## 📸 Demo
<img width="1024" height="1536" alt="WhatsApp Image 2026-05-26 at 12 27 18 PM" src="https://github.com/user-attachments/assets/3b1f175a-ffe2-4dbf-8d13-d984138caedb" />


https://github.com/user-attachments/assets/82ea225b-09c7-44f8-ae6e-780522389a6e



## 🔌 Circuit Diagram

![Circuit Diagram](circuit_diagram.png)

> Full interactive circuit: [View on Cirkit Designer](https://app.cirkitdesigner.com/project/5d358feb-4b06-4ddb-8f59-7b4ea54a220e)
## 🧠 How It Works

1. A **Python script** on your PC captures your hand via webcam
2. **MediaPipe + OpenCV** detect and interpret your hand gestures in real time
3. Gesture commands (`f`, `b`, `l`, `r`, `s`) are sent wirelessly over **UDP via Wi-Fi**
4. The **ESP32 NodeMCU** receives the commands and drives the motors through an **L298N motor driver**

```
[Hand Gesture] → [Webcam] → [Python + MediaPipe] → [UDP over Wi-Fi] → [ESP32] → [L298N] → [DC Motors]
```

Alternatively, the car can also be controlled directly from your smartphone:

```
[ESP Car Controller App] → [Wi-Fi (same network)] → [ESP32] → [L298N] → [DC Motors]
```

---

## 🕹️ Gesture Controls (PC)

| Gesture | Action |
|---|---|
| Hand in **top box** (palm forward) | ⬆️ Forward |
| Hand in **bottom box** (palm forward) | ⬇️ Backward |
| Rotate wrist **right (>20°)** | ➡️ Turn Right |
| Rotate wrist **left (<-20°)** | ⬅️ Turn Left |
| Hand idle / no gesture | ⏹️ Stop |

---

## 📱 Mobile Control — ESP Car Controller App

As an alternative to hand gestures, the car can be controlled remotely from a smartphone using the **ESP Car Controller** app.

### Setup
1. Make sure your phone is connected to the **same Wi-Fi network** as the ESP32
2. Install the app by sideloading the provided `base.apk` on your Android device
   - Go to **Settings → Security → Enable "Install from unknown sources"**
   - Open the APK file and install
3. Open the app and enter the **ESP32's IP address** (visible on Serial Monitor at startup)
4. Use the on-screen controls to drive the car

> The app communicates with the ESP32 over UDP on port `12345`, using the same command protocol as the Python gesture script.

---

## 🛠️ Tech Stack

### Software
| Tool | Purpose |
|---|---|
| Python 3.x | Main gesture recognition script |
| OpenCV | Webcam capture & frame rendering |
| MediaPipe | Real-time hand landmark detection |
| Socket (UDP) | Wireless command transmission |
| Arduino IDE | ESP32 firmware development |
| ESP Car Controller (APK) | Mobile remote control app |

### Hardware
| Component | Role |
|---|---|
| ESP32 NodeMCU | Microcontroller + Wi-Fi receiver |
| L298N Motor Driver | Controls DC motor speed & direction |
| DC Motors (x2) | Drives the car wheels |
| Chassis + Wheels | Physical car body |
| Power Supply | Battery pack for ESP32 & motors |

---

## 📁 Project Structure

```
iot-hand-gesture-controlled-car/
│
├── Hand(allab).py                # Python gesture recognition & UDP sender
├── aigesturecontrolmaincode.ino  # ESP32 Arduino firmware (UDP receiver + motor control)
├── base.apk                      # ESP Car Controller Android app
└── README.md
```

---

## ⚙️ Setup & Installation

### 1. Python Side (PC / Laptop)

**Install dependencies:**
```bash
pip install opencv-python mediapipe
```

**Run the gesture controller:**
```bash
python "Hand(allab).py"
```

> Make sure your PC and the ESP32 are connected to the **same Wi-Fi network**.

---

### 2. ESP32 Side (Arduino IDE)

**Required Libraries** (install via Arduino Library Manager):
- `WiFi.h` (built-in with ESP32 board support)
- `AsyncUDP`
- `DataParser`

**Steps:**
1. Open `aigesturecontrolmaincode.ino` in Arduino IDE
2. Update the Wi-Fi credentials:
```cpp
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";
```
3. Update the IP address in `Hand(allab).py` to match your PC's local IP:
```python
robotAddressPort = ("YOUR_PC_IP", 12345)
```
4. Flash the firmware to your ESP32 and open Serial Monitor at `115200` baud to verify connection

---

## 📡 Communication Protocol

- **Protocol:** UDP (User Datagram Protocol)
- **Port:** `12345`
- **Message Format:** CSV string — `command,speed,0,0,0`
  - Example: `f,100,0,0,0` → Move Forward at speed 100

| Command | Meaning |
|---|---|
| `f` | Forward |
| `b` | Backward |
| `l` | Left |
| `r` | Right |
| `s` | Stop |

---

## 🔌 ESP32 Pin Configuration

| Pin | Connected To |
|---|---|
| GPIO 27 (IN1) | L298N Left Motor Input 1 |
| GPIO 26 (IN2) | L298N Left Motor Input 2 |
| GPIO 14 (ENA) | L298N Left Motor Enable |
| GPIO 25 (IN3) | L298N Right Motor Input 1 |
| GPIO 33 (IN4) | L298N Right Motor Input 2 |
| GPIO 32 (ENB) | L298N Right Motor Enable |

---

## 🚀 Future Improvements

- Add obstacle detection using ultrasonic sensors
- Expand gesture vocabulary (speed control, honk, lights)
- Integrate camera feed on the car for FPV (First Person View)

---

## 📄 License

Feel free to fork and build on it!
