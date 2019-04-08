# Installation using Ansible
In this section there are all the necessary files for an automated installation and configuration of openvswitch using Ansible. The last Ansible version used in this document is 2.7.9

## 1. Pre-requisites
You only need have Ansible installed in the computer you are going to launch the installation. You can install it using the command `yum install ansible` or you can find more information [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW#latest-release-via-dnf-or-yum)

### - Network configuration and Openvswitch version:
You **need change the network configuration**. To set the `bridge_exterior` you need find out your ip address, netmask and gateway. Use commands `ip a` and `ip r` to know them. To do this, modify the file `all` located in the folder `group_vars`. In this file you can also modify the Openvswitch version.

### - Change installation host
You can install openvswitch in localhost or in a remote host. By default the installation is in localhost, if you want install openvswitch in a remote host you need to:
- Setting up public key authentication from localhost to the remote host. You can find more information [here](https://www.ssh.com/ssh/copy-id)

- Modify the file hosts commenting the local section, uncommenting the remotehost section and replacing the IP.

**Example hosts file:**
<br />
#[local]
<br />
#localhost ansible_connection=local
<br />
[remotehost]
<br />
192.168.1.5

## 2. Installation
To install Openvswitch you only need run the command `ansible-playbook -i hosts site.yml` inside the ansible directory.
