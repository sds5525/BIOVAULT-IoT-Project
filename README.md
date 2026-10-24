# BIOVAULT 🧬🏛️

> **Smart Biological Specimen Preservation & IoT Threat Correlation System**  
> An intelligent multi-archive preservation platform integrating ESP32 sensor telemetry, Node-RED correlation logic, and a real-time React command center.

[![React](https://img.shields.io/badge/React-v19.2-blue?logo=react)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-v8.1-646CFF?logo=vite)](https://vitejs.dev/)
[![Hardware](https://img.shields.io/badge/ESP32-Arduino_C%2B%2B-teal?logo=espressif)](https://www.espressif.com/)
[![Middleware](https://img.shields.io/badge/Node--RED-Flows-red?logo=nodered)](https://nodered.org/)
[![MQTT](https://img.shields.io/badge/MQTT-EMQX_Broker-008080?logo=mqtt)](https://www.emqx.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#license)

---

## 📋 Overview

**BIOVAULT** is an IoT-enabled biological archive security and specimen preservation monitoring system. Designed for bio-repositories and high-security research archives (such as the Bengaluru Archive Facility), it tackles the dual challenges of critical specimen preservation and unauthorized physical intrusion.

By combining low-latency microcontrollers (**ESP32**), edge-layer rule and threat scoring engines (**Node-RED**), and a responsive web dashboard (**React + Vite**), BIOVAULT provides round-the-clock protection against environmental degradation, acoustic anomalies, unauthorized access, and cross-archive intrusion patterns.

---

## ✨ Features

- **🌡️ Environmental Condition Tracking**: Continuous sampling of ambient temperature and relative humidity via DHT11 sensors to safeguard organic tissue specimens from climate damage.
- **🚨 Specimen Displacement & Intrusion Radar**:
  - Ultrasonic sensor (HC-SR04) detects physical specimen displacement (`> 5 cm` delta triggers immediate security alert).
  - Multi-tier radar proximity zones in restricted vaults (caution at `<= 100 cm`, alert at `<= 50 cm`, critical breach at `<= 20 cm`).
- **🎙️ Acoustic Anomaly Detection**: Real-time microphone monitoring (analog levels + digital trigger threshold) catching suspicious disturbances during and after museum operating hours.
- **🧠 Cross-Archive Threat Correlation**: Node-RED edge correlation engine links suspicious events occurring across separate vaults (Archive A, B, and C) within an 8-second window to detect coordinated breaches.
- **👥 Dual-Sensor Directional Visitor Counter**: State-machine counter utilizing twin ultrasonic sensors to distinguish incoming vs. outgoing personnel and compute live facility occupancy.
- **🎛️ Operator Command Center**:
  - Interactive facility layout map highlighting live status (`SAFE`, `WARNING`, `CRITICAL`).
  - Specimen repository drilldown view with gauges, motion radar visualizers, and acoustic wave animations.
  - Manual operator overrides: Emergency Facility Lockdown and Archive C Restricted/Public mode toggle.
  - Safe alert acknowledgement protocol: Operators cannot dismiss alerts until displaced specimens or triggered emergency buttons are physically restored.
- **💡 Bi-Directional IoT Actuation**: Node-RED automatically publishes LED indicator signals (Red, Yellow, Green, White) and activates siren buzzers over MQTT based on dynamic threat levels.

---

## 🛠️ Tech Stack

### Frontend (Dashboard)
- **Framework**: [React 19](https://react.dev/) (`react`, `react-dom`)
- **Build Tool**: [Vite 8](https://vitejs.dev/) with Fast Refresh & HMR
- **Styling**: Pure Vanilla CSS (cyber-ops dark theme, glassmorphism, radar & wave animations)
- **Linting**: ESLint 10

### Edge Middleware & Business Logic
- **Engine**: [Node-RED](https://nodered.org/) (`BIOVAULT - NodeRed Flow.json`)
- **Protocols**: HTTP REST API (port `1880`) & MQTT Client
- **Rule Engine**: Threat score aggregation, multi-room correlation, visitor counter state machine

### Firmware & IoT Hardware
- **Microcontroller**: ESP32 Dev Module
- **Firmware Framework**: Arduino C++ (`biovault-arduino.ino`)
- **Libraries**: `WiFi.h`, `PubSubClient.h`, `DHT.h`
- **Sensors & Actuators**:
  - DHT11 Temperature & Humidity Sensor (Pin `GPIO 5`)
  - PIR Motion Sensor (Pin `GPIO 25`)
  - HC-SR04 Ultrasonic Distance Sensor (Trig: `GPIO 33`, Echo: `GPIO 32`)
  - Sound Sensor Module (Analog: `GPIO 27`, Digital: `GPIO 26`)
  - Status LEDs: Red (`GPIO 15`), Yellow (`GPIO 18`), Green (`GPIO 19`), White (`GPIO 23`)
  - External Siren Buzzer & Physical Emergency Push Button

### Communication Broker
- **MQTT Broker**: `broker.emqx.io` (Port `1883`)

---

## 📁 Project Structure

```bash
BIOVAULT-IoT-Project/
├── BIOVAULT - NodeRed Flow.json # Complete Node-RED middleware flow with API & threat logic
├── biovault-arduino.ino         # ESP32 firmware sketch handling sensor polling & MQTT pub/sub
├── index.html                   # Single-page HTML entry point
├── package.json                 # Node dependencies and project run scripts
├── vite.config.js               # Vite build configuration
├── eslint.config.js             # ESLint configuration
├── public/                      # Static web assets
│   ├── favicon.svg              # Facility vault favicon
│   └── icons.svg                # System icon sprite definitions
└── src/
    ├── main.jsx                 # React root mount script
    ├── App.jsx                  # Main application controller, polling loop & state machine
    ├── App.css                  # Core CSS design system, layout, and animations
    ├── index.css                # Base typography and resets
    ├── assets/
    │   └── hero.png             # UI branding and preview asset
    ├── components/
    │   ├── Header.jsx           # Top facility navigation and system branding bar
    │   ├── MuseumMap.jsx        # Architectural floor map showing active archive states
    │   ├── GalleryCard.jsx      # Summary card for individual specimen archives
    │   ├── Gauge.jsx            # Threat score visualizer gauge
    │   └── ManualControls.jsx   # Operator overrides (Lockdown, Access Mode, Latch Ack)
    ├── data/
    │   └── dummyData.js         # Initial archive metadata and fallback schemas
    └── pages/
        ├── Dashboard.jsx        # Main overview command center
        └── GalleryPage.jsx      # Granular archive telemetry view (Radar, Sound waves, Logs)
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js**: v18.0.0 or higher ([Download Node.js](https://nodejs.org/))
- **Node-RED**: Installed globally or locally:
  ```bash
  npm install -g --unsafe-perm node-red
  ```
- **Arduino IDE**: With [ESP32 Board Support](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html) installed.
- **Required Arduino Libraries**:
  - `PubSubClient` by Nick O'Leary
  - `DHT sensor library` by Adafruit

---

### Installation & Setup

#### 1. Clone the Repository
```bash
git clone <ADD REPOSITORY URL>
cd BIOVAULT-IoT-Project
```

#### 2. Install Frontend Dependencies
```bash
npm install
```

#### 3. Configure and Upload Firmware (ESP32)
1. Open [`biovault-arduino.ino`](biovault-arduino.ino) in the Arduino IDE.
2. Update WiFi credentials and broker configuration:
   ```cpp
   const char* ssid = "<YOUR_WIFI_SSID>";
   const char* password = "<YOUR_WIFI_PASSWORD>";
   const char* mqtt_server = "broker.emqx.io"; // or your local broker IP
   ```
3. Connect your ESP32 board via USB, select the corresponding COM port and **ESP32 Dev Module** board, then click **Upload**.
4. Open the Serial Monitor at **115200 baud** to verify WiFi and MQTT connection status.

#### 4. Import Node-RED Flows
1. Start Node-RED in your terminal:
   ```bash
   node-red
   ```
2. Open your browser and navigate to `http://localhost:1880`.
3. Click the menu (top right) ➔ **Import** ➔ select [`BIOVAULT - NodeRed Flow.json`](BIOVAULT%20-%20NodeRed%20Flow.json).
4. Verify the MQTT broker nodes match your broker configuration.
5. Click **Deploy**.

---

### Environment & Configuration Reference

| Parameter | Location | Description | Default Value |
| :--- | :--- | :--- | :--- |
| `ssid` | `biovault-arduino.ino` | Local Wi-Fi network SSID | `"OnePlus Nord CE 2"` *(sample)* |
| `password` | `biovault-arduino.ino` | Local Wi-Fi network password | `"santhu00"` *(sample)* |
| `mqtt_server` | `biovault-arduino.ino` | Target MQTT broker host | `"broker.emqx.io"` |
| `mqttPort` | `biovault-arduino.ino` | Target MQTT broker port | `1883` |
| Node-RED Port | Node-RED runtime | HTTP API base port for telemetry | `1880` |
| Polling Interval | `src/App.jsx` | Client API polling rate | `2000 ms` |

> [!CAUTION]
> Never commit real production Wi-Fi passwords or private broker credentials to version control. Replace default values before deploying in production.

---

### Running the Application

#### Start the React Frontend in Development Mode:
```bash
npm run dev
```
The application will launch at `http://localhost:5173`. Ensure Node-RED is running at `http://localhost:1880` to receive live telemetry.

#### Build for Production:
```bash
npm run build
```

#### Preview Production Build:
```bash
npm run preview
```

---

## 📡 Usage & System Interfaces

### REST API Endpoints (Node-RED)

The frontend polls and controls the IoT network through HTTP endpoints exposed by Node-RED:

| Method | Endpoint | Description | Payload Example |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/archive/A` | Retrieve telemetry & threat status for Archive A | — |
| `GET` | `/api/archive/B` | Retrieve telemetry & threat status for Archive B | — |
| `GET` | `/api/archive/C` | Retrieve telemetry & threat status for Archive C | — |
| `GET` | `/api/visitors` | Current directional visitor headcount | — |
| `POST` | `/api/archiveC/mode` | Toggle Archive C Restricted/Public mode | `{"restricted": true}` |
| `POST` | `/api/lockdown` | Activate/deactivate facility emergency lockdown | `{"enabled": true}` |
| `POST` | `/api/acknowledge` | Acknowledge active alerts (validated by sensor state) | — |

---

### Key MQTT Topics

| Topic | Direction | Payload | Description |
| :--- | :--- | :--- | :--- |
| `Articol/Temp` | ESP32 ➔ Broker | Float (e.g. `24.50`) | Ambient temperature in °C |
| `Articol/Humid` | ESP32 ➔ Broker | Float (e.g. `55.20`) | Relative humidity in % |
| `Articol/Distance` | ESP32 ➔ Broker | Float (e.g. `4.20`) | Ultrasonic proximity/displacement in cm |
| `Articol/PIR` | ESP32 ➔ Broker | `1` or `0` | Motion detection state |
| `Articol/SAnalog` | ESP32 ➔ Broker | Integer (`0` - `4095`) | Analog acoustic intensity |
| `Articol/SDigital` | ESP32 ➔ Broker | `1` or `0` | Noise threshold exceeded flag |
| `Articol/LED` | Broker ➔ ESP32 | `on1`, `off1`, `on2`... | LED signaling actuation commands |
| `Articol/Buzzer` | Broker ➔ Actuator | `"ON"` / `"OFF"` | High-threat audible alarm activation |

---

## 🧪 Testing & Code Quality

Run ESLint to check for syntax and code formatting issues:
```bash
npm run lint
```

### Hardware Verification Steps:
1. **Sensor Loopback**: Open the ESP32 Serial Monitor (`115200 baud`) to verify readings for DHT11, PIR, sound, and ultrasonic sensors.
2. **Node-RED Debug Tab**: Inspect the debug nodes in Node-RED (`debug 11`, `debug 13`) to ensure inbound MQTT packets update the global context.
3. **API Validation**: Test the endpoint with curl or your browser:
   ```bash
   curl http://localhost:1880/api/archive/A
   ```

---

## 🗺️ Roadmap & Future Improvements

- [ ] **WebSocket Migration**: Replace 2-second HTTP polling with native WebSocket communication for true sub-second reactivity.
- [ ] **Role-Based Authentication**: Introduce JWT authentication for operator overrides and mode controls.
- [ ] **Time-Series Telemetry Store**: Connect Node-RED to InfluxDB + Grafana for archival climate auditing and specimen exposure reporting.
- [ ] **TLS / Secure MQTT**: Implement MQTTS (`port 8883`) with x509 client certificates on ESP32 nodes.
- [ ] **Hardware Watchdog**: Implement ESP32 hardware watchdog timers for automatic self-recovery during network partitions.

