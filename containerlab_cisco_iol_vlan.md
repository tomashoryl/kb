# ContainerLab - Cisco IOL - VLAN persistence

By default Cisco IOL keeps VLANs in `vlan.dat` file which does not persist across reboots. That way, if using VLAN interfaces they will be always down until VLANs are recreated after reboot. The clean solution is to switch VTP into transparent mode and define the VLANs in the startup config.

```
vtp mode transparent
!
vlan 10,20,30
```
