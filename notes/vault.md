# [Ansible Vault notes](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html)

## Create vault
### Example variable vault file, formatted in yaml
```yaml
variable1: value1
variable2: value2
variable3: value3
```
### Encrypting that file
```bash
$ ansible-vault encrypt /path/to/file
New Vault password: 
Confirm New Vault password: 
Encryption successful
```
## Use vault for variables in playbook
### Playbook calling for variable
```yaml
...
    env:
        username: "{{ variable1 }}"
        password: "{{ variable2 }}"
...        
```
### Using the vault to pass variables into playbook(s)
```bash
# Used flags:
# -e, -extra-vars
#   set additional variables as key=value or YAML/JSON, if filename prepend with @
# --ask-vault-password, --ask-vault-pass
#   ask for vault password
$ ansible-playbook -e @/path/to/file --ask-vault-pass playbook.yml
```
## Decrypt vault file
### Show contents of encrypted vault
```bash
$ cat /path/to/encrypted/file
$ANSIBLE_VAULT;1.1;AES256
12345678987654321234567898765432123456789876543212345678987654321234567898765432
12345678987654321234567898765432123456789876543212345678987654321234567898765432
12345678987654321234567898765432123456789876543212345678987654321234567898765432
```
### Decrypt vault
```bash
$ ansible-vault decrypt /path/to/encrypted/file
Vault password: 
Decryption successful
```
### Show contents of decrypted file
```bash
$ cat /path/to/decrypted/file
variable1: value1
variable2: value2
variable3: value3
```