# Ansible Homelab
This is repository hold the ansible playbook and additional documentation for my homelab
## Notes
### Ansible
### Post install commands to automate
yum install qemu-guest-agent
systemctl start qemu-guest-agent

systemctl enable --now cockpit.socket
