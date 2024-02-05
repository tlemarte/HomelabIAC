
# Infrastructure Overview

* [Proxmox](#proxmox)
  * [RHEL01](#rhel01)
    * [Nginx Proxy Manager](#Nging-proxy-manager)
    * [Nextcloud](#Nextcloud)
    * [Jellyfin](#Jellyfin)
    * [Transmission](#Transmission)
    * [Bazarr](#Bazarr)
    * [Lidarr](#Lidarr)
    * [Prowlarr](#Prowlarr)
    * [Radarr](#Radarr)
    * [Readarr](#Readarr)
    * [Sonarr](#Sonarr)
    * [Guacamole](#Guacamole)
  * [Rhel02](#rhel02)
  * [Ubuntu01](#ubuntu01)
    * [Pi-hole](#pi-hole)
* [TrueNAS](#truenas)
* [Orangebox](#Orangebox)
* [Fedora01](#Fedora01)
  * [Ansible](#Ansible)
---
# [Proxmox](https://proxmox.lemarte.tech:8006/)
## [Cloud-init](https://pve.proxmox.com/wiki/Cloud-Init_Support)
```bash
# Download and import Ubuntu 22.04 QCow2 UEFI/GPT Bootable disk image with linux-kvm KVM optimised kernel
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64-disk-kvm.img

# The next step is to configure a CD-ROM drive, which will be used to pass the Cloud-Init data to the VM.

qm set 110 --ide2 local-lvm:cloudinit

# To be able to boot directly from the Cloud-Init image, set the boot parameter to order=scsi0 to restrict BIOS to boot from this disk only. This will speed up booting, because VM BIOS skips the testing for a bootable CD-ROM.

qm set 110 --boot order=scsi0

# For many Cloud-Init images, it is required to configure a serial console and use it as a display. If the configuration doesn’t work for a given image however, switch back to the default display instead.

qm set 110 --serial0 socket --vga serial0

# In a last step, it is helpful to convert the VM into a template. From this template you can then quickly create linked clones. The deployment from VM templates is much faster than creating a full clone (copy).

qm template 110
```
## [Rhel01](https://rhel01.lemarte.tech:9090)
### [Cockpit]{https://coockpit.lemarte.tech:9090}
### [Nging-proxy-manager](http://rhel01.lemarte.tech:81/login)
### [Authentik]()
---
## [Rhel02](https://rhel02.lemarte.tech:9090)
### [FreeIPA]?
---
## [Ubuntu01]()
### [Pi-hole](https://pi.hole/admin/)
### [Unbound](https://github.com/NLnetLabs/unbound)
---
# [Truenas](https://truenas.local)
# [Fedora01](https://fedora01.lemarte.tech:9090)
## [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
```bash
[user@ControlNode ~] sudo dnf install ansible-core
```