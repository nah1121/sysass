# Topic 1 — Introduction

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Overview](#1-overview)
2. [Why This Integration Matters](#2-why-this-integration-matters)
3. [Basic Principles of Aircraft Hydraulic Systems](#3-basic-principles-of-aircraft-hydraulic-systems)
4. [Basic Principles of Aircraft Electrical Systems](#4-basic-principles-of-aircraft-electrical-systems)
5. [Historical Development of Electrical-Hydraulic Integration](#5-historical-development-of-electrical-hydraulic-integration)
6. [Scope of Electrical-Hydraulic Interaction on a Modern Airliner](#6-scope-of-electrical-hydraulic-interaction-on-a-modern-airliner)
7. [Key Terminology](#7-key-terminology)
8. [References](#8-references)

---

## 1. Overview

Modern aircraft are extraordinarily complex machines that rely on the seamless integration of multiple engineering disciplines. Among the most critical of these integrations is the marriage of **hydraulic power** and **electrical control**. Neither system operates efficiently in isolation:

- **Hydraulic systems** provide the enormous mechanical force needed to move flight control surfaces, raise and lower landing gear, operate brakes, operate thrust reversers, and drive cargo loading systems.
- **Electrical systems** provide the intelligence, precision, and automation that make those hydraulic operations safe, repeatable, and reliable under all flight conditions.

The result is an **electrohydraulic system** — a tightly integrated architecture where electrical signals command, monitor, protect, and automate hydraulic actuators across the entire aircraft.

---

## 2. Why This Integration Matters

### Scale of Force Required

A large transport aircraft such as the **Boeing 747-400** has a maximum take-off weight of approximately **412,775 kg (910,000 lb)**. At high airspeeds, the aerodynamic loads on flight control surfaces are enormous — a pilot's hand or foot force alone is completely inadequate to move them. Hydraulic actuators, energised by pumps delivering pressures of **3,000 to 5,000 psi**, provide the required force.

### Complexity of Modern Aircraft Systems

| Aircraft | Hydraulic Systems | Operating Pressure | Approximate Actuators |
|---|---|---|---|
| Boeing 737NG | 2 (A and B) | 3,000 psi | ~40 |
| Boeing 747-400 | 4 (1, 2, 3, 4) | 3,000 psi | ~100 |
| Boeing 777 | 3 (Left, Centre, Right) | 3,000 psi | ~250 |
| Airbus A320 | 3 (Blue, Green, Yellow) | 3,000 psi | ~80 |
| Airbus A380 | 4 (Green, Yellow + 2×EHA) | 3,000 / 5,000 psi | ~300+ |

With hundreds of actuators spread across a large aircraft, manual control of every hydraulic valve would be impossible. Electrical systems provide the automation layer that makes centralised control feasible.

### Safety

Any failure in a hydraulic system that affects flight controls is potentially catastrophic. Electrical sensors provide the continuous monitoring needed to detect failures early, enabling pilots and automatic systems to isolate affected systems and maintain aircraft controllability.

---

## 3. Basic Principles of Aircraft Hydraulic Systems

### Pascal's Law

Aircraft hydraulic systems are based on **Pascal's Law**: pressure applied to an enclosed fluid is transmitted equally and undiminished in all directions.

```
Force = Pressure × Area

F = P × A
```

A pump generating 3,000 psi acting on a piston of area 10 in² produces a force of **30,000 lbf (133 kN)** — far beyond any human muscular capability.

### System Components

A basic aircraft hydraulic system consists of:

- **Reservoir:** Stores hydraulic fluid and accommodates volume changes.
- **Pump:** Engine-driven (EDP) or electric motor-driven (EMP/ACMP) — converts mechanical energy to hydraulic pressure.
- **Pressure relief valve:** Limits maximum system pressure to protect components.
- **Filters:** Remove particulate contamination from the fluid.
- **Selector valves:** Direct flow to the correct actuator.
- **Actuators:** Convert hydraulic pressure back into linear or rotary mechanical motion.
- **Return lines:** Return fluid from actuators to the reservoir.

### Hydraulic Fluids

Most modern transport aircraft use **Skydrol** (a phosphate ester-based synthetic fluid), which is:
- Fire-resistant (auto-ignition temperature ~200°C)
- Compatible with seals made of EPDM or butyl rubber
- Toxic to humans — skin and eye protection required during maintenance

---

## 4. Basic Principles of Aircraft Electrical Systems

### Power Generation

Aircraft generate electrical power from:
- **Integrated Drive Generators (IDGs):** AC generators driven by the engines via a constant-speed drive.
- **Auxiliary Power Unit (APU) Generator:** Provides power on the ground and as an in-flight backup.
- **Ram Air Turbine (RAT):** Emergency generator/hydraulic pump deployed into the airstream.
- **Batteries:** DC backup for essential loads during complete generator failure.

### Power Distribution

| Bus Type | Voltage | Typical Uses |
|---|---|---|
| AC Main Bus | 115 VAC, 400 Hz | Large motor-driven hydraulic pumps, galleys |
| DC Main Bus | 28 VDC | Solenoid valves, sensors, relays, avionics |
| Essential Bus | 28 VDC | Critical systems: flight controls, instruments |
| Emergency Bus | 28 VDC (battery) | Absolute minimum: attitude indicator, radio |

### Signal Types

- **Analogue signals:** Continuous voltage or current proportional to a physical quantity (e.g., 0–5V for 0–5,000 psi). Used for pressure transducers and temperature sensors.
- **Digital signals:** Discrete ON/OFF states (e.g., gear up/down limit switches).
- **Serial data buses:** ARINC 429, ARINC 629, CAN bus — transmit digital data between avionics computers and hydraulic control units.

---

## 5. Historical Development of Electrical-Hydraulic Integration

### 1930s–1940s: Early Hydraulics

- **Douglas DC-3 (1935):** First widespread use of hydraulic brakes and retractable landing gear on a civil transport.
- **Boeing B-17 (1938):** Hydraulic bomb bay doors and turret actuation.
- Control: entirely manual valves — no electrical integration beyond cockpit warning lights.

### 1940s–1950s: Introduction of Electrical Control

- **de Havilland Comet (1949):** First jet airliner; introduced electrical switching of hydraulic selector valves from cockpit.
- **Boeing 707 (1958):** Electrohydraulic flight spoilers; electrical cockpit controls for hydraulic pumps.
- First use of **pressure switches** to warn of low hydraulic system pressure.

### 1960s–1970s: Analogue Servo Control

- **Lockheed L-1011 (1970) and Boeing 747 (1969):** Four-axis autopilot systems using **electrohydraulic servo valves (EHSVs)** driven by analogue autopilot computers.
- First **analogue fly-by-wire** (FBW) on **Concorde (1969)** — electrical signals replace mechanical cables for primary flight controls.
- **Apollo Lunar Module** thrusters and control surfaces used early EHSVs.

### 1980s–1990s: Digital Fly-By-Wire

- **Airbus A320 (1988):** World's first production digital FBW transport aircraft. All primary flight controls commanded via digital computers driving EHSVs.
- **Boeing 777 (1995):** First Boeing digital FBW aircraft. Triple-redundant Primary Flight Computers (PFCs) drive EHSVs via ARINC 629 data bus.
- **FADEC** (Full Authority Digital Engine Control) links engine control to hydraulic pump demand management.

### 2000s–Present: More Electric and EHA Era

- **Airbus A380 (2005):** Introduced **Electro-Hydrostatic Actuators (EHAs)** as backup actuators — completely self-contained electrical-hydraulic units with no connection to the main hydraulic system.
- **Boeing 787 (2011):** "More Electric Architecture" — eliminates pneumatic bleed systems; increases reliance on electrical power. Hydraulic systems retained but significantly reduced in scope.
- **Airbus A350 (2013):** Extended use of EHAs and **Electrical Backup Hydraulic Actuators (EBHAs)**.
- **F-35 Lightning II (2015):** EHAs on multiple flight control surfaces; integrated power management.

---

## 6. Scope of Electrical-Hydraulic Interaction on a Modern Airliner

On a modern transport aircraft, electrical systems interact with hydraulic systems in every phase of flight:

| Flight Phase | Electrical-Hydraulic Interaction |
|---|---|
| Pre-flight | BITE self-test of all solenoid valves and sensors; hydraulic pressure built up by electric pumps |
| Taxi | Nose-wheel steering solenoids energised; brake pressure monitored by pressure transducers |
| Take-off | Flap/slat servo actuators driven by EHSVs; spoiler deployment prohibited until airborne (WOW switches) |
| Climb | Landing gear retraction sequence (solenoid valves, limit switches, proximity sensors); autopilot EHSVs active |
| Cruise | Continuous pressure, temperature, and level monitoring; trim actuator control |
| Descent | Flap/slat extension; speed brake actuation |
| Landing | Brake pressure modulation (BSCU with pressure sensors); thrust reverser deployment (limit switches) |
| Ground | Hydraulic system depressurisation; maintenance BITE tests |

---

## 7. Key Terminology

| Term | Definition |
|---|---|
| **Actuator** | Device converting hydraulic pressure into mechanical motion |
| **BITE** | Built-In Test Equipment — electronic self-diagnosis within a system |
| **ECAM** | Electronic Centralised Aircraft Monitor (Airbus) |
| **EICAS** | Engine Indication and Crew Alerting System (Boeing) |
| **EDP** | Engine-Driven Pump |
| **EHA** | Electro-Hydrostatic Actuator |
| **EHSV** | Electrohydraulic Servo Valve |
| **EMP/ACMP** | Electric Motor Pump / AC Motor Pump |
| **FBW** | Fly-By-Wire |
| **FCC** | Flight Control Computer |
| **LVDT** | Linear Variable Differential Transformer |
| **psi** | Pounds per square inch (unit of pressure) |
| **RAT** | Ram Air Turbine |
| **Skydrol** | Proprietary phosphate ester hydraulic fluid used in most transport aircraft |
| **WOW** | Weight on Wheels — sensor indicating aircraft is on the ground |

---

## 8. References

1. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008. ISBN 978-0-470-05996-8.
2. Pallett, E.H.J. — *Aircraft Electrical Systems* (3rd ed.). Longman, 1987. ISBN 978-0-582-98728-0.
3. Langton, R., Clark, C., Hewitt, M., & Richards, L. — *Aircraft Fuel Systems*. Wiley-AIAA, 2009.
4. Scholz, D. — *Hydraulic Systems Lecture Notes*. Hamburg University of Applied Sciences (HAW Hamburg). Available at: http://www.profscholz.de
5. Boeing Commercial Airplanes — *737 Next Generation Systems Description*. The Boeing Company.
6. Airbus S.A.S. — *A320 Aircraft Maintenance Manual (AMM), Chapter 29 — Hydraulic Power*.
7. Davies, D.P. — *Handling the Big Jets* (3rd ed.). CAA, 1971.
8. Federal Aviation Administration — *Aviation Maintenance Technician Handbook — Airframe, Vol. 2, Chapter 12: Aircraft Hydraulic Systems*. FAA-H-8083-31A.

---

*Next topic: [Topic 2 — The Role of Electrical Systems in Hydraulic Systems](./02_Role_of_Electrical_Systems.md)*
