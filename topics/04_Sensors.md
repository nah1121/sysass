# Topic 4 — Types of Sensors Used in Aircraft Hydraulic Systems

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Overview and Sensor Selection Criteria](#1-overview-and-sensor-selection-criteria)
2. [Pressure Sensors (Transducers)](#2-pressure-sensors-transducers)
3. [Temperature Sensors](#3-temperature-sensors)
4. [Position Sensors](#4-position-sensors)
   - [LVDT — Linear Variable Differential Transformer](#41-lvdt--linear-variable-differential-transformer)
   - [RVDT — Rotary Variable Differential Transformer](#42-rvdt--rotary-variable-differential-transformer)
   - [Hall Effect Sensors](#43-hall-effect-sensors)
   - [Potentiometer-Based Position Sensors](#44-potentiometer-based-position-sensors)
5. [Fluid Level Sensors](#5-fluid-level-sensors)
6. [Flow Rate Sensors](#6-flow-rate-sensors)
7. [Vibration and Acoustic Sensors](#7-vibration-and-acoustic-sensors)
8. [Sensor Signal Conditioning and Output Standards](#8-sensor-signal-conditioning-and-output-standards)
9. [Sensor Redundancy and Voting Logic](#9-sensor-redundancy-and-voting-logic)
10. [References](#10-references)

---

## 1. Overview and Sensor Selection Criteria

Sensors in aircraft hydraulic systems convert **physical quantities** (pressure, temperature, displacement, fluid volume) into **electrical signals** that can be processed by flight computers, control units, and monitoring systems.

### Why Sensors Are Critical

Without sensors:
- Pilots cannot know the state of the hydraulic system
- Automatic fault detection is impossible
- Closed-loop control of actuators cannot function
- Predictive maintenance is impossible

### Sensor Selection Criteria for Aircraft Hydraulic Systems

| Criterion | Requirement |
|---|---|
| **Accuracy** | Sufficient for the safety criticality of the function |
| **Range** | Covers entire operating envelope plus margins |
| **Response time** | Fast enough for the control loop (e.g., EHSV feedback: <1 ms) |
| **Reliability (MTBF)** | Typically >10,000 flight hours for DAL B sensors |
| **Environmental tolerance** | DO-160G: −55°C to +125°C; 6g vibration; 95% humidity; hydraulic fluid immersion |
| **EMI resistance** | Shielded output; differential signal; DO-160G Section 19 |
| **Size and weight** | Minimised (aircraft weight is always a premium) |
| **Qualification** | DO-160G environmental; RTCA DO-178C/DO-254 for embedded electronics |

---

## 2. Pressure Sensors (Transducers)

### What They Measure

A pressure transducer converts hydraulic fluid pressure into a **proportional electrical signal** (analogue voltage, analogue current, or digital output).

Unlike a pressure *switch* (which only provides on/off), a pressure transducer provides a **continuous output** proportional to pressure — e.g., 0.5 V at 0 psi rising to 4.5 V at 5,000 psi.

### Operating Principles

#### a) Strain Gauge (Piezoresistive) Transducer

The most common type in aircraft hydraulic systems.

```
    Hydraulic Pressure (P)
           │
    ┌──────▼──────┐
    │  Stainless  │
    │  Diaphragm  │ ← Deforms under pressure
    └──────┬──────┘
           │  Strain transferred to:
    ┌──────▼──────┐
    │ Strain Gauge│ ← Bonded to diaphragm surface
    │  Wheatstone │   Resistance changes with strain
    │   Bridge    │
    └──────┬──────┘
           │ Millivolt output → Signal conditioning → 0–5V or 4–20mA
```

- **Output:** 0–5V (ratiometric) or 4–20 mA (current loop — immune to long-wire voltage drops)
- **Accuracy:** Typically ±0.25% FS (full scale)
- **Used for:** All general hydraulic system pressure monitoring

#### b) Piezoelectric Transducer

- A quartz or ceramic crystal generates an electric charge when mechanically stressed.
- **Advantages:** Very fast response (microseconds); good for dynamic pressure measurement.
- **Disadvantage:** Cannot measure static (steady-state) pressure (charge leaks away).
- **Used for:** Pump pressure ripple analysis; vibration-induced pressure monitoring.

#### c) Capacitive Transducer

- Pressure deflects a diaphragm, changing the gap between two capacitor plates.
- The capacitance change is measured electronically.
- **Advantages:** Very low power; good long-term stability.
- **Used for:** MEMS-based miniature sensors in modern avionics.

#### d) MEMS Pressure Sensor

- Silicon microfabrication creates a tiny diaphragm with integrated piezoresistors.
- Digital SPI or I²C bus output directly readable by microcontrollers.
- **Advantages:** Small, light, cheap.
- **Used for:** On newer aircraft, embedded in hydraulic manifold blocks to monitor individual actuator circuits.

### Aircraft Pressure Measurement Ranges

| Application | Pressure Range | Sensor Accuracy Required |
|---|---|---|
| System pressure (3,000 psi system) | 0–3,500 psi | ±15 psi (±0.5% FS) |
| Actuator differential pressure | ±3,000 psi | ±30 psi (±1% FS) |
| Filter differential pressure | 0–100 psi | ±2 psi |
| Brake pressure | 0–3,000 psi | ±15 psi |
| Accumulator pre-charge | 0–1,500 psi | ±25 psi |

### Aircraft Example — Boeing 777 HSMU

Three systems (Left, Centre, Right) each have:
- **2 × EDP outlet pressure transducers** (one per EDP)
- **2 × ACMP outlet pressure transducers**
- **1 × system return line pressure transducer**
- **2 × differential pressure switches** (filter monitoring)

All 15+ transducer outputs are read by the **Hydraulic System Monitoring Unit (HSMU)** at 100 ms intervals. The HSMU displays pressures on the Systems Display and logs trends.

---

## 3. Temperature Sensors

### What They Measure

Temperature sensors monitor the thermal state of hydraulic fluid, pump casings, and actuators. They prevent overheating damage to seals, fluid, and pumps.

### Types Used

#### a) Thermocouple (Type K Most Common)

- **Principle:** The **Seebeck effect** — two dissimilar metal wires (Chromel and Alumel for Type K) joined at a junction generate a voltage proportional to the temperature difference between the junction and a reference.
- **Output:** ~41 µV/°C (Type K)
- **Range:** −200°C to +1,200°C
- **Accuracy:** ±2°C
- **Advantages:** Very wide temperature range; rugged; self-powered (no excitation required)
- **Disadvantages:** Requires cold junction compensation; relatively low signal level (susceptible to EMI)
- **Aircraft use:** Engine nacelle hydraulic line temperature monitoring; high-temperature pump monitoring

#### b) Resistance Temperature Detector (RTD) — PT100/PT1000

- **Principle:** Platinum wire resistance increases linearly with temperature. PT100 = 100 Ω at 0°C. PT1000 = 1,000 Ω at 0°C.
- **Range:** −200°C to +850°C
- **Accuracy:** ±0.1°C (Class A)
- **Advantages:** High accuracy; stable; standardised (IEC 60751)
- **Disadvantages:** Requires excitation current; self-heating error if current too high; fragile in high-vibration environments
- **Aircraft use:** Hydraulic reservoir temperature monitoring (most common for accuracy-critical applications)

#### c) Thermistor (NTC)

- **Principle:** A semiconductor whose resistance **decreases** non-linearly with temperature (Negative Temperature Coefficient — NTC).
- **Range:** −40°C to +150°C
- **Accuracy:** ±0.5°C
- **Advantages:** Very high sensitivity at low temperatures; cheap; small
- **Disadvantages:** Non-linear (requires linearisation); narrow range; ageing
- **Aircraft use:** Hydraulic pump inlet temperature monitoring; fluid temperature warning threshold detection

### Temperature Thresholds in Aircraft Hydraulic Systems

| Threshold | Temperature | Action |
|---|---|---|
| Normal operation | −40°C to +100°C | No action |
| Viscosity increase warning (cold) | Below −30°C | Ground warm-up required before flight |
| High temperature caution | >100°C | Monitor; increase monitoring frequency |
| High temperature warning | >120°C | Reduce hydraulic load; crew alert |
| Pump shutdown threshold | >135°C | Automatic pump offload or shutdown |
| Skydrol auto-ignition | ~200°C | Fire risk zone — isolation required |

### Aircraft Example — Airbus A380

The A380 has four hydraulic systems. Each system's reservoir is fitted with an **RTD sensor**. Outputs are read by the **Hydraulic Monitoring Computer (HMC)**. When fluid temperature exceeds 120°C:
- ECAM displays: **"HYD [SYS] OVHT"** advisory (amber)
- Crew procedures: Reduce simultaneous actuator use; if temperature continues rising, isolate system

---

## 4. Position Sensors

Position sensors provide the **position feedback** necessary for closed-loop control of hydraulic actuators and for confirming that mechanical components (landing gear, doors, valves) have reached their commanded positions.

### 4.1 LVDT — Linear Variable Differential Transformer

#### What It Is

The LVDT is the most widely used linear position sensor in aerospace. It is **contactless**, has **no friction**, **infinite resolution**, and is **immune to fluid contamination** — making it ideal for hydraulic actuator feedback.

#### How It Works

```
    ┌───────────────────────────────────────────────────────┐
    │  Primary Coil (AC excitation, 3–10 kHz)               │
    │  ═══════════════════════════════════════              │
    │            ┌──────────────────┐                       │
    │ SecondaryA │ ╔══════════════╗ │ SecondaryB            │
    │ ═══════════│║  FERROMAGNETIC║│═══════════             │
    │            │║     CORE      ║│ (moves with            │
    │            │║  (attached to ║│  actuator rod)         │
    │            │║  actuator rod)║│                        │
    │            └╚══════════════╝┘                        │
    └───────────────────────────────────────────────────────┘

Core Centred → V(SecA) = V(SecB) → Output = 0 V

Core towards A → V(SecA) > V(SecB) → Positive output (proportional to displacement)

Core towards B → V(SecB) > V(SecA) → Negative output (proportional to displacement)
```

The **output voltage magnitude** indicates displacement magnitude; the **phase** (0° or 180°) indicates direction.

#### LVDT Specifications (Typical Aircraft Grade)

| Parameter | Value |
|---|---|
| Excitation frequency | 3–10 kHz (typically 5 kHz) |
| Excitation voltage | 3–10 V RMS |
| Full-scale stroke | ±0.5 mm to ±250 mm (various models) |
| Output sensitivity | 10–100 mV/V/mm |
| Resolution | Theoretically infinite (practically limited by signal conditioning noise) |
| Linearity | ±0.1% FS |
| Temperature range | −55°C to +125°C |
| Qualification | MIL-PRF-17693 (aerospace LVDTs) |

#### Aircraft Applications

| Application | Stroke | Why LVDT |
|---|---|---|
| EHSV spool position | ±1–3 mm | High accuracy needed for closed-loop valve control |
| Aileron/elevator actuator position | ±50–150 mm | Provides surface angle feedback to FCC |
| Rudder travel limiter | ±30 mm | Measures allowable rudder travel vs. airspeed |
| Nose-wheel steering actuator | ±80 mm | Provides steering angle feedback to BSCU |
| Flap/slat actuation monitoring | ±200 mm | Confirms correct track position |

---

### 4.2 RVDT — Rotary Variable Differential Transformer

The RVDT is the rotational equivalent of the LVDT. It measures **angular displacement** using the same electromagnetic induction principle but with a rotating core.

- **Output:** Proportional voltage for angular position from ±30° to ±60° (varies by model)
- **Applications:**
  - Flap track angular feedback (output shaft of gearbox)
  - Control column and control wheel position sensing
  - Throttle lever angle (TLA) sensing linked to hydraulic thrust reverser control
  - Helicopter rotor pitch angle

---

### 4.3 Hall Effect Sensors

#### Principle

The **Hall effect** is the production of a voltage (Hall voltage) across a conductor when a magnetic field is applied perpendicular to current flow. A Hall effect sensor uses this principle to detect the presence and strength of a magnetic field.

In aircraft proximity switches, a small permanent magnet is mounted on the moving component. When it passes within range (~5–20 mm) of the Hall sensor:
- The sensor output switches from LOW to HIGH (digital mode)
- Or produces a voltage proportional to field strength (analogue mode)

#### Advantages over Mechanical Microswitches

| Attribute | Mechanical Microswitch | Hall Effect Sensor |
|---|---|---|
| Moving parts | Yes (plunger, spring, contacts) | None |
| Contact wear | Yes | No |
| Response to vibration | May chatter | Immune |
| Sensitivity to contamination | Contact fouling | None (sealed solid-state) |
| Response time | 10–30 ms | <1 ms |
| Temperature range | −40°C to +85°C | −55°C to +150°C |

#### Aircraft Applications

- **Landing gear proximity sensors (Boeing 737 NG/MAX):** Hall effect sensors replaced older mechanical switches for main gear uplock/downlock detection, improving reliability and reducing false indications.
- **Cargo door latch detection (Boeing 787):** Hall sensors confirm each latch bolt is in the locked position before pressurisation.
- **Thrust reverser sleeve position (Airbus A320):** Hall sensors confirm reversers are fully stowed.

---

### 4.4 Potentiometer-Based Position Sensors

A **linear potentiometer** is a resistive track with a sliding wiper. As the actuator rod moves, the wiper moves along the track, changing the measured resistance proportionally to displacement.

- **Output:** Resistance or voltage (ratiometric)
- **Advantages:** Simple; cheap; easy to interface
- **Disadvantages:** Mechanical wear on sliding contact; limited life; susceptible to contamination
- **Applications:** Non-critical position monitoring only (cargo door coarse position, fuel panel lever position) — not used for primary flight control feedback due to wear limitations.

---

## 5. Fluid Level Sensors

### What They Measure

Fluid level sensors monitor the **amount of hydraulic fluid in the reservoir**. This is the primary early indicator of a hydraulic leak.

### Why Level Monitoring Is Critical

- A reservoir running completely dry allows the pump to ingest air → **cavitation** → pump destroyed in seconds.
- Modern aircraft implement **two-level detection:**
  - **Low-level warning** (e.g., reservoir at 50%): "Monitor and plan for maintenance"
  - **Critically low warning** (e.g., reservoir at 20–25%): "Isolate the system now"

### Sensor Types

#### a) Float Switch

```
    Reservoir Wall
         │
    ┌────┴────┐
    │  Float  │ ← Buoyant; rises and falls with fluid surface
    │  (ball) │
    └────┬────┘
         │ (pivot arm)
    [Switch Contact] ← Opens when float drops below set level
```

- Simple; reliable; cheap.
- Provides ON/OFF only — does not show trending.
- Susceptible to aeration (bubbles can float the switch falsely).

#### b) Capacitive Level Sensor

- Measures the **dielectric constant difference** between hydraulic fluid (high dielectric) and air (low dielectric).
- As fluid level drops, the capacitance measured by the sensor decreases.
- **Continuous output** proportional to fluid level (e.g., 0–100%).
- **Advantages:** No moving parts; continuous level indication; not affected by aeration.
- **Used on Boeing 777, Airbus A380** for continuous reservoir level display to crew.

#### c) Ultrasonic Level Sensor

- An ultrasonic transducer emits a pulse toward the fluid surface; measures the **time-of-flight** of the echo.
- **Advantages:** Non-contact; continuous measurement; works with any fluid.
- **Disadvantages:** Affected by foam on fluid surface; complex electronics.
- **Used on:** Some modern aircraft for fuel quantity measurement; adapted for hydraulic reservoirs on advanced platforms.

#### d) Magnetic Float with Reed Switches

- A float with embedded permanent magnets rises and falls with fluid level.
- Reed switches at specific positions on the reservoir wall activate as the magnet passes.
- Provides **discrete level indication** (e.g., "above half," "below half," "critically low").
- Used on many transport aircraft as a backup to capacitive sensors.

### Aircraft Example — Boeing 737NG

Each hydraulic reservoir (A and B) has a **float-type level transmitter** that provides a continuous signal to the hydraulic indicators on the overhead panel. A red **LOW LEVEL** warning light illuminates when the fluid level drops below a pre-set minimum (approximately 1/3 of normal capacity). The QRH directs crew to check for leaks and cross-feed if necessary.

---

## 6. Flow Rate Sensors

### What They Measure

Flow rate sensors measure the **volumetric flow rate** of hydraulic fluid (litres/minute or gallons/minute), providing more diagnostic information than simple flow switches.

### Turbine Flow Meter

```
    Flow direction →
    ┌─────────────────────────────┐
    │   ╔═══╗  ╔═══╗  ╔═══╗      │
    │   ║   ║  ║   ║  ║   ║      │
    │   ╚═══╝  ╚═══╝  ╚═══╝      │  ← Turbine spins proportional to flow
    └─────────────────────────────┘
                 │
           [Magnetic pickup]  ← Counts blade passes per unit time
                 │
           Pulse output → Flow computer → Litres/min
```

- **Accuracy:** ±1–2%
- **Typical range:** 0–50 litres/min (per system)
- **Aircraft use:** Pump performance monitoring; maintenance diagnostics

### Coriolis Flow Meter

- **Most accurate flow measurement technology** (±0.1%)
- Measures the Coriolis effect on a vibrating U-tube through which fluid flows.
- Mass flow measurement (not just volumetric).
- Expensive and heavy — used only where very high accuracy is required.

### Applications

- **Pump performance trending:** A pump delivering 20% less than rated flow may have worn vanes or a developing internal leak — HUMS detects this trend before failure.
- **Leak detection:** Comparing flow in vs. flow out of a circuit can detect small internal actuator leaks.
- **Maintenance diagnostics:** During ground test, flow measurements verify that valve orifices are not restricted.

---

## 7. Vibration and Acoustic Sensors

### What They Detect

These sensors monitor the **mechanical condition** of hydraulic pumps, actuators, and valves by detecting abnormal vibration signatures or acoustic emissions.

### Accelerometer (Vibration Sensor)

- A piezoelectric crystal generates charge proportional to acceleration.
- Mounted on pump housing or actuator body.
- **Fast Fourier Transform (FFT) analysis** of vibration spectrum detects:
  - Bearing defect frequencies (BPFI, BPFO, BSF, FTF)
  - Pump vane passing frequency changes (indicating worn vanes)
  - Resonance changes (indicating structural looseness)

### Acoustic Emission Sensor

- Detects **high-frequency stress waves** (100 kHz–1 MHz) from:
  - **Cavitation** in pumps (collapsing vapour bubbles create distinct acoustic signature at >100 kHz)
  - **Crack propagation** in highly stressed components
  - **Spool valve stiction** (stick-slip generates characteristic low-frequency noise)

### Integration with HUMS

Data from vibration and acoustic sensors feeds into **Health and Usage Monitoring Systems (HUMS)**, which:
- Process signal data in real time
- Compare against baseline and degradation thresholds
- Generate maintenance alerts when thresholds are exceeded
- Enable **Predictive Maintenance** — replacing components based on condition rather than fixed hours

**Example:** A helicopter hydraulic pump's vibration spectrum shows increasing energy at the vane passing frequency over 200 flight hours → HUMS generates an alert: "Hydraulic pump vane wear detected — replace at next 50-hour check" → pump is replaced before catastrophic failure.

---

## 8. Sensor Signal Conditioning and Output Standards

Raw sensor signals (millivolts, resistance, charge) must be conditioned before use by flight computers or displays.

### Signal Conditioning Functions

| Function | Purpose |
|---|---|
| **Amplification** | Boost low-level sensor signals (mV → V range) |
| **Filtering** | Remove high-frequency noise (EMI, vibration) |
| **Linearisation** | Convert non-linear sensor output (e.g., thermistor) to linear temperature |
| **Offset removal** | Zero the output at known reference condition |
| **Analogue-to-Digital Conversion** | Convert analogue voltage to digital for processor |
| **Isolation** | Prevent ground loops between sensor and system |

### Output Standards

| Standard | Signal Type | Application |
|---|---|---|
| 0–5 V ratiometric | Analogue voltage | Pressure transducers, LVDT demodulators |
| 4–20 mA current loop | Analogue current | Temperature sensors; long cable runs |
| ±10 V DC | Analogue differential | LVDT/RVDT position output |
| ARINC 429 | 100 kbps serial (differential) | Sensor data to avionics displays |
| RS-422 | Serial differential | Maintenance data |
| CAN Bus | 1 Mbps serial | Modern integrated sensor networks |

---

## 9. Sensor Redundancy and Voting Logic

For **DAL A and B** functions (see [Topic 10](./10_Certification_and_Safety_Standards.md)), single sensor failures cannot cause loss of function. **Redundancy** and **voting logic** are used.

### Triplex Redundancy (Most Common for DAL A)

Three independent sensors measure the same parameter. The flight computer uses a **median select (two-out-of-three voting)** algorithm:

```
Sensor 1: 28.3°
Sensor 2: 28.5°   →   Median = 28.5°  (Sensor 3 voted out as outlier)
Sensor 3: 55.0°   ←   FAILED (stuck high)
```

The median selects the middle value. A single failed sensor (either high or low) is automatically rejected. The system continues to operate normally. A maintenance alert is generated.

### Duplex Redundancy (DAL B)

Two independent sensors. If they disagree beyond a threshold:
- System cannot identify which has failed.
- Generates a fault alert; system may revert to a conservative (safe) mode.
- A third source (computed from other data) may be used to arbitrate.

### Cross-Checking

Even without hardware redundancy, sensors can be cross-checked against **physically related parameters**:
- If actuator position LVDT shows full extension but actuator pressure sensor shows zero extend pressure → LVDT may be failed.
- If fluid temperature is 40°C but fluid level sensor shows rapidly decreasing level → active leak detected.

---

## 10. References

1. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
2. Fraden, J. — *Handbook of Modern Sensors: Physics, Designs, and Applications* (5th ed.). Springer, 2016. ISBN 978-3-319-19302-1.
3. Doebelin, E.O. — *Measurement Systems: Application and Design* (5th ed.). McGraw-Hill, 2004.
4. Herceg, E.E. — *Handbook of Measurement and Control*. Schaevitz Engineering, 1976. (LVDT reference)
5. Parker Hannifin — *Hydraulic Sensor Product Catalogue*. Parker, 2020.
6. Emerson Rosemount — *Pressure Transmitter Reference Manual*. 2018.
7. FAA — *Aviation Maintenance Technician Handbook — Airframe, Vol. 2*. FAA-H-8083-31A.
8. DO-160G — *Environmental Conditions and Test Procedures for Airborne Equipment*. RTCA, 2010.
9. MIL-PRF-17693 — *Linear Variable Differential Transformers (LVDTs), General Specification for*. US DoD.
10. IEC 60751 — *Industrial Platinum Resistance Thermometers and Platinum Temperature Sensors*. IEC, 2008.

---

*Previous topic: [Topic 3 — Types of Electrical Components Used in Hydraulic Systems](./03_Electrical_Components.md)*
*Next topic: [Topic 5 — How Electrical Signals Interact with Hydraulic Valves and Actuators](./05_Electrical_Signals_and_Hydraulic_Components.md)*
