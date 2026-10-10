# 04 · Analog Electronics Fundamentals

[← PCB Fabrication](03-pcb-fabrication-workflow.md) · [README](../README.md) · Next: [Power Electronics →](05-power-electronics-learning.md)

**Evidence level:** conceptual. This document explains fundamentals relevant to the Week 1 hardware. It does not claim that each component type was individually measured during training. Where a component appears in the supplied images this is stated.

## Why analog fundamentals matter in hardware design

Digital systems still run on analog physics: supply rails sag, edges ring, and logic levels are voltages. Both Week 1 circuits show this:

- the **R-2R DAC** ([doc 06](06-r2r-ladder-dac.md)) converts digital bits into a voltage through resistor ratios;
- the **UC3845 converter** ([doc 05](05-power-electronics-learning.md)) uses a resistor divider, an RC timing network, and a current-sense resistor to control power flow.

Reading a schematic is largely reading analog relationships.

## Core quantities

| Quantity | Symbol | Unit | Relationship |
|---|---|---|---|
| Voltage | V | volt | Potential difference |
| Current | I | ampere | Charge flow |
| Resistance | R | ohm | Ohm's law: V = I·R |
| Power | P | watt | P = V·I = I²R = V²/R |

Power dissipated matters for part ratings. For example, the 1 Ω resistor R5 in the schematic carries the switch current in the power stage; its dissipation is I²·R and its power rating must be chosen accordingly (the rating is not visible in the screenshot).

## Components

### Resistors
Limit current, set bias, divide voltage. Series resistance adds; parallel resistance combines as 1/R = 1/R₁ + 1/R₂. Tolerance (e.g. 1 %, 5 %) matters most where *ratios* matter: the R-2R ladder and the feedback divider (R6/R7). **Seen in images:** R1–R7 in the schematic; resistors on the perfboard.

### Capacitors
Store charge; block DC and pass AC; smooth ripple; decouple supplies. Key ratings: capacitance, voltage, tolerance, ESR and ripple current. Electrolytic capacitors are **polarised**: reversing them can damage them. **Seen in images:** C1–C3 as small disc capacitors and C4 (1000 µF/50 V) as an electrolytic.

### Inductors
Store energy in a magnetic field, resist changes in current (v = L·di/dt). Ratings: inductance, saturation current, RMS current, DC resistance. **Seen in images:** L1, labelled "10uf" in the schematic. Inductance is measured in henries (µH), so that label looks like a unit slip I would correct to "10uH" if that was the intent. I cannot confirm the intended value.

### Diodes
Conduct in one direction (forward drop typically a few tenths of a volt to about a volt, depending on type). In power circuits the key ratings are reverse voltage, average and peak forward current and recovery speed. **Seen in images:** D1, labelled only "D"; no part number is shown.

### Transistors and switches (preview)
A MOSFET acts as a voltage-controlled switch: see [doc 05](05-power-electronics-learning.md).

## Polarity and orientation checklist

| Part | Polarity marking |
|---|---|
| Electrolytic capacitor | Stripe / minus mark on can; longer lead is positive |
| Diode | Band at cathode (K) |
| IC | Notch or dot marks pin 1 |
| MOSFET (TO-220) | Check datasheet for gate, drain, source order |
| Resistors, ceramic discs | None |

The KiCad PCB screenshot marks D1's cathode with "K" and shows an IC pin-1 notch for U1.

## Reading a schematic

1. Find the supply rails and ground.
2. Identify the controller/active device and its pin names.
3. Trace each net, including long wires that route around the page.
4. Group sub-circuits (timing, feedback, power stage, protection).
5. Check values and ratings against expected operating conditions.

## Theory versus practice

Theory assumes ideal parts. In practice: resistors have tolerance, capacitors have ESR and voltage derating, inductors saturate, logic outputs are not ideal voltage sources, and wires and breadboard/perfboard connections add resistance and noise. Good design explicitly checks which ideal assumptions the circuit depends on.

## Self-assessment

| Skill | Level |
|---|---|
| Reading and tracing a schematic | Working conceptual ability |
| Identifying polarity-sensitive parts | Yes |
| Multimeter/oscilloscope verification | Future goal ([doc 08](08-key-takeaways-and-future-learning.md)) |
