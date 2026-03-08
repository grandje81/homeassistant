# Network setup

Depending on the use case there can be diffrent configurations.
The default bridge in KVM is a NAT bridge which can make some trouble.
My take is to remove it and have my real firewall handle the traffic, instead of using ufw or ip-tables on the rPi

## rPi Network configuration
Below is an example with a bridge attached to vlan 50
The example is made for netplan

```
network:
  version: 2
  ethernets:
    eth0:
      optional: true
      dhcp4: false
  vlans:
   vlan50:
     id: 50
     link: eth0
  bridges:
      br101:
          interfaces: [ vlan50 ]
          dhcp4: yes
          dhcp6: no
          link-local: [ ]
          parameters:
            stp: true
            forward-delay: 4
```

If unsure about the outcome you can let netplan test the config first

```
sudo netplan --debug generate
```

If no issues were found, apply the config
```
sudo netplan apply
```

Check the network status

```
ip a show br0
ip route
```

And ping a suitable IP, for example your gateway on the specified vlan.

## KVM Network configuration
For adding the bridge created above we need to create an XML file that virsh can read.

```
<interface type='bridge' name='br101'>
  <mtu size='1500'/>
  <bridge stp='on' delay='400'>
    <interface type='ethernet' name='eth0'>
      <link speed='1000' state='up'/>
      <mac address='2c:cf:67:ed:6a:5f'/>
    </interface>
  </bridge>
</interface>
```

When its done add it to KVM with below command

```
sudo virsh net-define br101.xml
```

If everything went well, start it and set it to autostart.
```
sudo virsh net-start br101
sudo virsh net-autostart br101
```
