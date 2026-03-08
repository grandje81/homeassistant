# Filesystem setup

Depending on setup of LVM or other filesystems there are a bunch of commands
Consult the manual. 
The goal is the have the entire disk dedicated to Home Assistant in case it should need to grow the disk image.

## Create KVM Storage pool

```
virsh pool-create-as --name my-storage-pool --type dir --target /var/lib/libvirt/images/hassos-vm
```
