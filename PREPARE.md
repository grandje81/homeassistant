# Fullfill the requirements

To start off a smooth ride in this setup, make sure to have the software listed prepared.

## Software used in this setup

- Ubuntu Server 24.04.03 LTS for ARM64
- Debian/Ubuntu specific packages for virtualization
   -  qemu-system-arm
   -  libvirt-daemon-system
   -  libvirt-clients
   -  virtinst
   -  bridge-utils
   -  ovmf
- Home Assistant QCOW2 disk image
   - https://github.com/home-assistant/operating-system/releases/download/17.1/haos_generic-aarch64-17.1.qcow2.xz
   - Newer or older version of Home Assistant is available under this URL:
      - https://github.com/home-assistant/operating-system/releases/tag/

## Troubleshooting package install

With the rPi 4 i got some issues with qemu-system-arm installation. It was blocked because problems with package dependency could not be fulfilled.

The package for ACL needed libacl1 in a specific build but the install wanted to install the newest version
- Required: libacl1-2.3.2-1build1
- Blocked by: libaxl1-2.3.2-1build1.1

Solution was to make apt-get install the required version at the same time as the main package.

```
 sudo apt-get install acl libacl1=2.3.2-1build1
```

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
