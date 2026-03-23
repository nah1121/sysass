# Topic 5 — How Electrical Signals Interact with Hydraulic Valves and Actuators

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Overview — From Signal to Motion](#1-overview--from-signal-to-motion)
2. [Open-Loop vs. Closed-Loop Control](#2-open-loop-vs-closed-loop-control)
3. [The Electrohydraulic Servo Loop in Detail](#3-the-electrohydraulic-servo-loop-in-detail)
4. [Valve Types and Their Electrical Control](#4-valve-types-and-their-electrical-control)
5. [Actuator Types in Aircraft Hydraulic Systems](#5-actuator-types-in-aircraft-hydraulic-systems)
6. [Real Aircraft Example 1 — Landing Gear Operation (Boeing 737)](#6-real-aircraft-example-1--landing-gear-operation-boeing-737)
7. [Real Aircraft Example 2 — Aileron Flight Control (Airbus A320)](#7-real-aircraft-example-2--aileron-flight-control-airbus-a320)
8. [Real Aircraft Example 3 — Anti-Skid Braking (Boeing 777)](#8-real-aircraft-example-3--anti-skid-braking-boeing-777)
9. [Digital Signal Paths and Data Buses](#9-digital-signal-paths-and-data-buses)
10. [Latency and Control Loop Timing](#10-latency-and-control-loop-timing)
11. [References](#11-references)

---

## 1. Overview — From Signal to Motion

Every movement of a hydraulic actuator in a modern aircraft begins with an electrical signal. Whether it is a pilot moving a flight control, a computer executing an automated sequence, or an autopilot correcting for a gust of wind, the path from command to physical motion follows the same fundamental chain:

```
┌───────────────────────────────────────────────────────────────────────┐
│                     SIGNAL TO MOTION CHAIN                            │
│                                                                       │
│  Pilot/Computer ──► Electrical Signal ──► Valve ──► Hydraulic Flow   │
│       Input           (voltage/current)   (Solenoid/  (High-pressure  │
│                                           EHSV)       fluid)          │
│                                                           │           │
│                                                           ▼           │
│                                                      Actuator         │
│                                                      (Linear or        │
│                                                       rotary)          │
│                                                           │           │
│                                                           ▼           │
│                                                   Physical Motion     │
│                                                   (Control surface,   │
│                                                    landing gear,      │
│                                                    brakes, etc.)      │
│                                                           │           │
│                                                           │           │
│                         Sensor Feedback ◄─────────────────┘          │
│                        (LVDT, proximity,                              │
│                         pressure)                                     │
└───────────────────────────────────────────────────────────────────────┘
```

The **sensor feedback** path is what separates a precise, controlled system from a simple "push and hope" approach. With feedback, the system continuously corrects itself to achieve exactly the commanded position.

---

## 2. Open-Loop vs. Closed-Loop Control

### Open-Loop Control

In open-loop control, an electrical signal commands a valve to move. There is **no measurement** of the actual result.

```
Command ──► Controller ──► Valve ──► Actuator ──► Motion
                                                      │
                                            No feedback — output
                                            not monitored
```

**When used:** Simple on/off functions where position precision is not required:
- Hydraulic cargo door open/close (confirmed by limit switches, but not position-controlled)
- Hydraulic ground service equipment
- Simple one-shot actions (e.g., breaking a shear pin to release a landing gear uplatch)

**Limitations:**
- No automatic correction for disturbances (aerodynamic loads, friction, leakage)
- No confirmation of correct position (unless limit switches added)
- Cannot achieve precise intermediate positions

### Closed-Loop (Feedback) Control

In closed-loop control, a sensor continuously measures the actuator's actual output, and the controller adjusts its command to eliminate any error between commanded and actual positions.

```
                           ┌────────────────────────────────────────┐
                           │                                        │
Command  ──► [+]Summing ──► Controller ──► Valve ──► Actuator ──►  Output
             Junction                                               │
               ▲                                                    │
               │                                                    │
               └──────────── Sensor (LVDT) ◄────────────────────────┘
                             (Feedback Signal)

Error = Command − Feedback
Controller drives Error → 0
```

**When used:** All precision applications:
- Primary flight control surfaces (ailerons, elevators, rudder)
- Trim actuators
- Nose-wheel steering
- Anti-skid brake pressure modulation

**Advantages:**
- Automatic correction for aerodynamic loads
- Precise positioning (±0.05 mm with EHSV + LVDT)
- Self-correcting response to disturbances
- Detects actuator failure (LVDT not following command = fault)

---

## 3. The Electrohydraulic Servo Loop in Detail

### Block Diagram

```
θ_cmd ──► [Summing] ──► [FCC Amplifier] ──► [EHSV] ──► [Hydraulic Actuator]
(Command)   Junction      (Error Gain K)      │              │
              ▲                               │              │
              │                        (Hydraulic          LVDT
              │                         Pressure)        (Position
              │                                           Feedback)
              └───────────────────────────────────────────────┘
                                  (Feedback)
```

### Signal Flow

1. **Command signal (θ_cmd):** Digital command from Flight Control Computer — typically 12-bit or 16-bit value representing desired surface position in degrees.

2. **Digital-to-analogue conversion:** FCC converts digital command to analogue voltage (e.g., ±10V representing ±30° surface deflection).

3. **Summing junction:** The feedback signal (LVDT output) is subtracted from the command. The result is the **error signal (ε)**:
   - ε = θ_cmd − θ_actual
   - ε = +5° → surface needs to move 5° more positive
   - ε = 0 → surface is exactly at commanded position

4. **Controller (gain):** The FCC amplifies the error signal by a gain factor K (e.g., K = 5 mA/degree). So ε = +5° → EHSV command = +25 mA.

5. **EHSV:** Receives ±25 mA command → torque motor deflects flapper → spool shifts → high-pressure hydraulic fluid flows to extend side of actuator.

6. **Actuator:** Piston moves → control surface deflects.

7. **LVDT:** Measures new surface position → sends feedback voltage to FCC.

8. **Loop repeats** until ε = 0 (surface at commanded position).

### Loop Stability and Dynamics

A key engineering challenge is ensuring the loop is **stable** (does not oscillate) while also being fast enough for the aircraft's control needs.

- **Too high gain (K):** Loop oscillates — surface flutters uncontrollably
- **Too low gain (K):** Response is sluggish — aircraft handles poorly

Flight control engineers use **Bode plots** and **root locus analysis** to design gain schedules that are stable across all flight conditions. Most modern FCCs vary their gain with airspeed (reducing gain at high speed to prevent overly sensitive surface deflections).

---

## 4. Valve Types and Their Electrical Control

### 4.1 On/Off Solenoid Directional Control Valve

**Electrical input:** Binary (0 V = de-energised, 28 V = energised)
**Hydraulic output:** Two discrete positions

```
De-energised:  P ──► T (pressure to tank — actuator holds)
Energised:     P ──► A, B ──► T (pressure to extend, retract returns to tank)
```

**Application:** Landing gear retraction circuits, flap actuators, cargo doors

### 4.2 Proportional Valve

**Electrical input:** Variable current (e.g., 4–20 mA)
**Hydraulic output:** Continuously variable flow rate

As current increases from 4 mA to 20 mA:
- Valve spool moves from fully closed to fully open
- Flow increases proportionally
- Intermediate positions provide intermediate flow rates

**Application:** Variable hydraulic consumers where precise control is needed but EHSV performance is unnecessary (e.g., nose-wheel steering on some aircraft).

### 4.3 Electrohydraulic Servo Valve (EHSV) — Precision Control

**Electrical input:** ±10 to ±100 mA (differential signal from FCC)
**Hydraulic output:** Precisely proportional bidirectional flow

The EHSV combines a **first-stage torque motor/flapper-nozzle** with a **second-stage spool valve**. Inner LVDT feedback makes the spool position proportional to input current to within 0.1%.

**Application:** ALL primary flight control surfaces on fly-by-wire aircraft.

### 4.4 Electrohydraulic Proportional Pressure Control Valve

Controls hydraulic **pressure** rather than flow, proportional to input signal.

**Application:** Anti-skid braking systems — BSCU outputs a pressure command (0–3,000 psi) proportional to the desired brake torque.

---

## 5. Actuator Types in Aircraft Hydraulic Systems

### 5.1 Linear (Cylinder) Actuator

The most common type. A piston slides inside a cylinder; hydraulic pressure on one side extends or retracts the rod.

```
         Extend Port (A)        Retract Port (B)
              │                       │
    ┌─────────┼───────────────────────┼─────────┐
    │         │  ┌─────────────┐      │         │
    │         └─►│   Piston   │◄─────┘         │
    │            │             │                │
    │            └──────┬──────┘                │
    │                   │ Rod                   │
    └───────────────────┼───────────────────────┘
                        │
                  To control surface
```

- **Single-acting:** Hydraulic pressure on one side only; spring or load returns piston
- **Double-acting:** Hydraulic pressure on both sides; direction determined by which port has pressure (most common in aircraft)

### 5.2 Rotary Actuator (Hydraulic Motor)

Converts hydraulic pressure into continuous rotary motion. Used where rotary output is required:
- **Flap drive motors:** Drive ball-screw or gearbox to move flap tracks
- **Cargo hold actuators:** Rotate conveyor systems
- **Slat drive motors:** Drive slat transmission shaft

### 5.3 Tandem Actuator

A tandem actuator has **two pistons on a single rod**, each served by a different hydraulic system. If one system fails, the other system's piston still provides full force.

```
System 1 ──► [Piston 1 | Rod | Piston 2] ◄── System 2
```

Used on all primary flight control surfaces on large aircraft (e.g., aileron tandem actuators on Boeing 777).

### 5.4 Power Control Unit (PCU) / Servo Actuator

A complete self-contained servo system consisting of:
- EHSV
- Tandem hydraulic actuator
- LVDT position feedback
- Bypass valve (for maintenance or failure)
- Integrated manifold

Installed directly at the control surface hinge. Used on all Airbus fly-by-wire aircraft for ailerons, elevators, rudder.

---

## 6. Real Aircraft Example 1 — Landing Gear Operation (Boeing 737)

### System Architecture

- **Normal hydraulic power:** Hydraulic System B (3,000 psi)
- **Alternate hydraulic power:** Hydraulic System A
- **Emergency extension:** Gravity/spring assisted with manual lockout/blowdown

The **Landing Gear Control Unit (LGCU)** is a dedicated microprocessor that manages the entire retraction/extension sequence.

### Complete Gear Retraction Sequence — Step by Step

| Step | Event | Signal Type | Component |
|---|---|---|---|
| 1 | Pilot moves gear lever UP | Mechanical → electrical | Cockpit gear lever switch |
| 2 | LGCU receives UP command | Digital (ARINC 429) | LGCU microprocessor |
| 3 | LGCU checks WOW switches = AIR | Discrete (28V=AIR, 0V=GND) | WOW proximity switch |
| 4 | LGCU checks airspeed ≤ VLO | Digital (ARINC 429) | Air Data Computer |
| 5 | LGCU energises GEAR UP solenoid | 28 VDC relay closure | Hydraulic manifold solenoid |
| 6 | Hydraulic pressure to gear doors | Hydraulic (3,000 psi) | Gear door actuator |
| 7 | Door-open limit switches activate | Discrete switch | Proximity switch |
| 8 | LGCU confirms door open; energises UPLOCK RELEASE solenoid | 28 VDC | Uplock actuator solenoid |
| 9 | Main gear actuators retract | Hydraulic | Main gear cylinder |
| 10 | Uplock hooks engage; proximity sensor confirms UPLOCK | Discrete | Uplock proximity switch |
| 11 | LGCU de-energises UP solenoid; energises DOOR CLOSE solenoid | 28 VDC relay switching | Door close solenoid |
| 12 | Door-closed proximity switches confirm closure | Discrete | Door close proximity switch |
| 13 | All solenoids de-energised; system returns to standby | — | LGCU logic |

**Total time:** Approximately 8–12 seconds from lever selection to gear up-and-locked.

### Fault Scenario — Step 10 Fails (Uplock Not Confirmed)

If the uplock proximity switch does NOT confirm within a time window (typically 15 seconds):
1. LGCU generates Fault Code: "MAIN GEAR UPLOCK NOT CONFIRMED"
2. LGCU stores fault in CMC.
3. EICAS/ECAM displays "GEAR NOT STOWED" advisory.
4. LGCU attempts to recycle: de-energises then re-energises UP solenoid.
5. If still not confirmed after second attempt → crew notified; alternate landing gear procedures may be required.

### Alternate (Manual) Extension

If the LGCU fails or all electrical power is lost:
1. Crew opens the alternate gear extension panel.
2. Manual valves are operated by cable/pull handles.
3. Uplocks are mechanically released.
4. Gear falls under gravity; downlocks engage under load.
5. Crew confirms gear position via alternative indicator (manual observation or alternate lighting).

This demonstrates that while electrical control is primary, **mechanical backup** is always provided for safety.

---

## 7. Real Aircraft Example 2 — Aileron Flight Control (Airbus A320)

### System Architecture

- **Blue, Green, and Yellow hydraulic systems** supply the aileron actuators.
- Each aileron (left and right) has **two hydraulic actuators** (Green and Blue systems).
- One actuator is in **Active mode** (pressurised; electrically controlled by EHSV).
- The other is in **Damping mode** (not pressurised; provides resistance to prevent flutter).

The **ELAC (Elevator and Aileron Computer)** — two computers, ELAC 1 and ELAC 2 — provides commands to the aileron EHSVs.

### Normal Control Loop — Pilot Rolls Right

1. **Pilot pushes sidestick right.** Sidestick LVDT measures deflection → sends analogue position signal to ELAC 1.

2. **ELAC 1 computes:** 
   - Right roll command from sidestick LVDT
   - Current roll rate from INS (Inertial Navigation System)
   - Airspeed from ADR (Air Data Reference)
   - Bank angle from ADIRS
   → ELAC computes required aileron deflection using control law

3. **ELAC outputs:** ±10 mA analogue current command to EHSV on right aileron actuator (retract command → right aileron deflects up); mirror command to left aileron EHSV (extend → left aileron deflects down).

4. **EHSV on right aileron:** Receives −8 mA → torque motor deflects → spool shifts to port Green system pressure to retract side → right aileron actuator retracts → right aileron deflects upward by ~5°.

5. **LVDT on right aileron actuator:** Measures actual surface position → sends feedback to ELAC.

6. **ELAC error correction:** If actual = commanded, maintain signal. If actual ≠ commanded (aerodynamic load on surface), increase/decrease EHSV current to correct.

7. **Loop cycle rate:** ELAC updates command 25 times per second (40 ms cycle). EHSV inner loop runs at 1,000 Hz (1 ms).

### Envelope Protection — Bank Angle Limiting

If the pilot pushes the sidestick to maximum deflection and holds:
- ELAC detects that bank angle has reached 67°.
- ELAC **limits further aileron command** to prevent exceeding 67° of bank.
- Sidestick input no longer increases bank beyond this point.
- Pilot can see the ELAC is "fighting" the sidestick — this is normal and expected.

This is a key FBW safety feature: the **flight computer protects the aircraft from dangerous pilot-induced attitudes**, something impossible with conventional cable-and-pulley systems.

---

## 8. Real Aircraft Example 3 — Anti-Skid Braking (Boeing 777)

### System Architecture

- **Left and Right hydraulic systems** supply the inboard brakes.
- **Centre system** supplies the outboard brakes.
- The **Braking and Steering Control Unit (BSCU)** controls individual brake pressure via **electrohydraulic pressure control valves**.

### How It Works

During landing rollout:

1. Pilot depresses brake pedals. Brake pedal position LVDT sends displacement signal to BSCU.

2. BSCU computes **commanded brake pressure** proportional to pedal position (e.g., full pedal = 3,000 psi; half pedal = 1,500 psi).

3. BSCU outputs current command to **electrohydraulic pressure control valve** (one per main gear wheel).

4. Pressure control valve ports hydraulic pressure to the wheel brake cylinder, proportional to BSCU command.

5. **Wheel speed sensor** (tachometer) on each wheel reports rotational speed to BSCU.

6. **BSCU anti-skid algorithm:** If a wheel begins to decelerate faster than others (skid developing), BSCU **reduces brake pressure** on that wheel by reducing the valve command.

7. When wheel speed recovers (skid stops), BSCU increases pressure again.

8. **Cycle rate:** BSCU modulates brake pressure up to **10 times per second** per wheel — far faster than any human could achieve.

### Result

Anti-skid braking stops the aircraft in the **minimum possible distance** without causing tyre flat-spotting or hydroplaning — a function only achievable through electrical control of hydraulic brakes.

---

## 9. Digital Signal Paths and Data Buses

Modern aircraft transmit commands and sensor data between computers and hydraulic control units over **digital data buses** rather than individual analogue wires.

### ARINC 429 (Airbus A320, Boeing 737NG)

- **Type:** Unidirectional serial digital bus (one transmitter, up to 20 receivers)
- **Speed:** 12.5 kbps (low speed) or 100 kbps (high speed)
- **Differential signal:** ±10 V bipolar → immune to EMI
- **Message format:** 32-bit words containing label (data type), source/destination, data value, sign/status matrix (SSM)
- **Latency:** Each sensor update typically every 10–100 ms (depending on message priority)

### ARINC 629 (Boeing 777)

- **Type:** Bidirectional serial bus (multiple transmitters and receivers)
- **Speed:** 2 Mbps
- **Each LRU** (Line-Replaceable Unit) has its own transmitter
- All three Primary Flight Computers (PFCs) and all three Actuator Control Electronics (ACEs) share the same bus → highly integrated system

### AFDX / ARINC 664 (Airbus A380, A350)

- **Type:** Switched Ethernet (100 Mbps)
- **Deterministic timing:** Virtual links with guaranteed bandwidth allocation
- **Replaces:** Multiple separate ARINC 429 buses with a single high-speed network
- **Used for:** All major system data exchange including hydraulic monitoring

---

## 10. Latency and Control Loop Timing

In closed-loop hydraulic control, **latency** (time delay) in the feedback path affects stability. Excessive latency can cause the system to oscillate.

### Latency Sources in the Loop

| Source | Typical Latency |
|---|---|
| Sensor (LVDT signal conditioning) | 0.5–2 ms |
| Analogue-to-digital conversion | 0.1–0.5 ms |
| Data bus transmission (ARINC 429) | 10–100 ms (low speed) |
| Flight computer processing | 20–40 ms (25 Hz update) |
| Digital-to-analogue conversion | 0.1 ms |
| EHSV response time | 2–5 ms |
| Hydraulic fluid compression delay | 0.5–2 ms |
| **Total loop latency** | **~35–150 ms** |

### Impact on System Design

For the outer (surface position) loop, 35–150 ms of latency is acceptable because control surfaces do not need to respond faster than ~10 Hz.

For the **inner EHSV spool position loop** (which runs inside the EHSV itself), the loop must close at 1,000 Hz or faster to maintain precise proportional control of hydraulic flow. This inner loop uses hardwired analogue signals (not digital bus), with total latency <1 ms.

---

## 11. References

1. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
2. Merritt, H.E. — *Hydraulic Control Systems*. Wiley, 1967.
3. Ogata, K. — *Modern Control Engineering* (5th ed.). Prentice Hall, 2010.
4. Airbus S.A.S. — *A320 Aircraft Maintenance Manual, Chapter 27 — Flight Controls*. Airbus.
5. Airbus S.A.S. — *A320 Flight Crew Operating Manual (FCOM), Vol. 1 — Systems Description*. Airbus.
6. Boeing Commercial Airplanes — *777 Systems Description, Chapter 32 — Landing Gear*. Boeing.
7. Boeing Commercial Airplanes — *777 Systems Description, Chapter 27 — Flight Controls*. Boeing.
8. Boeing Commercial Airplanes — *737 Quick Reference Handbook (QRH) — Landing Gear Non-Normal Procedures*. Boeing.
9. FAA Advisory Circular AC 25.629-1B — *Flutter Substantiation*. FAA.
10. Curtiss-Wright Controls — *Electrohydraulic Servo Valve Application Notes*. Curtiss-Wright, 2019.

---

*Previous topic: [Topic 4 — Types of Sensors Used in Aircraft Hydraulic Systems](./04_Sensors.md)*
*Next topic: [Topic 6 — Failure of Electrical Components and Sensors](./06_Failure_Modes_and_Causes.md)*
