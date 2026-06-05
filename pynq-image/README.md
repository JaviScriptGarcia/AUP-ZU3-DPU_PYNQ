# Preparación de la SD con PYNQ 3.1.1

## Imagen oficial AUP-ZU3 8 GB

URL: `https://download.amd.com/opendownload/pynq/AUP-ZU3-3.1.1-8gb.zip`

Tamaño aproximado: 6 GB comprimido / ~16 GB descomprimido.

## Descarga

```bash
cd /tools2/AUPZU3-DPU_PYNQ/pynq-image-cache
wget https://download.amd.com/opendownload/pynq/AUP-ZU3-3.1.1-8gb.zip
unzip AUP-ZU3-3.1.1-8gb.zip
# debería aparecer un AUP-ZU3-3.1.1-8gb.img (o similar)
```

## Flasheo a microSD

Necesaria microSD de **16 GB o más**, según doc oficial.

Recomendado usar `dd` (Etcher también vale).

**Identifica primero el dispositivo correcto:**
```bash
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,MODEL
```

Busca la microSD (debería ser `/dev/sdX`, NO el disco del sistema).

```bash
# CON CUIDADO. Cambia sdX por el dispositivo correcto.
sudo umount /dev/sdX*    # por si hay particiones montadas
sudo dd if=AUP-ZU3-3.1.1-8gb.img of=/dev/sdX bs=4M status=progress conv=fsync
sync
```

Tarda ~10-20 min según velocidad de la SD.

## Boot en la placa

1. Insertar SD en el socket microSD de la AUP-ZU3.
2. **Switch BOOT en posición "SD"**.
3. Conectar USB-C "EXT PWR" (alimentación).
4. (Para acceso) conectar USB-C "PROG UART" al ordenador.
5. Encender.

Tras ~30-60 s la placa habrá arrancado.

## Acceso

### Vía Ethernet Gadget (USB-C "PROG UART")

La placa se presenta como interfaz Ethernet sobre USB.

- IP de la placa: `192.168.3.1`
- Jupyter Lab: `http://192.168.3.1/lab` — password: `xilinx`
- SSH: `ssh xilinx@192.168.3.1` — password: `xilinx`

### Vía red cableada (Ethernet RJ45) — opcional

La placa pide IP por DHCP. Para saber qué IP le ha dado el router,
mirar el dashboard del router o usar `nmap`.

## Verificación rápida

Desde un notebook nuevo en Jupyter:

```python
import pynq
print(pynq.__version__)              # esperado: 3.1.1
print(pynq.PL.bitfile_name)          # nombre del overlay base cargado

from pynq.overlays.base import BaseOverlay
overlay = BaseOverlay("base.bit")
print(overlay.ip_dict.keys())        # IPs disponibles en el overlay
```

Si esto no falla, **v0.1-pynq-vanilla** está cumplida.
