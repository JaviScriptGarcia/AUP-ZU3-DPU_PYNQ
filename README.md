# AUPZU3-DPU_PYNQ

Implementation of a CNN inference accelerator for computer vision on the
**Real Digital AUP-ZU3** board (XCZU3EG, 8 GB DDR4), using **PYNQ 3.1**
as the operating system and Xilinx's **DPUCZDX8G v4.1** as the
accelerator, with model compilation via **Vitis-AI 3.5**.

Master's thesis (TFM).

## Hardware

- Real Digital AUP-ZU3 (8 GB variant)
- ZynqMP XCZU3EG-SFVC784-2-e
- 8 GB DDR4-2400T on 32-bit bus
- microSD socket (SD0, MIO 13..16, 21, 22; CD MIO 24)
- USB-UART FTDI (UART1, MIO 32..33)

## Software / firmware stack

- **PYNQ 3.1.1** — official image from AMD / Real Digital
- **DPU-PYNQ** branch `design_contest_3.5` — runtime integration
- **Vivado 2024.1** + **Vitis 2024.1** — hardware synthesis
- **Vitis-AI 3.5** — model compilation
- **XRT** — runtime that connects the PS with Vitis kernels

## Instantiated DPU

- `DPUCZDX8G v4.1`
- Architecture: **B1600** (only one that fits in ZU3EG without saturating DSPs)
- 1 core, clk 275 MHz, clk_dsp 550 MHz

## Repository layout

```
.
├── docs/                 Per-phase documentation (Markdown)
├── pynq-image/           Scripts and notes to prepare the SD with PYNQ
├── overlay/              DPU overlay build (adapted DPU-PYNQ)
├── models/               Compiled models (.xmodel) for this DPU
├── notebooks/            Validation and inference Jupyter notebooks
├── results/              Metrics, benchmarks, comparisons
├── build/                Vivado/Vitis outputs (gitignored)
└── pynq-image-cache/     Downloaded PYNQ image (gitignored)
```

## Releases (git tags)

| Tag | Contents | Status |
|---|---|---|
| `v0.1-pynq-vanilla` | PYNQ 3.1 booting + base overlay + SSH | ✅ done |
| `v0.2-dpu-overlay`  | DPU overlay loadable from notebook     | pending |
| `v0.3-dummy-model`  | Dummy inference with trivial xmodel    | pending |
| `v0.4-mobilenet`    | MobileNet from Model Zoo running       | pending |
| `v1.0-tfm`          | Full thesis + benchmarks               | pending |

## Physical location

The repository lives at `/tools2/AUPZU3-DPU_PYNQ/` for disk-space
reasons. A symlink at `~/Documents/AUPZU3-DPU_PYNQ` provides access
from the usual path.
