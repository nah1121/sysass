# Topic 8 — Fly-By-Wire and Electro-Hydrostatic Actuators (EHA)

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Fly-By-Wire (FBW) — Principles](#2-fly-by-wire-fbw--principles)
3. [FBW Architecture — Control Laws and Computers](#3-fbw-architecture--control-laws-and-computers)
4. [FBW Aircraft Examples and Development History](#4-fbw-aircraft-examples-and-development-history)
5. [How FBW Commands Hydraulic Actuators](#5-how-fbw-commands-hydraulic-actuators)
6. [Electro-Hydrostatic Actuators (EHA)](#6-electro-hydrostatic-actuators-eha)
7. [Electrical Backup Hydraulic Actuators (EBHA)](#7-electrical-backup-hydraulic-actuators-ebha)
8. [The "More Electric Aircraft" (MEA) Concept](#8-the-more-electric-aircraft-mea-concept)
9. [EHA vs. Conventional Hydraulic Actuator — Detailed Comparison](#9-eha-vs-conventional-hydraulic-actuator--detailed-comparison)
10. [Future Trends](#10-future-trends)
11. [References](#11-references)

---

## 1. Introduction

Fly-By-Wire (FBW) represents the complete replacement of **mechanical linkages** between the pilot's controls and the hydraulic servo valves with **electrical signal paths**. Electro-Hydrostatic Actuators (EHAs) go one step further — they eliminate even the central hydraulic distribution system for certain surfaces, incorporating a self-contained hydraulic circuit within each actuator unit.

Together, these technologies define the current and future state of aircraft flight control actuation.

---

## 2. Fly-By-Wire (FBW) — Principles

### 2.1 What FBW Replaces

In a **conventional aircraft**, the flight control path is:

```
Pilot's Control Column/Stick
           │
      Cable/Rod Run
    (100+ metres on large aircraft)
           │
    Bell-cranks and Pulleys
           │
     Hydraulic Input Lever
           │
      Servo Tab or
    Power Control Unit
           │
     Control Surface
```

This mechanical system has several disadvantages:
- **Weight:** Cables, rods, pulleys, and bell-cranks on a Boeing 707 weigh approximately 450 kg
- **Space:** Complex routing through fuselage frames and wing ribs
- **Force feedback:** Pilots feel aerodynamic loads through the controls (good for feel, but requires artificial feel systems at high speed)
- **No envelope protection:** Pilot can command beyond structural limits
- **Complexity:** Precise rigging required; susceptible to wear and stretch

### 2.2 The FBW Signal Path

In a **fly-by-wire aircraft**, the mechanical path is replaced:

```
Pilot's Sidestick or Control Column
           │
         LVDT
           │
   Electrical Signal (analogue or digital)
           │
  Flight Control Computer (FCC)
           │
   Electrical Command Signal
           │
   EHSV (Electrohydraulic Servo Valve)
           │
   Hydraulic Actuator
           │
   Control Surface
```

The mechanical complexity is replaced by **wiring, sensors, and computers**. The FCC adds intelligence: it processes the pilot's input through **control laws** that determine the optimal actuator command.

### 2.3 Benefits of FBW

| Benefit | Explanation |
|---|---|
| **Weight reduction** | Wiring replaces heavy cable/rod runs |
| **Envelope protection** | FCC enforces structural and aerodynamic limits |
| **Improved handling qualities** | Control laws tailor aircraft response across entire flight envelope |
| **Active load alleviation** | FCC deflects surfaces to reduce gust loads on wing |
| **Redundancy management** | FCC automatically reconfigures after component failures |
| **Fault tolerance** | Multiple redundant FCCs; sensor voting |
| **Fatigue life improvement** | Gust load alleviation and active manoeuvre load control extend wing life |
| **Design freedom** | Naturally unstable aircraft (delta wing, tailless) are flyable only with FBW |

---

## 3. FBW Architecture — Control Laws and Computers

### 3.1 Normal Law (Full Envelope Protection)

In normal law (all systems healthy), the FCC implements control laws that:
- Convert pilot sidestick angle to aircraft response (load factor for pitch; roll rate for roll)
- Apply manoeuvre load control (MLC) — deflect ailerons to reduce wing bending
- Apply gust load alleviation (GLA) — deflect spoilers and ailerons to cancel gust loads
- Enforce hard limits (stall AOA, structural load factor, maximum roll angle)

The pilot commands **aircraft response** (e.g., "2g turn"), not **surface deflection** — the FCC figures out the required surface positions automatically.

### 3.2 Alternate Law and Direct Law (Degraded Modes)

As FBW system components fail, the control laws degrade gracefully:

| Mode | Condition | Behaviour |
|---|---|---|
| **Normal Law** | All FCCs/sensors healthy | Full protection and automation |
| **Alternate Law 1** | One FCC failed | Most protections remain; pitch protection reduced |
| **Alternate Law 2** | Multiple FCC failures | Bank angle and pitch limitations applied; no AOA protection |
| **Direct Law** | Severe degradation | Pilot sidestick deflection commands surface deflection directly (no FCC processing) — pilot feels the aircraft directly |
| **Mechanical Backup** | All FCCs failed | Mechanical pitch trim + rudder only (A320: pitch trim wheel + rudder pedals) |

### 3.3 Redundancy Architecture — Airbus A320

```
                    3 × ADIRUs (Air Data/Inertial)
                    3 × FCMCs
                    │
    ELAC 1 ─────────┤──────── ELAC 2
    (Elevator & Aileron)      (Elevator & Aileron — backup)
         │                           │
    SEC 1 ────────────────────── SEC 2, SEC 3
    (Spoilers, secondary surfaces)
         │
    FAC 1 ─────────────────────── FAC 2
    (Yaw damper, rudder travel limiter)
         │
    ┌────┘
    Blue Hydraulic EHSV ─── Green Hydraulic EHSV ─── Yellow Hydraulic EHSV
    (each surface has EHSVs from 2–3 different hydraulic systems)
```

Single computer failure: corresponding backup computer takes over within 50 ms — no crew awareness required.

### 3.4 Boeing 777 FBW Architecture

Boeing's approach uses three independent **Primary Flight Computers (PFCs)** on ARINC 629 data buses, plus four **Actuator Control Electronics (ACEs)** units. The ACEs are the direct interface to EHSVs.

- Three PFCs independently compute the required surface position and vote on the result.
- The median of the three PFC outputs is used as the command to the ACEs.
- Four ACEs independently drive the EHSVs on each actuator from the voted PFC command.

This architecture provides triple-redundant computation and quadruple-redundant actuation electronics.

---

## 4. FBW Aircraft Examples and Development History

### Timeline

| Year | Aircraft | FBW Type | Notes |
|---|---|---|---|
| 1969 | Concorde (BAC/Aérospatiale) | Analogue FBW | First production civil FBW; primary flight controls |
| 1972 | F-16 Fighting Falcon (General Dynamics) | Analogue FBW | First production military FBW; inherently unstable design |
| 1978 | Space Shuttle Orbiter (NASA) | Digital FBW | First digital FBW; quadruple redundant |
| 1985 | Airbus A310 (early mod) | FBW spoilers only | Partial FBW; precursor to A320 |
| 1988 | Airbus A320 | Digital FBW | First production digital FBW civil transport |
| 1990 | Airbus A340 | Digital FBW | Long-range FBW |
| 1992 | Boeing 777 | Digital FBW | First Boeing FBW production aircraft |
| 1994 | Airbus A330 | Digital FBW | Wide-body FBW |
| 2003 | Airbus A380 | Digital FBW + EHA | Added EHAs as backup actuators |
| 2011 | Boeing 787 | Digital FBW | More-electric architecture |
| 2013 | Airbus A350 | Digital FBW + EHA/EBHA | Extended EHA use |
| 2015 | Airbus A320neo | Digital FBW (updated) | New engine option; same FBW system |

---

## 5. How FBW Commands Hydraulic Actuators

In a FBW system, the interface between the FCC and the hydraulic actuator is the **EHSV** (electrohydraulic servo valve). The FCC command is an analogue current signal (typically ±10 mA) sent via screened twisted-pair cables to the EHSV coil.

### Full Signal Path — A320 Left Aileron Roll Command

```
1. Pilot moves sidestick right → sidestick LVDT outputs +4V (rightward deflection)

2. ELAC 1 receives +4V input
   → Control law computation (40 ms cycle):
     Target: 5° right aileron down (left aileron up)
     Current bank rate: +12°/s (from gyro)
     Airspeed: 280 KIAS (from ADR)
     Computed aileron command: −3° (corrected for roll rate)

3. ELAC 1 outputs: −8 mA to left aileron EHSV (retract command = aileron deflects up)

4. EHSV receives −8 mA:
   → Torque motor deflects flapper
   → Green hydraulic pressure (3,000 psi) directed to retract side
   → Actuator retracts (piston moves left)
   → Left aileron deflects upward

5. Actuator LVDT reports new position (+3° up)

6. ELAC 1 reads feedback: error = +3° − 3° = 0 → maintains current EHSV signal

7. Right aileron receives symmetric opposite command:
   → Right aileron deflects downward 3°

8. Aircraft rolls right
```

### Time Breakdown

| Stage | Time |
|---|---|
| Sidestick LVDT to ELAC (ARINC 429) | 10 ms |
| ELAC computation | 40 ms (25 Hz) |
| ELAC to EHSV (analogue wire) | <1 ms |
| EHSV response (inner loop) | 2–5 ms |
| Actuator movement to new position | 20–100 ms (depends on load) |
| LVDT feedback to ELAC | 10 ms |
| **Total (first correction)** | ~83–156 ms |
| **Subsequent corrections** | Every 40 ms |

---

## 6. Electro-Hydrostatic Actuators (EHA)

### 6.1 What Is an EHA?

An **Electro-Hydrostatic Actuator (EHA)** is a completely self-contained electrohydraulic actuation system that requires only an electrical power supply and a position command signal — no central hydraulic system connection.

Inside the EHA:

```
┌─────────────────────────────────────────────────────────┐
│                      EHA UNIT                           │
│                                                         │
│  Aircraft ──► Motor ──► Hydraulic ──► Hydraulic ──► Rod │
│  Electrical   Controller  Pump (bi-    Actuator    (to  │
│  Bus (270VDC)   │       directional)    Cylinder   surface)
│                 │                          │              │
│          Position ◄── LVDT ◄───────────────┘              │
│          Command                                          │
│          (from FCC)                                       │
│                                                         │
│  Small reservoir (< 1 litre) — self-contained fluid      │
└─────────────────────────────────────────────────────────┘
```

### 6.2 EHA Operation

1. FCC outputs position command to EHA motor controller.
2. Motor controller drives **brushless DC motor** (typically 270 VDC on modern aircraft).
3. Motor drives a **variable-displacement hydraulic pump** bidirectionally — pump output direction and flow rate are controlled by motor speed and direction.
4. Pump pressurises one side of the hydraulic cylinder; the other side returns fluid to the small internal reservoir.
5. Actuator rod moves; LVDT measures position and feeds back to motor controller.
6. Inner control loop maintains precise position.

### 6.3 EHA Operating Modes

| Mode | Description | When Used |
|---|---|---|
| **Active mode** | Motor/pump running; EHA drives the actuator | When central hydraulic system unavailable |
| **Damping mode** | Motor not running; actuator ports connected through bypass | Surface driven by other actuator; EHA provides damping only |
| **Bypass mode** | Full hydraulic bypass; free movement | Maintenance; motor failed |

On Airbus A380, EHAs operate in **damping mode** during normal operations (the primary central hydraulic actuator drives the surface). The EHA activates to **active mode** only if the hydraulic system fails.

### 6.4 EHA Power Requirements

| Parameter | Value |
|---|---|
| Supply voltage | 270 VDC (High-Voltage DC) |
| Peak power (typical aileron) | 5–15 kW |
| Continuous power (holding) | <500 W |
| Motor type | Permanent magnet brushless DC |
| Cooling | Convection (fluid-to-motor thermal path) |

### 6.5 EHA Advantages

| Advantage | Explanation |
|---|---|
| **No central hydraulic connection** | Eliminates high-pressure pipes routing through fuselage to tail (~100 m on large aircraft) |
| **Weight saving** | 200 kg of hydraulic piping eliminated on A380-scale aircraft |
| **Reduced fluid volume** | Total aircraft hydraulic fluid volume reduced; lower fire risk |
| **Simplified maintenance** | No hydraulic line inspection/replacement between EHA and central system |
| **Independent operation** | Can operate even if all main hydraulic systems are failed |

### 6.6 EHA Disadvantages and Challenges

| Disadvantage | Explanation |
|---|---|
| **High electrical power requirement** | 270 VDC bus must carry significant peak current |
| **Heat rejection** | Motor and pump heat must be rejected internally — thermal management is critical |
| **Fluid contamination** | Small internal reservoir accumulates contamination faster than large central system |
| **Motor reliability** | Brushless motors require sophisticated controllers; electronic failures can cause EHA loss |
| **Noise** | Electric motor + pump creates vibration-borne noise (hydraulic pump whine) |

### 6.7 Aircraft Using EHAs

| Aircraft | EHA Application |
|---|---|
| Airbus A380 | Ailerons (backup), horizontal stabiliser (backup) |
| Airbus A350 | Ailerons (backup/primary on some surfaces), elevators |
| F-35 Lightning II | Multiple primary and secondary flight control surfaces |
| Boeing 787 | Studies but not implemented; Boeing chose to retain central hydraulics |
| Lockheed F-22 Raptor | Flight control surfaces (limited EHA-type actuators) |

---

## 7. Electrical Backup Hydraulic Actuators (EBHA)

An **EBHA** is a hybrid actuator that can operate in two modes:

1. **Normal mode:** Powered by the central hydraulic system, controlled by EHSV. Conventional operation.
2. **Backup mode:** Central hydraulic pressure lost → an internal electric motor + pump takes over. Acts like an EHA.

```
Normal:   Central Hydraulic ──► EHSV ──► Actuator Cylinder
Backup:   Aircraft Electrical ──► Motor ──► Internal Pump ──► Actuator Cylinder
```

**Advantage:** An EBHA provides hydraulic performance in normal conditions but can continue to operate from electrical power if the hydraulic system fails — the best of both worlds.

**Aircraft use:** Airbus A380 spoilers (some); Airbus A350 ailerons.

---

## 8. The "More Electric Aircraft" (MEA) Concept

The **More Electric Aircraft** concept aims to replace **all pneumatic and hydraulic power off-takes** from aircraft engines with **electrical power** — reducing the fuel penalty of engine bleed and increasing system efficiency.

### Traditional vs. More-Electric Power Architecture

| System | Traditional Aircraft | More-Electric Aircraft |
|---|---|---|
| Wing anti-icing | Hot engine bleed air | Electrothermal mats (electrically heated) |
| Environmental Control System (ECS) | Engine bleed + pneumatic compressor | Electric compressor + heat exchangers |
| Hydraulic pumps | Engine-driven (mechanical + hydraulic) | Electric motor pumps (HVDC bus) |
| Flight control actuation | Central hydraulic + EHSVs | EHAs + EBHAs |
| Engine starting | Bleed air + pneumatic starter | Electric motor-generator (integrated starter-generator) |

### Boeing 787 as MEA Example

- **No pneumatic bleed system** — all anti-ice and ECS is electrically powered
- Hydraulic systems retained but electric pumps dominate — three 350 VAC motor pumps per system
- Electrical power generation: 1 MVA per engine (five times more than 767)
- Reduces fuel burn by ~3% compared to equivalent bleed-air architecture

### Limits of MEA with Respect to Hydraulics

Complete replacement of central hydraulics with EHAs on large transport aircraft faces challenges:
- Peak hydraulic power demand on a large aircraft can exceed 500 kW
- Current aircraft electrical systems (115 VAC / 270 VDC) are designed for peak demand of ~1 MW — margins exist but are limited
- Full electrification of all actuation would require dedicated high-power electrical buses per major surface group

---

## 9. EHA vs. Conventional Hydraulic Actuator — Detailed Comparison

```
CONVENTIONAL HYDRAULIC ACTUATOR SYSTEM
──────────────────────────────────────
Engine ──► EDP ──► Hydraulic Manifold ──► High-Pressure Lines ──► Solenoid Manifold
                    (3,000–5,000 psi)    (running through fuselage,         │
                                          50–100 metres)                    ▼
                                                                     EHSV ──► Actuator ──► Surface
                                                                              │
                                                                    LVDT feedback ──► FCC

EHA SYSTEM
──────────────────────────────────────
Aircraft Electrical Bus (270 VDC)
           │
    EHA Motor Controller
           │
    Brushless DC Motor
           │
    Bi-directional Hydraulic Pump
           │
    [Self-contained hydraulic cylinder] ──► Surface
           │
    LVDT feedback ──► EHA Controller ──► FCC
```

| Attribute | Conventional + EHSV | EHA |
|---|---|---|
| Response time | <5 ms | 5–15 ms |
| Peak force output | Limited by pipe diameter | Limited by motor peak power |
| Continuous force | Unlimited (while pump runs) | Limited by motor thermal rating |
| Fluid volume | Large (entire aircraft system) | Very small (<1 litre) |
| Maintenance access | Multiple pipe sections | Single unit |
| Weight | Higher (pipes + fluid) | Lower (no external pipes) |
| Reliability | Proven; >60 years service history | Newer; growing service history |
| Electrical power needed | Small (signal only) | Large (full actuation power) |
| Failure mode | Hydraulic system failure | Motor or controller failure |

---

## 10. Future Trends

### 10.1 All-Electric Flight Controls

Research programs (including EU Clean Sky 2 and NASA X-57) are investigating **fully electric flight control actuation** where high-power electric motors directly drive control surfaces — with no hydraulic fluid involved at all. Challenges: efficiency, force output, and certification.

### 10.2 Smart Actuators with Embedded Health Monitoring

Future EHAs and servo actuators will include embedded accelerometers, thermal sensors, fluid condition sensors, and AI-based analytics that:
- Continuously self-monitor health
- Predict remaining useful life
- Recommend maintenance before failure
- Report health data via AFDX to HUMS

### 10.3 Distributed Electrohydraulic Networks

Instead of one central hydraulic system serving all surfaces, future aircraft may use **local EHA clusters** — groups of EHAs powered by local high-voltage DC distribution nodes, eliminating the need for any centralised hydraulic infrastructure.

### 10.4 Shape Memory Alloy (SMA) and Piezoelectric Actuators

For small, low-force, high-frequency applications (e.g., active aeroelastic control, variable camber leading edge), **solid-state actuators** using SMA wires or piezoelectric stacks — electrically commanded, no hydraulic fluid — may supplement or replace hydraulic actuators in the 2030s–2040s timeframe.

---

## 11. References

1. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
2. Maré, J.C. — *Aerospace Actuators, Volume 1: Needs, Reliability and Hydraulic Power Solutions*. ISTE/Wiley, 2016. ISBN 978-1-84821-815-0.
3. Maré, J.C. — *Aerospace Actuators, Volume 2: Signal-by-Wire and Power-by-Wire*. ISTE/Wiley, 2017.
4. Airbus S.A.S. — *A380 Systems Description — Hydraulic Power and Flight Controls*. Airbus, 2007.
5. Boeing Commercial Airplanes — *787 Airplane Characteristics for Airport Planning*. Boeing, 2012.
6. Chakraborty, I., Mavris, D., Emenike, M., & Overmeyer, A. — "Investigating the Influence of Actuation Architecture on Aircraft Fuel Burn." *AIAA SciTech Forum*, 2019.
7. Cutts, S.J. — "A Collaborative Approach to the More Electric Aircraft." *Proceedings of the PEMD Conference*, 2002.
8. Jones, R.I. — "The More Electric Aircraft — Assessing the Benefits." *Proceedings of the Institution of Mechanical Engineers Part G*, 2002.
9. Bennett, J., Mecrow, B., Jack, A., & Atkinson, G. — "A Prototype Electrical Actuator for Aircraft Flaps." *IEEE Transactions on Industry Applications*, 2010.
10. SAE AS5440 — *Hydraulic Actuators: Aircraft, Selection and Application of*. SAE International.

---

*Previous topic: [Topic 7 — Advantages, Limitations, and Maintenance Concerns](./07_Advantages_Limitations_Maintenance.md)*
*Next topic: [Topic 9 — Hydraulic System Redundancy and Electrical Backup](./09_Hydraulic_Redundancy_and_Electrical_Backup.md)*
