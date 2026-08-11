![SL-OmniTrack Banner](Readme%20Resources/Banner_1.png)

## 1. What is this Project?

Name — SL-OmniTrack
"SL" stands for my future startup called "Silicon Labs" and "OmniTrack" is the kit name. This is a kit which includes 2 main things:

- **Ground Station** — a 3D-printed enclosure housing a microcontroller, LoRa transceiver, and a USB cable that connects it to a laptop/PC. It receives telemetry from the avionics bay and displays system health on an onboard OLED.

- **Avionics Bay** — a cylindrical enclosure that flies inside the rocket. It houses a microcontroller, IMU, barometric sensor, GPS module, MicroSD module (for onboard flight data logging), and a power package. (Component list changes are tracked in `BOM.md` — treat that as the source of truth over this README.)

- Both modules use the same microcontroller (ESP32-S3). As soon as each board powers on, it connects to the other wirelessly over LoRa — the avionics bay transmits telemetry, and the ground station receives and displays it.
- Enclosures are designed to give freedom to flash your own code.

---

## 2. Declaration of Use of AI

- Finding and comparing libraries for sensors
- GitHub Copilot for small errors whose causes are buried deep in library changelogs
- Learning Quaternions, sensor fusion algorithms, and the underlying math in detail

---

## 3. Steps to Recreate This Project

> **Build status:** This project has not yet been physically built. This submission is at the design and component-sourcing stage.

### Scope Note
This section currently covers component sourcing for SL-OmniTrack V1, which is scoped to data-gathering only (no active thrust vector control or onboard closed-loop sensor fusion).

Several steps below are **paused** or **design-stage** rather than complete, each for a specific reason explained in that step. Nothing here is presented as built, tested, or verified unless explicitly stated. All remaining steps will be completed as soon as the components arrive and physical assembly begins.

---

### Prerequisites
Before starting, you'll need:
- A 3D printer capable of printing PLA (this project uses an Anycubic Kobra 2 Neo)
- Arduino IDE or PlatformIO, for flashing the firmware
- Basic soldering equipment, for when wiring work begins

---

### Sourcing the Components
The full, current component list including every component needed for this project is listed in [`BOM.md`](./Docs/BOM.md).

---

### 3D Printing the Enclosures — Paused pending funded components
**Why paused:** Final enclosure dimensions depend on physically wiring all components together first (ESP32-S3, IMU, barometer, GPS, LoRa module, MicroSD module, battery, MT3608, BMS) to confirm actual fit and clearance. Several of these components aren't sourced yet. Sizing the enclosure before that step risks a design that doesn't match the real component footprint, so this step is intentionally held until the funded components arrive.

Two prototype iterations exist in `CAD Files/STEP Files/` and are kept for reference only — neither is final:
- **`Avionic Bay Prot 2 (Optimized for 3d printing).step`** — later iteration, print-optimized geometry
- **`Avionics Bay Prot 1.step`** — earlier prototype
- **`Perf Board 49.62mm.step`** — model of the perforated board intended to mount inside the avionics bay holder

Final enclosure dimensions, print settings, and the confirmed CAD file will be published here once components are sourced, wired, and physically fitted.

---

### Mechanical Assembly — Paused, same reason as above
**Why paused:** Assembly steps depend on the confirmed enclosure from the previous step, which itself depends on funded components arriving and being physically fitted. This section will be completed once that happens.

---

### Planned Wiring — Design-stage, not yet physically built
**Why not final:** Wiring can only be physically confirmed once all components are on hand and connected. Exact GPIO pin assignments aren't published because they haven't been verified via silkscreen cross-check, vendor documentation, or multimeter/I2C address scanning yet. What follows is the intended bus-level architecture, not a build record.

The system uses two ESP32-S3 boards (7Semi 1U-N8R8), one per module, communicating with each other over LoRa (SX1278 @ 433MHz) rather than any direct wired link.

**Avionics Bay — planned bus layout:**

| Component | Interface | Power Source |
|---|---|---|
| MPU9250/MPU6500 (IMU) | I2C | ESP32-S3 onboard 3.3V regulator |
| BMP280 (barometer) | I2C | ESP32-S3 onboard 3.3V regulator |
| SX1278 LoRa module | SPI | ESP32-S3 onboard 3.3V regulator |
| NEO-6M GPS | UART | 5V rail (via MT3608 boost) |
| MicroSD module | SPI | 5V rail (via MT3608 boost) |
| ESP32-S3 | — | 5V rail (via MT3608 boost) |

Power chain: single 18650 cell → 1S BMS → MT3608 boost converter (set to 5V output) → ESP32-S3, GPS, MicroSD. Sensors and LoRa draw off the ESP32-S3's own onboard 3.3V regulator rather than the boosted 5V rail.

**Ground Station — planned bus layout:**

| Component | Interface | Power Source |
|---|---|---|
| SX1278 LoRa module | SPI | ESP32-S3 onboard 3.3V regulator |
| 1.3" I2C OLED | I2C | ESP32-S3 onboard 3.3V regulator |
| ESP32-S3 | — | USB (laptop) |

The ground station is USB-powered only — no battery, no separate regulation stage. If the onboard 3.3V regulator proves insufficient for the LoRa module once built, a dedicated regulator with a decoupling capacitor is the planned fallback.

---

### Planned Firmware Architecture — Design-stage, not yet flashed or tested
**Why not final:** Firmware logic depends on the wiring above, which itself is unverified. What follows is the intended software architecture, not working or tested code.

**Avionics Bay firmware plan:**
1. **Sensor init** — bring up I2C bus, initialize IMU and barometer, initialize GPS over UART, mount SD card over SPI.
2. **Sensor read loop** — poll IMU (accel/gyro), barometer (pressure/altitude), and GPS (lat/long/altitude/fix status) at a fixed interval.
3. **LoRa packet structure** — pack a compact telemetry frame (timestamp, orientation, altitude, GPS coordinates, system status flags) and transmit over SX1278.
4. **SD logging** — write the same telemetry frame to the microSD card as a local backup, independent of LoRa transmission success.

**Ground Station firmware plan:**
1. **LoRa receive loop** — listen for incoming telemetry packets from the avionics bay.
2. **OLED display update** — parse the received packet and show system health/status (link status, last-received timestamp, battery/GPS fix indicators).
3. **USB serial forwarding** — relay raw received telemetry over USB to the companion desktop application for EKF-based fusion and live plotting.

This architecture is scoped to data collection and relay only — no onboard closed-loop control or sensor fusion happens on either microcontroller in V1; that processing is deferred to the desktop companion app (fusion) and V2 hardware (active control).

---

### What's Not Covered Yet
The following are next steps, contingent on this funding round supplying the remaining components:
- Enclosure sizing and 3D printing
- Mechanical assembly
- Physical wiring and confirmed GPIO pin assignments (`WIRING.md`)
- Firmware implementation, flashing, and on-hardware testing
- Sensor calibration procedures
- End-to-end LoRa link and telemetry verification

---

**IMPORTANT** — AI has only been used as mentioned in Section 2, unless explicitly mentioned otherwise.