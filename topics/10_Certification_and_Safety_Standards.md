# Topic 10 — Certification and Safety Standards (DO-178C / ARP4754A)

> **Part of:** [Electrical Systems and Sensors in Aircraft Hydraulic Systems](../Electrical_Systems_and_Sensors_in_Aircraft_Hydraulics.md)

---

## Table of Contents

1. [Overview of the Certification Framework](#1-overview-of-the-certification-framework)
2. [Regulatory Basis — FAR Part 25 and EASA CS-25](#2-regulatory-basis--far-part-25-and-easa-cs-25)
3. [Safety Assessment Process — From FHA to FMEA](#3-safety-assessment-process--from-fha-to-fmea)
4. [Design Assurance Levels (DAL)](#4-design-assurance-levels-dal)
5. [DO-178C — Software Certification](#5-do-178c--software-certification)
6. [DO-254 — Hardware Certification](#6-do-254--hardware-certification)
7. [ARP4754A — System Development Process](#7-arp4754a--system-development-process)
8. [DO-160G — Environmental Qualification](#8-do-160g--environmental-qualification)
9. [Hydraulic System Certification Examples](#9-hydraulic-system-certification-examples)
10. [The Certification Challenge of New Technologies](#10-the-certification-challenge-of-new-technologies)
11. [References](#11-references)

---

## 1. Overview of the Certification Framework

Aircraft certification in the context of electrical and hydraulic systems involves demonstrating to the airworthiness authorities (**FAA** in the USA, **EASA** in Europe) that the integrated system is safe, reliable, and that all foreseeable failure conditions are either extremely improbable or have acceptable consequences.

### The Certification Stack

```
┌─────────────────────────────────────────────────────────────────┐
│           REGULATORY REQUIREMENTS                               │
│  FAA FAR Part 25 / EASA CS-25 (Airworthiness Standards)        │
└──────────────────────────────┬──────────────────────────────────┘
                               │ Implemented via
┌──────────────────────────────▼──────────────────────────────────┐
│           SYSTEM DEVELOPMENT STANDARDS                          │
│  SAE ARP4754A (Aircraft/System Development Process)             │
└──────────────────────────────┬──────────────────────────────────┘
                               │ Allocates to
                    ┌──────────┴──────────┐
                    ▼                     ▼
┌──────────────────────┐      ┌──────────────────────┐
│   SOFTWARE           │      │   HARDWARE           │
│ RTCA DO-178C         │      │ RTCA DO-254           │
│ (Software DAL A–E)   │      │ (Hardware DAL A–E)   │
└──────────────────────┘      └──────────────────────┘
                    │                     │
                    └──────────┬──────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│         ENVIRONMENTAL QUALIFICATION                             │
│  RTCA DO-160G (Temperature, vibration, EMI, lightning, etc.)   │
└─────────────────────────────────────────────────────────────────┘
```

Every piece of electrical hardware and software in an aircraft hydraulic system must be shown to comply with all levels of this stack.

---

## 2. Regulatory Basis — FAR Part 25 and EASA CS-25

### 2.1 Key Regulatory Paragraphs Affecting Hydraulic-Electrical Systems

#### 14 CFR §25.1309 / CS 25.1309 — Equipment, Systems, and Installations

This is the **master safety requirement** for aircraft systems:

> "The equipment, systems, and installations whose functioning is required by this subchapter must be designed to ensure that they perform their intended functions under any foreseeable operating condition."

And:

> "Each item of equipment, each system, and each installation… must be designed so that the occurrence of any failure condition that would prevent the continued safe flight and landing of the airplane is extremely improbable."

**"Extremely improbable"** is quantified as < **1×10⁻⁹ per flight hour** for catastrophic failure conditions.

#### §25.671 / CS 25.671 — Control Systems (Flight Controls)

Requires that:
- Flight control systems be designed so that failure of any single part does not prevent continued safe flight and landing
- Jammed or failed components do not prevent use of remaining controls
- There is at least one means to control pitch, roll, and yaw after any single failure

This directly drives the requirement for redundant hydraulic and electrical flight control architectures.

#### §25.735 / CS 25.735 — Brakes

Requires that:
- Brakes must be adequate to stop the aircraft within an acceptable distance
- Brakes must remain operative after hydraulic system failure (accumulator backup requirement)
- Anti-skid systems must not create a failure condition worse than without anti-skid

#### §25.729 / CS 25.729 — Landing Gear Retracting Mechanism

Requires:
- Position indication (electrically via proximity switches) for crew
- Downlock before gear retraction command accepted
- Fail-safe design of gear retraction/extension systems

#### §25.1301 / CS 25.1301 — Function and Installation

Each item of equipment must:
- Be of a kind and design appropriate for its intended function
- Be labelled as to its identification, function, or operating limitations
- Be installed in accordance with limitations specified for that equipment

This drives the need for **DO-160G environmental qualification** of all installed equipment.

### 2.2 Advisory Circulars and Acceptable Means of Compliance (AMC)

Regulations provide requirements but not methods. Advisory Circulars (FAA) and AMC/GM documents (EASA) provide guidance on **acceptable means of compliance**:

| Document | Topic |
|---|---|
| AC 25.1309-1A | System Design and Analysis (FMEA, FTA methodology) |
| AC 25.629-1B | Flutter substantiation (relevant to active flutter suppression) |
| AMC 25.1309 | Safety analysis methods |
| AMC-20-115D | DO-178C approval as AMC for airborne software |
| AMC-20-152A | DO-254 approval as AMC for complex hardware |

---

## 3. Safety Assessment Process — From FHA to FMEA

### 3.1 Functional Hazard Assessment (FHA)

The **FHA** is conducted at the **aircraft and system levels** to identify all functions, the failure conditions that could result from loss of those functions, and their severity classification.

**Example FHA entry for hydraulic flight controls:**

| Function | Failure Condition | Phase | Severity | Probability Req. |
|---|---|---|---|---|
| Pitch control | Loss of all pitch control | All | Catastrophic | < 10⁻⁹/FH |
| Pitch control | Loss of pitch control on one channel | All | Hazardous | < 10⁻⁷/FH |
| Aileron control | Loss of all aileron control | All | Catastrophic | < 10⁻⁹/FH |
| Hydraulic pressure indication | Loss of hydraulic pressure indication | All | Major | < 10⁻⁵/FH |
| Hydraulic level indication | Loss of hydraulic level indication | All | Minor | < 10⁻³/FH |

### 3.2 Preliminary System Safety Assessment (PSSA)

The PSSA allocates safety requirements from the FHA to specific subsystems and components. It uses **FTA (Fault Tree Analysis)** to show how the required system-level probability is met by the individual component reliabilities.

**Example PSSA FTA — Loss of All Pitch Control:**

```
LOSS OF ALL PITCH CONTROL (< 10⁻⁹/FH)
              │
         [AND gate]
        /     |     \
       /      |      \
Loss of    Loss of  Loss of
System 1   System 2  System 3
pitch ctrl  pitch ctrl  pitch ctrl
(< 10⁻³)   (< 10⁻³)   (< 10⁻³)

Combined probability: (10⁻³)³ = 10⁻⁹ ✓
```

The PSSA shows that if each individual system has a pitch control failure probability of 10⁻³, three independent systems give 10⁻⁹ overall — meeting the requirement.

### 3.3 System Safety Assessment (SSA)

The **SSA** is the final, completed safety analysis that **proves** (with supporting data) that the actual system as built meets the safety requirements identified in the FHA.

The SSA includes:
- Completed FMEA for every component
- Completed FTAs for all hazardous and catastrophic failure conditions
- Common Cause Analysis (Zonal, Particular Risk, Common Mode)
- Evidence that failure rates used in the analysis are validated by component test data

### 3.4 FMEA in Practice — Solenoid Valve

| Item | Failure Mode | Failure Rate (per FH) | Local Effect | System Effect | Aircraft Effect | Detection | Severity |
|---|---|---|---|---|---|---|---|
| Gear up solenoid valve | Coil open circuit | 3×10⁻⁶ | Valve remains de-energised | Gear cannot be commanded up | Gear remains down — fly with gear extended | LGCU BITE; EICAS advisory | Minor |
| Gear up solenoid valve | Spool stuck open | 1×10⁻⁶ | Valve permanently in gear-up position | Gear retracts; cannot be extended electrically | Must use alternate (gravity) extension | LGCU BITE; EICAS advisory | Major |
| Gear up solenoid valve | External leak | 5×10⁻⁶ | Hydraulic fluid loss | Hydraulic system level dropping | Low-level warning → potential system failure | Level sensor; ECAM | Hazardous (if undetected) |

---

## 4. Design Assurance Levels (DAL)

### 4.1 Definition

The **Design Assurance Level (DAL)** — also called **Development Assurance Level** in ARP4754A — indicates the **rigour of development** required for software and hardware, based on the severity of the failure condition they contribute to.

### 4.2 DAL Table

| DAL | Failure Condition | Probability | Examples |
|---|---|---|---|
| **A** | Catastrophic | < 10⁻⁹/FH | Flight control FCC; landing gear LGCU critical functions |
| **B** | Hazardous | < 10⁻⁷/FH | Single hydraulic system pressure indication; hydraulic pump control |
| **C** | Major | < 10⁻⁵/FH | Hydraulic temperature indication; level indication |
| **D** | Minor | < 10⁻³/FH | Hydraulic filter differential pressure indication |
| **E** | No Safety Effect | No requirement | Hydraulic system diagnostic logging |

### 4.3 DAL Assignment Process

DAL is assigned through the safety assessment process:

1. FHA identifies failure condition severity for each function
2. PSSA allocates DAL requirements to software/hardware items
3. Development follows DAL-specific processes (see DO-178C / DO-254)
4. SSA verifies final system meets requirements

### 4.4 Implications of DAL A

For DAL A software (e.g., flight control computer for primary flight controls):
- Every software requirement must be documented, reviewed, and formally tested
- Every test case must be reviewed for completeness
- Source code must be reviewed line by line by an independent party
- **Modified Condition/Decision Coverage (MC/DC)** testing required — every decision in the code must be shown to individually affect the outcome
- **Cost:** ~\$1,000–\$5,000 per line of code for certification alone
- **Typical flight control computer:** 100,000–500,000 lines of code

---

## 5. DO-178C — Software Certification

### 5.1 Background

**RTCA DO-178C** ("Software Considerations in Airborne Systems and Equipment Certification") is the primary standard for certifying safety-critical aircraft software. It was first issued in 1982 (DO-178), revised in 1992 (DO-178B), and most recently revised in 2011 (DO-178C).

### 5.2 Development Process Requirements by Level

| Activity | DAL A | DAL B | DAL C | DAL D | DAL E |
|---|---|---|---|---|---|
| Software requirements | Formal | Formal | Formal | Informal | None |
| Software design | Formal | Formal | Formal | Informal | None |
| Coding standards | Required | Required | Required | Required | None |
| Test coverage (structural) | MC/DC | Decision | Statement | None | None |
| Independent review | Required | Required | Required | Not required | None |
| Traceability (req → code → test) | Bidirectional | Bidirectional | Bidirectional | None | None |

### 5.3 Key Concepts in DO-178C

#### Requirements Traceability

Every software requirement must be traceable to:
- A system requirement (showing the software requirement is necessary)
- One or more test cases (showing the requirement is verified)
- Source code sections (showing the requirement is implemented)

Any change to the code triggers re-analysis of all affected requirements and re-execution of associated tests.

#### Modified Condition/Decision Coverage (MC/DC)

For DAL A, every boolean condition in every decision must be independently shown to affect the decision outcome. For example:

```python
if (pressure > 2200) and (pump_running):
    # condition must be tested with:
    # (True, True) → True
    # (False, True) → False (pressure condition independently affects result)
    # (True, False) → False (pump_running independently affects result)
```

This ensures that no dead code (code that can never execute) exists in safety-critical paths.

#### Independence

All review activities (design review, code review, test review) must be performed by a person or organisation that did not perform the original work. This "second set of eyes" prevents blind spots.

### 5.4 DO-178C in Hydraulic System Context

**Affected software in hydraulic systems:**

| Software | Aircraft | DAL |
|---|---|---|
| LGCU landing gear sequence software | All transport aircraft | A |
| ELAC/SEC/FAC flight control laws | Airbus A320 family | A |
| BSCU anti-skid algorithm | Boeing 737/777 | A |
| HSMU pump management logic | Boeing 777 | B |
| ECAM/EICAS hydraulic display software | All | C |
| Hydraulic filter status display | All | D |

---

## 6. DO-254 — Hardware Certification

### 6.1 Background

**RTCA DO-254** ("Design Assurance Guidance for Airborne Electronic Hardware") covers **complex electronic hardware** such as FPGAs, ASICs, and complex PLDs used in flight control computers, LGCU ECUs, and HSMU units.

### 6.2 When DO-254 Applies

DO-254 applies to hardware where:
- The design is too complex for traditional analysis methods
- An FPGA or ASIC is used (programmable or custom logic)
- The hardware function contributes to a safety function

**Examples in hydraulic systems:**
- FPGA implementing LVDT signal demodulation in a flight control computer
- ASIC in the BSCU performing high-speed anti-skid control loops
- Custom gate array in the LGCU managing gear sequencing logic

### 6.3 Process Requirements

Similar to DO-178C, DO-254 requires:
- Requirements → architecture → design → implementation traceability
- Functional verification testing at component and integration level
- Structural coverage analysis for complex logic
- Independence in review activities

---

## 7. ARP4754A — System Development Process

### 7.1 What ARP4754A Covers

**SAE ARP4754A** ("Guidelines for Development of Civil Aircraft and Systems") provides the framework for **system-level development**, from initial requirements through to certified system. It sits above DO-178C and DO-254 in the hierarchy.

### 7.2 Key Concepts

#### System Development Assurance Level (SDAL)

Analogous to DAL, the SDAL is assigned to systems based on FHA severity classification. It drives the rigour of the system development process (not just the software or hardware).

#### Verification and Validation

ARP4754A distinguishes:
- **Verification:** "Was the system built correctly?" (Formal test against specification)
- **Validation:** "Was the correct system built?" (Was the specification correct to begin with?)

Both are required for safety-critical hydraulic systems. The 737 MAX MCAS failure demonstrated what happens when validation is inadequate — the system met its requirements, but the requirements were wrong.

#### Integration Assurance

ARP4754A addresses the **integration of software, hardware, and mechanical systems** — recognising that the overall system safety depends on how components interact, not just how each component performs individually.

### 7.3 The V-Model Development Cycle

```
                          System Requirements (FHA/PSSA)
                        /                              \
              System Design                          System Verification
             /                                               \
    Software/Hardware Requirements                  S/W & H/W Integration Test
    /                                                              \
Software Design                                        Software Integration Test
/                                                                          \
Coding ──────────────────────────────────────────────────────── Unit Test
```

Each step down the left side creates increasingly detailed design. Each step up the right side performs increasingly comprehensive testing, ultimately demonstrating that the system meets the original system requirements.

---

## 8. DO-160G — Environmental Qualification

### 8.1 Purpose

**RTCA DO-160G** ("Environmental Conditions and Test Procedures for Airborne Equipment") defines the standard tests that all aircraft electronic and electrical equipment must pass to demonstrate suitability for the aircraft installation environment.

### 8.2 Relevant Test Categories for Hydraulic System Components

| Section | Test | Relevance to Hydraulic Systems |
|---|---|---|
| 4 | Temperature and Altitude | ECU, sensors: −55°C to +70°C (or +85°C) at altitude |
| 5 | Temperature Variation | Thermal cycling 100 cycles; simulates repeated ground-to-cruise transitions |
| 7 | Humidity | 95% RH for 6 days; connector seals and insulation |
| 8 | Shock | 6g to 40g half-sine; simulates landing impact, runway roughness |
| 9 | Vibration | Random and sinusoidal; 6g continuous (wheel well category) |
| 16 | Power Input | Voltage/frequency variations; power interruptions; surge |
| 17 | Voltage Spike | ±600V spike on power lines; simulates switching transients |
| 18 | Audio Frequency Conducted Susceptibility | Induced EMI on power lines |
| 19 | Induced Signal Susceptibility | Magnetic and electric field induction into signal cables |
| 20 | Radio Frequency Susceptibility (HIRF) | High-intensity RF fields up to 200 V/m |
| 21 | Emission of RF Energy | Equipment must not emit excessive RF |
| 22 | Lightning Direct Effects | Physical mounting of hydraulic lines near metallic structure |
| 22 | Lightning Induced Transients | Surge/spike on wiring from nearby lightning strike |
| 24 | Icing | Ice formation on external sensors (e.g., wheel well position sensors) |
| 26 | Fluid Susceptibility | Exposure to hydraulic fluid, fuel, lubricating oil, cleaning agents |
| 27 | Actions of Fluids | Hydraulic fluid on seals, connectors, PCBs |

### 8.3 Equipment Category Selection

DO-160G categorises equipment by installation zone. The zone determines which test levels apply:

| Category | Location | Vibration Level | Temperature |
|---|---|---|---|
| A | Pressurised cabin | Low | Moderate |
| B | Unpressurised fuselage | Medium | Wide range |
| B1 | Wheel well | High | Extreme |
| C | Engine zone 1/2 | Very high | Very extreme |

A proximity sensor in the landing gear bay must meet **Category B1** vibration and temperature requirements — among the most stringent on the aircraft.

---

## 9. Hydraulic System Certification Examples

### 9.1 Certifying a New Anti-Skid Control Algorithm

The BSCU anti-skid algorithm is DAL A software controlling the hydraulic brake pressure modulation.

**Certification activities required:**

1. **System requirements:** Define required stopping performance; maximum brake pressure; maximum wheel slip before anti-skid intervention.

2. **Software requirements:** Define algorithm inputs (wheel speed, aircraft speed, brake pressure command); outputs (modulated pressure command); timing requirements (<10 ms response); limit values.

3. **Design:** Implement anti-skid PID control loop in Ada or C; document control law derivation.

4. **Code review:** Independent review of every line; MISRA-C compliance for coding standards.

5. **Unit testing:** Test each function in isolation with boundary conditions, nominal values, and adversarial inputs.

6. **Integration testing:** Test complete BSCU with simulated wheel speed sensors and pressure sensors.

7. **MC/DC verification:** Demonstrate every condition in every decision is independently tested.

8. **Hardware-in-the-loop (HIL) testing:** BSCU hardware connected to a real-time aircraft dynamics simulator; execute thousands of landing scenarios.

9. **Aircraft ground tests:** Actual rejected take-off tests on aircraft demonstrating required stopping distance.

10. **Aircraft flight tests:** Demonstrate system performance across flight envelope (various speeds, weights, runway conditions).

11. **DER/ODA approval:** Designated Engineering Representative (FAA) or Organisation Designation Authorisation reviews and approves all evidence.

### 9.2 Certifying a New Hydraulic Pressure Transducer

Even a single pressure transducer — if it has a safety function — requires:

1. **FMEA:** Identify all failure modes (stuck high, stuck low, drift, null) and their effects on the aircraft.

2. **DO-160G qualification:** Test the sensor at worst-case temperature, vibration, HIRF, and fluid susceptibility levels for its installation zone.

3. **Accuracy verification:** Test sensor accuracy across full temperature range, pressure range, and after DO-160G conditioning.

4. **Long-term stability:** Accelerated ageing tests to demonstrate accuracy maintained over 50,000 operating hours.

5. **Installation test:** After installation on aircraft, perform functional test confirming sensor reads correctly at known hydraulic pressures.

---

## 10. The Certification Challenge of New Technologies

### 10.1 EHA Certification

Electro-Hydrostatic Actuators present unique certification challenges:

- **Software:** Motor controller software is DAL A or B — must be DO-178C certified
- **Hardware:** Power electronics (FPGAs in motor controller) must be DO-254 certified
- **Novel failure modes:** Motor demagnetisation; controller failure modes not present in conventional EHSVs — require new FMEA categories
- **Integration:** EHA interacts with aircraft electrical bus (high-power draw affects bus stability) — must be assessed in system safety analysis

### 10.2 Machine Learning in Hydraulic Health Monitoring

Emerging use of **machine learning / artificial intelligence (AI)** for predictive maintenance of hydraulic systems creates certification challenges:

- Traditional DO-178C was designed for deterministic software — if X then Y
- ML models are non-deterministic — their behaviour cannot be fully described by requirements in the traditional sense
- EASA is developing **EASA AI Roadmap** and **proposed regulation on AI in aviation** (2023)
- Interim approach: AI used only for advisory/monitoring functions (DAL D or E), not for safety-critical control

### 10.3 Cyber Security

As hydraulic control systems become more connected (ACARS data transmission of hydraulic health data; airline data analytics), **cyber security** becomes a certification concern:

- **DO-326A** ("Airworthiness Security Process Specification") — new standard addressing cyber security in aircraft systems
- Hydraulic system health data transmitted via ACARS must be protected from tampering (an attacker falsely reporting "system healthy" when it is not)
- Security requirements now integrated into the safety assessment process

---

## 11. References

1. SAE ARP4761 — *Guidelines and Methods for Conducting the Safety Assessment Process on Civil Airborne Systems and Equipment*. SAE International, 1996.
2. SAE ARP4754A — *Guidelines for Development of Civil Aircraft and Systems*. SAE International, 2010. ISBN 978-0-7680-7540-6.
3. RTCA DO-178C — *Software Considerations in Airborne Systems and Equipment Certification*. RTCA, 2011.
4. RTCA DO-254 — *Design Assurance Guidance for Airborne Electronic Hardware*. RTCA, 2000.
5. RTCA DO-160G — *Environmental Conditions and Test Procedures for Airborne Equipment*. RTCA, 2010.
6. FAA Advisory Circular AC 25.1309-1A — *System Design and Analysis*. FAA, 1988.
7. EASA CS-25 — *Certification Specifications for Large Aeroplanes*. EASA, 2018 (with amendments).
8. FAA Order 8110.49A — *Software Approval Guidelines*. FAA, 2011.
9. Leanna Rierson — *Developing Safety-Critical Software: A Practical Guide for Aviation Software and DO-178C Compliance*. CRC Press, 2013.
10. EASA AI Roadmap 2.0 — *A Human-Centric Approach to AI in Aviation*. EASA, 2023.
11. RTCA DO-326A — *Airworthiness Security Process Specification*. RTCA, 2014.
12. Mahdinejad, S. — "Certification Challenges for Electro-Hydrostatic Actuators in Civil Aviation." *SAE Technical Paper 2019-01-1855*. SAE International, 2019.

---

*Previous topic: [Topic 9 — Hydraulic System Redundancy and Electrical Backup](./09_Hydraulic_Redundancy_and_Electrical_Backup.md)*
*Next topic: [Topic 11 — Summary and Critical Analysis](./11_Summary_and_Critical_Analysis.md)*
