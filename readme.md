
# Homelab Infrastructure as Code (IAC)

---

This repository contains documentation and configuration files for my homelab setup.

---

## Infrastructure Overview

* [Proxmox](#proxmox)
  * [RHEL01](#rhel01)
    * [Nginx Proxy Manager](#nginx-proxy-manager)
    * [Nextcloud]

    * Jellyfin
    * Transmission
    * Bazarr
    * Lidarr
    * Prowlarr
    * Radarr
    * Readarr
    * Sonarr
    * Nagios
    * Guacamole
  * [RHEL02](#rhel02)
  * [UBUNTU01](#ubuntu01)
    * [Pi-hole](#pi-hole)
* [TrueNAS](#truenas)

---

## Proxmox

---

## RHEL01

---

## RHEL02

RHEL 8.6
12 vcpu
16gb mem 256gb storage

---

### Done: [Install Ansible](#https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

```bash
[user@ControlNode ~] sudo dnf install ansible-core
```

Generate an SSH key pair on the Ansible Controller to establish passwordless connections. Press enter twice to accept the default file location and password.

```bash
[user@ControlNode ~] ssh-keygen -t rsa
```

Copy the public SSH key generated in the previous step to the Ansible Host

```bash
[user@ControlNode ~]  ssh-copy-id user@ManagedNode
```

I want to enable passwordless sudo access for my credentials. On the machine targeted previously, open the /etc/sudoers file using a text editor like nano or vi with sudo privileges:

```bash
[user@ManagedNode ~] sudo visudo
```

Add the following line at the end of the file, replacing $user with the appropriate username:

```bash
$user ALL=(ALL:ALL) NOPASSWD: ALL
```

#### Done: QEMU quest agent for proxmox

```bash
yum install qemu-guest-agent
systemctl start qemu-guest-agent
```

#### Install ansible collections

```bash
ansible-galaxy collection install containers.podman
ansible-galaxy collection install community.general
```

#### Done: Enable Cockpit

```bash
systemctl enable --now cockpit.socket
```

### TO DO: Encrypt ansible vault items

Create an encrypted version of the OpenVPN configuration file using the ansible-vault command:

```bash
ansible-vault encrypt /home/ansible/conf.ovpn --output /home/ansible/conf_vaulted.ovpn
```

You will be prompted to enter a new vault password. Make sure to remember this password, as you will need it to decrypt the file later.\
Now, you can update the OpenVPN role's task that copies the configuration file to use the encrypted version:

```yaml
- name: Copy OpenVPN configuration file
  ansible.builtin.copy:
    src: "{{ openvpn_config_file }}"
    dest: "/opt/openvpn/config/"
    mode: 0644
  vars:
    openvpn_config_file: "{{ vaulted_openvpn_config_file }}"
  become: true
  no_log: true
```

In the site.yml playbook, update the variable assignment to use the vaulted file:

```yaml
- role: openvpn
  openvpn_config_file: /home/ansible/conf_vaulted.ovpn
```

Finally, when running the playbook, you need to provide the vault password. You can do this in multiple ways:

1. Pass the password interactively with the --ask-vault-pass option:

```bash
ansible-playbook -i inventory.ini site.yml --ask-vault-pass
```

2. Store the password in a file and provide the path to the file with the --vault-password-file option:

```bash
ansible-playbook -i inventory.ini site.yml --vault-password-file /path/to/vault_password_file
```

By using Ansible Vault, the OpenVPN configuration file will be securely encrypted on the control node, and it will only be decrypted when deploying it to the managed node.

---

## UBUNTU01

---

### [Pi-hole](https://pi-hole.net/)
<https://docs.pi-hole.net/guides/dns/unbound/>

### Unbound

[Unbound](https://github.com/NLnetLabs/unbound)
