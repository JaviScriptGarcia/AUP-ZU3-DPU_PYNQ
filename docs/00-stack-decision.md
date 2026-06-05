# Decisión del stack — junio 2026

## Por qué este camino

Tras múltiples intentos con un PetaLinux propio + DPU integrado en el
Block Design de Vivado (basado en el tutorial Hackster para
MYD-CZU3EG), se evidenció que el preset del PS (`zynq_ultra_ps_e_0`)
del tutorial está hecho para otra placa y configura el controlador
DDR, MIO de UART y MIO de SD con valores que no encajan con la
AUP-ZU3. Síntoma observado: LEDs PS encendidos un instante al
power-on y silencio total por UART (FSBL se cuelga antes de
inicializar el UART en los pines correctos).

Comparativa documentada en `01-psu-mismatch.md`.

## Stack elegido

| Componente | Versión | Por qué |
|---|---|---|
| PYNQ image AUP-ZU3 | 3.1.1 (oficial Real Digital) | Es la única imagen oficial para la placa; PSU init garantizado correcto |
| DPU-PYNQ | branch `design_contest_3.5` | Soporta Vitis-AI 3.5 y DPUCZDX8G v4.1 |
| Vitis-AI | 3.5 | Compatible con DPU v4.1 |
| Vivado / Vitis | 2024.1 | Versión que Real Digital usa para construir el board file |
| DPU arch | B1600 | Cabe en ZU3EG sin saturar DSPs |

## Riesgos conocidos

- **DPU-PYNQ declara soporte para PYNQ 3.0**, no 3.1.1 explícitamente.
  La AUP-ZU3 solo tiene imagen 3.1.1, así que probaremos sobre 3.1.1.
  Si rompe, evaluaremos alternativas.
- **Ningún proyecto público documentado** combina AUP-ZU3 + DPU-PYNQ;
  partimos de la plantilla Ultra96v2 (mismo silicio ZU3EG).

## Plan B

Si el camino falla antes del día 14, vuelta a la implementación previa
FINN+PYNQ ya funcionante.

## Fuentes consultadas

- Manual oficial AUP-ZU3 (ZU3_RM_A1-1.pdf)
- Board file Real Digital: `aup-zu3-bsp-master/board-files/aup-zu3-8gb/`
- https://xilinx.github.io/AUP-ZU3/
- https://github.com/Xilinx/DPU-PYNQ (archived)
- http://www.pynq.io/boards.html
