# Topic 3 — Types of Electrical Components Used in Hydraulic Systems

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Overview](#1-overview)
2. [Switches](#2-switches)
   - [Pressure Switches](#21-pressure-switches)
   - [Limit Switches](#22-limit-switches)
   - [Flow Switches](#23-flow-switches)
   - [Thermal (Temperature) Switches](#24-thermal-temperature-switches)
3. [Relays](#3-relays)
4. [Solenoids and Solenoid Valves](#4-solenoids-and-solenoid-valves)
5. [Electrohydraulic Servo Valves (EHSV)](#5-electrohydraulic-servo-valves-ehsv)
6. [Electronic Control Units (ECUs / HCUs)](#6-electronic-control-units-ecus--hcus)
7. [Circuit Protection Devices](#7-circuit-protection-devices)
8. [Wiring and Connectors in Hydraulic Environments](#8-wiring-and-connectors-in-hydraulic-environments)
9. [References](#9-references)

---

## 1. Overview

Aircraft hydraulic systems require a variety of electrical components to translate pilot commands and computer outputs into precise hydraulic actions. These components must operate reliably across extreme temperature ranges (−55°C to +125°C), in the presence of hydraulic fluid contamination, vibration, and electromagnetic interference.

The principal categories of electrical components in hydraulic systems are:

- **Switches** (pressure, limit, flow, thermal)
- **Relays** (electromechanical and solid-state)
- **Solenoids** (in directional control valves)
- **Electrohydraulic servo valves (EHSVs)**
- **Electronic control units (ECUs)**
- **Circuit protection devices** (fuses, circuit breakers)

---

## 2. Switches

A switch is a device that opens or closes an electrical circuit in response to a physical condition. In hydraulic systems, switches respond to **pressure**, **position**, **flow**, or **temperature**.

### 2.1 Pressure Switches

#### What It Is

A pressure switch is an electromechanical device that **changes its electrical state** (open ↔ closed contact) when hydraulic fluid pressure crosses a predetermined threshold.

#### Construction

```
    Hydraulic Port (P)
           │
    ┌──────┴──────┐
    │   Diaphragm  │  ← Exposed to hydraulic pressure
    │   or Piston  │
    └──────┬──────┘
           │  Force
    ┌──────┴──────┐
    │    Spring   │  ← Pre-loaded to set-point pressure
    └──────┬──────┘
           │
    ┌──────┴──────┐
    │  Electrical  │
    │   Contact   │  ← Opens or closes when spring is overcome
    └──────┬──────┘
           │
    To Control Circuit
```

#### Types

| Type | Behaviour | Application |
|---|---|---|
| **Normally Open (NO)** | Contact closes when pressure exceeds set point | "Pressure available" signal |
| **Normally Closed (NC)** | Contact opens when pressure exceeds set point | "Overpressure" warning |
| **Differential pressure switch** | Activates when pressure *difference* between two ports exceeds set point | Filter clog detection |

#### Operating Characteristics

- **Set point:** The pressure at which the switch activates (e.g., 2,700 psi low-pressure warning)
- **Deadband (hysteresis):** The difference between activation and deactivation pressure (prevents chatter). Typically 50–150 psi.
- **Response time:** 10–50 ms
- **Leak rate:** Must be < specified limit (often 0 drops per minute at set pressure)

#### Aircraft Examples

- **Boeing 737 (Systems A & B):** Pressure switches at ~2,700 psi illuminate the **HYD** low-pressure warning on the overhead panel.
- **Weight-on-Wheels (WOW) switches:** Some WOW systems use air-spring and hydraulic-pressure sensing to determine if the aircraft is on the ground, gating systems such as ground spoiler arming.
- **Landing gear downlock:** A hydraulic pressure switch in the downlock actuator circuit confirms the actuator has built sufficient pressure to hold the gear locked down.

#### Failure Modes

| Failure Mode | Effect |
|---|---|
| Contact stuck closed | False "pressure normal" indication when pressure is low |
| Contact stuck open | Permanent "low pressure" warning; crew may incorrectly shut down a functional pump |
| Diaphragm rupture | Hydraulic fluid enters wiring harness; potential short circuit |
| Spring fatigue | Set point drifts low; spurious low-pressure warnings |

---

### 2.2 Limit Switches

#### What It Is

A limit switch (or position switch) detects when a **mechanical component has reached a specific physical position**. In hydraulic systems, it confirms that valves, doors, or landing gear have fully opened, closed, locked, or unlocked.

#### Types Used in Aircraft Hydraulic Systems

**a) Mechanical Limit Switch (Microswitch)**

- A physical plunger or roller lever is depressed by the moving component.
- Contact snaps open or closed at a precise actuation force.
- Robust, simple, and cheap.
- **Weakness:** Moving parts wear; vulnerable to contamination.

**b) Inductive Proximity Switch (IPS)**

- No physical contact — senses metal presence via electromagnetic induction.
- Generates a small eddy current in the target metal; this changes the sensor's oscillator circuit and triggers a switch output.
- Zero wear; not affected by fluid contamination.
- **Standard on modern aircraft** for landing gear proximity sensing.

**c) Reed Switch**

- A permanent magnet on the moving component closes two ferromagnetic contact strips sealed inside a glass tube.
- No external power required for the switch element.
- Used where mechanical contact is difficult (e.g., inside sealed actuators).

#### Aircraft Applications

| Application | Switch Type | Signal Provided |
|---|---|---|
| Landing gear uplock confirmed | Inductive proximity | "Gear retracted and locked" |
| Landing gear downlock confirmed | Inductive proximity | "Gear extended and locked" |
| Gear door open confirmed | Mechanical microswitch | "Doors open — safe to extend gear" |
| Gear door closed confirmed | Inductive proximity | "Doors closed" |
| Cargo door latch engaged | Reed switch | "Door secure — pressurisation permitted" |
| Thrust reverser stowed | Mechanical microswitch | "Reverser stowed — engine cleared for high thrust" |
| Flap/slat track alignment | Inductive proximity | "Flap/slat correctly seated" |

#### Certification Requirement

FAA FAR 25.729 / EASA CS 25.729 requires that landing gear uplock and downlock be positively indicated to the flight crew. Limit/proximity switches are the primary means of providing this indication.

---

### 2.3 Flow Switches

#### What It Is

A flow switch detects the **presence or absence of hydraulic fluid flow** in a line, typically using a paddle or flapper inside the pipe.

#### How It Works

```
    Flow Direction →
    ┌──────────────────────────────┐
    │    ┌──────┐                  │
    │    │Paddle│ ← Deflects when  │
    │    │ /   │    fluid flows    │
    │    └──┬───┘                  │
    │       │ (pivot)              │
    │       └──► Electrical contact│
    └──────────────────────────────┘
```

When fluid flows, the paddle deflects and closes (or opens) the electrical contact.

#### Applications

- **Pump output monitoring:** Confirms a hydraulic pump is actually delivering flow — important because a pump can maintain pressure briefly even when flow has stopped (due to accumulator pressure).
- **Distinguishing fault types:**
  - Low pressure + no flow → pump failure
  - Low pressure + flow present → relief valve stuck open or major downstream leak
- **Air bleeding confirmation:** After maintenance, a flow switch can confirm fluid is flowing correctly through a refilled actuator or line.

---

### 2.4 Thermal (Temperature) Switches

A **thermal switch** opens or closes its contacts at a specific temperature. It is distinct from a temperature *sensor* (which provides a continuous output proportional to temperature) — a thermal switch provides only an ON/OFF signal.

#### Types

| Type | Principle | Temperature Range |
|---|---|---|
| **Bimetallic strip switch** | Two metals with different expansion rates bow at set temperature | −40 to +150°C |
| **Thermistor threshold switch** | NTC thermistor resistance change triggers comparator circuit | −55 to +150°C |

#### Applications

- **Hydraulic fluid high-temperature warning:** A thermal switch in the reservoir activates a cockpit warning light when temperature exceeds ~120°C.
- **Pump thermal protection:** A switch on the pump body activates if the pump case temperature exceeds safe limits.
- **Hydraulic line fire detection:** In the engine bay, thermal switches or fusible links detect hydraulic fluid fires.

---

## 3. Relays

### What It Is

A relay is an **electrically operated switch**. A small electrical current through an electromagnetic coil creates a magnetic field that moves one or more sets of heavier-duty contacts.

### Why Relays Are Necessary

Cockpit switches and flight computer outputs carry only **milliamperes** of current — insufficient to:
- Directly power large solenoid valve coils (which may need 1–3 amps)
- Start electric motor pumps (which draw 30–50 amps at start-up)

A relay bridges this power gap: the pilot's 5 mA switch current energises the relay coil; the relay contacts then connect the high-current load to the aircraft bus.

```
     Pilot Switch (5mA)
          │
     [Relay Coil]  ← Creates magnetic field
          │ (magnetic force)
     [Armature moves]
          │
     [Heavy Contacts Close] ← 28 VDC / 115 VAC bus (up to 50A)
          │
     Load (ACMP motor, solenoid valve, etc.)
```

### Types of Relays Used in Aircraft Hydraulic Systems

#### a) General-Purpose Electromechanical Relay (EMR)

- Coil voltage: 28 VDC
- Contact rating: 10–50 A
- Used for: ACMP on/off switching, solenoid valve power switching
- Advantages: Simple, proven technology
- Disadvantages: Contact wear over time; contact arcing on high-current loads

#### b) Latching Relay (Bistable Relay)

- Has two stable positions. Closing coil energises to set; opening coil energises to reset.
- Retains its last state with no power applied.
- Used for: Landing gear position indication (retains gear position across brief power interruptions), cargo door lock status
- Essential where loss of electrical power must not change the system state.

#### c) Solid-State Relay (SSR)

- Uses **transistors or SCRs (silicon-controlled rectifiers)** instead of mechanical contacts.
- No moving parts → no arcing, no contact wear.
- Switching speed: microseconds (vs. milliseconds for EMR).
- Used on modern aircraft for high-cycle switching applications (e.g., brake system control units).
- Disadvantage: More susceptible to overvoltage damage than EMR.

#### d) Contactor (Heavy-Duty Relay)

- A relay rated for very high currents (100–500 A).
- Used to connect/disconnect electric motor pump motors.
- Includes arc suppression features (magnetic blowout or vacuum contacts).

### Relay Circuit Example — ACMP Control

```
NORMAL OPERATION:
  Crew selects ACMP ON (overhead panel)
  →  28V signal applied to relay coil K1
  →  K1 contacts close
  →  115 VAC Phase A/B/C connected to ACMP motor
  →  ACMP starts; pressure rises
  →  Pressure switch confirms normal pressure

AUTOMATIC OPERATION (EDP failed):
  HSMU detects EDP low pressure (pressure transducer <2,200 psi)
  →  HSMU logic output energises relay K2 (ACMP AUTO)
  →  K2 contacts close (in parallel with crew switch)
  →  ACMP starts automatically
```

---

## 4. Solenoids and Solenoid Valves

### What a Solenoid Is

A **solenoid** is an electromechanical transducer that converts **electrical current** into **linear mechanical force**. It consists of:

- A cylindrical coil of insulated copper wire
- A ferromagnetic core (plunger) inside the coil
- A spring to return the plunger when current is removed

When current flows through the coil, a magnetic field is created that **pulls the plunger into the coil** against the spring force. When current stops, the spring pushes the plunger back.

### Solenoid-Operated Directional Control Valves (DCVs)

In hydraulic systems, solenoids are built into **directional control valves** to shift the valve spool electrically.

#### 2-Position, 2-Way (2/2) Valve

```
Energised (solenoid ON):    Port A ──► Port B (open)
De-energised (solenoid OFF): Port A ─X─ Port B (closed)
```
Used for: Hydraulic fluid shutoff, accumulator isolation

#### 2-Position, 4-Way (4/2) Valve (most common in aircraft)

```
Position 1 (solenoid A ON):
    Pressure (P) ──► Actuator Extend Port (A)
    Actuator Retract Port (B) ──► Return (T)

Position 2 (solenoid B ON, or spring return):
    Pressure (P) ──► Actuator Retract Port (B)
    Actuator Extend Port (A) ──► Return (T)
```
Used for: Landing gear retraction/extension, flap actuators, thrust reversers

#### 3-Position, 4-Way (4/3) Valve

Adds a **centre/neutral position** where all ports are blocked (actuator holds position):
- **Spring-centred:** Solenoid off → valve centres automatically (failsafe hold)
- **Detented:** Solenoid off → valve stays in last position

Used for: Flight control surfaces requiring hold position when no command

### Types of Solenoid Valves

| Type | Construction | Control Signal | Application |
|---|---|---|---|
| **On/Off solenoid valve** | Simple pull-in solenoid; two positions | Binary (0/28V) | Landing gear, flaps, brakes |
| **Proportional solenoid valve** | Force-controlled plunger; proportional to current | 4–20 mA | Variable-flow hydraulic consumers |
| **Servo valve (EHSV)** | Torque motor + flapper-nozzle + spool | ±10–100 mA | All primary flight controls |

### Solenoid Valve Specifications (Typical Aircraft Grade)

| Parameter | Value |
|---|---|
| Operating voltage | 28 VDC ±4V |
| Coil resistance | 20–100 Ω |
| Operating current | 250–1,000 mA |
| Response time (energise) | 15–50 ms |
| Response time (de-energise) | 10–30 ms |
| Proof pressure | 1.5× rated system pressure |
| Burst pressure | 4× rated system pressure |
| Operating temperature | −55°C to +125°C |
| Qualification | DO-160G Environmental Conditions |

### Solenoid Valve Failure Modes

| Mode | Cause | Effect on Hydraulic System |
|---|---|---|
| Coil open circuit | Wire break, burn-out | Valve remains de-energised (spring-return position) |
| Coil short circuit | Insulation failure | Circuit breaker trips; valve de-energises |
| Spool stuck | Contamination, corrosion | Valve fails to shift; function lost |
| Spool hardover | Manufacturing defect; contamination | Valve locks in commanded position; cannot return |
| External leak | Failed O-ring or seal | Hydraulic fluid loss; potential fire hazard |

---

## 5. Electrohydraulic Servo Valves (EHSV)

### What It Is

An **electrohydraulic servo valve (EHSV)** is an extremely precise proportional hydraulic valve that translates a milliampere-level electrical signal from a flight computer into a precisely proportional hydraulic flow to a control surface actuator.

EHSVs are the key interface between digital fly-by-wire flight computers and hydraulic actuators.

### Two-Stage Flapper-Nozzle Design (Most Common)

```
Stage 1 — Torque Motor and Flapper-Nozzle:

  Current Input (±mA)
        │
   [Torque Motor Coils]   ← Two opposing coils create proportional force
        │
   [Armature/Flapper]     ← Deflects proportional to current
        │
   [Two Nozzles]          ← One nozzle partially closed, other opens
        │
   Differential Pressure  ← Creates pilot pressure to move Stage 2 spool


Stage 2 — Main Control Spool:

   Pilot Pressure ──► [Spool shifts proportionally]
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
       Actuator Port A          Actuator Port B
       (extend side)            (retract side)
```

**Feedback loop within the EHSV:**
An LVDT measures the spool position and feeds back to the torque motor, correcting any error. This inner loop operates at ~1,000 Hz, giving the EHSV its exceptional precision.

### Performance Specifications (Typical EHSV for Flight Controls)

| Parameter | Value |
|---|---|
| Input current range | ±10 mA to ±100 mA (depending on model) |
| Rated flow (at rated pressure drop) | 5–50 litres/min |
| Null leakage | <0.5 litres/min |
| Threshold (minimum signal to cause movement) | <0.1% of full input |
| Hysteresis | <3% of rated flow |
| Frequency response (at 90° phase lag) | 50–300 Hz |
| Proof pressure | 1.5× system pressure (4,500 psi for 3,000 psi system) |
| Temperature range | −55°C to +125°C |

### Aircraft Examples

- **Airbus A320:** Each aileron, elevator, and rudder actuator is driven by an EHSV receiving commands from the Flight Control Computers (ELAC, SEC, FAC). Commands are updated 25 times per second (40 ms cycle).
- **Boeing 777:** All six primary flight control surfaces (two ailerons, two elevators, rudder) are driven by EHSVs commanded by the Primary Flight Computers (PFCs) via ARINC 629.
- **F/A-18 Hornet:** EHSVs drive leading edge flaps, trailing edge flaps, ailerons, horizontal stabilisers — all at high update rates for supersonic manoeuvring.

---

## 6. Electronic Control Units (ECUs / HCUs)

### Role

An **Electronic Control Unit (ECU)** or **Hydraulic Control Unit (HCU)** is a dedicated microprocessor-based controller that:
- Receives commands from flight computers or crew switches
- Reads sensor inputs
- Executes control logic and sequencing
- Energises/de-energises solenoids and relays
- Performs BITE and fault logging

### Architecture (Typical)

```
INPUTS                          OUTPUTS
──────                          ───────
Pressure transducers ──┐   ┌─── Solenoid valve drivers
Temperature sensors  ──┤   ├─── Relay coil drivers
Level sensors        ──┤   ├─── ACMP start/stop
Proximity switches   ──┤   ├─── Warning/advisory signals to ECAM
Position LVDTs       ──┤   ├─── Data to CMC (maintenance)
ARINC 429/629 bus    ──┤   ├─── ARINC data output
Crew switches        ──┘   └─── BITE fault codes

                    [ECU Microprocessor]
                    - Control laws
                    - Sequencing logic
                    - Limit monitoring
                    - BITE
                    - Fault management
```

### Key Aircraft ECUs

| ECU | Aircraft | Function |
|---|---|---|
| LGCU (Landing Gear Control Unit) | Most transport aircraft | Controls landing gear retraction/extension sequence |
| HSMU (Hydraulic System Management Unit) | Boeing 777 | Manages all pump switching, monitoring, system isolation |
| BSCU (Braking and Steering Control Unit) | Airbus A320, Boeing 737 | Anti-skid brake pressure modulation; nose-wheel steering |
| SFCC (Slat/Flap Control Computer) | Airbus A320 | Controls slat and flap deployment sequencing |
| ELAC/SEC/FAC | Airbus A320 | Primary flight control computers driving EHSVs |

---

## 7. Circuit Protection Devices

### Fuses

- A fusible element that melts and breaks the circuit on excessive current.
- **One-time use:** must be replaced after blowing.
- Used for: Some sensor circuits; legacy aircraft.

### Circuit Breakers (CBs)

- A resettable protective device that trips on overcurrent.
- **Types in aircraft:**
  - **Thermal CB:** Trips on sustained overcurrent (bimetallic trip element).
  - **Hydraulic-magnetic CB:** Trips rapidly on large fault currents; slower trip on moderate overcurrent.
- **Pull-to-trip / push-to-reset** operation.
- Located on CB panels in flight deck and E&E bay.
- **Solenoid valve circuit breakers** are typically 3–7.5 A (28 VDC).
- **ACMP motor contactors** are protected by thermal-magnetic CBs in the E&E bay.

### Transient Voltage Suppression (TVS)

Solenoid coils store energy in their magnetic field. When de-energised, this energy is released as a **voltage spike** (back-EMF) that can damage the switching transistor or relay contacts.

**Protection methods:**
- **Freewheeling diode:** Installed across the coil; conducts back-EMF current, dissipating it as heat.
- **Zener diode clamp:** Limits back-EMF to a safe voltage.
- **RC snubber:** Resistor-capacitor network absorbs the spike.

---

## 8. Wiring and Connectors in Hydraulic Environments

### Wiring Standards

Aircraft hydraulic control wiring must meet **MIL-W-22759** (aerospace wire) or **MIL-W-81044**:
- Extruded PTFE insulation (resistant to Skydrol hydraulic fluid)
- Rated −65°C to +200°C
- Shielded for EMI protection in sensor circuits

### Connectors

Hydraulic system connectors are typically **MIL-DTL-38999** circular connectors with:
- Stainless steel or aluminium shell with keyway to prevent misengagement
- Gold-plated contacts for corrosion resistance
- Silicone rubber grommet seals against fluid and moisture ingress
- Backshell with grounding provisions for shielded cables

### Maintenance of Wiring and Connectors

| Concern | Maintenance Action |
|---|---|
| Hydraulic fluid contamination | Clean with isopropyl alcohol; inspect insulation for swelling/cracking |
| Pin corrosion | Replace corroded pins; apply Kopr-Shield compound |
| Chafing | Install additional chafe protection (PTFE sleeving); reroute if necessary |
| Connector backshell looseness | Torque to specified value; safety-wire if required |
| Insulation resistance | Test with Megger (min. 1 MΩ at 500 VDC in most cases) |

---

## 9. References

1. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
2. Ogata, K. — *Modern Control Engineering* (5th ed.). Prentice Hall, 2010.
3. Merritt, H.E. — *Hydraulic Control Systems*. Wiley, 1967.
4. Parker Hannifin Corporation — *Industrial and Aerospace Solenoid Valve Design Guide*. Parker, 2018.
5. Moog Inc. — *Electrohydraulic Servo Valve Technical Manual*. Moog Controls Division.
6. FAA — *AMT Handbook — Airframe Vol. 2, Chapter 12*. FAA-H-8083-31A.
7. MIL-DTL-38999 — *Connectors, Electrical, Circular, Miniature, High Density, Environment Resistant*. US DoD.
8. DO-160G — *Environmental Conditions and Test Procedures for Airborne Equipment*. RTCA, 2010.

---

*Previous topic: [Topic 2 — The Role of Electrical Systems in Hydraulic Systems](./02_Role_of_Electrical_Systems.md)*
*Next topic: [Topic 4 — Types of Sensors Used in Aircraft Hydraulic Systems](./04_Sensors.md)*
