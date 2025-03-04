# PXE Booting

This is a repo for all the various configs needed for the CTA PXE booting environment

## Prequisites

- `isc-dhcpd`
- `tfptd-hpa`
- `busybox`

## Services outline

- `isc-dhcpd` is set up to act as a DHCP server, pointing iPXE clients at http://10.0.0.1/menu.ipxe, else serve up ipxe.efi to chain load and then end up loading menu.ipxe.
- `tfptd-hpa` listens on :69 to serve ipxe.efi.
- `ipxe.efi` is an EFI executable from the iPXE.org project.
- `busybox` acts as an httpd server on :80 serving up other files required (eg. menu.ipxe).

## Boot process

1. Device tries to boot over the next work, `isc-dhcpd` hands out an IP and points devices to ipxe.efi.
2. Devices chain load ipxe.efi after downloading via `tfptd-hpa`. 
3. `isc-dhcpd` then points devices at http://10.0.0.1/menu.ipxe.
4. `busybox` serves http://10.0.0.1/menu.ipxe and devices execute the simple script, presenting a menu to the user.
5. User selects an OS to boot. The relavent sections point to files served by `busybox`. For example:
```ipxe
:hardwareOS
kernel http://${next-server}/HardwareOS/bzImage lftp_user="netboot-log" lftp_pass="ThreeInOne!" 
boot
```
6. iPXE then loads the files and proceeds to boot them.

## Setup

TBC
