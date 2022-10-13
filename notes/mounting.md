# [Control active and configured mount points](https://docs.ansible.com/ansible/latest/collections/ansible/posix/mount_module.html)
```yaml
- name: Mount DVD read-only
  ansible.posix.mount:
    path: /mnt/dvd
    src: /dev/sr0
    fstype: iso9660
    opts: ro,noauto
    state: present

- name: Mount up device by label
  ansible.posix.mount:
    path: /srv/disk
    src: LABEL=SOME_LABEL
    fstype: ext4
    state: present

- name: Mount up device by UUID
  ansible.posix.mount:
    path: /home
    src: UUID=b3e48f45-f933-4c8e-a700-22a159ec9077
    fstype: xfs
    opts: noatime
    state: present

- name: Unmount a mounted volume
  ansible.posix.mount:
    path: /tmp/mnt-pnt
    state: unmounted

- name: Remount a mounted volume
  ansible.posix.mount:
    path: /tmp/mnt-pnt
    state: remounted

# The following will not save changes to fstab, and only be temporary until
# a reboot, or until calling "state: unmounted" followed by "state: mounted"
# on the same "path"
- name: Remount a mounted volume and append exec to the existing options
  ansible.posix.mount:
    path: /tmp
    state: remounted
    opts: exec

- name: Mount and bind a volume
  ansible.posix.mount:
    path: /system/new_volume/boot
    src: /boot
    opts: bind
    state: mounted
    fstype: none

- name: Mount an NFS volume
  ansible.posix.mount:
    src: 192.168.1.100:/nfs/ssd/shared_data
    path: /mnt/shared_data
    opts: rw,sync,hard
    state: mounted
    fstype: nfs

- name: Mount NFS volumes with noauto according to boot option
  ansible.posix.mount:
    src: 192.168.1.100:/nfs/ssd/shared_data
    path: /mnt/shared_data
    opts: rw,sync,hard
    boot: no
    state: mounted
    fstype: nfs
```
