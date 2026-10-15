# PDB Hardware Files

KiCad hardware design files for the **Power Distribution Board (PDB)**. The board takes power from the battery and distributes it to the system's loads. It also has an interface connector to the Raspberry Pi / STM32 controller.

> **Status: work in progress.** Components are placed in the schematic, but wiring hasn't started and the PCB layout hasn't begun.

## Repository contents

| File / folder | Description |
|---|---|
| `PDC_new.kicad_pro` | KiCad project file (open this one) |
| `PDC_new.kicad_sch` | Schematic |
| `PDC_new.kicad_pcb` | PCB layout |
| `fp-lib-table` | Project footprint library table |
| `pdc_new.pretty/` | Custom project footprints (e.g. TPSI3050-Q1 `SOIC-8_DWZ_7.5x5.85mm_P1.27mm`) |

## Key components

| Ref | Part | Purpose |
|---|---|---|
| U1 | [TI TPSI3050-Q1](https://www.ti.com/product/TPSI3050-Q1) | Isolated switch driver with integrated isolated bias supply, used to drive the power switch. The datasheet is embedded in the schematic. |
| C1–C4 | Polarized capacitors | Bulk / decoupling capacitance |
| Bat | 2-pin connector | Battery input |
| To_pi/stm32 | 2-pin connector | Interface to the Raspberry Pi / STM32 controller |
| J1–J10 | 2-pin connectors | Power output / distribution connectors |

## Getting started

### Requirements
- [KiCad](https://www.kicad.org/download/) **10.0** or newer. The files are saved in KiCad 10 format and won't open in older versions.

### Opening the project
```bash
git clone https://github.com/ailakshmeesha01-coder/PDB_HW_FILES.git
```
Then open `PDC_new.kicad_pro` in KiCad. The custom footprint library loads automatically from `fp-lib-table` through `${KIPRJMOD}`, so you don't need to set up any libraries yourself.

## Contributing

- Close KiCad (or save everything) before committing, so you don't commit a half-saved state.
- Commit the `.kicad_pro`, `.kicad_sch`, `.kicad_pcb` and footprint files only. Lock files, backups, local history, `*.kicad_prl` and fabrication outputs are excluded by `.gitignore`.
- Put new custom footprints in `pdc_new.pretty/` so the project stays self-contained.
- Use clear commit messages, e.g. `sch: wire TPSI3050 enable path` or `pcb: route battery input`.

## Roadmap

- [ ] Complete schematic wiring and run ERC
- [ ] Assign footprints to all connectors and capacitors
- [ ] PCB layout, routing and DRC
- [ ] Generate BOM and fabrication outputs (Gerbers, drill files)
