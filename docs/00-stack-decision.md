# Stack decision — June 2026

## Why this path

After multiple attempts using a custom PetaLinux + DPU integrated in
the Vivado Block Design (based on the Hackster tutorial for the
MYD-CZU3EG), it became clear that the tutorial's `zynq_ultra_ps_e_0`
preset is wired for a different board and configures the DDR
controller, UART MIOs and SD MIOs with values that do not match the
AUP-ZU3. Observed symptom: PS LEDs blink briefly on power-on and the
UART stays completely silent (FSBL hangs before initialising the UART
on the correct pins).

Comparison documented in `01-psu-mismatch.md`.

## Chosen stack

| Component | Version | Why |
|---|---|---|
| PYNQ image AUP-ZU3 | 3.1.1 (official Real Digital) | The only official image for the board; PSU init guaranteed correct |
| DPU-PYNQ | `design_contest_3.5` branch | Supports Vitis-AI 3.5 and DPUCZDX8G v4.1 |
| Vitis-AI | 3.5 | Compatible with DPU v4.1 |
| Vivado / Vitis | 2024.1 | The version Real Digital uses to build the board file |
| DPU architecture | B1600 | Fits in ZU3EG without saturating DSPs |

## Known risks

- **DPU-PYNQ declares support for PYNQ 3.0**, not 3.1.1 explicitly.
  AUP-ZU3 only has a 3.1.1 image so we will try on 3.1.1 first. If it
  breaks, alternatives will be evaluated.
- **No public project documented** that combines AUP-ZU3 + DPU-PYNQ;
  we start from the Ultra96v2 template (same ZU3EG silicon).

## Fallback plan

If the path fails before day 14, fall back to the previously working
FINN+PYNQ implementation.

## Sources consulted

- AUP-ZU3 reference manual (ZU3_RM_A1-1.pdf)
- Real Digital board files: `aup-zu3-bsp-master/board-files/aup-zu3-8gb/`
- https://xilinx.github.io/AUP-ZU3/
- https://github.com/Xilinx/DPU-PYNQ (archived)
- http://www.pynq.io/boards.html
