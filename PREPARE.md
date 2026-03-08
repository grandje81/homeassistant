# Fullfill the requirements

To start off a smooth ride in this setup, make sure to have the software listed prepared.

## Software used in this setup

- Ubuntu Server 24.04.03 LTS
- Debian/Ubuntu specific packages for virtualization
   -  qemu-kvm
   -  libvirt-daemon-system
   -  libvirt-clients
   -  virtinst
   -  bridge-utils
   -  ovmf
- Home Assistant QCOW2 disk image
   - https://github.com/home-assistant/operating-system/releases/download/17.1/haos_generic-aarch64-17.1.qcow2.xz
   - Newer or older version of Home Assistant is available under this URL:
      - https://github.com/home-assistant/operating-system/releases/tag/

## Make sure system is up to date

Issue update and upgrade command

```
sudo apt-get update && sudo apt-get upgrade -y
```

## Configure user accounts
Add the user that will be adding VMs to groups libvirt and kvm

```
sudo usermod -aG libvirt,kvm $USER
```
Where **$USER** is the logged in user.

## Check on installation of libvirtd

Make sure the installation of libvirtd has completed successfully by checking its systemd status

```
sudo systemctl status libvirtd
```

If its there but not running, start it by issueing below command

```
systemctl enable --now libvirtd
```

## Create directories for storing VMs

Make directory in the proper place and change **pwd**, then download the disk image.

```
sudo mkdir -vp /var/lib/libvirt/images/hassos-vm && cd /var/lib/libvirt/images/hassos-vm
sudo wget https://github.com/home-assistant/operating-system/releases/download/17.1/haos_generic-aarch64-17.1.qcow2.xz
```

Unpack the disk image

```
unxz haos_generic-aarch64-17.1.qcow2.xz
```

Now you start with storage configuration
