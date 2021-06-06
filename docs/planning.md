# Planning

This document covers the initial brainstorming and planning phase of his project.

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