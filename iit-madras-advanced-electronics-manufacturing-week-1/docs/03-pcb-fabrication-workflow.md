# 03 · PCB Fabrication Workflow

[← PCB Design](02-pcb-design-using-kicad.md) · [README](../README.md) · Next: [Analog Electronics →](04-analog-electronics-fundamentals.md)

This document describes the standard path from a finished KiCad design to a populated board. It is a **conceptual workflow**. No Gerber files, drill files, fabrication quotes, or photographs of a manufactured board were supplied, and none are claimed.

## End-to-end flow

```
Schematic → ERC → Footprints → PCB layout → DRC → 3D check
   → Plot Gerbers + drill → Inspect in Gerber viewer
   → Fabricator upload / DFM check → Fabrication
   → Incoming inspection → Assembly (soldering) → Bring-up and test
```

## 1. Design freeze and checks

Run ERC and DRC, review the 3D view ([doc 02](02-pcb-design-using-kicad.md)), and check each footprint against the datasheet. Keep a revision number on the silkscreen.

## 2. Manufacturing outputs

| Output | Purpose | Status here |
|---|---|---|
| Gerber layers (copper, mask, silkscreen, edge) | Define each layer image | Not supplied |
| Excellon drill file(s) | Hole positions and sizes (plated and non-plated) | Not supplied |
| Bill of materials (BOM) | Parts, values, quantities, references | Not supplied; values visible in the schematic screenshot |
| Pick-and-place / assembly drawing | Needed for machine assembly; optional for hand assembly | Not applicable |
| Fabrication notes | Layers, thickness, copper weight, finish, mask colour | Not supplied |

## 3. Verifying outputs

Open the Gerbers in an independent viewer. Check that the outline is closed, that the drill hits pad centres, that mask openings expose pads and that silkscreen does not cover pads. Many fabricators run their own DFM check as well.

## 4. Fabrication, in outline

1. Copper-clad laminate is drilled.
2. Holes are plated so layers connect.
3. Copper is patterned by imaging and etching.
4. Solder mask and silkscreen are applied.
5. Surface finish (e.g. HASL or ENIG) protects exposed copper.
6. Electrical test and routing/cutting to outline.

These are standard industry steps; I did not observe a fabrication line in the supplied material.

## 5. Incoming inspection

Compare the board against the layout: hole sizes, pad finish, scratches or shorts between close tracks, and board dimensions. A quick continuity check of supply and ground nets before populating avoids discovering shorts after assembly.

## 6. Assembly and bring-up

Through-hole parts are placed and soldered ([doc 07](07-through-hole-soldering.md)); lowest-profile parts first. Bring-up for a power circuit should use a current-limited supply and staged checks ([doc 05](05-power-electronics-learning.md)).

## Adding real fabrication files later

If actual outputs become available, add them under a `hardware/` folder, for example:

```
hardware/
  kicad/        (.kicad_pro, .kicad_sch, .kicad_pcb, custom libraries)
  gerbers/      (zipped Gerber + drill set, with a README listing layers)
  bom/          (CSV BOM)
```

Check first that you have the right to publish them. Do not publish files containing third-party or confidential content.

## Self-assessment

| Item | Level |
|---|---|
| Can explain what each output file is for | Yes (concept) |
| Have generated and inspected Gerbers myself | Not documented here; needs hands-on practice |
| Have ordered a board | Not documented |
