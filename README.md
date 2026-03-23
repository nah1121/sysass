# Electrical Systems and Sensors in Aircraft Hydraulic Systems

## Group Presentation — Detailed Reference Document

---

## Table of Contents

Each topic below has a dedicated detailed document in the [`topics/`](./topics/) folder.

1. [Introduction](./topics/01_Introduction.md) — [In-page summary](#1-introduction)
2. [The Role of Electrical Systems in Hydraulic Systems](./topics/02_Role_of_Electrical_Systems.md) — [In-page summary](#2-the-role-of-electrical-systems-in-hydraulic-systems)
3. [Types of Electrical Components Used in Hydraulic Systems](./topics/03_Electrical_Components.md) — [In-page summary](#3-types-of-electrical-components-used-in-hydraulic-systems)
4. [Types of Sensors Used in Aircraft Hydraulic Systems](./topics/04_Sensors.md) — [In-page summary](#4-types-of-sensors-used-in-aircraft-hydraulic-systems)
5. [How Electrical Signals Interact with Hydraulic Valves and Actuators](./topics/05_Electrical_Signals_and_Hydraulic_Components.md) — [In-page summary](#5-how-electrical-signals-interact-with-hydraulic-valves-and-actuators)
6. [Failure of Electrical Components and Sensors — Causes and Consequences](./topics/06_Failure_Modes_and_Causes.md) — [In-page summary](#6-failure-of-electrical-components-and-sensors--causes-and-consequences)
7. [Advantages, Limitations, and Maintenance Concerns](./topics/07_Advantages_Limitations_Maintenance.md) — [In-page summary](#7-advantages-limitations-and-maintenance-concerns)
8. [Fly-By-Wire and Electro-Hydrostatic Actuators (EHA)](./topics/08_Fly_By_Wire_and_EHA.md) — [In-page summary](#8-fly-by-wire-and-electro-hydrostatic-actuators-eha)
9. [Hydraulic System Redundancy and Electrical Backup](./topics/09_Hydraulic_Redundancy_and_Electrical_Backup.md) — [In-page summary](#9-hydraulic-system-redundancy-and-electrical-backup)
10. [Certification and Safety Standards (DO-178C / ARP4754A)](./topics/10_Certification_and_Safety_Standards.md) — [In-page summary](#10-certification-and-safety-standards-do-178c--arp4754a)
11. [Summary and Critical Analysis](./topics/11_Summary_and_Critical_Analysis.md) — [In-page summary](#11-summary-and-critical-analysis)
12. [References and Further Reading](./topics/12_References.md) — [In-page summary](#12-references-and-further-reading)

---

## 1. Introduction

Modern aircraft are extraordinarily complex machines that rely on the seamless integration of multiple engineering disciplines. Among the most critical of these is the marriage of **hydraulic power** and **electrical control**. Neither system operates efficiently in isolation; hydraulic systems provide the enormous mechanical force needed to move flight control surfaces, raise and lower landing gear, operate brakes, and drive thrust reversers, while electrical systems provide the intelligence, precision, and automation that make those hydraulic operations safe, repeatable, and reliable.

### Why This Integration Matters

- A commercial aircraft such as the **Boeing 747** uses three independent hydraulic systems, each operating at **3,000 psi (pounds per square inch)**, providing power to over 100 individual actuators.
- The **Airbus A380** features four hydraulic circuits, two operating at 3,000 psi and two at the newer **5,000 psi** standard, reducing fluid volume and pipe diameter while maintaining force output.
- Without electrical sensors and controls, a pilot would need to directly and manually command every hydraulic valve, which is physically impossible in a modern multi-axis control environment.

### Historical Context

Early aircraft (1930s–1950s) used simple hydraulic circuits with manual pump controls and basic pressure relief valves. As aircraft grew larger and faster, human muscle power alone could not move control surfaces against aerodynamic loads. Electrical servo mechanisms were introduced to translate small pilot inputs (stick or rudder pedal deflection) into large hydraulic valve movements. The development of **transistors** (1950s), **integrated circuits** (1960s), and **microprocessors** (1970s) enabled the sophisticated electronic flight control systems that are now standard.

---

## 2. The Role of Electrical Systems in Hydraulic Systems

### 2.1 Why Electrical and Hydraulic Systems Are Used Together

Hydraulic systems excel at transmitting large forces through small-diameter pipes. However, they are essentially "dumb" — a hydraulic actuator will simply extend or retract based on which port has higher pressure. To give hydraulic systems intelligence — to make them respond correctly in all flight phases, to monitor their health, and to reconfigure automatically when a component fails — electrical systems are required.

The combination provides:

| Capability | Hydraulics Alone | With Electrical Integration |
|---|---|---|
| Force output | Very high | Very high |
| Precision control | Poor (manual valves) | Excellent (servo valves) |
| Response time | Slow (manual) | Milliseconds (electronic) |
| Condition monitoring | None | Continuous |
| Automatic reconfiguration | None | Automatic |
| Remote operation | Limited | Full remote capability |

### 2.2 Control

Electrical control of hydraulic systems operates through a chain:

```
Pilot Input / Flight Computer
        ↓
Electrical Signal (voltage/current)
        ↓
Solenoid Valve / Servo Valve
        ↓
Hydraulic Flow Direction and Volume
        ↓
Actuator Movement (control surface, landing gear, brakes)
```

**Example — Landing Gear Retraction on a Boeing 737:**
When the pilot moves the landing gear lever to "UP," an electrical signal energises a solenoid valve in the hydraulic manifold. This shifts the valve spool, directing hydraulic pressure to the retraction side of each gear actuator. Electrical limit switches confirm the gear doors are open before the actuator extends; position sensors confirm full retraction before the doors close. The entire sequence is automated by a dedicated electronic control unit (ECU).

### 2.3 Monitoring

Electrical sensors continuously monitor:

- **System pressure** — to confirm pumps are delivering adequate flow.
- **Fluid temperature** — to detect overheating caused by excessive cycling or pump cavitation.
- **Fluid level** — to detect leaks before they become catastrophic.
- **Actuator position** — to give the flight computer feedback on the actual position of control surfaces.
- **Pump speed/torque** — on electric motor-driven pumps (EMPs), to detect motor degradation.

Monitoring data is fed into the **Central Maintenance Computer (CMC)** or **Aircraft Condition Monitoring System (ACMS)**, which logs faults, generates maintenance messages, and on some aircraft alerts the crew via the **Electronic Centralised Aircraft Monitor (ECAM)** (Airbus) or **Engine Indication and Crew Alerting System (EICAS)** (Boeing).

### 2.4 Automation

Automation eliminates the need for continuous pilot input to maintain safe hydraulic system operation:

- **Automatic pump switching:** If hydraulic system pressure drops below a threshold (e.g., 2,200 psi on a Boeing 777), the Air Driven Generator (ADG) or Electric Pump automatically starts without crew action.
- **Priority valve logic:** In low-pressure situations, electrical logic sheds non-essential hydraulic consumers (e.g., cargo door actuation) to maintain pressure for essential services (e.g., flight controls, brakes).
- **Thermal shutoff:** If fluid temperature exceeds safe limits, the pump is electrically commanded to offload or shut down, preventing fluid degradation and potential fire.
- **Built-In Test Equipment (BITE):** Microprocessors within hydraulic control units execute self-tests at power-up and continuously during flight, detecting open-circuit sensors, failed solenoids, and valve failures.

---

## 3. Types of Electrical Components Used in Hydraulic Systems

### 3.1 Switches

#### 3.1.1 Pressure Switches

A **pressure switch** is a simple electromechanical device that changes its electrical state (open or closed contact) when the hydraulic fluid pressure reaches a predetermined level.

**Construction:**
- A diaphragm or piston exposed to hydraulic pressure.
- A spring pre-loaded to the set-point pressure.
- An electrical contact that opens or closes when the spring is overcome.

**Operation:**
- **Normally Open (NO):** Contact closes when pressure rises above the set point. Used to signal "pressure available."
- **Normally Closed (NC):** Contact opens when pressure rises above set point. Used to signal "overpressure."

**Aircraft Examples:**
- **Boeing 737 Hydraulic System A & B** use pressure switches set at approximately 2,700 psi to illuminate the hydraulic pressure warning light on the overhead panel when pressure falls below this value.
- **Landing gear weight-on-wheels (WOW) switches** use a combination of air/hydraulic pressure and mechanical switching to indicate whether the aircraft is on the ground or airborne, gating many systems such as ground spoiler arming.

**Diagram — Pressure Switch Schematic:**

```
Hydraulic Port
      |
   [Piston/Diaphragm]
      |
   [Spring (set point)]
      |
   [Electrical Contact]
      |
   To Control Circuit
```

#### 3.1.2 Limit Switches

A **limit switch** (also called a proximity switch or position switch) detects when a mechanical component has reached a specific physical position.

**Types used in hydraulics:**
- **Mechanical limit switch:** A physical arm or plunger is depressed by the moving component.
- **Inductive proximity switch:** Senses the presence of metal without physical contact (more reliable, no moving parts).
- **Reed switch:** Uses a magnetic field from a magnet mounted on the moving part.

**Aircraft Applications:**
- **Landing gear uplock/downlock detection:** Switches confirm the gear is fully retracted and locked (uplock) or fully extended and locked (downlock). Both states must be confirmed before the gear doors can be closed or opened respectively.
- **Cargo door latching:** Limit switches confirm all latch bolts have engaged before the pressurisation system allows the aircraft to climb.
- **Thrust reverser deployment:** Limit switches confirm the reverser sleeves are fully stowed before the engine can be advanced to high thrust.

#### 3.1.3 Flow Switches

A **flow switch** detects the presence or rate of hydraulic fluid flow. It typically uses a paddle or flapper inside the hydraulic line that deflects when fluid flows, closing an electrical contact.

- Used to detect whether a hydraulic pump is actually delivering flow (even if pressure appears correct).
- Helps distinguish between a pump running dry (low pressure, no flow) and a stuck relief valve (low pressure, flow present).

### 3.2 Relays

A **relay** is an electrically operated switch: a small electrical current through a coil creates an electromagnetic field that opens or closes one or more sets of heavier-duty contacts.

**Why relays are used in hydraulic control circuits:**
- A cockpit switch or flight computer output carries only milliamps of current, which is insufficient to directly energise a large solenoid valve coil.
- A relay bridges the gap: the small signal closes the relay, which then connects the solenoid to the aircraft bus (28 VDC or 115 VAC).

**Types used in aircraft:**
- **General-purpose relay:** Single coil, one set of contacts.
- **Latching relay:** Stays in its last commanded state even when power is removed (used for landing gear position indication to retain status during brief power interruptions).
- **Solid-state relay (SSR):** Uses transistors instead of mechanical contacts; faster switching, no arc, longer life.

**Example — Hydraulic Pump Relay:**
```
Flight Deck Switch (5mA) → Relay Coil → Relay Contacts Close
                                               ↓
                                      115VAC Bus → Pump Motor
```

### 3.3 Solenoids and Solenoid Valves

A **solenoid** is an electromechanical device that converts electrical energy into linear mechanical motion. In hydraulic systems, solenoids are almost always built into **solenoid-operated directional control valves (DCVs)**.

**Construction:**
- A coil of wire wound around a ferromagnetic core.
- When current flows through the coil, the core (plunger) is pulled into the coil.
- The plunger is mechanically linked to a valve spool.

**Types:**
- **On/Off solenoid valve:** Simple two-position valve (fully open or fully closed). Used for landing gear, flaps, brakes.
- **Proportional solenoid valve:** The valve spool position is proportional to the current through the coil. Allows intermediate flow rates. Used in modern fly-by-wire flight controls.
- **Servo valve (electrohydraulic servo valve — EHSV):** A very high-precision proportional valve controlled by milliamp-level signals from a flight computer. The gold standard for flight control actuation.

**Servo Valve Detail:**

An EHSV typically uses a **torque motor** (a precision electromagnetic actuator) to deflect a flapper between two nozzles. The differential pressure between the nozzles shifts a spool valve. The spool valve then ports pressure to the actuator. A feedback wire (LVDT — see Section 4.3) measures spool position and feeds back to the torque motor to achieve precise position control.

```
Electrical Signal (milliamps)
         ↓
    Torque Motor
         ↓
 Flapper deflects between nozzles
         ↓
 Differential nozzle pressure shifts spool
         ↓
 Hydraulic flow to actuator
         ↓
 LVDT feedback → Flight Computer → Corrects command
```

**Aircraft Example — Airbus A320 Flight Controls:**
Each control surface actuator (aileron, elevator, rudder) is driven by an EHSV. The **Flight Control Computers (FCCs)** send ±10mA signals to the servo valves. The entire system operates at 3,000 psi. Servo valve response time is less than 10 milliseconds, enabling the FCC to make thousands of corrections per second for turbulence, trim, and pilot inputs.

---

## 4. Types of Sensors Used in Aircraft Hydraulic Systems

### 4.1 Pressure Sensors (Transducers)

A **pressure transducer** (or pressure sensor) converts hydraulic fluid pressure into a proportional electrical signal, unlike a simple pressure switch which only provides on/off output.

**Operating Principles:**

| Type | Principle | Output |
|---|---|---|
| Strain gauge | Pressure deforms a diaphragm; strain gauges measure micro-strain | 0–5V or 4–20mA analog |
| Piezoelectric | Pressure on a crystal generates charge | Charge/voltage output |
| Capacitive | Pressure changes gap between plates, altering capacitance | Digital or analog |
| MEMS (Micro-ElectroMechanical Systems) | Miniaturised silicon diaphragm | Digital SPI/I²C bus |

**What it measures and why it matters:**
- **System pressure** (e.g., 0–5,000 psi): Confirms the pump is running and delivering pressure. Low pressure = pump failure or major leak.
- **Differential pressure across a filter:** A rising differential pressure indicates the filter is becoming clogged. When a threshold is reached, the filter bypass valve opens and maintenance is triggered.
- **Actuator load pressure:** By comparing extend-side and retract-side pressures, the flight computer can calculate the aerodynamic load on a control surface — useful for flutter suppression and load alleviation.

**Aircraft Example — Boeing 777 Hydraulic System:**
Three independent hydraulic systems (Left, Centre, Right) each have multiple pressure transducers. Outputs are read by the **Airplane Information Management System (AIMS)**, which displays pressures on the **Systems Display page** and logs any deviations for maintenance review. Pressure data is also used by the **Hydraulic System Monitoring Unit (HSMU)** to schedule pump cycling.

### 4.2 Temperature Sensors

**Types used:**

| Type | Construction | Accuracy | Range |
|---|---|---|---|
| Thermocouple (Type K/J) | Two dissimilar metals; Seebeck effect | ±2°C | −200 to +1200°C |
| RTD (Resistance Temperature Detector) | Platinum wire; resistance increases with temperature | ±0.1°C | −200 to +850°C |
| Thermistor | Semiconductor; NTC (resistance decreases with temperature) | ±0.5°C | −40 to +150°C |

**Hydraulic fluid temperature ranges:**
- Normal operation: **−40°C to +100°C** (Skydrol hydraulic fluid)
- Warning threshold: **>120°C** — fluid begins to degrade; vapour bubbles form
- Shutdown threshold: **>135°C** — pump unloads or shuts down

**Why temperature monitoring is critical:**
1. **Fluid degradation:** Hydraulic fluid (Skydrol LD-4 / Skydrol 500B-4 in most transport aircraft) loses its viscosity and lubrication properties at high temperatures, accelerating pump wear.
2. **Vapour lock:** At high temperatures, dissolved gases come out of solution, causing air bubbles that create spongy or unresponsive actuators.
3. **Seal deterioration:** Elevated temperatures cause o-ring and seal swelling, cracking, or extrusion, leading to external leaks.
4. **Fire risk:** If hydraulic fluid contacts a hot surface (engine nacelle) above its **auto-ignition temperature** (~200°C for Skydrol), a hydraulic fire can occur.

**Aircraft Example — Airbus A380:**
The A380's hydraulic system includes temperature sensors in each reservoir. If fluid temperature exceeds the warning threshold, the ECAM displays a "HYD [SYS] OVHT" warning, prompting the crew to reduce hydraulic load (e.g., extend flaps only one stage, avoid simultaneous actuation of multiple large actuators).

### 4.3 Position Sensors

Position sensors measure the mechanical displacement of hydraulic actuator rods, valve spools, or control surfaces, providing the flight computer with real-time feedback.

#### 4.3.1 LVDT — Linear Variable Differential Transformer

The **LVDT** is the most widely used position sensor in aircraft hydraulic systems. It is robust, contactless, and extremely accurate.

**How it works:**
```
Primary Coil (AC excitation)
        |
   Ferromagnetic Core (attached to moving rod)
        |
   Secondary Coil A — Secondary Coil B
```
- An AC voltage (typically 3–10 kHz) is applied to the primary coil.
- The ferromagnetic core moves with the actuator rod.
- When the core is centred, the induced voltages in Secondary A and Secondary B are equal and the output is zero.
- When the core moves, the output voltage magnitude indicates displacement and the phase indicates direction.
- Resolution: typically 0.01 mm or better.

**Aircraft Applications:**
- **Servo valve LVDT:** Measures spool position, providing feedback to the flight computer for closed-loop control.
- **Actuator LVDT:** Measures the extension of the actuator rod, enabling the flight computer to know the exact angle of a flight control surface.
- **Rudder travel limiter:** An LVDT measures airspeed-compensated rudder travel, and a hydraulic actuator physically limits rudder deflection at high speeds to prevent structural overload.

#### 4.3.2 RVDT — Rotary Variable Differential Transformer

Similar to the LVDT but measures angular (rotational) displacement. Used for:
- **Flap position feedback** (flap track mechanism rotary output)
- **Control column deflection measurement**
- **Aileron angular position**

#### 4.3.3 Potentiometer-based Position Sensors

A **linear potentiometer** connected to the actuator rod provides a variable resistance proportional to stroke. Simpler and cheaper than an LVDT but has mechanical wear (sliding contact).

- Used for non-critical applications such as **cargo door position** or **fuel boost pump lever position**.

#### 4.3.4 Hall Effect Sensors

A **Hall effect sensor** detects the presence of a magnetic field. When used with a permanent magnet mounted on the moving component:
- Provides a digital signal (ON/OFF) when the component passes through the sensor's field.
- More resistant to vibration and contamination than mechanical limit switches.
- Used for **landing gear proximity detection** on newer aircraft (replacing mechanical microswitches).

### 4.4 Fluid Level Sensors

Hydraulic fluid level monitoring detects reservoir depletion — the primary early indicator of a hydraulic leak.

**Types:**

| Type | Principle | Output |
|---|---|---|
| Float switch | A float rises/falls with fluid level, operating a switch | ON/OFF |
| Capacitive level sensor | Fluid dielectric changes measured capacitance | Analog (continuous) |
| Ultrasonic level sensor | Sound pulse time-of-flight to fluid surface | Analog/digital |
| Magnetic level indicator | Magnets on float align with hall-effect sensors on tube | Multiple-level discrete |

**Why fluid level monitoring is critical:**
- A reservoir running dry allows the pump to ingest air, causing **cavitation** — violent implosion of vapour bubbles that destroys pump vanes and pistons within seconds.
- A hydraulic fluid loss of as little as **1 litre in 5 minutes** should prompt crew action because it suggests an active leak that could lead to complete system loss.
- Many aircraft implement a **low-level warning** (e.g., reservoir at 50% capacity) and a **critically low warning** (e.g., reservoir at 25% capacity), allowing flight crews to isolate the affected system before it runs dry.

**Aircraft Example — Boeing 737NG:**
The hydraulic reservoir is equipped with a float-type level transmitter that provides a continuous signal to the **Hydraulic System Indication** section of the overhead panel. A red LOW LEVEL light illuminates when the fluid falls below a safe minimum. Pilots are trained to cross-isolate systems and increase pump pressure checks as a response.

### 4.5 Flow Rate Sensors

Beyond the simple flow switch (Section 3.1.3), more sophisticated **flow transducers** measure the volumetric flow rate (litres/minute or gallons/minute).

- **Turbine flow meter:** A small turbine in the flow path spins at a rate proportional to flow. Pulses from the turbine are counted electronically.
- **Coriolis flow meter:** Measures the Coriolis effect on oscillating tubes; very accurate but complex.

**Aircraft Use:**
- Detecting pump output degradation (a pump delivering less flow than specified is likely worn).
- Identifying which consumer is drawing excessive flow (useful for fault isolation during maintenance).

### 4.6 Vibration and Acoustic Sensors

Some advanced hydraulic systems include **accelerometers** or **acoustic emission sensors** mounted on pumps and actuators to detect:
- **Bearing wear** (changes in vibration frequency spectrum)
- **Cavitation** (characteristic high-frequency noise)
- **Spool valve stiction** (irregular valve movement)

This data feeds into **Health and Usage Monitoring Systems (HUMS)**, enabling **Predictive Maintenance** — replacing components before they fail rather than on fixed calendar intervals.

---

## 5. How Electrical Signals Interact with Hydraulic Valves and Actuators

### 5.1 The Closed-Loop Control Concept

In open-loop control, a signal commands an actuator to move but there is no feedback to confirm the actual position. In **closed-loop (feedback) control**, a position sensor continuously reports the actual state, and the controller adjusts the command until the desired position is achieved.

```
Desired Position (Command)
         ↓
    [+] Summing Junction
         ↓
   Controller / Flight Computer
         ↓
   Electrical Signal → Servo Valve
         ↓
   Hydraulic Actuator
         ↓
   Actual Position (Output)
         ↓
   LVDT / Position Sensor
         ↑
   [Feedback signal back to summing junction]
```

This is a **negative feedback control loop** (also called a servo loop). The difference between command and actual position is the **error signal**, and the controller drives the error to zero.

### 5.2 Real Aircraft Example — Landing Gear Operation (Boeing 737)

The landing gear operation on the Boeing 737 is an excellent case study in electrical-hydraulic integration.

#### System Architecture

- **Hydraulic System B** (normal) and **Hydraulic System A** (alternate) provide power.
- System B operates at **3,000 psi**.
- The landing gear hydraulic control unit contains:
  - Main gear selector valve (solenoid-operated)
  - Nose gear selector valve (solenoid-operated)
  - Gear door sequencing valves (hydraulic-pilot operated, sequenced by pressure)
  - Four uplock actuators (one per gear)
  - Three downlock actuators (main gear: two; nose gear: one)
  - Three door actuators

#### Sequence of Events — Gear Retraction

1. **Pilot selects GEAR UP** on the flight deck lever.
2. An **electrical signal** is sent to the **landing gear control unit (LGCU)** — a dedicated microprocessor.
3. The LGCU checks:
   - Is the aircraft airborne? (WOW switches confirm: weight off wheels)
   - Is airspeed below the gear operating limit (260 KIAS)? (Air Data Computer confirms)
4. LGCU energises the **gear up solenoid valve** → Hydraulic pressure directed to gear up circuit.
5. **Door open actuator** extends → Doors open.
6. **Mechanical door-open limit switch** closes → LGCU confirms doors are open.
7. **Uplock actuator** releases → Gear free to move.
8. **Main gear actuators** retract under hydraulic pressure → Gear moves up.
9. **Uplock hook** engages automatically as gear reaches fully retracted position.
10. **Proximity sensor** (uplock indicator) confirms gear is up and locked.
11. LGCU de-energises solenoid and energises **door close solenoid** → Doors close.
12. **Door-closed proximity switches** confirm closure.
13. **Green DOWN and LOCKED lights** extinguish; **Gear lever light** extinguishes.
14. LGCU de-energises all solenoids; system returns to static pressurised state.

#### Fault Scenario

If at step 10 the proximity sensor does NOT confirm uplock within a time window, the LGCU:
- Generates a maintenance fault code (stored in CMC).
- Illuminates the **GEAR** advisory on EICAS/ECAM.
- On some aircraft, commands an alternate extension attempt.

This entire automated sequence demonstrates how electrical signals, solenoid valves, sensors, and hydraulic actuators work together as a tightly integrated system.

### 5.3 Flight Control Actuation — Aileron Example (Airbus A320)

The A320's aileron is controlled by a **servo-actuator** in a closed-loop electrohydraulic servo system:

1. **Pilot moves sidestick** → LVDT in sidestick generates position signal.
2. **ELAC (Elevator and Aileron Computer)** receives position signal.
3. ELAC computes the required aileron deflection, factoring in:
   - Air speed (reducing deflection at high speed)
   - Roll rate (to prevent over-roll)
   - Flight envelope protection (limiting bank angle)
4. ELAC sends a ±10 mA signal to the **EHSV (electrohydraulic servo valve)** on the aileron actuator.
5. EHSV shifts its spool, porting hydraulic pressure (3,000 psi) to the appropriate side of the actuator piston.
6. Actuator piston moves → Aileron deflects.
7. **LVDT on actuator** measures actual deflection and feeds back to ELAC.
8. ELAC continues to adjust the EHSV signal until actual deflection matches commanded deflection.
9. Full loop cycle time: **<5 milliseconds**.

---

## 6. Failure of Electrical Components and Sensors — Causes and Consequences

### 6.1 Failure Modes

#### 6.1.1 Electrical Open Circuit

An open circuit means the electrical signal path is broken — no current can flow. For a solenoid valve, this means the valve remains in its de-energised (spring-return) position regardless of commands.

**Causes:**
- Wire breakage (fatigue, chafing against structure)
- Connector pin corrosion or pin push-back
- Coil wire burn-out (thermal overload)
- PCB track fracture

**Consequence:**
- Loss of commanded function (e.g., gear will not retract; flap will not move)
- Detected by: BITE (open circuit detected as zero current), ECAM/EICAS warning

#### 6.1.2 Electrical Short Circuit

A short circuit provides an unintended low-resistance path, causing excessive current.

**Causes:**
- Wire insulation chafing against metal structure
- Fluid ingress into electrical connectors
- Failed component within the solenoid coil

**Consequence:**
- Circuit breaker trips → system loses power → solenoid de-energises
- Potential for wiring fire if protective device fails

#### 6.1.3 Sensor Failure — Stuck Output (Hardover)

A sensor fails and provides a constant, incorrect output.

**Example:** An LVDT provides a constant 5V output (full extension signal) regardless of actual position.

**Consequence:** Flight computer believes the control surface is deflected maximum and continually commands correction in the opposite direction → **actuator fights itself** → **potential loss of control**.

**Mitigation:** Modern fly-by-wire systems use **three independent sensors per surface** (triplex redundancy). The flight computer uses a voting algorithm — it takes the median of three sensor outputs. A single failed sensor is identified as the outlier and voted out.

#### 6.1.4 Sensor Drift

A sensor's output gradually shifts from its true value over time, due to temperature changes, vibration-induced zero-offset, or component ageing.

**Consequence:** Subtle position errors accumulate; control surface trim changes unnoticed; in critical applications, structural overloads could occur.

**Detection:** Periodic calibration checks during maintenance (sensor output verified against a known physical reference), and cross-comparison between redundant sensors during flight.

#### 6.1.5 Relay Contact Welding

Under high inrush current (e.g., when energising a large electric pump motor), relay contacts can arc and weld in the closed position. The relay then cannot be opened.

**Consequence:** A welded relay may hold a pump running when it should be off, overheating the pump, or hold a solenoid valve energised when it should be de-energised.

#### 6.1.6 Solenoid Coil Burnout

A solenoid coil will overheat if continuously energised beyond its duty cycle rating.

**Causes:**
- Incorrect voltage (over-voltage from a failed voltage regulator)
- Coil intended for intermittent duty is held energised for extended periods
- Contaminated hydraulic fluid increasing the force required to move the valve spool

### 6.2 Root Causes — Environmental Factors

| Factor | Effect on Electrical/Sensor Components |
|---|---|
| **Vibration** | Fatigue fractures in wire conductors and solder joints; sensor zero-drift |
| **Temperature extremes** | Insulation cracking; electronic component thermal derating; sensor calibration shift |
| **Hydraulic fluid contamination** | Fluid ingress into connectors and solenoid coils; degradation of wiring insulation |
| **Electromagnetic interference (EMI)** | False signals induced into sensor cables; corrupted digital bus data |
| **Moisture / corrosion** | Connector pin corrosion increases resistance → voltage drops → solenoid does not fully actuate |
| **Mechanical impact** | Damaged sensors in wheel wells (gear retraction damage) |

### 6.3 Safety Analysis Methods

Aircraft designers use rigorous methods to analyse how electrical component failures affect hydraulic system safety:

- **FMEA (Failure Mode and Effects Analysis):** Systematically identifies every component failure mode and its effect on the system.
- **FTA (Fault Tree Analysis):** Top-down logical diagram tracing from a hazardous event (e.g., "total hydraulic failure") to its root causes.
- **Common Cause Analysis (CCA):** Identifies failures that could simultaneously affect redundant systems (e.g., a single lightning strike disabling multiple flight computers).

Per **EASA CS-25** and **FAA FAR Part 25**, a **catastrophic failure condition** (one that would prevent continued safe flight and landing) must have a probability of less than **10⁻⁹ per flight hour**. This extremely low requirement drives the design of redundant, dissimilar electrical and hydraulic architectures.

---

## 7. Advantages, Limitations, and Maintenance Concerns

### 7.1 Advantages

#### Precision and Responsiveness
- Electrohydraulic servo valves can respond in **<5 milliseconds** and position actuators with accuracies of **±0.05 mm**.
- This precision is impossible with purely mechanical systems and enables advanced functions like active flutter suppression.

#### Automation and Reduced Pilot Workload
- Automated sequencing (landing gear operation, flap/slat deployment) frees pilots to focus on navigation and communication.
- Automatic fault detection and reconfiguration (e.g., switching to an alternate hydraulic system) reduce time-critical pilot decisions.

#### Remote Operation and Signal Routing
- Electrical signals can be routed through lightweight, thin cables rather than heavy hydraulic pipes, reducing weight and complexity in complex routings (e.g., fly-by-wire replaces heavy push-pull control rods from the cockpit to tail surfaces).

#### Continuous Health Monitoring
- Sensors provide real-time data enabling predictive maintenance, reducing unexpected in-service failures and improving dispatch reliability.

#### Integration with Digital Systems
- Electrical sensors output digital data that can be processed, logged, and transmitted to ground stations via ACARS, enabling airlines to plan maintenance before the aircraft lands.

### 7.2 Limitations

#### Electrical Power Dependency
- Hydraulic systems with electrical control lose their functionality if the electrical power bus fails. Multiple electrical buses and emergency power supplies (APU, RAT — Ram Air Turbine) mitigate but do not eliminate this risk.

#### Complexity and Skill Requirements
- The integration of electrical and hydraulic systems requires specialist engineers with cross-discipline knowledge. Troubleshooting faults that may be electrical or hydraulic requires expensive test equipment and extensive training.

#### Electromagnetic Susceptibility
- Sensor cables can pick up interference from other systems (HIRF — High-Intensity Radiated Fields from radar or communication transmitters). Shielding, filtering, and differential signal transmission are required, adding cost and weight.

#### Software Reliability
- Fly-by-wire systems depend on software in flight control computers. Software defects can introduce systematic failures not detectable by redundancy (both computers run the same software and fail the same way). **DO-178C Level A** (most critical) software certification aims to address this.

#### Environmental Vulnerability of Sensors
- Sensors in the landing gear bay are subject to impacts from runway debris, water ingestion, extreme temperature cycles, and vibration — one of the harshest environments on the aircraft.

### 7.3 Maintenance Concerns

#### 7.3.1 Connector Integrity

Hydraulic control connectors in wheel wells and engine pylons are subject to:
- Fluid ingress (hydraulic fluid, water)
- Corrosion of contact pins (especially in coastal/saltwater environments)
- Pin push-back from vibration

**Maintenance Actions:**
- Periodic inspection for corrosion, fluid contamination, and damaged pins.
- Application of approved corrosion-inhibiting compound (e.g., Kopr-Shield) on connector pins.
- Pressure testing of connector seals.

#### 7.3.2 Sensor Calibration and Testing

- LVDTs, pressure transducers, and temperature sensors require periodic calibration verification.
- **BITE tests** (commanded via maintenance computer) can verify sensor output against known pressures and temperatures applied by test equipment.
- **Out-of-tolerance sensors** must be replaced rather than adjusted; most aircraft sensors are sealed units.

#### 7.3.3 Wiring Inspection

- Hydraulic control wiring runs through areas exposed to hydraulic fluid spills and high temperatures (near engines).
- Fluid-contaminated wiring insulation becomes brittle and prone to cracking.
- Inspections use visual checks and **insulation resistance testing (Megger test)** to identify degraded insulation before it fails.

#### 7.3.4 Solenoid Valve Testing

- Solenoid valves are tested by commanding them electrically while monitoring:
  - Coil current draw (indicates coil resistance; high resistance → degraded coil)
  - Response time (slow response → stiction due to contamination)
  - Leakage past the valve seat (measured with downstream pressure gauge)

#### 7.3.5 Software and Firmware Updates

- Flight control computers running hydraulic system management software may require periodic updates to address operational programme software (OPS) issues discovered in service.
- Updates require approved maintenance procedures and functional testing of the complete system before return to service.

#### 7.3.6 Contamination-Induced Electrical Failures

- Hydraulic fluid (Skydrol) is an excellent electrical insulator in its pure state but becomes conductive as it degrades and picks up metallic wear particles.
- Conductive hydraulic fluid bridging connector contacts can create partial short circuits.
- Regular **hydraulic fluid sampling and analysis** (viscosity, acid number, particle count) also serves as early warning for electrical connector exposure risk.

---

## 8. Fly-By-Wire and Electro-Hydrostatic Actuators (EHA)

### 8.1 Fly-By-Wire (FBW)

Fly-By-Wire replaces the mechanical linkage between the cockpit controls and the hydraulic servo valves with **electrical signal transmission**. The pilot's control inputs are converted to electrical signals, processed by **Flight Control Computers (FCCs)**, and then sent as commands to EHSVs on the control surface actuators.

**History:**
- First production FBW aircraft: **Concorde** (1969) — analogue FBW
- First digital FBW transport aircraft: **Airbus A320** (1988)
- Boeing adopted digital FBW with the **Boeing 777** (1995)

**Advantages over conventional (cable/rod) systems:**
- Elimination of heavy, complex cable runs and bell-cranks
- Flight envelope protection (the computer prevents dangerous pilot inputs)
- Active load alleviation (reducing structural loads by deflecting surfaces in response to turbulence)
- Gust load alleviation reducing wing fatigue life consumption

### 8.2 Electro-Hydrostatic Actuators (EHA)

An **Electro-Hydrostatic Actuator (EHA)** is a self-contained unit that incorporates:
- A **brushless DC electric motor** (powered by aircraft electrical bus)
- A **small hydraulic pump** (driven by the electric motor)
- A **hydraulic actuator** (cylinder and piston)
- A **reservoir** (typically <1 litre)
- **Control electronics** (motor controller and position feedback)

All hydraulic fluid is contained within the unit — no connection to the aircraft's main hydraulic system is required.

**Why EHAs represent the future:**
- Eliminates hydraulic pipes running through the fuselage to the tail (weight saving: ~200 kg on a large aircraft)
- No risk of hydraulic fluid contaminating the environment (environmentally controlled)
- Reduced maintenance (no central hydraulic fluid servicing for EHA-controlled surfaces)

**Current aircraft using EHAs:**
- **Airbus A380:** EHAs are used as backup actuators on ailerons and horizontal stabiliser
- **Airbus A350:** Increased use of EHAs and EBHAs (Electrical Backup Hydraulic Actuators)
- **F-35 Lightning II:** EHAs for several control surfaces

**Diagram — EHA vs. Conventional Hydraulic Actuator:**

```
CONVENTIONAL:                    EHA:
Aircraft Hydraulic Bus           Aircraft Electrical Bus
    (3,000 psi pipe)                 (270 VDC)
         |                               |
    Servo Valve                    Motor Controller
         |                               |
   Hydraulic Cylinder            Brushless DC Motor
         |                               |
  Control Surface              Internal Hydraulic Pump
                                         |
                                   Hydraulic Cylinder
                                         |
                                  Control Surface
```

---

## 9. Hydraulic System Redundancy and Electrical Backup

### 9.1 Multi-Channel Hydraulic Architecture

Modern transport aircraft use multiple independent hydraulic systems to ensure that no single failure causes loss of flight control. These systems are isolated so that an electrical fault or hydraulic leak in one cannot propagate to another.

**Boeing 777 (three-system architecture):**
- **Left hydraulic system:** Powers left aileron, left elevator, left ground spoilers, nose-wheel steering.
- **Centre hydraulic system:** Powers left and right inboard ailerons, all elevator surfaces, all flight spoilers, nose gear steering, inboard brakes.
- **Right hydraulic system:** Powers right aileron, right elevator, right ground spoilers, right outboard brakes.

Each system has **two engine-driven pumps (EDPs)** and **two AC motor pumps (ACMPs)**. The ACMPs are powered by separate electrical buses, ensuring that a single electrical bus failure does not disable both pumps on any system.

### 9.2 Priority Valves and Load Shedding

In a hydraulic emergency (e.g., pump pressure low due to pump failure), **electrically controlled priority valves** ensure that critical functions (flight controls, brakes) receive hydraulic power before non-essential functions (cargo door actuators, hydraulic-powered ground service equipment).

The priority valves are spring-loaded closed and open only when system pressure is adequate. Electrical monitoring confirms each valve's state.

### 9.3 Ram Air Turbine (RAT)

The **Ram Air Turbine** is an emergency power device that deploys automatically (triggered by loss of normal electrical and/or hydraulic power). The RAT uses aerodynamic drag (a propeller exposed to the airstream) to drive either:
- A **hydraulic pump** (providing a minimum hydraulic flow for essential flight controls), or
- A **generator** (providing AC electrical power to operate electric motor pumps), or
- Both (on some aircraft).

**Example — Airbus A320 RAT:**
The A320 RAT deploys when both engines are not supplying power (dual engine failure). It provides approximately **5 kVA** of electrical power and drives a small hydraulic pump, maintaining sufficient control authority to execute a controlled glide and landing.

---

## 10. Certification and Safety Standards (DO-178C / ARP4754A)

### 10.1 Design Assurance Levels (DAL)

The **Design Assurance Level (DAL)** (also called Development Assurance Level) classifies each function by the worst consequence of its failure:

| DAL | Failure Condition Category | Probability per Flight Hour | Example |
|---|---|---|---|
| A | Catastrophic | <10⁻⁹ | Loss of flight control |
| B | Hazardous | <10⁻⁷ | Single hydraulic system failure |
| C | Major | <10⁻⁵ | Loss of hydraulic indication |
| D | Minor | <10⁻³ | Slow hydraulic filter clog indication |
| E | No Effect | No requirement | Cabin reading light |

### 10.2 DO-178C (Software)

All software controlling hydraulic system functions (flight control computers, landing gear control units) must be developed to the appropriate DAL under **DO-178C (RTCA)**. DAL A requires:
- Complete traceability from requirements to source code to test
- Modified Condition/Decision Coverage (MC/DC) testing
- Independent review of all activities

### 10.3 DO-254 (Hardware)

Complex electronic hardware (FPGAs, ASICs used in hydraulic control electronics) is governed by **DO-254**. Similar DAL-based process requirements apply.

### 10.4 ARP4754A (System Development)

**SAE ARP4754A** governs the overall aircraft and system development process, ensuring that system-level safety requirements (from FHA and PSSA) are properly allocated to software and hardware with appropriate DALs.

---

## 11. Summary and Critical Analysis

### 11.1 Key Takeaways

1. **Electrical and hydraulic systems are inseparable in modern aircraft.** The hydraulic system provides the muscle; the electrical system provides the brain.

2. **Sensors are the eyes of the system.** Without accurate pressure, temperature, position, and level data, neither the pilots nor the automatic systems can respond correctly to failures.

3. **Redundancy is designed in at every level.** Multiple hydraulic systems, multiple electrical buses, multiple sensors per function, and voting logic ensure that no single failure results in a catastrophic outcome.

4. **Fly-by-wire is the current standard; EHAs are the future.** The progressive replacement of central hydraulic systems with self-contained EHAs will continue to reduce weight, improve reliability, and simplify maintenance.

5. **Maintenance of electrical-hydraulic interfaces requires specialist knowledge.** The skills required to diagnose and rectify faults spanning electrical and hydraulic domains are among the most valuable in aviation maintenance.

### 11.2 Critical Analysis

#### Are Current Systems Sufficiently Reliable?

The record of modern fly-by-wire and electrohydraulic systems is excellent. However:

- **The 2001 crash of American Airlines Flight 587** was caused by composite rudder overload partly triggered by excessive rudder pedal inputs enabled by the FBW system. The FBW system allowed inputs that would have been physically impossible in a conventional system. This raised questions about whether FBW envelope protection for the rudder was adequately designed.

- **The Boeing 737 MAX MCAS failure (2018/2019)** was not a hydraulic failure, but it demonstrated how an incorrectly designed automated system that commands hydraulic actuators (in this case, the horizontal stabiliser trim) can overpower crew inputs. It highlighted the critical importance of system monitoring, failure annunciation, and crew training for automated hydraulic control systems.

#### The Increasing Reliance on Software

As EHAs and more-electric architectures become standard, hydraulic system safety increasingly depends on software correctness. DO-178C DAL A certification is rigorous but not infallible. Industry and regulators must continue to evolve standards as systems become more complex.

#### Maintenance Skills Gap

The progressive reduction in traditional hydraulic maintenance (as EHAs replace central hydraulic systems) requires a new generation of technicians equally skilled in power electronics and software diagnostics. Aviation training organisations need to evolve their curricula to match these technological shifts.

---

## 12. References and Further Reading

> **Full references:** See the dedicated [Topic 12 — References and Further Reading](./topics/12_References.md) document, which contains 78 fully annotated references organised by category.

Key references:

1. **Airbus A320 Aircraft Maintenance Manual (AMM)** — Chapter 29 (Hydraulic Power), Chapter 27 (Flight Controls). Airbus S.A.S.
2. **Boeing 737 Next Generation AMM** — Chapter 29 (Hydraulic Power). The Boeing Company.
3. **Boeing 777 Systems Description** — Hydraulic System. The Boeing Company.
4. **Pallett, E.H.J.** — *Aircraft Electrical Systems* (3rd ed.). Longman, 1987.
5. **Langton, R., Clark, C., Hewitt, M., & Richards, L.** — *Aircraft Fuel Systems*. Wiley-AIAA, 2009.
6. **Davies, D.P.** — *Handling the Big Jets* (3rd ed.). CAA, 1971.
7. **EASA CS-25** — Certification Specifications for Large Aeroplanes. European Union Aviation Safety Agency.
8. **FAA Advisory Circular AC 25.1309-1A** — System Design and Analysis.
9. **SAE ARP4754A** — Guidelines for Development of Civil Aircraft and Systems. SAE International, 2010.
10. **RTCA DO-178C** — Software Considerations in Airborne Systems and Equipment Certification. RTCA, 2011.
11. **Scholz, D.** — *Hydraulic Systems Lecture Notes*. Hamburg University of Applied Sciences (HAW Hamburg).
12. **Moir, I. & Seabridge, A.** — *Aircraft Systems: Mechanical, Electrical and Avionics Subsystems Integration* (3rd ed.). Wiley-AIAA, 2008.
13. **Jukes, M.** — *Aircraft Display Systems*. Professional Engineering Publishing / AIAA, 2004.

---

*Document prepared for group presentation on Electrical Systems and Sensors in Aircraft Hydraulic Systems.*
*All technical data is representative of general industry practice and publicly available documentation.*
*For specific maintenance procedures, always refer to the approved Aircraft Maintenance Manual (AMM).*
