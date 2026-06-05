# AUPZU3-DPU_PYNQ

Implementación de un acelerador de inferencia para CNN de visión sobre la
placa **Real Digital AUP-ZU3** (XCZU3EG, 8 GB DDR4), usando **PYNQ 3.1.1**
como sistema operativo y la **DPUCZDX8G v4.1** de Xilinx como acelerador,
con compilación de modelos vía **Vitis-AI 3.5**.

Trabajo Fin de Máster.

## Hardware

- Real Digital AUP-ZU3 (variante 8 GB)
- ZynqMP XCZU3EG-SFVC784-2-e
- 8 GB DDR4-2400T en bus 32-bit
- microSD socket (SD0, MIO 13..16, 21, 22; CD MIO 24)
- UART-USB FTDI (UART1, MIO 32..33)

## Stack software/firmware

- **PYNQ 3.1.1** — imagen oficial de AMD/Real Digital
- **DPU-PYNQ** branch `design_contest_3.5` — integración runtime
- **Vivado 2024.1** + **Vitis 2024.1** — síntesis de hardware
- **Vitis-AI 3.5** — compilación de modelos
- **XRT** — runtime que conecta PS con kernels Vitis

## DPU instanciado

- `DPUCZDX8G v4.1`
- Arquitectura: **B1600** (única que cabe en ZU3EG sin saturar DSPs)
- 1 core, clk 275 MHz, clk_dsp 550 MHz

## Estructura del repositorio

```
.
├── docs/                 Documentación de cada fase (Markdown)
├── pynq-image/           Scripts y notas para preparar la SD con PYNQ
├── overlay/              Build del overlay DPU (DPU-PYNQ adaptado)
├── models/               Modelos compilados (.xmodel) para esta DPU
├── notebooks/            Jupyter notebooks de validación e inferencia
├── results/              Métricas, benchmarks, comparativas
├── build/                Outputs de Vivado/Vitis (gitignored)
└── pynq-image-cache/     Imagen PYNQ descargada (gitignored)
```

## Releases (tags git)

| Tag | Contenido | Estado |
|---|---|---|
| `v0.1-pynq-vanilla` | PYNQ 3.1.1 arrancando + base overlay + SSH | pendiente |
| `v0.2-dpu-overlay`  | Overlay DPU cargable desde notebook         | pendiente |
| `v0.3-dummy-model`  | Inferencia dummy con xmodel trivial         | pendiente |
| `v0.4-mobilenet`    | MobileNet del Model Zoo ejecutando          | pendiente |
| `v1.0-tfm`          | Memoria completa + benchmarks               | pendiente |

## Ubicación física

El repositorio vive en `/tools2/AUPZU3-DPU_PYNQ/` por motivos de espacio
en disco. Hay un symlink en `~/Documents/AUPZU3-DPU_PYNQ` para acceso
desde la ruta habitual.
