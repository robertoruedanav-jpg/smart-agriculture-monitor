# Smart Agriculture Monitor

A solar-powered IoT monitoring system designed for precision agriculture. The device continuously measures soil moisture, soil temperature, ambient temperature and humidity, transmitting telemetry through an NB-IoT network to a cloud platform for remote monitoring.

The project has been developed around the Seeed Studio XIAO ESP32-C6 and is optimized for autonomous outdoor operation with ultra-low power consumption.

> **Current status:** Prototype completed and deployed in the field. The hardware is fully operational and sensor acquisition works correctly. NB-IoT communication is still under debugging after field deployment.

<p align="center">
  <img src="https://github.com/user-attachments/assets/1867d891-9a4f-42f5-8e89-9db010226c25" width="350" alt="Foto del Sensor 1">
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://github.com/user-attachments/assets/ef573ff3-573a-4905-888d-0805166f8f36" width="350" alt="Foto del Sensor 2">
</p>

---

# Features

- Solar-powered autonomous operation
- Ultra-low power design using ESP32 Deep Sleep
- Industrial Modbus RTU communication over RS485
- Dual industrial soil sensors
- Ambient temperature and humidity monitoring
- NB-IoT cellular connectivity
- Cloud data visualization
- Modular architecture for future irrigation automation

---

# Hardware

| Component | Description |
|-----------|-------------|
| Seeed Studio XIAO ESP32-C6 | Main microcontroller |
| SIM7070G | NB-IoT / LTE Cat-M modem |
| 2x UNSEN600 RS485 Sensors | Soil moisture and temperature |
| DHT22 | Ambient temperature and humidity |
| MAX3485 | RS485 transceiver |
| XL6019 Step-Up Converter | 5V power regulation |
| Solar panel | 3W |
| Integrated Battery | 4000 mAh |
| Relay Module | Power switching |
| MOSFET | Sensor power management |

---

# System Architecture

```
                 Solar Panel
                      │
                Battery (4000mAh)
                      │
                XL6019 Step-Up
                      │
              XIAO ESP32-C6
          ┌───────────┴───────────┐
          │                       │
      RS485 Bus               UART
          │                       │
  2x UNSEN600 Sensors       SIM7070G
          │                       │
      Soil Data             NB-IoT Network
                                  │
                             Thinger.io Cloud
                                  │
                          Future Raspberry Pi
                                  │
                             Water Pump Relay
```

---

# Technologies

- ESP32-C6
- Arduino Framework
- C++
- Modbus RTU
- RS485
- NB-IoT
- AT Commands
- Deep Sleep
- UART
- Thinger.io
- Hologram SIM
- Low Power Design

---

# Software Design

The firmware follows a single execution cycle optimized for minimum energy consumption.

```
Wake Up

↓

Power Sensors

↓

Read DHT22

↓

Read RS485 Sensors

↓

Connect NB-IoT

↓

Upload Data

↓

Sleep Modem

↓

Power Off Sensors

↓

Deep Sleep (20 minutes)
```

The `loop()` function is intentionally left empty since every measurement cycle is executed inside `setup()` before entering Deep Sleep.

---

# Communication

## Soil Sensors

- Protocol: Modbus RTU
- Physical Layer: RS485
- Transceiver: MAX3485
- Two sensors connected in parallel
- Individual Modbus Slave IDs

The firmware manually builds Modbus frames and parses the received registers.

---

## Cellular Network

Communication is performed using:

- SIM7070G
- NB-IoT
- UART AT Commands

The ESP32 communicates directly with the modem by sending AT commands to:

- Register on the network
- Activate PDP context
- Open TCP socket
- Send HTTP POST requests
- Close connection

---

# Power Optimization

One of the project's main goals is minimizing power consumption.

Implemented techniques include:

- ESP32 Deep Sleep
- Modem Sleep Mode
- Sensor power switching
- MOSFET-controlled peripherals
- Relay-controlled 5V devices
- RTC memory for preserving previous measurements
- Solar-powered battery charging

The device wakes up every **20 minutes**, performs all measurements, uploads data, and immediately returns to Deep Sleep.

---

# Cloud Platform

Telemetry is sent directly to **Thinger.io** through raw HTTP POST requests over the NB-IoT modem.

Collected variables include:

- Soil moisture (Sensor 1)
- Soil temperature (Sensor 1)
- Soil moisture (Sensor 2)
- Soil temperature (Sensor 2)
- Ambient temperature
- Ambient humidity

---

# Challenges

Several engineering challenges were addressed during development:

- Selecting an ultra-low-power architecture
- Providing enough UART interfaces for simultaneous peripherals
- Implementing industrial RS485 communication
- Changing Modbus slave addresses
- Optimizing battery consumption
- Integrating NB-IoT connectivity
- Designing a fully autonomous solar-powered device

---

# Current Status

✅ Hardware completed

✅ PCB and enclosure assembled

✅ Solar power system operational

✅ RS485 communication working

✅ Modbus protocol implemented

✅ Deep Sleep optimization completed

✅ Cloud platform configured

⚠️ Field deployment completed

⚠️ NB-IoT communication under debugging in rural coverage conditions

---

# Future Improvements

- Automatic irrigation control
- Raspberry Pi irrigation gateway
- MQTT communication
- OTA firmware updates
- LoRaWAN version
- Local SD card backup
- Battery monitoring
- Weather forecast integration

---

# Repository Structure

```
Smart-Agriculture-Monitor/

├── firmware/
├── docs/
├── hardware/
├── images/
├── README.md
└── LICENSE
```

---

# Author

Industrial IoT Project developed using ESP32-C6, RS485, NB-IoT and low-power embedded design for precision agriculture.
