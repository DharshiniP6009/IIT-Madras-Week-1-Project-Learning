# 02 · PCB Design Using KiCad

[← Overview](01-week-1-learning-overview.md) · [README](../README.md) · Next: [PCB Fabrication Workflow →](03-pcb-fabrication-workflow.md)

This document walks through the schematic-to-PCB workflow in KiCad. Each stage is tagged:

- **[Visible]**: evidenced in my supplied screenshots.
- **[Concept]**: a standard workflow step I learned about; not shown in the screenshots.
- **[Practice]**: needs more hands-on repetition before I would claim proficiency.

Only the three screenshots below were supplied, so the project files themselves cannot be inspected.

![KiCad schematic editor showing a UC3845 controller, IRF540N MOSFET, diode, inductor, resistors and capacitors](../assets/images/kicad-power-electronics-schematic.jpg)
*Figure 1. KiCad Schematic Editor: power-electronics design (see [doc 05](05-power-electronics-learning.md)).*

![KiCad PCB editor showing the routed layout with front and back copper](../assets/images/kicad-power-electronics-pcb-layout.jpg)
*Figure 2. KiCad PCB Editor: the corresponding board layout. The status bar reads 39 pads, 0 vias, 98 track segments, 12 nets, 0 unrouted.*

## A. Schematic capture [Visible]

Schematic capture records circuit *intent*: which components exist and how they connect. Figure 1 shows a complete drawing with reference designators (U1, Q1, D1, L1, R1–R7, C1–C4), values (e.g. 22k, 10k, 1000uf/50v) and wires with junction dots. A clean schematic prevents misreading later: grouping the controller, power stage and feedback network makes errors easier to spot.

## B. Component symbols [Visible]

A symbol is the electrical abstraction of a part: pins, names and numbers (e.g. UC3845 pins 1 COMP … 8 VREF). It carries no physical size. Symbol pin numbers must match the datasheet; a wrong pin assignment is a classic source of boards that are "correct" in KiCad but wrong on the bench.

## C. Electrical nets and connections [Visible]

A net is a set of pins that are electrically the same node. Wires, labels and power symbols all create nets. In Figure 1 the feedback node (R2/R6/R7 junction) is routed to the FB pin by a long wire along the bottom, which is easy to misread as a floating line; checking net membership rather than visual proximity is the correct habit. The PCB status bar shows 12 nets.

## D. Assigning footprints [Visible / Practice]

A footprint is the physical land pattern: pad size, pitch, drill and outline. Figure 1's status bar shows C3 assigned `Capacitor_THT:C_Disc_D3.8mm_W2.6mm_P2.50mm`, a through-hole disc capacitor with 2.5 mm lead pitch. Footprints must be matched to the *actual part purchased*: lead spacing, body width and pin-1 orientation. A symbol with the wrong footprint gives a board that cannot accept the component. I have seen the assignment mechanism; selecting footprints against real datasheets for every part is still [Practice].

## E. Electrical Rules Checker (ERC) [Concept]

ERC analyses the schematic for unconnected pins, conflicting pin types, missing power-driver flags and similar problems. The `PWR_FLAG` symbols in Figure 1 are used to tell ERC that a net is driven by a source, which suppresses "power input pin not driven" warnings. The screenshots do not show an ERC report, so I cannot state it was run or passed.

**Limit:** ERC checks *connectivity logic*, not whether the circuit is a good design. A wrong resistor value passes ERC.

## F. Moving the design to the PCB editor [Visible]

The schematic is transferred as a netlist: footprints appear on the board and connections appear as ratsnest lines. Figure 2 has the footprints placed and every net routed. The schematic and PCB remain linked; changing a connection should be done in the schematic and re-synchronised, never only on the board, otherwise the two diverge.

## G. Board outline [Visible]

Figure 2 shows a rectangular outline (thin line on Edge.Cuts) enclosing all parts, with two circular items labelled REF1 and REF2 near opposite corners. They are likely mounting holes, but this cannot be confirmed from the image. The outline defines the manufactured shape; copper and parts must stay clear of it by the fabricator's minimum edge clearance.

## H. Component placement [Visible]

Placement drives routability, noise and thermal behaviour. In Figure 2 the MOSFET Q1 and diode D1 sit near the top edge with L1 beside them (the high-current switching group), while U1 and the small resistors/capacitors are placed in the lower-left area. Whether this placement was optimised for loop area cannot be judged from one screenshot (see [doc 05](05-power-electronics-learning.md)).

## I. Track routing [Visible]

Tracks are copper conductors. Figure 2 uses red tracks on F.Cu (front) and blue on B.Cu (back), with 0 unrouted connections. Track width should follow the current: signal nets can be thin; the inductor, MOSFET, diode and output-capacitor path carries more current and needs wider copper. Track widths in this design cannot be measured from the screenshot.

## J. Copper layers and vias [Visible]

The board is two-layer. The status bar reports **0 vias**: layer changes are made through the plated through-hole pads of the through-hole components, which connect both layers. That is a valid technique on simple THT boards. A via is a plated hole used only to move a track between layers.

## K. Design Rules Checker (DRC) [Concept]

DRC compares the layout against rules: minimum clearance, track width, annular ring, drill size, unconnected nets, and courtyard overlaps. Rules should be set from the chosen fabricator's capabilities *before* routing. No DRC report is visible in the screenshots. The title bar's leading asterisk (`*Naren — PCB Editor`) indicates unsaved changes.

## L. 3D visualisation [Visible]

![KiCad 3D Viewer showing a green board with TO-220 MOSFET, axial diode, DIP-8 IC, resistors and capacitors](../assets/images/kicad-3d-pcb-visualization.jpg)
*Figure 3. KiCad 3D Viewer: the same component set as Figures 1–2 rendered on the board.*

The 3D viewer renders each footprint with its 3D model, so I can check things a flat layout hides: whether a TO-220 body clears neighbours, whether the electrolytic capacitor's height and polarity orientation look right, whether the axial diode and resistors have sensible lead spans, and whether silkscreen is legible.

**Why a board can look right in 3D and still be wrong:** the 3D view is a *geometric* check. It does not verify that nets are connected correctly, that clearances meet the fabricator's minimum, that track widths carry the current, that a footprint matches the real part's pinout (the model may be generic), or that the schematic itself is correct. Pin-1 and polarity can also appear plausible while being mirrored relative to the datasheet.

## M. Gerber generation [Concept]

Gerber files are the per-layer image description used by fabricators: copper (F.Cu, B.Cu), solder mask, silkscreen, board outline and, optionally, paste. Typical practice is to plot from the PCB editor, then **open the files in a Gerber viewer** to confirm that each layer is present and the outline is closed. No Gerber files were supplied or generated in the material documented here.

## N. Drill file generation [Concept]

The drill file (Excellon format) lists hole positions and diameters for plated and non-plated holes. Gerbers alone do not define holes. Drill and Gerber origins must use the same reference, or holes will be offset from pads. Not generated in the supplied material.

## O. Fabrication preparation [Concept]

Zip the Gerbers and drill files, confirm layer count, thickness, copper weight, finish and minimum track/space against the fabricator's capability sheet, and provide a BOM and assembly notes. See [doc 03](03-pcb-fabrication-workflow.md).

## P. Common PCB design mistakes

- Footprint pad pitch or drill not matching the real component.
- Mirrored or wrong pin numbering for transistors and ICs.
- Polarity markings missing for diodes and electrolytic capacitors.
- Power tracks too narrow for the current.
- Large switching-current loops and ground returns shared with sensitive feedback nets.
- Clearances too small for the working voltage.
- No input/output connection points (see the observation below).
- Running DRC with default rules instead of the fabricator's rules.
- Editing the PCB without updating the schematic.

**Observation on the supplied design.** The schematic has `PWR_FLAG` markers at the input and output rails, but I can see no connector symbols or terminal footprints in Figures 1–2. Wires would have to be soldered to existing component pads or additional pads added. That is a practical point to fix in a next revision, and I note it as an observation, not as a verified fault, since I only have the screenshots.

## Q. Engineering lessons learned

1. The schematic is the source of truth; the PCB is a physical realisation of it.
2. Footprints connect software to the physical world and deserve as much checking as the circuit.
3. 3D view, ERC and DRC are screening tools. Passing all three means "not obviously wrong", not "will work".
4. Plan connectors, test points and mounting before routing.
5. A high-current switching layout is a different problem from a low-speed signal layout.

## Summary of evidence level

| Stage | Level |
|---|---|
| Schematic capture, symbols, nets | Visible in screenshot |
| Footprint assignment | Partly visible (C3 footprint); more practice needed |
| ERC / DRC | Concept; no reports supplied |
| Placement, routing, layers | Visible in screenshot |
| 3D visualisation | Visible in screenshot |
| Gerber / drill generation | Concept; no files supplied |
