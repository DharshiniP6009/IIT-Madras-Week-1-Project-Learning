# 07 · Through-Hole Soldering

[← R-2R Ladder DAC](06-r2r-ladder-dac.md) · [README](../README.md) · Next: [Key Takeaways →](08-key-takeaways-and-future-learning.md)

**Evidence level:** Through-hole soldering and circuit assembly were part of the Week 1 training. No soldering assessment, component count, temperature setting or measured result was supplied; none is stated. The practical guidance below is general good practice.

## Observed assembly

![Perfboard with IC sockets, resistors, green jumper wires and coloured leads](../assets/images/r2r-ladder-hardware-implementation.jpg)
*Figure 1. Perfboard assembly (crop of the supplied photograph). Seen: IC sockets, resistors, a dense bundle of green jumper wires, and red/black/red leads with stripped ends.*

What the photo shows about assembly: the circuit is hand-wired point to point on a perforated board rather than a custom PCB, with IC sockets used so chips can be inserted or replaced. The wiring is dense, which makes tracing and debugging harder than on a designed PCB. The angle prevents me from judging individual joint quality.

## Process

### 1. Identify leads and pads
Match each component to its location using the schematic or silkscreen. Confirm resistor values (colour code or meter), IC pin 1, diode cathode band and capacitor polarity.

### 2. Place components
Insert from the board side marked for components, seat the part flat (or at the intended standoff) and bend leads slightly to hold it. Fit low-profile parts first (resistors, diodes), then sockets, then tall parts.

### 3. Heat the joint
Touch the iron tip so it contacts **both the pad and the lead** at the same time. Heating only the lead or only the pad is a common cause of poor joints.

### 4. Apply solder
Feed solder to the joint (pad/lead junction), not to the iron tip. Let it flow around the lead, then remove solder first and iron second. Hold the part still while the joint solidifies.

### 5. Trim and clean
Trim leads above the joint with flush cutters (wear eye protection; clipped leads fly). Clean flux residue if required.

## Recognising joint quality

| Joint | Appearance | Consequence |
|---|---|---|
| **Good** | Smooth, shiny/satin, concave fillet wetting both pad and lead | Reliable |
| **Cold joint** | Dull, grainy or lumpy; poor wetting; may form a ball | Intermittent or high resistance |
| **Insufficient solder** | Lead or pad partly uncovered | Weak or open |
| **Excess solder / bridge** | Blob or solder spanning two pads or tracks | Short circuit |
| **Lifted pad** | Pad peeled from board | Needs repair; overheating is a cause |

## Inspection and continuity testing

1. Visual check under good light (magnification helps).
2. **Power off**; use a multimeter continuity mode to check connections that should exist (e.g. pin to the correct net).
3. Check that **no shorts** exist between supply and ground and between adjacent pins.
4. Compare against the schematic point by point; wire-by-wire for perfboard builds.
5. Only then apply power, ideally current-limited.

## Safe handling

- Soldering irons are hot enough to burn; always return to the stand, never leave one unattended.
- Ventilate or extract fumes; wash hands after handling solder (especially leaded).
- Protect eyes when clipping leads.
- Keep the area clear of flammables; tidy cables.
- Switch off and cool the iron before leaving.

## Hand-wiring versus PCB

Point-to-point wiring is quick for a one-off, but a PCB gives repeatability, shorter and controlled routing, and easier inspection. That contrast is exactly what the KiCad work in [doc 02](02-pcb-design-using-kicad.md) is aimed at.

## Self-assessment

| Skill | Level |
|---|---|
| Explain the soldering process and joint criteria | Yes |
| Judge soldering quality of a specific joint from a photo | Not from the supplied images |
| Consistent high-quality joints | Needs continued practice |
