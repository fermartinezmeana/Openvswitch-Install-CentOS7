### Installation using Ansible
In this section there are all the necessary commands for an automated installation and configuration of openvswitch with Ansible. 

### Installation options

#### Hosts

You can install openvswitch in localhost or in a remote host. By default the installation is in localhost, if you want install openvswitch in a remote host modify the file site.yml and replace the line `hosts: local` by `hosts: remotehost`.

##### Example:
---
<br />
- name: Install openvswitch
<br />
  hosts: `remotehost`
  <br />
  become: yes
  <br />
  roles:
  <br />
    - openvswitch
