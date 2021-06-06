# Planning

This document covers the initial brainstorming and planning phase of his project.

## Potential Workflow for Provisioning Process

1.  Set Netbooting as primary option on the physical device (using BIOS/MBR/Legacy).
2.  Device attempts to netboot and makes a DHCP request.
3.  DHCPD server provides the "nextserver"/TFTP server to the device.
4.  Device attempts to get boot loader over TFTP.
5.  TFTPD server provides lpxelinux.0 (syslinux) boot loader.
6.  lpxelinux.0 will attempt to chainload to iPXE over HTTP/HTTPS.
7.  HTTPD server provides iPXE.
8.  iPXE requests kernel over HTTP/HTTPS.
9.  HTTPD server provides OS kernel for "prospector".
10. Kernel loads OS into RAM (Alpine or custom image based on Ubuntu/RedHat/CentOS/Rocky).
11. OS runs scripts to gather data on the system, and potentially configure some things (wipe MBR, etc.).
12. Gathered system data is sent to Substruct server to "discover" the connected system.
13. Admin sets options to tie a profile to the system (unique key could be serial number or UUID?).
14. Substruct generates kickstart/preseed file based on profile configuration.
15. Substruct updates DHCPD or boot loaders to point to desired OS image.
12. Desired OS is loaded with parameters that point to the kickstart/preseed file over HTTP/HTTPS.
13. HTTPD server provides kickstart/preseed file and provisioning starts on device.
14. Provisioning completes and post steps runs a script to ping Substruct the completed status.
15. Substruct either removes the nextserver entry on DHCPD or updates the boot loader to boot locally first.

## Exploring the Boot Pipeline

The boot process pipeline starts with netbooting.

| CLIENT                                | <-->   | SERVER                     |
| :---                                  | :---:  | ---:                       |
| Netboot with PXE                      | -->    | DHCP gives TFTP server     |
| lpxelinux.0                           | <--    | TFTP server                |
| chainload iPXE                        | -->    | HTTP(S) server             |
| OS kernel                             | <--    | HTTP(S) gives kernel       |
| Preseed/Kickstart                     | <--    | HTTP(S) provides file      |
| Post Provision Data                   | -->    | HTTP(S) API endpoints      |
| Netboot fails and GRUB loads locally  | <--    | DHCPD conf updated for MAC |
| iPXE/lpxelinux.0 set to boot locally  | <--    | (alt.) set local boot      |