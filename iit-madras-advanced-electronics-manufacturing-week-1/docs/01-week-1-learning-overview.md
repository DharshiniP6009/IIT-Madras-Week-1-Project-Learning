# 01 · Week 1 Learning Overview

[← Back to README](../README.md) · Next: [PCB Design Using KiCad →](02-pcb-design-using-kicad.md)

## Programme context

| Item | Detail |
|---|---|
| Programme | Advanced Electronics Manufacturing Training Program |
| Institution | Indian Institute of Technology Madras (IIT Madras) |
| Location | Central Electronics Centre (CEC), IIT Madras |
| Period documented | Week 1 |
| Focus areas | KiCad schematic and PCB design, PCB visualisation and fabrication workflow, through-hole soldering, analog electronics, power electronics, R-2R ladder DAC |

This repository is an independent, personal learning record. It is not an official IIT Madras publication and is not endorsed by the institute.

## How the evidence is labelled

Throughout the documentation, every claim falls into one of five categories, as requested in the project brief. The labels are used so a reader can tell experience from textbook knowledge.

| Label | Meaning |
|---|---|
| **Learned** | A concept covered in the Week 1 topic list. Explained here in my own words; not necessarily individually tested. |
| **Designed / worked on** | Visible in a supplied KiCad screenshot (schematic, PCB layout, 3D view). |
| **Assembled / observed** | Visible in the supplied photograph of the R-2R ladder worksheet and perfboard hardware. |
| **Can explain** | Theory I can walk through at a conceptual level (e.g. R-2R operation, boost-converter energy flow). |
| **Needs verification** | Anything that would need measurement, a datasheet check, or more hands-on practice before being claimed. |

## Evidence supplied for Week 1

Four image files were supplied. One of them (the R-2R photograph) contains two things in a single frame: a printed circuit diagram and the assembled hardware beneath it. Derived crops of each half are provided for readability; the unaltered original is kept too.

| # | File | What it shows |
|---|---|---|
| 1 | `r2r-ladder-worksheet-and-hardware-original.jpg` | Original photo: printed "6.0 R-2R Circuit Diagram" above a perfboard build ("7.0 Observe and Draw the output Ladder Network") |
| 1a | `r2r-ladder-circuit-diagram.jpg` | Crop of the diagram half |
| 1b | `r2r-ladder-hardware-implementation.jpg` | Crop of the hardware half |
| 2 | `kicad-3d-pcb-visualization.jpg` | KiCad 3D Viewer showing a through-hole board |
| 3 | `kicad-power-electronics-pcb-layout.jpg` | KiCad PCB Editor layout |
| 4 | `kicad-power-electronics-schematic.jpg` | KiCad Schematic Editor |

**Mapping note.** Comparing the references and values in the three KiCad screenshots (Q1, D1, L1, U1, R1–R7, C1–C4), the 3D board, PCB layout and schematic appear to be the **same design**: a UC3845-based power-electronics circuit. The 3D board is therefore *not* the R-2R DAC. This is an inference from matching component references, not from a project file.

## Topic map

| Topic | Document | Evidence type |
|---|---|---|
| PCB design in KiCad | [02](02-pcb-design-using-kicad.md) | Screenshots + workflow concepts |
| Fabrication workflow | [03](03-pcb-fabrication-workflow.md) | Concepts only (no outputs supplied) |
| Analog fundamentals | [04](04-analog-electronics-fundamentals.md) | Concepts |
| Power electronics | [05](05-power-electronics-learning.md) | Schematic + PCB screenshots |
| R-2R ladder DAC | [06](06-r2r-ladder-dac.md) | Diagram + hardware photo |
| Through-hole soldering | [07](07-through-hole-soldering.md) | Concepts + hardware photo |
| Takeaways and roadmap | [08](08-key-takeaways-and-future-learning.md) | Reflection |

## What this repository does not contain

- No KiCad project files (`.kicad_sch`, `.kicad_pcb`), Gerbers, drill files or BOM were supplied, so none are included.
- No measurements, oscilloscope captures, simulation results or test reports were supplied, so none are claimed.
- No fabricated board photographs were supplied; the KiCad 3D view is a rendering, not a physical board.
