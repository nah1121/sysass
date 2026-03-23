# Topic 9 — Hydraulic System Redundancy and Electrical Backup

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [The Need for Redundancy](#1-the-need-for-redundancy)
2. [Multi-Channel Hydraulic Architectures](#2-multi-channel-hydraulic-architectures)
3. [Hydraulic System Cross-Connections and Power Transfer](#3-hydraulic-system-cross-connections-and-power-transfer)
4. [Priority Valves and Electrical Load Shedding](#4-priority-valves-and-electrical-load-shedding)
5. [Ram Air Turbine (RAT)](#5-ram-air-turbine-rat)
6. [Electric Motor Pumps (EMPs) as Electrical Backup](#6-electric-motor-pumps-emps-as-electrical-backup)
7. [Accumulator-Based Electrical Backup](#7-accumulator-based-electrical-backup)
8. [Electrical Power Bus Architecture Supporting Hydraulics](#8-electrical-power-bus-architecture-supporting-hydraulics)
9. [Case Study — Total Hydraulic System Management on Boeing 777](#9-case-study--total-hydraulic-system-management-on-boeing-777)
10. [Common Cause Failure Prevention in Redundant Systems](#10-common-cause-failure-prevention-in-redundant-systems)
11. [References](#12-references)

---

## 1. The Need for Redundancy

Hydraulic systems on modern transport aircraft power flight controls, landing gear, brakes, and steering — all of which are **essential for continued safe flight and landing**. Yet hydraulic systems can and do fail. The question is not whether failures will occur, but whether the aircraft can survive them.

### The Fail-Safe Principle

Aircraft hydraulic system design is governed by the **fail-safe principle**: the failure of any single component must not cause loss of the aircraft. This is implemented through:

1. **Redundancy:** Multiple independent systems performing the same function
2. **Fail-safe states:** Components default to a safe position when they fail
3. **Isolation:** Failure of one system cannot propagate to another
4. **Detection and annunciation:** All failures are detected quickly and crew are alerted

### Regulatory Requirement

EASA CS-25.1309 and FAA FAR 25.1309 require that any **catastrophic failure condition** (one that would prevent continued safe flight and landing) must have a probability of less than **1×10⁻⁹ per flight hour**.

For a single hydraulic system with a typical failure rate of ~1×10⁻⁴ per flight hour, three **independent** systems give a combined probability of ~1×10⁻¹² — well within requirements.

---

## 2. Multi-Channel Hydraulic Architectures

### 2.1 Three-System Architecture (Boeing 777)

The Boeing 777 uses **three completely independent hydraulic systems**:

```
         LEFT SYSTEM              CENTRE SYSTEM             RIGHT SYSTEM
         ─────────────           ─────────────             ─────────────
Pumps:   2×EDP + 2×ACMP         2×EDP + 2×ACMP            2×EDP + 2×ACMP
Pressure: 3,000 psi             3,000 psi                 3,000 psi

Consumers (primary):
- Left aileron (outboard)     - Left inboard aileron      - Right aileron (outboard)
- Left elevator               - Right inboard aileron     - Right elevator
- Left ground spoilers        - ALL elevator (secondary)  - Right ground spoilers
- Nose wheel steering (left)  - ALL flight spoilers       - Right outboard brakes
- Left inboard brakes         - Nose gear steering        - Upper rudder
                              - Body gear steering
                              - Inboard brakes
                              - Lower rudder
```

**Key design principle:** Each primary flight control surface receives hydraulic power from at least **two independent systems**. Loss of any single system leaves the remaining systems capable of controlling the aircraft.

### 2.2 Three-System Architecture (Airbus A320)

The A320 uses **Blue, Green, and Yellow systems**:

| System | Normal Source | Backup Source | Key Consumers |
|---|---|---|---|
| **Green** | Engine 1 EDP (3,000 psi) | Engine 1 Blue PTU | Landing gear, normal brakes, ground spoilers, left aileron, elevators |
| **Blue** | Electric pump (3,000 psi) | RAM Air Turbine (emergency) | Left aileron, elevators, right spoilers, yaw damper |
| **Yellow** | Engine 2 EDP (3,000 psi) | Electric pump | Right aileron, elevators, spoilers, alternate brakes, cargo door |

The **Power Transfer Unit (PTU)** can transfer power (but not fluid!) between Green and Yellow systems. When one engine fails, the PTU on the other engine's system provides power to the failed-engine side.

### 2.3 Four-System Architecture (Airbus A380)

The A380 uses **four hydraulic systems** (Green, Yellow, plus two electrohydraulic systems using EHAs):

- Two conventional systems at **3,000 psi** (Green and Yellow)
- Two additional systems at **5,000 psi** using EHA/EBHA actuators
- The 5,000 psi systems have smaller pipes and valves despite same force output — saving weight

### 2.4 Hydraulic System Segregation

Physical segregation is as important as functional redundancy. The hydraulic systems on modern aircraft are routed through **different physical zones** so that a single event (uncontained engine failure, fuselage rupture, fire) cannot damage more than one system:

- Left system: Runs along port side of fuselage
- Right system: Runs along starboard side
- Centre system: Runs along fuselage centreline
- Separation distances are typically 300–600 mm

This satisfies **Zonal Safety Analysis** (part of Common Cause Analysis — see [Topic 6](./06_Failure_Modes_and_Causes.md)).

---

## 3. Hydraulic System Cross-Connections and Power Transfer

### 3.1 Power Transfer Unit (PTU)

A **PTU** is a hydraulic motor-pump unit that transfers power (not fluid) between two hydraulic systems:

```
SYSTEM A (failing, low pressure)          SYSTEM B (healthy, 3,000 psi)
         │                                         │
    ┌────┴────┐                              ┌─────┴─────┐
    │Hydraulic│ ←── mechanical shaft ──────► │ Hydraulic │
    │  Motor  │     (power transfer)          │   Pump    │
    └────┬────┘                              └─────┬─────┘
         │                                         │
    Returns to System A reservoir            Returns to System B
    (no fluid exchange between systems!)
```

- When System A pressure drops below ~1,500 psi and System B is >2,500 psi, the PTU activates automatically.
- The PTU is controlled by an **electrically actuated PTU shut-off valve** (solenoid valve).
- ECAM/EICAS monitors PTU activation (audible "woof-woof" sound heard during PTU operation on A320 at gate is normal PTU pressure differential checking).

### 3.2 System Interconnect Valve (Cross-Feed Valve)

On some aircraft (e.g., Boeing 737), a **cross-feed valve** allows the fluid from System A pumps to supply System B actuators (and vice versa) in an emergency. This is normally closed.

- **When used:** If all pumps on one system fail but the system piping is intact, cross-feeding from the healthy system can provide pressure.
- **Limitation:** Fluid loss (leak) in one system cannot be helped by cross-feeding — cross-feeding would deplete the healthy system too.
- The cross-feed valve is electrically operated (solenoid) and monitored (position proximity switch).

---

## 4. Priority Valves and Electrical Load Shedding

### 4.1 What Priority Valves Do

In a hydraulic emergency (low pressure, reduced pump capacity), it is essential that **critical hydraulic consumers** (flight controls, brakes) receive hydraulic power before **non-essential consumers** (cargo door, ground equipment).

**Priority valves** are spring-loaded directional control valves that:
- Are normally **open** when system pressure is adequate (spring overcome by pressure)
- **Close automatically** (spring returns) when pressure drops below a set threshold
- Close off non-essential consumers first, maintaining pressure for critical consumers

### 4.2 Priority Hierarchy

```
System pressure drops below 2,200 psi:

Priority 1 (always supplied): 
  ├── Flight control actuators (highest)
  ├── Brakes (high)
  └── Nose wheel steering (high)

Priority 2 (supplied when pressure > 2,200 psi):
  ├── Flap/slat actuators
  ├── Landing gear
  └── Thrust reversers

Priority 3 (not supplied in emergency):
  ├── Cargo door actuators
  ├── Ground loading equipment
  └── Non-essential services
```

### 4.3 Electrical Control of Priority Valves

On modern aircraft, priority valves may be **electrically actuated** (solenoid-operated) rather than purely pressure-actuated, allowing the HSMU to:
- Shed specific loads **selectively** (not just by pressure threshold)
- Restore shed loads when pressure recovers
- Override the automatic shedding logic if required (e.g., crew commands cargo door despite low pressure)

**Monitoring:** Priority valve position is confirmed by proximity switches. ECAM displays which consumers are active and which have been shed.

---

## 5. Ram Air Turbine (RAT)

### 5.1 What Is the RAT?

The **Ram Air Turbine (RAT)** is an emergency device that deploys into the aircraft's slipstream when normal electrical and/or hydraulic power generation is lost. A small propeller converts kinetic energy from the airflow into either hydraulic pressure or electrical power.

```
                    Aircraft Velocity (V)
              ─────────────────────────────►
    ┌────────────────────────────────────┐
    │         AIRCRAFT FUSELAGE         │
    │                                   │
    │      [RAT]──► Propeller ──► Drive │
    └──────────────────────────────┬────┘
                                   │
                     ┌─────────────┴─────────────┐
                     │    (parallel output)       │
               Hydraulic pump               Generator
               (small flow:              (small power:
                ~20 L/min)               ~5–15 kVA)
               ↓                              ↓
          Essential                    Essential
          Hydraulic Bus               Electrical Bus
          (flight controls,           (FCCs, avionics,
           brakes only)                EHSV signal power)
```

### 5.2 RAT Deployment

**Automatic deployment trigger:**
- Loss of both generators AND below a pressure threshold on both hydraulic systems
- On some aircraft: loss of all generator buses triggers automatic RAT deployment

**Manual deployment:**
- Crew can manually deploy RAT via a guarded cockpit switch (emergency procedure)

**Deployment time:** Typically 4–8 seconds from trigger to full RPM output

### 5.3 RAT Capabilities and Limitations

| Capability | Value (Typical A320 RAT) |
|---|---|
| Hydraulic output | ~5 L/min at ~2,500 psi |
| Electrical output | ~5 kVA at 115 VAC |
| Minimum airspeed for operation | ~150 KIAS |
| Control surfaces powered | Ailerons + elevators (Blue system only) |
| Brakes powered | Emergency brakes only |
| Gear extension | Gravity (no hydraulic gear extension power) |

**Critical limitation:** The RAT provides enough power to **control the aircraft** but not enough to power all normal hydraulic consumers. The crew must shed non-essential loads to maintain adequate hydraulic pressure.

### 5.4 Electrical Monitoring of RAT

- **RAT deployed status:** Proximity switch confirms deployment
- **RAT generator output:** Voltage and frequency monitors
- **RAT hydraulic pump pressure:** Pressure transducer on RAT pump output line
- All monitored by ECAM with crew alerting

### 5.5 Historical Example — Air Transat Flight 236 (2001)

A Boeing 767 lost all engine power over the Atlantic due to fuel exhaustion. The RAT deployed and provided emergency hydraulic power. The crew performed a **dead-stick (no-thrust) landing** at Lajes Air Base, Azores. Amazingly, all 306 on board survived.

This event demonstrated the effectiveness of the RAT as an electrical/hydraulic backup — but also demonstrated the importance of proper fuel management (pilot error in fuel transfer procedures was the root cause).

---

## 6. Electric Motor Pumps (EMPs) as Electrical Backup

### 6.1 Role of Electric Motor Pumps

**Engine-Driven Pumps (EDPs)** are the primary hydraulic power source — they run whenever the engine is running. **AC Motor Pumps (ACMPs)** or **Electric Motor Pumps (EMPs)** are the electrical backup:

- Powered by aircraft AC electrical bus (115 VAC, 400 Hz, typically 3-phase)
- Automatically start when EDP pressure falls below threshold
- Can supply full system pressure (3,000 psi) and approximately 60–80% of EDP flow rate

### 6.2 Automatic Starting Logic

The **HSMU** (or equivalent) monitors EDP pressure. If EDP output pressure drops below ~2,200 psi:

```
EDP pressure < 2,200 psi (detected by pressure transducer)
                    │
             HSMU logic module
                    │
    ┌───────────────┴───────────────┐
    │                               │
EDP fault code generated        ACMP enable relay energised
(to CMC, ECAM)                       │
                                 ACMP motor contactor closes
                                       │
                                 ACMP motor starts
                                       │
                          System pressure rises to 3,000 psi
                                       │
                             ECAM EDP low-pressure advisory
                             (crew notified, action may not be required)
```

### 6.3 ACMP Electrical Power Consumption

| Aircraft | ACMP Motor Rating | Supply |
|---|---|---|
| Boeing 737NG | 5 kW per pump | 115 VAC, 3-phase |
| Airbus A320 | 10 kW per pump | 115 VAC, 3-phase |
| Boeing 777 | 15 kW per pump | 115 VAC, 3-phase |
| Boeing 787 | 30 kW per pump | 230 VAC, 3-phase |

### 6.4 ACMP Independence from Engine State

A critical advantage of ACMPs: they can operate when **engines are not running**:
- On the ground: Ground power or APU provides electrical power → ACMP can provide hydraulic pressure for ground maintenance, cargo loading, flight control rigging
- In flight after engine failure: APU can power ACMPs to replace failed EDP
- After all engine failure: RAT → electrical power → ACMP → hydraulic pressure (where RAT has sufficient electrical output)

---

## 7. Accumulator-Based Electrical Backup

### 7.1 What Is a Hydraulic Accumulator?

A hydraulic accumulator stores a volume of pressurised hydraulic fluid that can be delivered instantly when the pump cannot keep up with demand. It consists of a gas charge (nitrogen) separated from the hydraulic fluid by a membrane or piston.

```
      Hydraulic Fluid Port
              │
    ┌─────────┴─────────┐
    │   Hydraulic Fluid  │
    │  (at system press) │
    ├───────────────────┤
    │    Flexible        │
    │    Membrane        │
    ├───────────────────┤
    │   Nitrogen Gas     │
    │  (pre-charge press)│
    └───────────────────┘
```

When system pressure is available, the accumulator charges to system pressure. When system pressure drops (pump fails), the accumulator discharges, maintaining pressure for a period.

### 7.2 Accumulator Applications in Hydraulic Backup

- **Brake accumulator:** Provides sufficient hydraulic pressure for several brake applications even with no pump running. Critically important for a rejected take-off where immediate maximum braking is required at the moment of power loss.
- **Landing gear emergency extension:** Accumulators provide the pressure pulse needed to unlock landing gear uplocks if normal hydraulic power is unavailable.
- **Flight control transient demand:** Accumulators prevent pressure dips during simultaneous large control inputs by multiple surfaces.

### 7.3 Electrical Monitoring of Accumulators

- **Accumulator pressure gauge:** Pressure transducer reads nitrogen pre-charge status
- **Low pre-charge warning:** If nitrogen pressure is below specification, accumulator cannot perform its backup function → ECAM maintenance advisory
- **Brake accumulator pressure:** Dedicated pressure indicator in cockpit (often direct-reading gauge + electrical pressure switch)

---

## 8. Electrical Power Bus Architecture Supporting Hydraulics

### 8.1 Why Bus Architecture Matters

Hydraulic system electrical components (ACMP motors, solenoid valves, ECUs, sensors) are distributed across multiple electrical buses. The bus architecture determines which hydraulic functions survive an electrical bus failure.

### 8.2 Boeing 777 Bus Architecture for Hydraulic Systems

```
Left Generator ──► Left AC Bus ──► Left ACMP-1
                                 └─► Left ACMP-2 (via transfer bus)

Right Generator ──► Right AC Bus ──► Right ACMP-1
                                   └─► Right ACMP-2 (via transfer bus)

APU Generator ──► APU Bus ──► Centre ACMP-1, ACMP-2

RAT Generator ──► Emergency AC Bus ──► Minimum essential loads only
```

**Design principle:** ACMP-1 and ACMP-2 on each hydraulic system are powered from **different buses**. A single bus failure cannot deprive any hydraulic system of both its ACMPs.

### 8.3 Airbus A320 Bus Architecture for Hydraulics

| Bus | Source | Hydraulic Loads |
|---|---|---|
| AC Bus 1 | Engine 1 IDG | Green system ACMP; Blue system ACMP; hydraulic ECU power |
| AC Bus 2 | Engine 2 IDG | Yellow system ACMP; Green system ACMP (secondary) |
| DC Bus 1 / DC Bus 2 | TRU from AC Bus 1/2 | Solenoid valves; sensor excitation; ECU logic power |
| DC Essential Bus | Battery / static inverter | Minimum solenoid valves; essential ECU power |
| Emergency Bus | Battery only | Critical solenoid valve power (last resort) |

### 8.4 Bus Tie Logic

Electrical buses can be **tied together** or **isolated** by electrically controlled bus tie breakers (BTBs) and bus tie contactors:

- Normally: BTBs closed → all buses fed normally from respective generators
- Single generator failure: BTB opens to isolate failed generator; APU or bus tie feeds from remaining generators
- Complete generator loss: Emergency bus powered from battery; RAT provides AC for emergency ACMP

---

## 9. Case Study — Total Hydraulic System Management on Boeing 777

### Scenario: Complete Loss of Left Hydraulic System in Cruise

#### Initial State
- Boeing 777 in cruise at FL370, Mach 0.84
- All three hydraulic systems normal (3,050 psi each)
- All 6 EDPs + 6 ACMPs on AUTO standby

#### Event: Left Engine Fire Causes Hydraulic Leak

1. **T+0 sec:** Left engine fire → fire extinguishing agent deployed → engine shut down.

2. **T+2 sec:** Left system hydraulic line in engine nacelle severed by fire → fluid vents → Left system pressure drops from 3,050 psi → 0 psi.

3. **T+2.5 sec:** **HSMU detects** Left system pressure < 200 psi (both pressure transducers confirm).

4. **T+2.5 sec:** HSMU logic:
   - Energises **Left system isolation valve** solenoid → closes off remaining fluid to prevent further loss
   - Energises **Left ACMP-1 and ACMP-2** relays → both left ACMPs start (they cannot recover pressure since the line is broken, but HSMU attempts recovery before confirming total failure)
   - Sends fault data to CMC

5. **T+3 sec:** Left ACMP-1 and ACMP-2 cannot build pressure → HSMU confirms total Left system loss.

6. **T+3 sec:** EICAS displays: **"HYD L SYS PRESS"** warning (amber). Accompanying EICAS message: "HYD L ISOL VALVE."

7. **T+5 sec:** Crew acknowledges and executes QRH non-normal procedure for "HYD L SYS PRESS":
   - Verify: Left system pressure = 0 (confirmed)
   - Action: LEFT ISO valve confirm closed (CLOSED)
   - Note: Centre and Right systems normal; flight controls and brakes confirmed operating

8. **Ongoing:** Centre system priority valves ensure all primary flight surfaces have full hydraulic power. Right system provides backup brakes. Aircraft can complete the flight and land normally.

#### Left System Consumer Impact

| Consumer | Backup Source | Result |
|---|---|---|
| Left aileron (outboard) | Centre system (inboard aileron) | Reduced roll authority (compensated by FCC) |
| Left elevator | Centre + Right elevator | No noticeable effect |
| Left ground spoilers | Centre spoilers available | Slightly increased landing distance |
| Nose wheel steering (left circuit) | Centre system circuit | No effect |
| Left inboard brakes | Centre system crossfeed | Braking normal |

**Outcome:** Aircraft lands safely. Crew files hydraulic system failure report. Maintenance replaces the damaged hydraulic line and failed left engine.

This case study demonstrates how electrical monitoring, automatic valve logic, and hydraulic redundancy work together to manage what would have been catastrophic on pre-redundancy aircraft.

---

## 10. Common Cause Failure Prevention in Redundant Systems

### 10.1 Physical Separation (Spatial Segregation)

Hydraulic lines, electrical wiring to hydraulic components, and hydraulic ECUs are physically separated to prevent a single event from damaging more than one system:

| Zone | Hydraulic Systems Present | Separation |
|---|---|---|
| Forward fuselage | Left + Centre + Right lines (separated) | Minimum 300 mm apart |
| Wing root | Left (port), Right (starboard) | Separated by wing structure |
| Engine pylon | Left (on port engine), Right (starboard engine) | Separated by fuselage |
| Tail section | All three systems (vertically separated) | Minimum 450 mm |

### 10.2 Dissimilar Redundancy

Some aircraft use **dissimilar components** in redundant positions to prevent common-mode hardware failures:

- Different pressure transducer manufacturers for redundant sensors
- Different microprocessor types in redundant ECU channels
- Different software development teams for redundant flight control computers

### 10.3 Protection Against Specific Common Causes

| Threat | Protection |
|---|---|
| Lightning | Bonding straps; surge suppressors; faraday cage architecture |
| HIRF | Shielded cables; differential signalling; EMC testing per DO-160G |
| Uncontained engine failure | Systems routed away from engine debris zone (per EASA AMC 25.1309) |
| Fuselage decompression | Critical hydraulic lines run below cabin floor level; structural protection |
| Fire | Fire shutoff valves on all hydraulic lines in engine bays; fire-rated insulation |

---

## 12. References

1. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
2. Airbus S.A.S. — *A320 Aircraft Maintenance Manual, Chapter 29 — Hydraulic Power*. Airbus.
3. Boeing Commercial Airplanes — *777 Systems Description — Hydraulic System*. Boeing.
4. Boeing Commercial Airplanes — *737NG QRH — Hydraulic System Non-Normal Procedures*. Boeing.
5. FAA Advisory Circular AC 25.1309-1A — *System Design and Analysis*. FAA.
6. SAE ARP4761 — *Guidelines and Methods for Conducting the Safety Assessment Process*. SAE International, 1996.
7. NTSB Report DCA01MA055 — *Air Transat Flight 236, Fuel Exhaustion Event*. NTSB, 2003.
8. EASA CS-25 — *Certification Specifications for Large Aeroplanes, Subpart F*. EASA.
9. Scholz, D. — *Hydraulic Systems Lecture Notes*. HAW Hamburg.
10. FAA — *Aviation Maintenance Technician Handbook — Airframe, Vol. 2, Chapter 12*. FAA-H-8083-31A.

---

*Previous topic: [Topic 8 — Fly-By-Wire and Electro-Hydrostatic Actuators (EHA)](./08_Fly_By_Wire_and_EHA.md)*
*Next topic: [Topic 10 — Certification and Safety Standards](./10_Certification_and_Safety_Standards.md)*
