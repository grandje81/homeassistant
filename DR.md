# DR
Unfinished chapter

## Backup

```
virsh shutdown homeassistant
virsh dumpxml homeassistant > /backup/homeassistant.xml
tar czf /backup/homeassistant.qcow2.tgz /var/lib/libvirt/images/hassos/haos_ova-16.2.qcow2
sudo cp /etc/netplan/*.yaml /backup/
sudo cp -r /etc/libvirt /backup/libvirt-config
rsync -avz /backup/ user@nas:/srv/backup/ha-server/
```
## Restore

```
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst ovmf
sudo cp /backup/*.yaml /etc/netplan/
sudo netplan apply
sudo cp -r /backup/libvirt-config/* /etc/libvirt/
sudo cp /backup/homeassistant.qcow2 /var/lib/libvirt/images/hassos/
virsh define /backup/homeassistant.xml
virsh start homeassistant
```
