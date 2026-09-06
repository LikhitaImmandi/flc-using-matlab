# Smart Room Temperature Control Using Fuzzy Logic Controller

A Smart Air Conditioner system that regulates room temperature using a Fuzzy Logic Controller (FLC), designed in MATLAB Fuzzy Logic Designer and simulated in Simulink.

**Course:** Mathematics for Robotics — Project
**Professor:** Kumar Ujjwal
**Student:** Immandi Sri Likhita (SE25MROB009)

---

## Abstract

This project presents the design and implementation of a Smart Air Conditioner using a **Fuzzy Logic Controller (FLC)**. The controller regulates room temperature by dynamically adjusting cooling power based on the temperature error and the rate of change of temperature, without requiring an exact mathematical model of the system.

## Objective

To design a fuzzy logic–based temperature control system using **MATLAB Fuzzy Logic Designer** and **Simulink**.

## Inputs and Output

| Signal | Description |
|---|---|
| Input 1 | Temperature Error |
| Input 2 | Change in Temperature |
| Output | Cooling Power |

## Membership Functions

Triangular membership functions (`trimf`) were used for all inputs and the output.

**Temperature Error**
- NL (Negative Large): `[-10 -10 0]`
- ZE (Zero): `[-5 0 5]`
- PL (Positive Large): `[0 10 10]`

**Change in Temperature**
- N (Negative): `[-30 -30 0]`
- Z (Zero): `[-5 0 5]`
- P (Positive): `[0 30 30]`

**Cooling Power**
- Low: `[0 0 50]`
- Medium: `[25 50 75]`
- High: `[50 100 100]`

## Fuzzy Rule Base

| # | Rule |
|---|---|
| 1 | IF Error is NL AND Change is N THEN Cooling is High |
| 2 | IF Error is NL AND Change is Z THEN Cooling is High |
| 3 | IF Error is NL AND Change is P THEN Cooling is Medium |
| 4 | IF Error is ZE AND Change is N THEN Cooling is Medium |
| 5 | IF Error is ZE AND Change is Z THEN Cooling is Low |
| 6 | IF Error is ZE AND Change is P THEN Cooling is Low |
| 7 | IF Error is PL AND Change is N THEN Cooling is Low |
| 8 | IF Error is PL AND Change is Z THEN Cooling is Low |
| 9 | IF Error is PL AND Change is P THEN Cooling is Low |

## Mathematical Model

**Error Equation**

```
E = Tset - Troom
```

**Change in Error**

```
ΔE = E(k) - E(k-1)
```

**Transfer Function** (room/plant dynamics)

```
G(s) = 1 / (10s + 1)
```

## Simulink Implementation

The Simulink model consists of the following blocks:

- **Step** — reference temperature input
- **Sum** — computes error (setpoint − room temperature)
- **Unit Delay** — used to compute change in error over discrete steps
- **Mux** — combines error and change-in-error signals into the FLC input
- **Fuzzy Logic Controller** — computes cooling power based on the rule base
- **Transfer Function** — models the room's thermal response
- **Scope** — displays the temperature response over time

```
Step (Tset) → Sum (Error) ──┬──────────────► Mux ─► Fuzzy Logic Controller ─► Transfer Function ─► Scope
                             └─► Unit Delay ─► Sum (ΔE) ─┘
```

## Results

The response curve shows **stable and smooth temperature regulation**. The fuzzy controller successfully adjusts cooling power dynamically in response to changing temperature error and rate of change, converging the room temperature toward the setpoint without oscillation or overshoot.

## Conclusion

The fuzzy logic–based smart AC system effectively controls room temperature without requiring an exact mathematical model of the plant. The system provides smooth, intelligent cooling control suitable for real-time applications such as smart thermostats and HVAC automation.

## Repository Contents

> Update this section to match your actual repository layout.

```
.
├── fuzzy_ac_controller.fis     # Fuzzy Inference System (MATLAB Fuzzy Logic Designer)
├── smart_ac_model.slx          # Simulink model
├── docs/                       # Project report and documentation
└── README.md
```

## Getting Started

**Requirements:** MATLAB with the Fuzzy Logic Toolbox and Simulink.

1. Open MATLAB and load the fuzzy inference system:
   ```matlab
   fis = readfis('fuzzy_ac_controller.fis');
   ```
2. Open the Simulink model:
   ```matlab
   open('smart_ac_model.slx')
   ```
3. Run the simulation and view the temperature response on the **Scope** block.
4. Adjust the setpoint (`Step` block) or membership function parameters to test different scenarios.

## Tools Used

- MATLAB Fuzzy Logic Designer
- Simulink
