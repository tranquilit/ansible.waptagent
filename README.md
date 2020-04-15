# Ansible Role : tranquilit.waptagent

## Requirements

* Debian Linux or CentOS Virtual Machine
* Ansible 2.8+

## Install WAPT agent on remote hosts

* git clone and install that role in `/etc/ansible/roles`

```
cd /etc/ansible/roles/
git clone https://github.com/tranquilit/ansible.waptagent
```

* ensure you have a working ssh key deployed as root on your hosts, if not you can generate and copy one like so :

```
ssh-keygen -t ed25519
ssh-copy-id -i id_ed25519.pub root@hosts.mydomain.lan
```

* add your hosts to Ansible hosts file

```
[srvwapt]
computer1.mydomain.lan ansible_host=192.168.1.50
computer1.mydomain.lan ansible_host=192.168.1.60
```

* create a playbook with the following content in `/etc/ansible/playbooks/wapt.yml` :

```
- hosts: srvwapt
  roles:
    - { role: tranquilit.waptagent }
```

* run your playbook with the following command :

```
cd /etc/ansible/
ansible-playbook -i hosts playbooks/wapt.yml
```

That's it WAPT agent is installed on your hosts !

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

### WAPT agent vars

WAPT version that will be installed from WAPT Deb/RPM repository

    wapt_version: "1.8"

CentOS version used for RPM repository address

    centos_version: "centos7"

### wapt-get.ini vars

`wapt_server_url` points to your WAPT server and is used by default for the `wapt_repo_url`.

    wapt_server_url: "https://wapt.domain.lan"
    wapt_repo_url: "{{ wapt_server_url }}/wapt/"

You can override it like so :

    wapt_server_url: "https://wapt1.landomain.com"
    wapt_repo_url: "https://wapt2.otherdomain.com/wapt/"

Certificate filename located in `files/` subdirectory of the role :

    wapt_crt: "wapt_ca.crt"

## Dependencies

None.

## Example Playbook

    - hosts: hosts
      vars_files:
        - vars/main.yml
      roles:
        - tranquilit.waptagent

*Inside `vars/main.yml`*:

    wapt_server_url: "https://wapt.mydomain.fr"
    wapt_repo_url: "https://repo1.mydomain.fr/wapt/"
