# Planning

This document covers the initial brainstorming and planning phase of his project.

## Exploring the Boot Pipeline

The boot process pipeline starts with netbooting.

| CLIENT               | <-->   | SERVER                    |
| :---                 | :---:  | ---:                      |
| Netboot with PXE     | -->    | DHCP gives TFTP server    |
| pxelinux boot loader | <--    | TFTP server               |