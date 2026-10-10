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

## Status

🚧 Early stage: planning and layout design.

## Roadmap

- [ ] Layout design in Ergogen (key count, split/unibody, stagger)
- [ ] Choose switches, controller and firmware (e.g. QMK / ZMK)
- [ ] 3D model of the case and plate
- [ ] Test prints and fit checks
- [ ] Wiring / PCB
- [ ] Firmware and keymap
- [ ] Final assembly
- [ ] Daily use and iteration

## Repository structure (planned)

| Folder      | Contents                               |
|-------------|----------------------------------------|
| `layout/`   | Ergogen config (`config.yaml`)         |
| `cad/`      | Source 3D models                       |
| `stl/`      | Exported printable files               |
| `firmware/` | Keymap and firmware config             |
| `docs/`     | Build log, photos, notes               |

## Build log

Progress notes and photos will go in `docs/`.

## License

[MIT](LICENSE) © 2026 Lukas Hejnal. This covers everything in this repo, including the 3D models, layout files and firmware.
