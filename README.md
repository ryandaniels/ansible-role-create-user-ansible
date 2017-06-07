# ansible-role-create-user-ansible

Create the ansible user on each server in the hosts file.


Distros tested
------------

* Ubuntu 16.04 as a client. It should work on older versions of Ubuntu/Debian based systems.
* CentOS 7.3
* CentOS 6.5
* CentOS 5.9


Dependencies
------------

none

ansible-vault
------------

none

.gitignore
------------

```
vi .gitignore
#Insert the following lines
.vaultpass
.retry
```


Default Settings
------------

- **default_sudo_nopass**: yes|no (default yes). yes = passwordless sudo.


Example Playbook create-user-ansible.yml
------------

```
---
- hosts: '{{inventory}}'
  become: yes
  roles:
  - create-user-ansible
```


Prep
------------

- install ansible
- create keys
- ssh to client to add entry to known_hosts file
- configure client server authorized_keys
- run ansible commands

Usage
------------

Create initial ansible user, using an existing user

- **username**: new ansible user to be created
- **ansible_ssh_user**: existing user with ssh access
- **--become-method**: su|sudo
- **--ask-become-pass**: needed if using su or password for sudo
- **inventory**: servers from hosts file
- **default_sudo_nopass**: passwordless sudo


Passwordless sudo = true (don't prompt for user password when running playbook)
```
ansible-playbook create-user-ansible.yml --ask-pass --become --become-method=su --ask-become-pass --extra-vars "inventory=all-dev ansible_ssh_user=username123 username=ansible" -i hosts
```

Passwordless sudo = false (prompt for user password when running playbook)
```
ansible-playbook create-user-ansible.yml --ask-pass --become --become-method=su --ask-become-pass --extra-vars "inventory=all-dev default_sudo_nopass=false ansible_ssh_user=username123 username=ansible" -i hosts
```


## Notes
### RHEL5
RHEL5 has a dependency that needs to be installed: python-simplejson  
This command will use the raw module to install it:
```
ansible centos5 -m raw -a "yum install -y python-simplejson" --ask-pass --su --ask-su-pass --extra-vars="ansible_ssh_user=username123" -i hosts-dev
```

### SELinux
If SELinux is enabled/permissive a dependency is needed: libselinux-python  
This command will use the raw module to install it:
```
ansible centos5 -m raw -a "yum install -y libselinux-python" --ask-pass --su --ask-su-pass --extra-vars="ansible_ssh_user=username123" -i hosts-dev
```
