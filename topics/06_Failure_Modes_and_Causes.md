# Topic 6 — Failure of Electrical Components and Sensors — Causes and Consequences

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Introduction to Failure Analysis](#1-introduction-to-failure-analysis)
2. [Failure Modes of Electrical Components](#2-failure-modes-of-electrical-components)
   - [Open Circuit](#21-open-circuit)
   - [Short Circuit](#22-short-circuit)
   - [Relay Contact Failures](#23-relay-contact-failures)
   - [Solenoid Coil Failures](#24-solenoid-coil-failures)
   - [Electronic Control Unit Failures](#25-electronic-control-unit-failures)
3. [Failure Modes of Sensors](#3-failure-modes-of-sensors)
   - [Sensor Hardover (Stuck Output)](#31-sensor-hardover-stuck-output)
   - [Sensor Drift](#32-sensor-drift)
   - [Loss of Signal (Null Output)](#33-loss-of-signal-null-output)
   - [Intermittent Sensor Output](#34-intermittent-sensor-output)
4. [Root Causes — Environmental Factors](#4-root-causes--environmental-factors)
5. [Safety Analysis Methods](#5-safety-analysis-methods)
6. [Consequences of Specific Failures](#6-consequences-of-specific-failures)
7. [Failure Detection and Crew Alerting](#7-failure-detection-and-crew-alerting)
8. [Case Studies](#8-case-studies)
9. [References](#9-references)

---

## 1. Introduction to Failure Analysis

Failure analysis in aircraft hydraulic electrical systems is not simply an engineering exercise — it is a **regulatory requirement**. Both the FAA (FAR Part 25) and EASA (CS-25) require that manufacturers demonstrate that all foreseeable failure conditions have been identified, their effects assessed, and that catastrophic outcomes have been made extremely improbable.

### Why Failure Analysis Is Essential

- Aircraft hydraulic systems operate at pressures up to 5,000 psi, moving massive control surfaces at high speed. Uncontrolled failures can lead to loss of flight control.
- Electrical components (solenoids, sensors, wires) are far more susceptible to certain failure modes (corrosion, EMI, thermal degradation) than pure mechanical components.
- Modern systems have thousands of electrical connections — each one a potential failure point.

### Failure Classification

Per **SAE ARP4761**, failure conditions are classified by their effect on the aircraft:

| Classification | Effect | Probability Requirement |
|---|---|---|
| **Catastrophic** | Loss of aircraft / fatalities | <10⁻⁹ per flight hour |
| **Hazardous** | Large reduction in safety margin | <10⁻⁷ per flight hour |
| **Major** | Significant reduction in crew capability | <10⁻⁵ per flight hour |
| **Minor** | Slight reduction in safety margin | <10⁻³ per flight hour |
| **No Safety Effect** | No effect on safety | No requirement |

---

## 2. Failure Modes of Electrical Components

### 2.1 Open Circuit

#### Definition
An open circuit is a break in the electrical conduction path — no current can flow regardless of applied voltage.

#### Causes
- **Wire breakage:** Fatigue from repeated flexing (especially on moving structures like landing gear doors); chafing against metallic structure
- **Connector pin push-back:** Vibration causes the pin to retreat into the connector backshell, breaking contact
- **Connector pin corrosion:** Oxidation of contact surface increases resistance → eventually breaks contact completely
- **Coil wire breakage:** Fine copper wire in solenoid coil fatigues and breaks at stress concentrations
- **PCB track fracture:** Solder joint cracking under vibration; thermal cycling fatigue

#### Effect on Hydraulic System

For a **solenoid valve** with an open circuit:
- The valve cannot be energised.
- The valve remains in its **spring-return (de-energised) position** regardless of commands.
- **Spring-return to safe position** (fail-safe design): Most hydraulic solenoid valves are designed so that their de-energised (spring) position is the safe state (e.g., landing gear down-and-locked, thrust reversers stowed, brakes released).
- If the valve's safe position is not the desired flight position, function is lost (e.g., gear cannot retract; flaps cannot extend beyond current position).

#### Detection
- BITE detects zero current through coil when commanded ON → generates "solenoid open circuit" fault code
- ECAM/EICAS advisory
- Position sensor does not confirm commanded position within time limit

---

### 2.2 Short Circuit

#### Definition
A short circuit is an unintended low-resistance path that allows excessive current to flow, typically bypassing the load.

#### Causes
- **Insulation chafing:** Wire routed too close to metallic structure; repeated vibration erodes insulation → conductor contacts airframe
- **Hydraulic fluid ingress:** Degraded Skydrol (with metallic wear particles) becomes conductive; bridges connector contacts
- **Component failure:** Internal solenoid coil insulation breakdown (turn-to-turn short)
- **Moisture ingress:** Water in connectors reduces insulation resistance

#### Effect on Hydraulic System

For a **solenoid valve circuit**:
- Circuit breaker trips → valve is suddenly de-energised
- If the circuit was holding the valve in an important position (e.g., gear doors open, brakes applied), loss of solenoid power may be hazardous in that moment
- Potential for **wiring fire** if the short occurs before the circuit breaker and the breaker is slow to respond

For a **sensor circuit**:
- Short to ground → sensor output reads zero (null) → system may interpret as "zero pressure," "zero temperature," or "gear not locked"
- Short to power → sensor output reads maximum → system interprets as "maximum pressure," "overtemperature," etc.

#### Detection
- Circuit breaker trips (crew visible on CB panel)
- Sensor output at rail voltage (5V or 0V consistently) → BITE flags as sensor failure

---

### 2.3 Relay Contact Failures

#### Contact Welding (Stuck Closed)

Under high inrush current (e.g., starting an electric motor pump), the contacts arc as they close. If arc energy is sufficient, the contacts **weld together** in the closed position.

**Effect:**
- Relay cannot be opened by de-energising the coil
- The load (ACMP motor, solenoid valve) remains permanently energised
- An ACMP that cannot be stopped will overheat and fail
- A solenoid valve permanently energised may hold a system in an incorrect position

**Detection:**
- BITE monitors: command relay OFF → verify current drops to zero → if current remains → contact welded
- May require maintenance action (circuit breaker pull and relay replacement)

#### Contact Pitting and Erosion (Intermittent Open)

Repeated arcing gradually erodes contact surfaces → contact resistance increases → voltage drop across contacts increases → solenoid does not receive sufficient voltage to fully actuate.

**Effect:**
- Solenoid valve actuates slowly (stiction)
- Valve spool does not fully shift → hydraulic flow restricted
- System pressure takes longer to build → sequencing time-outs may trigger fault codes

#### Contact Contamination (False Open or False Close)

Hydraulic fluid mist or moisture in the relay enclosure contaminates contacts:
- **High resistance film** → contact appears open when closed (voltage drop)
- **Conductive contamination** → contact appears closed when open (leakage current)

---

### 2.4 Solenoid Coil Failures

#### Coil Burnout (Thermal Failure)

A solenoid coil is a resistive load. Its power dissipation is P = I² × R (typically 5–30 watts). This heat must be conducted to the valve body and surrounding structure. If heat cannot escape:

- Coil temperature rises above insulation rating (typically 155°C Class F insulation)
- Insulation carbonises → short circuits within the coil
- Coil resistance drops → current increases → runaway thermal failure

**Causes:**
- Coil rated for intermittent duty energised continuously
- Ambient temperature exceeds design limit (e.g., valve in engine nacelle close to hot bleed air duct)
- Incorrect voltage applied (over-voltage increases current disproportionately)
- Hydraulic fluid contamination increasing spool friction → plunger not fully pulled in → coil must sustain holding current longer than designed

#### Coil Impregnation Failure

Aircraft solenoid coils are potted in epoxy resin to resist vibration and moisture. If the potting compound delaminates or cracks:
- Moisture enters and corrodes coil windings
- Wire turns short together (inter-turn short) → reduced inductance → increased current → possible burnout

---

### 2.5 Electronic Control Unit Failures

#### Processor/Memory Failure

- SRAM or Flash memory bit flips due to cosmic radiation (higher risk at altitude)
- Processor executes incorrect instructions → outputs wrong solenoid commands
- **Mitigation:** Error-correcting code (ECC) memory; watchdog timers; voting between redundant computers

#### Power Supply Failure

- ECU internal power supply fails → unit loses power → all outputs go to de-energised state
- Aircraft electrical bus fluctuation may cause reset of ECU → momentary loss of hydraulic control
- **Mitigation:** EMC filtering; input protection; dual redundant power supplies in critical ECUs

#### Software Failure (Common-Cause)

- Both channels of a redundant ECU run the same software → a software bug affects both simultaneously
- Cannot be detected by inter-channel voting
- **Mitigation:** DO-178C certification; independent testing; dissimilar software on different channels (rare)

---

## 3. Failure Modes of Sensors

### 3.1 Sensor Hardover (Stuck Output)

A **hardover** failure is when a sensor outputs a constant incorrect value — typically stuck at the maximum or minimum of its range.

**Example:** A hydraulic pressure transducer (range 0–5,000 psi, output 0.5–4.5 V) fails stuck at 4.5 V.

- System reads maximum pressure (5,000 psi) at all times.
- If system pressure actually drops (e.g., pump failure), the sensor will NOT detect it.
- HSMU will not start backup pump → system pressure falls → loss of hydraulic power.

**Mitigation (Triplex Voting):**

```
Sensor 1 output: 3,050 psi ─┐
Sensor 2 output: 3,045 psi ─┼─► Median: 3,050 psi (Sensor 3 voted out)
Sensor 3 output: 5,000 psi ─┘   ← FAILED (stuck high)
```

The median selector automatically rejects the outlier. Maintenance alert is generated.

---

### 3.2 Sensor Drift

Sensor drift is a **gradual change** in sensor output that does not reflect the true physical quantity.

**Causes:**
- Thermal cycling over thousands of hours causes zero-point shift in strain gauges
- Vibration causes LVDT core position to shift within the transformer (zero drift)
- Moisture absorption into sensor circuitry changes reference voltages
- Age-related resistance change in RTDs

**Effect:**
- Gradual: hard to detect without calibration checks
- Closed-loop control: if LVDT zero drifts, the control surface will be trimmed slightly off-neutral → requires increased pilot trim input → possible structural fatigue implications over long-term

**Detection:**
- Periodic calibration checks against known reference (e.g., apply 3,000 psi of hydraulic pressure; sensor should read 3,000 ±15 psi)
- Cross-comparison between redundant sensors in flight
- Long-term trending in HUMS data

---

### 3.3 Loss of Signal (Null Output)

Sensor outputs zero regardless of physical quantity. Distinguished from hardover (stuck high) by being stuck at minimum.

**Causes:**
- Open circuit in sensor wiring (see Section 2.1)
- Short to ground in sensor signal wire
- Loss of sensor excitation voltage (LVDT excitation failure)
- Physical damage to sensor (impact, vibration fracture)

**Effect:**
- System reads "zero pressure," "zero degrees," or "fully retracted"
- May trigger spurious warnings ("hydraulic pressure failure" when system is normal)
- May cause flight computer to command actuator toward the "actual" position it reads (zero = no deflection) → fighting real deflection

---

### 3.4 Intermittent Sensor Output

**The most difficult failure to diagnose** — the sensor output is correct most of the time but occasionally drops out or spikes.

**Causes:**
- Partially fractured wire (breaks under vibration, reconnects at rest)
- Loose connector pin (intermittent contact)
- Partial short (resistance varies with temperature)

**Effect:**
- System alternately reads correct and incorrect values
- May generate spurious fault codes that cannot be reproduced on ground ("No Fault Found" — NFF maintenance results)
- In worst case, affects actuator position control momentarily → brief surface excursion

**Detection:**
- HUMS trend analysis: frequency of fault code generation increasing over time → wiring degradation predicted
- Connector wiggle test during maintenance

---

## 4. Root Causes — Environmental Factors

| Factor | Effect on Electrical/Sensor Components |
|---|---|
| **Vibration (continuous)** | Wire fatigue fractures; solder joint cracking; LVDT core zero drift; relay contact wear |
| **Shock (transient)** | Connector pin push-back; solder joint fracture; sensor zero shift |
| **Temperature extremes** | Insulation cracking at low temp; conductor resistance increase; seal extrusion at high temp |
| **Thermal cycling** | Differential expansion causes connector pin looseness; PCB via cracking |
| **Hydraulic fluid (Skydrol)** | Attacks connector seals; degrades wiring insulation; conductive when contaminated |
| **Moisture and condensation** | Corrosion of connector pins; electrolytic migration in connector backshell |
| **EMI (HIRF)** | Induced false signals in unshielded sensor cables; upset digital electronics |
| **Lightning** | Conducted surge through aircraft structure; can damage ECU inputs |
| **Mechanical impact** | Wheel well sensors damaged by runway debris; engine pylon sensors by tool drops |
| **Chemical contamination** | Cleaning solvents used during maintenance may attack insulation or connector potting |

### Environmental Zones in Aircraft

Not all hydraulic electrical components face the same environment:

| Zone | Location | Worst Hazards |
|---|---|---|
| Wheel well / landing gear bay | Undercarriage | Runway debris, water, extreme temperature, vibration |
| Engine nacelle / pylon | Near engines | High temperature, vibration, fuel/oil/hydraulic fluid |
| Wing leading edge | Inside wing | Cold soak (−55°C), hydraulic fluid |
| Rear fuselage (near hydraulic lines) | Service areas | Hydraulic fluid, vibration |
| Pressurised fuselage E&E bay | Electronics bay | Relatively benign — temperature and humidity controlled |

---

## 5. Safety Analysis Methods

### 5.1 Failure Mode and Effects Analysis (FMEA)

FMEA is a **bottom-up** analysis: starting from individual components, each failure mode is identified, and its effect at the system level is determined.

**Process:**
1. List every component (e.g., solenoid valve, pressure switch, LGCU)
2. For each component, list all possible failure modes (open circuit, short circuit, stuck, etc.)
3. For each failure mode, determine:
   - **Effect on the immediate system** (e.g., valve stuck open)
   - **Effect on the aircraft** (e.g., landing gear cannot retract)
   - **Failure condition category** (Catastrophic / Hazardous / Major / Minor)
   - **Detection method** (BITE, ECAM, crew procedure)
   - **Mitigations** (redundancy, fail-safe design, crew procedure)

### 5.2 Fault Tree Analysis (FTA)

FTA is a **top-down** analysis: starting from the worst-case failure event ("hazard top event"), logic trees trace all combinations of lower-level events (component failures) that could cause it.

**Example top event:** "Total loss of all hydraulic flight control"

```
Total Loss of All Hydraulic Flight Control
                    |
              [AND gate]  ← All three systems must fail simultaneously
             /    |    \
            /     |     \
         Sys 1   Sys 2   Sys 3
         fails  fails   fails
          |       |       |
        [OR]    [OR]    [OR]
       /   \   /   \   /   \
     EDP  ACMP ...  ...  ...
     fails fails
```

The probability of the top event = product of probabilities of all independent failures in the AND gate chains. For a catastrophic event, this product must be < 10⁻⁹.

### 5.3 Common Cause Analysis (CCA)

CCA identifies failures that could affect **multiple independent channels** simultaneously:

- **Zonal Analysis:** A fire, fluid spray, or structural failure in a zone could damage all hydraulic systems running through that zone.
- **Particular Risk Analysis:** Lightning, HIRF, bird strike, tire burst, engine uncontained failure — could each of these affect multiple redundant hydraulic or electrical systems?
- **Common-Mode Analysis:** Are redundant computers truly independent? Do they share power supplies, cooling, or software?

**Aircraft Design Responses to CCA:**
- Hydraulic systems routed through **different fuselage zones** — if one is severed by uncontained engine debris, the others survive.
- Electrical buses fed from **different generators** — loss of one generator does not affect all hydraulic pumps.
- Redundant flight control computers use **dissimilar microprocessors** and **dissimilar software** to prevent common-mode software failures.

---

## 6. Consequences of Specific Failures

### 6.1 Failure of All Pressure Sensors on One System

- HSMU cannot confirm system pressure → cannot start backup pump automatically
- Crew must manually monitor and manage pump switching
- **Outcome:** Higher crew workload; no loss of safety (hydraulic system still functions, monitoring degraded)
- **Classification:** Major

### 6.2 Failure of LVDT on Primary Flight Control Actuator

With triplex redundancy:
- One LVDT failed → voting detects outlier → system continues normally
- Maintenance alert generated
- **Classification:** Minor

Without redundancy (theoretical older design):
- Actuator receives incorrect feedback → control law drives surface to wrong position
- **Classification:** Hazardous (depending on surface)

### 6.3 Failure of LGCU (Landing Gear Control Unit)

- Landing gear lever has no effect (no electrical commands generated)
- Crew must use **alternate extension system** (gravity/manual hydraulic)
- **Outcome:** Extra workload; landing gear may not retract (accept if take-off, extend for landing)
- **Classification:** Major (not catastrophic because alternate means exist)

### 6.4 Simultaneous Failure of LGCU and Alternate System

This is an **extremely improbable** combination (probability ~ 10⁻¹⁰ per flight hour):
- Landing gear cannot be extended → forced belly landing
- **Classification:** Catastrophic
- **Probability required:** < 10⁻⁹ — met by the combination of two independent failure probabilities

---

## 7. Failure Detection and Crew Alerting

### 7.1 BITE (Built-In Test Equipment)

Most hydraulic control electronics include continuous or periodic self-testing:

- **Power-on BITE:** Tests all sensor excitation circuits; verifies sensor outputs are within range; tests solenoid coil continuity.
- **Continuous BITE:** Monitors sensor outputs for out-of-range values; detects impossible sensor combinations; monitors solenoid current during commanded actuation.
- **Maintenance BITE:** Ground-only tests commanded via Centralised Maintenance System (CMS); includes functional tests of all solenoid valves and actuators.

### 7.2 ECAM / EICAS Warning Philosophy

Modern aircraft use a **three-level alert hierarchy** for hydraulic electrical failures:

| Level | Display | Colour | Audio | Typical Cause |
|---|---|---|---|---|
| **Warning** | Large text, top of ECAM | Red | Continuous chime + voice | Complete system failure |
| **Caution** | Attention-getter + ECAM | Amber | Single chime | Degraded system (one pump failed) |
| **Advisory** | ECAM advisory page | Amber/cyan | No audio | Sensor failure; maintenance required |

### 7.3 Maintenance Messages (Post-Flight)

All fault codes are stored in the **Central Maintenance Computer (CMC)** with:
- Fault description
- Affected system/component
- ATA chapter and section
- Time-stamp (flight phase, UTC)
- Fault status (current, intermittent, historical)

Maintenance technicians access CMC via the **Maintenance Access Terminal (MAT)** or laptop connection to retrieve and action fault codes.

---

## 8. Case Studies

### Case Study 1 — United Airlines Flight 232 (1989)

**Event:** Uncontained engine failure severed all three hydraulic systems on a DC-10, leaving no hydraulic power for any flight control.

**Electrical failure contribution:** All three hydraulic systems passed through the same routing zone. Hydraulic fluid fire fed by severed lines. Electrical systems to the hydraulic pumps all lost simultaneously.

**Outcome:** Through exceptional airmanship (using differential thrust to control the aircraft), the crew achieved a forced landing at Sioux City, Iowa. 111 of 296 on board died.

**Lesson:** CCA requirements were updated. Post-1989, transport aircraft must demonstrate that no single external failure (including engine debris) can destroy all hydraulic systems. Modern aircraft route hydraulic systems through widely separated fuselage paths.

### Case Study 2 — Lauda Air Flight 004 (1991)

**Event:** Unexplained thrust reverser deployment in flight on a Boeing 767 led to loss of control and structural breakup.

**Electrical failure contribution:** An electrical fault allowed a thrust reverser inhibit circuit to fail in a way that permitted reverser deployment in cruise. A solenoid valve received an unintended electrical signal.

**Lesson:** Triggered FAA requirements for thrust reverser fail-safe electrical logic and redundant locking mechanisms. The solenoid valve failure path that enabled the accident was eliminated in subsequent aircraft designs.

### Case Study 3 — Boeing 737 MAX MCAS (2018/2019)

**Event:** The MCAS (Manoeuvring Characteristics Augmentation System) used a single Angle-of-Attack (AoA) sensor. When this sensor failed (stuck reading high AoA), MCAS commanded repeated nose-down stabiliser trim inputs that the crew could not overcome.

**Electrical failure contribution:** AoA sensor hardover (stuck high). No sensor voting (only single sensor used). No adequate BITE to detect sensor failure. Crew warning (stick shaker) was caused by the sensor failure itself, confusing the crew.

**Lesson:** Regulatory changes require that all safety-critical automated systems have adequate sensor redundancy and that crew-alerting for sensor failures is clear and unambiguous. This case demonstrates the catastrophic consequences of sensor hardover when redundancy is absent.

---

## 9. References

1. SAE ARP4761 — *Guidelines and Methods for Conducting the Safety Assessment Process on Civil Airborne Systems and Equipment*. SAE International, 1996.
2. SAE ARP4754A — *Guidelines for Development of Civil Aircraft and Systems*. SAE International, 2010.
3. EASA CS-25 — *Certification Specifications for Large Aeroplanes, Subpart F — Equipment*. EASA.
4. FAA AC 25.1309-1A — *System Design and Analysis*. Federal Aviation Administration.
5. NTSB Report AAR-90/06 — *United Airlines Flight 232, McDonnell Douglas DC-10-10, Sioux Gateway Airport*. NTSB, 1990.
6. Austrian Board of Accident Investigation — *Final Report on the Accident of Lauda Air Luftfahrt AG, Boeing 767-300ER*. 1993.
7. Joint Authorities Technical Review (JATR) — *Boeing 737 MAX Flight Control System — Observations, Findings, and Recommendations*. 2019.
8. Moir, I. & Seabridge, A. — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
9. Ogata, K. — *Modern Control Engineering* (5th ed.). Prentice Hall, 2010.
10. Pallett, E.H.J. — *Aircraft Electrical Systems* (3rd ed.). Longman, 1987.

---

*Previous topic: [Topic 5 — How Electrical Signals Interact with Hydraulic Valves and Actuators](./05_Electrical_Signals_and_Hydraulic_Components.md)*
*Next topic: [Topic 7 — Advantages, Limitations, and Maintenance Concerns](./07_Advantages_Limitations_Maintenance.md)*
