sudo virsh net-start default
sudo virsh net-autostart default

wget https://github.com/home-assistant/operating-system/releases/download/16.2/haos_generic-aarch64-16.2.qcow2.xz
unxz haos_generic-aarch64-16.2.qcow2.xz  

sudo qemu-img resize haos_generic-aarch64-16.2.qcow2 +32G
sudo virt-install --name haos --description "Home Assistant OS" --os-variant=generic --ram=6144 --vcpus=2 --disk haos_generic-aarch64-16.2.qcow2,bus=scsi --controller type=scsi,model=virtio-scsi --import --graphics none --boot uefi \
             --noautoconsole \
             --network network=default,model=virtio,mac=52:54:00:fa:72:01  \
             --network direct,source=eth0,source_mode=bridge,model=virtio,mac=52:54:00:4a:19:02  \
			 --boot uefi,firmware.feature0.name=enrolled-keys,firmware.feature0.enabled=no,firmware.feature1.name=secure-boot,firmware.feature1.enabled=no
sudo virsh autostart haos
