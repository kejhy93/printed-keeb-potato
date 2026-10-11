# printed-keeb-potato 🥔⌨️

A fully custom mechanical keyboard I'm designing, modeling and 3D printing myself,
from the first layout sketch to the board I type on every day.

The layout is designed with [Ergogen](https://ergogen.xyz), a tool that builds a
keyboard from a YAML config: key positions, plate and case outlines, and even a PCB.

## Goals

- Design a keyboard layout that fits my hands and workflow
- Model the case, plate and keycaps (optional) in 3D
- Print it on a hobby FDM printer
- Wire, flash and use it as my daily driver

## Current Spec (Decided)

| Aspect | Decision |
|--------|----------|
| **Form factor** | Split, wired (TRRS + JST-SH for I2C) |
| **Key count** | 34 keys (5×3 + 2 thumb left, 5×3 + 1 thumb right) + Ploopy Nano trackball |
| **Switches** | Kailh Choc v2 Silent Linear (low-profile, 11.5mm, quietest) |
| **Keycaps** | MBK (Choc profile) — buy, don't print |
| **Controller** | KB2040 (RP2040, USB-C) ×2 |
| **Firmware** | ZMK (wired, no-compile keymap changes via GitHub Actions) |
| **Interconnect** | TRRS (matrix) + JST-SH 4-pin (I2C for trackball) |
| **Trackball** | Ploopy Nano trackball (PAW3395, 25mm ball, 3 buttons) |
| **Case mount** | Gasket mount — TPU 95A gaskets between plate and bottom case |
| **Tenting** | Fixed 8° built into bottom case (no adjustable pucks) |
| **Plate** | 1.6mm PETG/PC, separate from case |
| **Bottom case** | PETG/ASA, 8mm tall, 2mm walls, heat-set M2 inserts |
| **Ball/socket** | 608ZZ bearing in printed socket, 24mm printed ball (sanded to 25mm) |
| **Noise reduction** | Silent switches + gasket mount + Poron foam (optional) + TPU feet |

## Status

🚧 Early stage: layout finalized, generating starter files.

## Roadmap

- [x] Layout design in Ergogen (34-key + trackball, column stagger, thumb angles)
- [x] Choose switches, controller, firmware (Choc v2 Silent, KB2040, ZMK)
- [x] Trackball integration (Ploopy Nano module, I2C routing, case cutouts)
- [ ] Generate Ergogen output → inspect PCB/case
- [ ] Print switch plate test (4 keys) → verify Choc v2 fit
- [ ] Order PCBs (JLCPCB from Ergogen gerbers)
- [ ] Print case parts (case, plate, gasket, socket, ball, feet)
- [ ] Solder right half (trackball side) first
- [ ] Solder left half
- [ ] Flash ZMK (GitHub Actions workflow)
- [ ] Assemble: plate → switches → gasket → case → screws → trackball
- [ ] Connect JST-SH cable between halves
- [ ] Test & iterate keymap

## Repository Structure

| Folder | Contents |
|--------|----------|
| `layout/` | Ergogen config (`keyboard.yaml`) |
| `cad/` | CadQuery source models (`case.py`) |
| `stl/` | Exported printable files (case, plate, gasket, socket, ball, feet) |
| `firmware/` | ZMK config (`keymap.keymap`, shield overlay, GitHub Actions) |
| `pcb/` | KiCad project (optional, for manual edits) |
| `docs/` | Build log, photos, notes |

## Key Files (Generated)

- `layout/keyboard.yaml` — Ergogen config: points, matrix, footprints, MCU pins, connectors, case params
- `cad/case.py` — Parametric CadQuery: case, plate, gasket, trackball socket, ball, feet
- `firmware/keymap.keymap` — ZMK keymap: 5 layers, home-row mods, combos, trackball behavior
- `firmware/boards/shields/potato/potato.overlay` — ZMK devicetree: I2C, trackball config
- `firmware/.github/workflows/zmk.yml` — GitHub Actions: builds `.uf2` on push

## Build Commands

```bash
# Generate PCB + case from Ergogen
cd layout
pip install ergogen
ergogen keyboard.yaml -o ../output

# Export STL from CadQuery
cd ../cad
pip install cadquery cq-cli
cq-cli case.py --output ../stl/case_left.stl  --param side:left  --param part:case
cq-cli case.py --output ../stl/case_right.stl --param side:right --param part:case
cq-cli case.py --output ../stl/plate_left.stl  --param side:left  --param part:plate
cq-cli case.py --output ../stl/plate_right.stl --param side:right --param part:plate
cq-cli case.py --output ../stl/gasket_left.stl  --param side:left  --param part:gasket
cq-cli case.py --output ../stl/gasket_right.stl --param side:right --param part:gasket
cq-cli case.py --output ../stl/socket.stl --param part:socket
cq-cli case.py --output ../stl/ball.stl --param part:ball
cq-cli case.py --output ../stl/feet_left.stl  --param side:left  --param part:feet
cq-cli case.py --output ../stl/feet_right.stl --param side:right --param part:feet

# Flash firmware (after GitHub Actions builds)
# 1. Put KB2040 in bootloader (double-tap reset)
# 2. Drag build/zephyr/zmk.uf2 to RPI-RP2 drive
# 3. Repeat for other half
```

## BOM (Per Half + Trackball)

| Item | Qty | Left | Right | Notes |
|------|-----|------|-------|-------|
| KB2040 | 2 | 1 | 1 | Adafruit / Keebio |
| Kailh Choc v2 Silent Linear | 34 | 17 | 17 | +3 spares |
| Kailh Choc hotswap PG1350 | 34 | 17 | 17 | |
| 1N4148W SOD-123 | 34 | 17 | 17 | |
| TRRS PJ-320A | 2 | 1 | 1 | |
| Reset button 6×6×5mm | 2 | 1 | 1 | |
| M2 heat-set inserts | 8 | 4 | 4 | |
| M2×6 screws | 8 | 4 | 4 | |
| **Ploopy Nano Trackball (assembled)** | **1** | **0** | **1** | ~€40 |
| **608ZZ Bearing** | **1** | **0** | **1** | For ball socket |
| **24mm Ball (printed)** | **1** | **0** | **1** | Print solid PETG, sand to 25mm |
| JST-SH 4-pin SMD | 2 | 1 | 1 | Inter-half I2C |
| JST-SH 4-pin cable 10cm | 1 | - | - | Connect halves |
| 4.7kΩ 0603 resistors | 2 | 0 | 2 | I2C pull-ups (right half) |

## Print Settings (FDM)

| Part | Material | Layer | Walls | Infill | Notes |
|------|----------|-------|-------|--------|-------|
| Case | PETG/ASA | 0.2mm | 3 | 25% gyroid | Heat-set inserts |
| Plate | PETG/PC | 0.15mm | 4 | 100% | Rigid, no flex |
| Gasket | TPU 95A | 0.2mm | 3 | 30% gyroid | Compliant |
| Socket | PETG/ASA | 0.15mm | 4 | 100% | Bearing press-fit |
| Ball | PETG/ASA | 0.1mm | 5 | 100% | Print 24mm, sand to 25mm |
| Feet | TPU 95A | 0.2mm | - | Solid | Press-fit |

## License

[MIT](LICENSE) © 2026 Lukas Hejnal. This covers everything in this repo, including the 3D models, layout files and firmware.