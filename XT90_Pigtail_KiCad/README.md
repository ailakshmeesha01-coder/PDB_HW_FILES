# XT90(2+2) female pigtail – KiCad library

Part on the cable: **Amass XT90(2+2)-F.G.Y** (female, LCSC C3040693).
The board only has solder pads for the pigtail wires; the connector itself is not board-mounted.

## Files
- `XT90_Pigtail.kicad_sym` – symbol `XT90_2p2_F_Pigtail` (pins: 1 B+, 2 B-, 3 S1, 4 S2; footprint pre-assigned)
- `XT90_Pigtail.pretty/XT90_2p2_F_Pigtail_10AWG_22AWG.kicad_mod` – footprint
- `footprint_preview.png` – dimensioned top view

## Install (KiCad 8)
1. Preferences → Manage Symbol Libraries → add `XT90_Pigtail.kicad_sym` (nickname `XT90_Pigtail`).
2. Preferences → Manage Footprint Libraries → add folder `XT90_Pigtail.pretty` (nickname **`XT90_Pigtail`** – must match, the symbol references `XT90_Pigtail:...`).

## Footprint data (mm)
| Item | Pad | Drill | Notes |
|---|---|---|---|
| B+ (pin 1, square) / B- (pin 2) | Ø6.0 | Ø3.2 PTH | 10 AWG stranded, tinned. 12.0 pitch. Solid zone connection (no thermal spokes). |
| S1 / S2 (pins 3, 4) | Ø2.0 | Ø1.0 PTH | 22 AWG stranded. 3.0 pitch, 7.0 from B-. |
| Strain relief, power | – | Ø5.5 NPTH | 9.0 behind each power pad. Fits 10 AWG silicone (OD ~4.5–5 mm). |
| Strain relief, signal | – | Ø2.0 NPTH | 5.0 behind each signal pad. |

Wiring method: pass each insulated wire up through its NPTH relief hole from the bottom, bend it over, push the stripped end down into its plated pad, solder on the bottom side.

## BOM / assembly
- Symbol fields: `MPN = XT90(2+2)-F.G.Y`, `LCSC = C3040693`, `Assembly = DNP by PCBA – hand-solder pigtail after assembly`.
- Footprint is marked *exclude from position files*, so a PCBA service won't try to place it.
- Before ordering, check your actual wire's conductor and insulation diameters; if you change gauge, the drills above must change.
- For ~30 A: use 2 oz copper and wide copper pours to B+/B-.
