# PYNQ 3.1.1 SD card preparation

## Official AUP-ZU3 8 GB image

URL: `https://download.amd.com/opendownload/pynq/AUP-ZU3-3.1.1-8gb.zip`

Sizes: ~2.1 GB compressed / ~9.1 GB decompressed.

## Download

```bash
cd /tools2/AUPZU3-DPU_PYNQ/pynq-image-cache
wget https://download.amd.com/opendownload/pynq/AUP-ZU3-3.1.1-8gb.zip
unzip AUP-ZU3-3.1.1-8gb.zip
# an AUP-ZU3-3.1.1-8gb.img (~9.1 GB) should appear
```

## Flashing to microSD

microSD of **16 GB or more** required (per official documentation).

Either `dd` or Balena Etcher will produce an identical result. The
official documentation recommends Etcher; we used `dd` here.

**Identify the correct device first:**
```bash
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,MODEL
```

Find the microSD (should be `/dev/sdX`, NOT the system disk).

```bash
# CAREFULLY. Replace sdX with the correct device.
sudo umount /dev/sdX*
sudo dd if=AUP-ZU3-3.1.1-8gb.img of=/dev/sdX bs=4M status=progress conv=fsync
sync
```

Takes ~10-15 min depending on SD speed.

## Boot on the board

1. Insert the SD card into the AUP-ZU3 microSD socket.
2. **BOOT switch in "SD" position**.
3. Connect the USB-C "EXT PWR" cable.
4. (For UART access) connect the USB-C "PROG UART" cable.
5. Power on.

After ~15-60 s the board has booted.

## Access

### Via USB Ethernet adapter on USB3 port

Plug a USB Ethernet adapter into one of the board's USB3-A ports and
connect an Ethernet cable from there to a router. The board picks up
an IP via DHCP.

To find the IP: open a serial terminal (`sudo picocom -b 115200
/dev/ttyUSB1`) and run `ip a show eth0`.

```bash
ssh xilinx@<board-IP>      # password: xilinx
```

### Via Ethernet Gadget (USB-C to USB-C between host and board)

If the host has a USB-C port and you connect host-to-board with a
USB-C-to-USB-C cable, a USB Ethernet Gadget interface appears on the
host and the board is reachable at `192.168.3.1`. Untested in our
setup; we used the USB Ethernet adapter route.

### Jupyter Lab

```
http://<board-IP>/lab
```
Password: `xilinx`.

## Quick verification

From a fresh notebook in Jupyter (or via SSH):

```python
import pynq
print(pynq.__version__)              # expected: 3.1
```

```bash
cat /etc/os-release | head -3        # expected: PynqLinux 3.0 (Carlisle)
df -h /                              # expected: ext4 ~116 GB mounted on /
```

If these succeed, **v0.1-pynq-vanilla is met**.

## Clean shutdown

**Never** power off via the switch while the board is running.
Always:

```bash
sudo poweroff
```

Wait for the prompt to disappear from UART before flipping the
power switch.
