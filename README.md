# PDB Hardware Files

KiCad hardware design files for the **NEXUS Power Distribution Board (PDB) V2.0**. The board takes power from the battery and distributes it to the humanoid's actuators. It precharges the bus, discharges it when powered off, and has a charging input.

> **Status: work in progress.** The schematic is complete and the PCB is routed (0 DRC errors). **Do not order boards yet:** the main-current via problem below is still open.

> [!WARNING]
> ## ⚠️ Open issue: the main battery current flows through vias
>
> **This limits the whole board to roughly 30–40 A, well below what the MOSFETs and connectors can carry. Fix it before fabrication.**
>
> **What's wrong:** the main MOSFETs (U6, U11, U12, U14) and their pads are on the **top layer (F.Cu)**, but the **BATT+** and **BUS+** copper pours are on the **bottom layer (B.Cu)**. Every amp of actuator current has to pass through vias to get between them:
>
> | Path | Vias per MOSFET | Via size |
> |---|---|---|
> | BATT+ pour → MOSFET drain (tab) | about 8–13 | 0.3 mm drill |
> | MOSFET source pins → BUS+ pour | about 5 | 0.3–0.35 mm drill |
>
> A small via carries only about 1–2 A before it heats up noticeably. The source side has the fewest vias, so it sets the limit of **about 30–40 A for the whole board**. The four MOSFETs together (about 0.43 mΩ) could carry far more.
>
> **How to fix it:**
> 1. Add a **BUS+ copper pour on F.Cu** around the MOSFET source pins, so the current can stay on the top layer.
> 2. Add **20 or more vias per MOSFET on each side** (drain and source), evenly spread. Use 0.4 mm drill or larger where they fit.
> 3. Optionally, leave the main current paths **without solder mask** so they can be thickened with solder.
> 4. Re-run DRC, then recheck with the expected peak actuator current.
>
> The 54 duplicate vias stacked under the MOSFET tabs added no current capacity, and have been removed.

## Other open items

- **Schematic, U4A run latch:** clock it from `TIMEOUT` with D = `PC_DONE`. Otherwise the board can get stuck in precharge-only mode if the bus is still charged at power-up.
- **Schematic, C21:** increase it from 47 nF to at least 1 µF so the power-on reset fires reliably.
- **Fuses:** F1 and F4 must be rated for **≥ 58 V DC**, since standard blade fuses are only 32 V DC. Size F1 to the real peak current.
- **Overvoltage:** nothing keeps BUS+ at or below 50.4 V for the ODrives during regen. Set the ODrive regen limits and overvoltage trip, or add a brake chopper.
- **Libraries:** the footprint libraries `QS8_S_Connectors` (J13) and `XT90PW(2+2)-M.G.Y` (J11) aren't in this repo yet. Please commit their `.pretty` folders.

## Repository contents

| File / folder | Description |
|---|---|
| `PDC_new (2)/` | **Current project.** Open `PDC_new (2).kicad_pro` |
| `PDC_new (2)/fp-lib-table` | Project footprint library table for the current project |
| `PDC_new.kicad_*` | Older version of the project |
| `pdc_new.pretty/`, `ul_*/`, part folders | Custom symbols and footprints |

## Getting started

### Requirements
- [KiCad](https://www.kicad.org/download/) **10.0** or newer. The files are saved in KiCad 10 format and won't open in older versions.

### Opening the project
```bash
git clone https://github.com/ailakshmeesha01-coder/PDB_HW_FILES.git
```
Then open `PDC_new (2)/PDC_new (2).kicad_pro` in KiCad. Custom footprint libraries load automatically from the project's `fp-lib-table` through `${KIPRJMOD}`.

## Contributing

- Close KiCad (or save everything) before committing, so you don't commit a half-saved state.
- Commit the `.kicad_pro`, `.kicad_sch`, `.kicad_pcb` and footprint files only. Lock files, backups, local history, `*.kicad_prl` and fabrication outputs are excluded by `.gitignore`.
- Use clear commit messages, e.g. `sch: wire TPSI3050 enable path` or `pcb: route battery input`.

## Roadmap

- [x] Complete schematic wiring and run ERC
- [x] Assign footprints
- [x] PCB layout, routing and DRC (0 errors)
- [ ] **Fix the main-current via problem (see the warning above)**
- [ ] Apply the open schematic fixes (U4A rewire, C21)
- [ ] Generate BOM and fabrication outputs (Gerbers, drill files)
