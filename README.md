# PDB Hardware Files

KiCad hardware design files for the **NEXUS Power Distribution Board (PDB) V2.0**. The board takes power from a 12S battery (50.4 V max) and distributes it to the humanoid's actuators through 12 XT60 outputs. It precharges the bus, discharges it when powered off, and has a charging input.

**Status:** schematic complete, PCB routed, Gerbers and BOM generated.

## Repository contents

| File / folder | Description |
|---|---|
| `PDB_2.0.kicad_pro` | KiCad project file (open this one) |
| `PDB_2.0.kicad_sch` | Schematic |
| `PDB_2.0.kicad_pcb` | PCB layout (2 layers, 4 oz copper) |
| `PDB_2.0_BOM.xlsx` | Bill of materials |
| `PDB_2.0 Gerber files.zip` | Gerber and drill files for fabrication |
| `NEXUS_PDB_V2_power_simulation.pdf` | Power copper simulation report (current and temperature) |
| `fp-lib-table`, `sym-lib-table` | Project footprint and symbol library tables |
| `Libraries/` | Symbols, footprints and 3D models used by the project |

## Work done

- **Schematic:** full wiring with ERC; precharge, discharge and charging circuits; precharge and charging indicator LEDs added; C21 increased to 1 µF for a reliable power-on reset.
- **PCB layout:** placement, routing and branding; power tracks widened; solid copper pours on the power nets.
- **Main current path:** fixed the via bottleneck between the top-layer MOSFETs and the bottom-layer pours. Added a solid BUS+ pour on F.Cu and more vias on the MOSFET drain and source sides; removed stacked duplicate vias.
- **Connectors:** fixed the XT30 footprint mismatch; added the QS8-S battery connector (J13) and XT90 footprints to `Libraries/`.
- **Assembly:** thermal relief on small SMD GND pads for easier hand soldering; large power pads stay solid.
- **Simulation:** checked the power copper at 100 A continuous and 300 A pulses (see the PDF).
- **Outputs:** Gerbers, 3D models and BOM generated.
- **Project:** files renamed from `PDC_new` to `PDB_2.0`; libraries moved into `Libraries/`.

## Simulation results (summary)

- **100 A continuous** works with a small fan (~1 m/s airflow): peak rise about +36 K. In still air the hottest spots reach about +65 K, which is too hot in a warm enclosure.
- **300 A pulses** of up to ~20 ms are fine (≤ +22 K extra). Avoid 300 A for longer than ~50 ms.
- Hottest areas: MOSFET tabs (U14 most), the J13 battery input, and a narrow BATT+ neck on the bottom layer.

## Open items

- **U4A run latch:** check that it is clocked from `TIMEOUT` with D = `PC_DONE`, so the board can't get stuck in precharge-only mode.
- **Fuses:** F1 and F4 must be rated for **≥ 58 V DC**. Size F1 to the real peak current.
- **Overvoltage:** set the ODrive regen limits and overvoltage trip (or add a brake chopper) to keep BUS+ at or below 50.4 V.
- **Cooling:** plan for airflow across the board if it will run near 100 A continuously.

## Getting started

Requires [KiCad](https://www.kicad.org/download/) **10.0** or newer.

```bash
git clone https://github.com/ailakshmeesha01-coder/PDB_HW_FILES.git
```

Open `PDB_2.0.kicad_pro` in KiCad. Libraries in `Libraries/` load automatically through `${KIPRJMOD}`.

## Contributing

- Save and close KiCad before committing.
- Lock files, backups, local history and `*.kicad_prl` are excluded by `.gitignore`.
- Put new symbols and footprints in `Libraries/`.
- Use clear commit messages, e.g. `sch: fix U4A latch` or `pcb: widen BATT+ neck`.
