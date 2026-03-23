# Topic 2 — The Role of Electrical Systems in Hydraulic Systems

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Why Electrical and Hydraulic Systems Are Used Together](#1-why-electrical-and-hydraulic-systems-are-used-together)
2. [Role 1 — Control](#2-role-1--control)
3. [Role 2 — Monitoring](#3-role-2--monitoring)
4. [Role 3 — Automation](#4-role-3--automation)
5. [Integration Architecture: How the Two Systems Connect](#5-integration-architecture-how-the-two-systems-connect)
6. [How Electrical Systems Improve Efficiency and Reliability](#6-how-electrical-systems-improve-efficiency-and-reliability)
7. [Worked Example: Hydraulic Pump Management on the Boeing 777](#7-worked-example-hydraulic-pump-management-on-the-boeing-777)
8. [References](#8-references)

---

## 1. Why Electrical and Hydraulic Systems Are Used Together

Hydraulic systems are outstanding at **transmitting large forces** through small-diameter pipes over long distances. A single hydraulic circuit running from an engine-driven pump to a wing-mounted actuator can deliver tens of thousands of newtons of force through a pipe no wider than 25 mm in diameter.

However, hydraulic systems are inherently passive. A hydraulic actuator will extend or retract based purely on **which port has higher pressure** — it has no awareness of:
- Where it currently is in its stroke
- How fast it is moving
- Whether it has reached its commanded position
- Whether the system pressure is adequate
- Whether the fluid temperature is within safe limits

**Electrical systems fill every one of these gaps.** They provide the sensing, the decision-making, and the control signals that give hydraulic systems their intelligence.

### The Fundamental Divide

| Attribute | Hydraulic System | Electrical System |
|---|---|---|
| **Energy form** | Pressurised fluid (mechanical) | Voltage/current (electromagnetic) |
| **Strength** | Very high force | Very low force |
| **Precision** | Low (without feedback) | Very high |
| **Intelligence** | None | Unlimited (with processors) |
| **Speed of response** | Limited by fluid dynamics | Near-instantaneous (speed of light) |
| **Weight per unit power** | Low (for high power) | High (for high power) |

The most effective aircraft systems **combine both**: the hydraulic system provides the power; the electrical system provides the control.

---

## 2. Role 1 — Control

### 2.1 What "Control" Means in a Hydraulic Context

Control means determining **when**, **how much**, and **in which direction** hydraulic fluid flows to an actuator. In a simple hand-operated system, the pilot physically moves a lever connected to a valve spool. In an electrohydraulic system, an electrical signal moves the valve spool — the pilot or computer need only generate that signal.

### 2.2 Open-Loop vs. Closed-Loop Control

**Open-loop (no feedback):** An electrical signal commands the valve to move; there is no confirmation of whether the actuator reached the commanded position.
- Used for: simple switching functions (e.g., nose wheel steering unlock, parking brake)

**Closed-loop (feedback):** A position sensor continuously reports the actuator's actual position back to the controller, which adjusts the valve signal until the commanded position is achieved.
- Used for: all primary flight control surfaces, autopilot, trim

```
CLOSED-LOOP CONTROL DIAGRAM

  Command Signal ──► [Summing Junction] ──► [Controller] ──► [Servo Valve] ──► [Hydraulic Actuator]
                            ▲                                                          │
                            │                                                          ▼
                            └─────────────── [Position Sensor / LVDT] ◄───────────────┘
                                              (Feedback Signal)
```

### 2.3 Electrical Control Chain — Step by Step

```
1. Pilot Input or Computer Command
         ↓
2. Signal Processing (Flight Control Computer)
         ↓
3. Electrical Command Signal (voltage or current)
         ↓
4. Solenoid / Servo Valve Coil Energised
         ↓
5. Valve Spool Shifts — Hydraulic Port Opens
         ↓
6. High-Pressure Fluid Flows to Actuator
         ↓
7. Actuator Piston Moves (force applied to control surface)
         ↓
8. Position Sensor Reports Actual Position
         ↓
9. Flight Computer Compares Command vs. Actual → Adjusts Signal
```

### 2.4 Precision of Electrical Control

| Control Method | Position Accuracy | Response Time |
|---|---|---|
| Manual cable/rod | ±5 mm | Limited by pilot muscle |
| Simple solenoid (on/off) | Two positions only | 50–200 ms |
| Proportional solenoid valve | ±1 mm | 20–50 ms |
| Electrohydraulic servo valve (EHSV) | ±0.05 mm | <5 ms |

This level of precision is what enables **fly-by-wire** systems to perform functions impossible with direct mechanical linkages:
- **Active flutter suppression:** Surface deflections of fractions of a millimetre at high frequency to cancel structural resonance
- **Gust load alleviation:** Anticipating and countering gusts before the airframe feels them
- **Envelope protection:** Preventing the pilot from commanding deflections that would overstress the structure

### 2.5 Real Example — Airbus A320 Rudder Control

On the A320, the rudder is controlled by the **Flight Augmentation Computer (FAC)**. The FAC sends current commands (±10 mA) to the EHSV on the rudder actuator. The FAC calculates:
- Pilot pedal input (LVDT on pedal assembly)
- Sideslip angle (yaw rate sensor)
- Airspeed (Air Data Computers)
- Required yaw damping correction

The resulting EHSV command deflects the rudder to within ±0.1° of the computed target, **thousands of times per second**, providing smooth, precise yaw control that no pilot could achieve manually.

---

## 3. Role 2 — Monitoring

### 3.1 What Monitoring Provides

Monitoring is the continuous, automatic measurement of hydraulic system health parameters. It answers the question: **"Is the system working correctly right now?"**

Without monitoring, a hydraulic fault that developed slowly (e.g., a small leak gradually depleting the reservoir) would go undetected until complete system failure — potentially at a critical moment such as approach and landing.

### 3.2 Parameters Monitored Electrically

| Parameter | Sensor Used | Why It Matters |
|---|---|---|
| **Fluid pressure** | Pressure transducer | Confirms pumps are operating; detects leaks |
| **Fluid temperature** | RTD / Thermocouple | Prevents fluid degradation and seal damage |
| **Fluid level** | Float switch / Capacitive sensor | Early detection of reservoir leak |
| **Actuator position** | LVDT / RVDT | Confirms surface has reached commanded position |
| **Pump speed/load** | Tachometer / Current sensor | Detects pump wear or overload |
| **Filter differential pressure** | Differential pressure switch | Indicates filter clogging |
| **Valve position** | Proximity switch / LVDT | Confirms solenoid valve has shifted |

### 3.3 Monitoring System Architecture

The sensor outputs are processed through several layers:

1. **Local signal conditioning:** Sensor output (millivolts) amplified and filtered close to the sensor.
2. **Hydraulic Control Unit (HCU) / System Controller:** Processes sensor data; compares against limits; generates fault codes.
3. **Central Maintenance Computer (CMC):** Logs fault codes with timestamp; generates maintenance messages.
4. **ECAM / EICAS:** Displays alerts and warnings to the flight crew in real time.
5. **ACARS:** Transmits fault data to ground maintenance teams during flight.

### 3.4 Warning and Alert Logic

A typical hydraulic system uses a **three-level alert hierarchy:**

| Level | Condition | Crew Action |
|---|---|---|
| **Advisory (blue/white)** | Low fluid level (>50% remaining) | Monitor; log for maintenance |
| **Caution (amber)** | Pressure below 2,200 psi; level <30% | Immediate crew action (switch pumps, isolate system) |
| **Warning (red)** | Total system failure; temperature critical | Emergency drill |

---

## 4. Role 3 — Automation

### 4.1 Why Automation Is Necessary

A modern transport aircraft has hundreds of hydraulic consumers and dozens of sensors generating thousands of data points per second. No crew of two could manually manage all of these systems while simultaneously flying the aircraft. Automation — implemented through microprocessors and logic circuits — handles routine system management, freeing the crew to focus on higher-level tasks.

### 4.2 Key Automated Functions

#### 4.2.1 Automatic Pump Switching

- If an **engine-driven pump (EDP)** fails (detected by: pressure below threshold on pressure transducer, zero flow on flow switch), the system automatically starts the corresponding **AC motor pump (ACMP)**.
- On the **Boeing 777**, this switching is managed by the **Hydraulic System Management Unit (HSMU)**, which continuously polls pump status sensors and executes switching logic within 0.5 seconds of fault detection.

#### 4.2.2 Priority Valve Logic

During a hydraulic low-pressure event:
- **Electrically controlled priority valves** close off non-essential consumers (cargo door, hydraulic-powered ground handling equipment).
- Essential consumers (primary flight controls, brakes, nose wheel steering) retain full pressure.
- On the Airbus A320: Yellow system electric pump runs on demand logic — automatically starts when the Yellow system is needed but the engine-driven pump is off.

#### 4.2.3 Thermal Protection

- Temperature sensors in the reservoir and pump outlet continuously report to the HCU.
- If temperature exceeds the **pump offload threshold** (typically 115–120°C):
  - Pump is electrically commanded to **reduce pressure** (offload valve opens — fluid circulates without building pressure).
  - System pressure is maintained by accumulators while the pump cools.
- If temperature exceeds the **pump shutdown threshold** (typically 130–135°C):
  - Pump is electrically shut down immediately.
  - Backup pump starts automatically.

#### 4.2.4 Automated Sequencing (Landing Gear)

The landing gear retraction/extension sequence involves:
- 14 steps on a typical aircraft (see [Topic 5](./05_Electrical_Signals_and_Hydraulic_Components.md))
- Managed entirely by the **Landing Gear Control Unit (LGCU)** microprocessor
- Crew input: a single lever movement
- System ensures correct sequencing even if individual sensors fail (using time-outs and alternate confirmation signals)

#### 4.2.5 Built-In Test Equipment (BITE)

BITE is automated self-testing embedded in the hydraulic control electronics:

- **Power-up BITE:** On aircraft electrical power application, all solenoid coil resistances are checked; all sensor outputs are verified within calibrated range.
- **Continuous BITE:** During flight, sensor outputs are cross-checked against redundant sensors; impossible combinations are flagged (e.g., gear simultaneously up and down).
- **Maintenance BITE:** On ground, maintenance computer can command specific solenoid valves and verify sensor response; can perform end-to-end actuator functional tests.

---

## 5. Integration Architecture: How the Two Systems Connect

### 5.1 Physical Interface

Electrical and hydraulic systems interface at the **solenoid valve** and at the **sensor**:

- The solenoid valve is **bolted to the hydraulic manifold**. Its coil terminals are connected to the aircraft wiring harness via a MIL-SPEC circular connector.
- The sensor is **threaded into the hydraulic line or reservoir** and provides an electrical signal output via a connector.

### 5.2 Signal Bus Architecture

On modern aircraft, hydraulic control electronics are integrated into the **avionics data bus network**:

| Bus Standard | Bit Rate | Used On |
|---|---|---|
| ARINC 429 | 12.5 or 100 kbps | Airbus A320, Boeing 737NG (digital sensors to displays) |
| ARINC 629 | 2 Mbps | Boeing 777 (primary flight control system) |
| CAN Bus | 1 Mbps | Some regional aircraft hydraulic monitoring |
| AFDX (ARINC 664) | 100 Mbps | Airbus A380, A350 (all major systems) |

### 5.3 Electrical Power Requirements

| Component | Power Supply | Typical Power |
|---|---|---|
| Solenoid valve coil | 28 VDC | 5–30 W |
| Electric motor pump (ACMP) | 115 VAC, 3-phase | 5–15 kW |
| Sensor signal conditioner | 28 VDC | <1 W |
| Flight control computer | 28 VDC or 115 VAC | 50–200 W |

---

## 6. How Electrical Systems Improve Efficiency and Reliability

### 6.1 Efficiency Improvements

1. **Demand-based pump operation:** Without electrical monitoring, all pumps run at full pressure continuously. With electrical monitoring, the HSMU schedules pump operation based on actual demand — reducing heat generation, fuel consumption, and pump wear.

2. **Optimised system pressure:** The A380 uses two systems at 5,000 psi (vs. traditional 3,000 psi). This was enabled by highly precise electrical pressure regulation, reducing pipe and actuator mass by ~30%.

3. **Load shedding:** During single-engine operation, non-essential hydraulic loads are electrically shed, ensuring sufficient hydraulic power for flight-critical systems without oversizing the pumps.

### 6.2 Reliability Improvements

1. **Fault detection speed:** An electrical pressure transducer detects a pressure drop in milliseconds. Without electrical monitoring, a pilot might not notice a leak until visual warnings appear — by which time the reservoir may be significantly depleted.

2. **Redundancy through dissimilarity:** Modern aircraft use **three independent hydraulic systems** powered by **different electrical buses**. A single electrical bus fault cannot deprive more than one hydraulic system of its electric pump.

3. **Predictive maintenance:** Continuous sensor data trending (rising pump temperature, increasing filter differential pressure, declining pump flow) predicts failures before they occur, enabling scheduled maintenance rather than reactive repairs.

4. **Reduced human error:** Automated sequencing eliminates the possibility of a crew member forgetting to complete a manual valve selection — a significant cause of hydraulic system incidents in early aircraft.

---

## 7. Worked Example: Hydraulic Pump Management on the Boeing 777

### System Configuration

- Three hydraulic systems: **Left (L)**, **Centre (C)**, **Right (R)**
- Each system: 2 × Engine-Driven Pumps (EDP) + 2 × AC Motor Pumps (ACMP)
- Total: 6 EDPs + 6 ACMPs = 12 pumps across 3 systems
- Each ACMP powered by a separate electrical bus

### Normal Operation

All 6 EDPs are running. ACMPs are set to AUTO (automatic).

**Electrical monitoring in steady state:**
- Pressure transducers on each EDP outlet report ~3,050 psi.
- HSMU confirms all systems normal; ACMP solenoids remain de-energised (pumps not running).
- Fluid level sensors confirm reservoirs within normal range.
- Filter differential pressure switches confirm clean filters.

### Failure Scenario — Right Engine-Driven Pump Failure

1. Right EDP fails. **Pressure transducer** on R system drops from 3,050 psi → <2,200 psi.
2. **HSMU detects** pressure below threshold within 100 ms.
3. HSMU logic: Right EDP failed → **Energise Right ACMP-1 relay**.
4. Right ACMP-1 starts. Pressure recovers to 3,050 psi within 3 seconds.
5. EICAS displays: "HYD R ENG PUMP LO PR" advisory.
6. **Maintenance fault code** generated: "R EDP 1 Low Pressure," stored in CMC.
7. Crew acknowledges advisory; no further action required (ACMP covers).

### Emergency Scenario — Total Loss of Right System

1. All four R system pumps fail. Pressure drops to zero.
2. HSMU energises **R system isolation valve** (electrically closed) to prevent fluid loss if there is a leak.
3. **Priority valve logic** verifies that the Centre system is covering the R-system consumers that have cross-system redundancy.
4. EICAS displays: "HYD R SYS PRESS" warning (amber).
5. Crew executes QRH (Quick Reference Handbook) non-normal checklist for hydraulic system failure.

This example demonstrates how electrical systems transform a potential catastrophic failure into a managed, monitored, crew-aware condition.

---

## 8. References

1. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
2. Boeing Commercial Airplanes — *777 Airplane Maintenance Manual, Chapter 29 — Hydraulic Power*. The Boeing Company.
3. Airbus S.A.S. — *A320 Aircraft Maintenance Manual (AMM), Chapter 29 — Hydraulic Power*. Airbus.
4. FAA — *Aviation Maintenance Technician Handbook — Airframe, Vol. 2, Chapter 12: Aircraft Hydraulic Systems*. FAA-H-8083-31A.
5. Scholz, D. — *Hydraulic Systems Lecture Notes*. HAW Hamburg. http://www.profscholz.de
6. Pallett, E.H.J. — *Aircraft Electrical Systems* (3rd ed.). Longman, 1987.
7. Langton, R. et al. — *Aircraft Fuel Systems*. Wiley-AIAA, 2009.
8. EASA — *CS-25 Certification Specifications for Large Aeroplanes*. European Union Aviation Safety Agency.

---

*Previous topic: [Topic 1 — Introduction](./01_Introduction.md)*
*Next topic: [Topic 3 — Types of Electrical Components Used in Hydraulic Systems](./03_Electrical_Components.md)*
