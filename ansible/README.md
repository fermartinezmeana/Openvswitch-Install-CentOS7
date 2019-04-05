## Installation using Ansible
In this section there are all the necessary files for an automated installation and configuration of openvswitch using Ansible. The last Ansible version used in this document is 2.7.9

### 1. Pre-requisites
You only need have Ansible installed in your computer you are going to launch the installation. You can install it using the command `yum install ansible` or you can find more information [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW#latest-release-via-dnf-or-yum)

### 2. Installation options

#### - Hosts
You can install openvswitch in localhost or in a remote host. By default the installation is in localhost, if you want install openvswitch in a remote host you need to:
- Modify the file site.yml and replace the line `hosts: local` by `hosts: remotehost`.

- Setting up public key authentication from localhost to the remote host. You can find more information in https://www.ssh.com/ssh/copy-id

- Modify the file hosts commenting the local section, uncommenting the remotehost section and replacing the IP.

##### Example hosts file:
#[local]
<br />
#localhost ansible_connection=local
<br />
[remotehost]
<br />
192.168.1.5



