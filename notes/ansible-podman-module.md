# [Ansible collection for podman notes](https://docs.ansible.com/ansible/latest/collections/containers/podman/index.html)
```yaml
-  containers.podman.podman_container:
    name: container
    image: quay.io/bitnami/wildfly
    state: started

- name: Create a data container
  containers.podman.podman_container:
    name: mydata
    image: busybox
    # Create a bind mount. If you specify, volume /HOST-DIR:/CONTAINER-DIR, podman bind mounts /HOST-DIR in the host to /CONTAINER-DIR in the podman container. 
    volume:
      - /tmp/data

- name: Re-create a redis container with systemd service file generated in /tmp/
  containers.podman.podman_container:
    name: myredis
    image: redis
    command: redis-server --appendonly yes
    state: present
    recreate: yes
    expose:
      - 6379
    volumes_from:
      - mydata
    generate_systemd:
      path: /tmp/
      restart_policy: always
      time: 120
      names: true
      container_prefix: ainer

- name: Restart a container
  containers.podman.podman_container:
    name: myapplication
    image: redis
    state: started
    restart: yes
    # Dict of host-to-IP mappings, where each host name is a key in the dictionary. Each host name will be added to the container’s /etc/hosts file.
    etc_hosts:
        other: "127.0.0.1"
    # Restart policy to follow when containers exit. Restart policy will not take effect if a container is stopped via the podman kill or podman stop commands. Valid values are * no - Do not restart containers on exit * on-failure[:max_retries] - Restart containers when they exit with a non-0 exit code, retrying indefinitely or until the optional max_retries count is hit * always - Restart containers when they exit, regardless of status, retrying indefinitely
    restart_policy: "no"
    # Add a host device to the container. The format is <device-on-host>[:<device-on-container>][:<permissions>] (e.g. device /dev/sdc:/dev/xvdc:rwm)
    # r for read, w for write, and m for mknod(2)
    device: "/dev/sda:/dev/xvda:rwm"
    # Publish a container’s port, or range of ports, to the host. Format - ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort | containerPort In case of only containerPort is set, the hostPort will chosen randomly by Podman.
    ports:
        - "8080:9000"
        - "127.0.0.1:8081:9001/udp"
    # Set environment variables. This option allows you to specify arbitrary environment variables that are available for the process that will be launched inside of the container.
    env:
        SECRET_KEY: "ssssh"
        BOOLEAN_KEY: "yes"

- name: Container present
  containers.podman.podman_container:
    name: mycontainer
    state: present
    image: ubuntu:14.04
    command: "sleep 1d"

- name: Stop a container
  containers.podman.podman_container:
    name: mycontainer
    state: stopped

- name: Start 4 load-balanced containers
  containers.podman.podman_container:
    name: "container{{ item }}"
    recreate: yes
    image: someuser/anotherappimage
    command: sleep 1d
  with_sequence: count=4

- name: remove container
  containers.podman.podman_container:
    name: ohno
    state: absent

- name: Writing output
  containers.podman.podman_container:
    name: myservice
    image: busybox
    log_options: path=/var/log/container/mycontainer.json
    log_driver: k8s-file

- name: Run three containers at once
  podman_containers:
    containers:
      - name: alpine
        image: alpine
        command: sleep 1d
      - name: web
        image: nginx
      - name: test
        image: python:3-alpine
        command: python -V

- name: Create secret
  containers.podman.podman_secret:
    state: present
    name: mysecret
    data: "my super secret content"

- name: Create container that uses the secret
  containers.podman.podman_container:
    name: showmysecret
    image: docker.io/alpine:3.14
    secrets:
      - mysecret
    detach: false
    command: cat /run/secrets/mysecret
    register: container

- name: Output secret data
  debug:
    msg: '{{ container.stdout }}'

- name: Remove secret
  containers.podman.podman_secret:
    state: absent
    name: mysecret
```