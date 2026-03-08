# Filesystem setup

Depending on setup of LVM or other filesystems there are a bunch of commands
Consult the manual. 
The goal is the have the entire disk dedicated to Home Assistant in case it should need to grow the disk image.

## Create KVM Storage pool

```
virsh pool-create-as --name my-storage-pool --type dir --target /var/lib/libvirt/images/hassos-vm
```

## Updating the owner and file access rights

Making sure everything is accessable by the right group and the right rights is applied, issue below command

```
sudo chown libvirt-qemu:kvm haos_ova-16.2.qcow2
sudo chmod 0644 haos_ova-16.2.qcow2
```
## Adding extra storage to Home Assistant disk image

To add some storage before starting the Home Assistant KVM, issue below command to add 32GB

```
sudo qemu-img resize haos_generic-aarch64-17.1.qcow2 +32G
```
