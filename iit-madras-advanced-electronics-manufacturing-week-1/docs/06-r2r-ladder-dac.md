# 06 · R-2R Ladder Digital-to-Analog Converter

[← Power Electronics](05-power-electronics-learning.md) · [README](../README.md) · Next: [Through-Hole Soldering →](07-through-hole-soldering.md)

> **Evidence note.** The analysis is drawn from one supplied photograph containing a printed circuit diagram and a perfboard build. No measured output, oscilloscope capture, calibration, or test result was supplied. This circuit is **not** described as a verified, calibrated or successfully tested DAC. A handwritten header on the printed sheet names another institution, so the diagram appears to come from third-party lab material (see the README licensing section).

## 1. Introduction

A digital-to-analog converter (DAC) turns a binary code into a proportional voltage or current. Digital systems compute with discrete codes, but actuators, audio stages, sensors and analog filters work with continuous quantities, so a DAC forms the interface. The R-2R ladder is a classic way to build one from only two resistor values.

## 2. Objective

To understand how a counter-driven R-2R ladder converts a clocked binary count into a stepped analog output, and to relate the printed diagram to the hand-wired perfboard hardware.

## 3. Basic DAC theory

An N-bit DAC has 2^N possible codes. The smallest output step (1 LSB) is the reference span divided by 2^N. Ideal output rises linearly with code; real converters show offset, gain error, differential nonlinearity (DNL) and integral nonlinearity (INL).

## 4. R-2R ladder structure

Resistors of value R run in series along the rail; resistors of value 2R connect each node to its bit switch, and one 2R terminates the LSB end to ground. Only two values are required, which makes matching easier than the binary-weighted resistors that would otherwise need R, 2R, 4R, 8R...

## 5. Working principle

At each node, looking toward the LSB end, the resistance equals 2R in parallel with 2R = R, so each stage "sees" a 2R leg next to an effective 2R continuation. Because of this repeated 2:1 relationship, each bit's contribution to the output is **half** that of the next more significant bit, which yields binary weighting without precision-weighted values. This is the reason for the 1:2 ratio.

## 6. Circuit diagram

![R-2R circuit diagram with 74LS393 counter, 7404 inverter sections and resistor ladder](../assets/images/r2r-ladder-circuit-diagram.jpg)
*Figure 1. Printed R-2R circuit diagram (crop of the supplied photo): clock input, 74LS393 counter, logic stages, resistor ladder and output.*

**What is readable:**

- Title "6.0 R-2R Circuit Diagram".
- A "5 VPP / 1 KHz Clock Pulse" input to 74LS393 pin 1; +5 V on pin 14 and ground on pin 7.
- Pin 2 connected to ground through 1 kΩ.
- Counter pins 3, 4, 5, 6 leading to four logic stages marked 7404.
- A ladder with **five 2 kΩ** and **four 1 kΩ** resistors; the last 1 kΩ leads to the output.

## 7. Hardware implementation

![Perfboard hardware of the R-2R build](../assets/images/r2r-ladder-hardware-implementation.jpg)
*Figure 2. Hand-wired perfboard (crop of the supplied photo).*

**What is visible:** four IC sockets in a row (they appear unpopulated), a tangle of green jumper wires, roughly eight axial resistors in two groups at the lower part of the board, and red, black and red leads with stripped ends (plausibly supply, ground and signal connections, but unconfirmed).

**Limits of this photo:** the top-down angle and the dense wire bundle hide connections; I cannot trace pin-to-pin wiring; the resistor values (colour bands) are unreadable; and I count fewer resistors than the diagram's nine ladder parts plus the 1 kΩ at pin 2, so the build may be unfinished, partly hidden, or still in progress. The original uncropped photo is [here](../assets/images/r2r-ladder-worksheet-and-hardware-original.jpg); the sheet below the diagram is headed "7.0 Observe and Draw the output Ladder Network", indicating a worksheet task.

## 8. Digital counter and logic-stage context

- **74LS393**: dual 4-bit binary ripple counter. Pin 1 is the clock input (1A), pin 2 the active-high clear (1CLR), pins 3–6 the four outputs (1QA–1QD). The diagram uses one counter, so the 4-bit output cycles through 16 states. Holding clear low via the 1 kΩ lets it count. The LS-series counter increments on the falling clock edge.
- **7404** (hex inverter): the diagram marks four stages with 7404 pin numbers. Conceptually, these buffer or condition the counter outputs before they drive the 2R legs. The drawing style of each stage (switch-like symbols with an inverter triangle) makes exact internal connections hard to read, so **I cannot reliably state the pin-by-pin wiring or whether the stages are single inverters or drive stages in parallel.**
- Because an inverter complements its input, the ladder code may be the *complement* of the counter code, which would turn an up-count into a descending staircase. Whether that applies depends on actual wiring. **Unverified.**

## 9. Clock input and digital state progression

At 1 kHz, a 4-bit counter completes one cycle of 16 states in 16 ms (62.5 Hz repetition). Each clock falling edge advances the count by one, so the ladder output should step through 16 levels and repeat, forming a staircase (sawtooth-like) waveform. This is the expected behaviour, **not a measured result**.

## 10. Expected theoretical behaviour

Counting 0000 → 1111, the ideal output rises by one LSB per clock until full scale, then returns to zero. If the inverter stages invert the code, the sequence runs in the opposite direction.

## 11. Output resolution and ideal quantisation

### Ideal equation

For an N-bit R-2R ladder, with bit b_i ∈ {0, 1} (b_0 is the LSB, b_(N−1) the MSB), the ideal output is

$$V_{out} = V_{ref}\,\frac{D}{2^{N}}, \qquad D=\sum_{i=0}^{N-1} b_i\,2^{i}$$

| Symbol | Meaning |
|---|---|
| V_out | Output voltage at the MSB end of the ladder |
| V_ref | Voltage a bit drives when it is "1" (0 V when "0") |
| N | Number of bits |
| b_i | State of bit i |
| D | Decimal value of the input code, 0 … 2^N − 1 |

**Assumptions:** voltage-mode output; ideal reference/switches (each bit is exactly V_ref or exactly 0 V with zero output resistance); exactly matched resistors (all 2R legs exactly twice R); the 2R termination to ground; output tapped at the MSB node; **no load** (or a load much larger than R); b_0 nearest the termination.

**Step size:** 1 LSB = V_ref / 2^N. **Full scale:** V_ref (2^N − 1)/2^N, never quite V_ref.

### Applied to this circuit (illustrative only)

Four bits visibly drive the ladder, so N = 4 if all four counter outputs are used, giving 16 levels and 1 LSB = V_ref/16. If V_ref were 5 V (the supply) that would be 0.3125 V per step. **That figure is illustrative.** In practice, TTL outputs are not at the supply rail (a logic-high of LS-series TTL is typically around 3 V rather than 5 V), so V_ref is whatever the stage actually delivers. Bit ordering was not verified; the ladder itself would place b_0 at the termination end. The extra 1 kΩ in series at the output is outside the ideal equation and acts as an output resistor.

## 12. Practical resistor tolerances

The weights depend on ratios, so mismatch in 2R/R shows up as DNL/INL errors, worst at major transitions (e.g. 0111 → 1000), where many bit contributions change together. Rule of thumb: the MSB needs resistor accuracy better than about half an LSB of the total span, which is demanding as N grows. Use matched or 1 % (or better) parts and measure values before soldering. Resistor values here ("1 kΩ", "2 kΩ") come from the diagram; the physical resistor values in the photo cannot be read.

## 13. Loading effects and output impedance

The ladder's Thevenin output resistance is about R at the MSB node (here, with the output series resistor, roughly 2 kΩ if R = 1 kΩ). A load of comparable resistance divides the output and compresses the staircase. For a real load, use a high-impedance buffer (op-amp follower) or high-input-impedance instrument. An oscilloscope with a 1 MΩ input is typically fine; a low-impedance meter or speaker is not.

## 14. Potential logic-level and interface issues

- TTL logic-high and logic-low levels are not exactly 5 V and 0 V and vary with load and temperature.
- High- and low-state output resistances differ, which breaks the ideal "equal switches" assumption and causes bit-weight error.
- Unused inputs, clear-pin level and clock edge quality can cause miscounts.
- Supply decoupling is needed near each IC; the perfboard has no visible decoupling capacitors.

## 15. Common wiring errors

- Swapped bit order (reversed staircase direction or scrambled waveform).
- Missing or open ladder resistor or termination.
- Counter clear (pin 2) floating or high, which holds the counter at zero.
- IC inserted backwards or powered incorrectly (+5 V on pin 14, ground on pin 7).
- Wrong resistor value (a colour-code misread).
- Short between adjacent pads or wires; floating ground between sections.

## 16. Testing methodology (proposed, not performed)

1. Power off: verify continuity of ground and supply, resistor values and the absence of shorts.
2. Insert ICs with correct orientation; apply 5 V from a current-limited supply.
3. Check the clock at pin 1 and the counter outputs, ideally with an oscilloscope.
4. Hold the counter to chosen codes (if possible), record V_out for all 16 codes with a multimeter.
5. Compute ideal values from the equation using the **measured** high-level voltage; compare and plot error in LSBs; compute DNL/INL.
6. Run the clock again and view the staircase on an oscilloscope; check step uniformity and largest step.

## 17. Safety precautions

Use low-voltage supplies with current limiting; verify polarity; do not plug or unplug ICs with power on; keep the tangle of wiring from shorting; confirm clock source amplitude does not exceed IC ratings.

## 18. Observations supported by the available evidence

- The diagram shows a clock-driven 74LS393, 7404 logic stages and a resistor ladder with 5 × 2 kΩ and 4 × 1 kΩ.
- A perfboard prototype with IC sockets, resistors and green jumper wires exists.
- A worksheet instruction ("Observe and Draw the output Ladder Network") is printed on the sheet.

## 19. Limitations of the available documentation

- No measured output or waveform.
- Exact pin wiring, bit order and inverter-stage function cannot be reliably read from the diagram.
- The hardware appears to have fewer resistors than the diagram and apparently empty sockets; completion state is unknown.
- Resistor values on the hardware are unreadable.
- The diagram is third-party material.

## 20. Future improvements

- Redraw the circuit in KiCad with traceable pin numbers.
- Replace the perfboard with a designed PCB.
- Add supply decoupling and an op-amp buffer.
- Use matched resistors; measure all 16 codes and compute DNL/INL.
- Capture waveforms with an oscilloscope and compare to the ideal equation.

## Theoretical vs measured vs actual

| Term | Meaning | Status here |
|---|---|---|
| Theoretical output | From the ideal equation | Can be computed once V_ref is known |
| Measured output | Readings from instruments | **Not supplied** |
| Actual performance | Behaviour with tolerance, loading, temperature, noise | **Unknown** |
