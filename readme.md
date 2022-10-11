# Ansible Homelab
This is repository hold the ansible playbook and additional documentation for my homelab
## Notes
### Ansible
### Post install commands to automate
yum install qemu-guest-agent
systemctl start qemu-guest-agent

systemctl enable --now cockpit.socket

sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    
sudo yum install docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-compose-plugin \
    --allowerasing

sudo systemctl start docker