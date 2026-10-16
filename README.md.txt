# Linux Security Hardening Lab

## Overview

This project documents basic Linux security hardening performed on a Linux Mint 22.3 virtual machine running in VirtualBox. The goal was to reduce unnecessary network exposure, improve file permissions, configure a host firewall, and verify the resulting system state.

## Environment

- **Operating System:** Linux Mint 22.3
- **Virtualization:** Oracle VirtualBox
- **Firewall:** UFW
- **Service management:** systemd

## Tasks Performed

### 1. Firewall Configuration

Enabled UFW and configured the default firewall policies:

- Deny incoming connections by default
- Allow outgoing connections
- Disable routed traffic
- Enable low-level firewall logging

Verified the configuration using `sudo ufw status verbose`.

### 2. Service Hardening

Inspected listening network sockets using `ss -tulpn` and identified Avahi, a service used for local network discovery.

Disabled both the Avahi daemon and its socket activation unit. Verified that both units were disabled and inactive and that the Avahi listening sockets disappeared.

### 3. File Permission Hardening

Reviewed file permissions in my home directory and identified files requiring restricted access.

- Restricted packet capture files to owner-only permissions (`600`).
- Changed ownership of the packet captures to my user account.
- Restricted the lab directories to owner-only access (`700`).
- Changed an update metadata file that had world-writable permissions (`666`) to owner-only access (`600`).

Also inspected system files with SUID permissions without modifying standard system binaries.

### 4. System Updates and Verification

Updated the system packages and performed post-update checks.

- `sudo dpkg --audit` returned no output.
- `sudo apt-get check` completed without dependency errors.
- Verified that UFW remained active.
- Verified that both Avahi units remained disabled and inactive.

## Verification Evidence

The included terminal screenshot shows the final verification results for:

- Operating system version
- Firewall status and default policies
- Avahi service and socket states
- Package audit and dependency checks

## Known Limitation

During the system upgrade, the VirtualBox Guest Additions video component (`vboxvideo`) failed to compile. The initramfs update and GRUB configuration steps continued, and package health checks completed without reported errors. The Guest Additions issue was not resolved as part of this project.

## What I Learned

- How to configure and verify a Linux host firewall
- How to inspect listening sockets and identify network services
- How to disable a systemd service and its socket activation unit
- How Linux file permissions affect access to sensitive files
- How to verify package health after system updates

## Disclaimer

This was a learning lab performed on my own virtual machine. Disabling services and changing permissions should be based on the needs of the specific system and its applications.