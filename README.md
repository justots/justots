You may be looking for our --> [DOCUMENTATION on wiki](https://github.com/DevelopersPL/otshosting-provisioning/wiki) <--

otshosting-provisioning
=======================
This is an Ansible playbook used to fully provision a Ubuntu machine for OTS Hosting.

__It works with Ubuntu versions with systemd -> Ubuntu >= 15.04__

Make sure to have universe, multiverse and restricted repositories enabled

```
./setup justots
```

or

A script to run on a standalone machine to provision it. If user "otsmanager" does not exist, it will be created with password: "otsmanager".
```bash
#!/bin/bash -ex
apt-get update
apt-get install -y -q python-simplejson git-core ansible aptitude
mkdir -p /home/output
ansible-pull -i localhost, -U https://github.com/justots/justots.git -d /srv/justots --purge 2>&1 | tee /home/output/ansible.txt
```

A cloud-init script to provision cloud instances with otshosting:
```
#cloud-config
users:
  - name: otsmanager
    gecos: OTS Manager
    lock-passwd: false
    
disable_root: true
ssh_pwauth: True
timezone: Europe/Warsaw

package_upgrade: true
package_update: true
apt_mirror: http://mirror.ovh.net/ubuntu/

packages:
 - python-simplejson
 - git
 - ansible
 - aptitude
 
runcmd:
  - 'ansible-pull -i localhost, -U https://github.com/justots/justots.git -d /srv/justots --purge'

```
