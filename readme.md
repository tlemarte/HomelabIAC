# Ansible Homelab
This is repository hold the ansible playbook and additional documentation for my homelab
## Notes
### Ansible
### Post install commands to automate
#### rhel01.local
##### Configuration to enable ansible connection
Ansible requires python3
```
yum install python3
```
On the machine running Ansible (AnsibleController), run the following command to create a key pair for SSH connection without a password
```
`ssh-keygen -t rsa`
```

Copy the is_rsa.pub from the machine running Ansible to the controlled node as authorized_keys
```
`scp user@AnsibleController:~/.ssh/id_rsa.pub ~/.ssh/`
mv id_rsa.pub authorized_keys
```
Enable sudo access without password
```
`echo "$USER ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers`
```
##### After ansible configuration
QEMU quest agent for proxmox
```bash
yum install qemu-guest-agent
systemctl start qemu-guest-agent
```
Cockpit
```
systemctl enable --now cockpit.socket
```

#### ubuntu01
On the machine running Ansible (AnsibleController), run the following command to create a key pair for SSH connection without a password
```
`ssh-keygen -t rsa`
```

Copy the is_rsa.pub from the machine running Ansible to the controlled node as authorized_keys
```
`scp user@AnsibleController:~/.ssh/id_rsa.pub ~/.ssh/`
mv id_rsa.pub authorized_keys
```
Enable sudo access without password
```
`echo "$USER ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers`
```