# substruct

An easy to deploy foundation for automating the provisioning 
of baremetal infrastructure via network booting, with a user-friendly web 
interface for managing OS configurations and tracking hardware inventory.

## Components 

The project consists of three major compontents:
- **substruct** which comprises three sub components:
    - **Database** for storing hardware and deployment information.
    - **API** for managing the information in the database.
    - **Web UI** for interacting with the API in a user-friendly way.
- **substruct-server** for network booting x86_64 and aarch64 hardware.
- **substruct-os** for gathering information about the hardware of a system.

## Goals

The ultimate goals of this project are to: 
- Automate the processes for provisioning and reprovisioning hardware.
- Streamline the manual actions required by an administrator to 
configure various operating system deployments.
- Keep track of hardware inventory, their deployed configuration, 
and their status.

## Requirements 

Ideally the only requirements are:

- A single server running Ubuntu 20.04 or RHEL 8.X.
- A single networking LAN interface (for server and clients).
- Any number of client hardware (x86_64 or aarch64) with 
local disks to provision onto.

The currently targeted hardware is my homelab inventory:

- HP T620 thin clients (x86_64) with M.2 SSDs.
- Raspberry Pi 4 (aarch64) with SATA SSDs connected via USB 3.0.

## Scope

This project is being implemented in a tiered scope.

The proposed tiers are:

### 1. Foundation

Research and determine the how best to leverage technologies
such as pxelinux, PXE, NFS, DHCP, and DNS.

Document the initial manual configuration of a Substruct server 
on both Ubuntu 20.04 and RHEL 8.

The Substruct server should provide everything necessary to 
implement a working PXE boot using a hard-coded preseed file for 
Ubuntu 20.04 and a kickstart file for RHEL 7 and 8.

Ideally the Substruct server should: 

- Act as the primary DHCP and DNS authority for the LAN.
- Include an NFS server for hosting OS base images.
- Include a web server for the management interface to be added later.
- Clone both Ubuntu apt and RHEL yum/dnf repositories, 
to reduce bandwidth and latency when provisioning.

### 2. Framework

Create bash scripts to configure the Substruct server backend 
on Ubuntu 20.04 and RHEL 8.

Convert the bash scripts to Ansible playbooks.

### 3. TBD

This step and further steps will largely depend on the tech stack
determined for the Substruct backend in step 1.
