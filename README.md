# homeassistant
Documentation for setting up homeassistant on SBC (rPi 5) and KVM

## Hardware used in this setup

- SBC -> Raspberry Pi 5 with 16 GB RAM
- Pimoroni NVMe Base
- NVMe SSD with 500GB (ADATA, that followed the Pimoroni purchase)
- WaveShare Poe+ (G) board
- Raspberry Pi 5 SKU Cooler

## Software used in this setup

- Ubuntu Server 24.04.03 LTS
- Debian/Ubuntu specific packages for virtualization
   -  qemu-kvm
   -  libvirt-daemon-system
   -  libvirt-clients
   -  virtinst
   -  bridge-utils
- Home Assistant kvm
  - https://github.com/home-assistant/operating-system/releases/download/17.1/haos_generic-aarch64-17.1.qcow2.xz
