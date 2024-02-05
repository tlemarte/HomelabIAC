Now what sort of use cases does this layout enable? Lots! If I want to reconfigure my whole infrastructure, it’s just:
```bash
ansible-playbook -i inventory.ini site.yaml
```

To reconfigure NTP on everything:
```bash
ansible-playbook -i inventory.ini site.yaml --tags ntp
```

To reconfigure just nginx_proxy_manager:
```bash
ansible-playbook -i inventory.ini nginx_proxy_manager.yaml
```

And of course just basic ad-hoc stuff is also possible:
```bash
ansible linux -i inventory.ini -m ping
ansible linux -i inventory.ini -m command -a '/sbin/reboot'
```

And there are some useful commands to know:
```bash
# confirm what task names would be run if I ran this command and said "just ntp tasks"
ansible-playbook -i inventory.ini webservers.yaml --tags ntp --list-tasks

# confirm what hostnames might be communicated with if I said "limit to boston"
ansible-playbook -i inventory.ini webservers.yaml --limit boston --list-hosts
```