# Installing the disk image

If the storage and network is already set its time to create the virtual machine and link the the disk image.

```
sudo virt-install --name haos --description "Home Assistant OS" --os-variant=generic --ram=8192 --vcpus=2 --disk haos_generic-aarch64-17.1.qcow2,bus=scsi --controller type=scsi,model=virtio-scsi --import --graphics none \
             --noautoconsole \
             --network network=br101,model=virtio,mac=52:54:00:fa:72:01  \
             --boot uefi,firmware.feature0.name=enrolled-keys,firmware.feature0.enabled=no,firmware.feature1.name=secure-boot,firmware.feature1.enabled=no
```
The important thing here is to make surr you didnt copy the MAC from the actual bridge interface of the host machine.

**Make it unique**

Set the image to autostart with OS
```
sudo virsh autostart haos
```

## Attach USB device

To attach USB device directly from HOST to VM you need to specify vendorid and productid on the specific USB to make it work.
The specification is saved in an XML file.

To find out the information needed, issue below command
```
lsusb
```

If you want vendorid and productid separated use below command
```
lsusb -v | grep 'id'
```

When you have found your device create an XML file like the one below

```
<hostdev mode='subsystem' type='usb' managed='yes'>
        <source>
                <vendor id='0x10c4' />
                <product id='0xea60' />
        </source>
</hostdev>
```

Then issue below command to attach

```
virsh attach-device haos --file /var/lib/libvirt/images/hassos-vm/myUSBDevice.xml --persistent
```

If you have two USB-devices from the same Vendor and its the same product you need to specify the bus number and device number instead of vendorid/productid

```
<hostdev mode='subsystem' type='usb' managed='yes'>
      <source>
        <address bus='3' device='2'/>
      </source
</hostdev>
<hostdev mode='subsystem' type='usb' managed='yes'>
      <source>
        <address bus='2' device='5'/>
      </source>
</hostdev>
```
