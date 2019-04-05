## Installation using Ansible
In this section there are all the necessary files for an automated installation and configuration of openvswitch using Ansible. 

## Installation options

### Hosts

You can install openvswitch in localhost or in a remote host. By default the installation is in localhost, if you want install openvswitch in a remote host you need to:
- Modify the file site.yml and replace the line `hosts: local` by `hosts: remotehost`.

- Setting up public key authentication from localhost to the remote host. You can find more information in https://www.ssh.com/ssh/copy-id

- Modify the file hosts commenting the local section, uncommenting the remotehost section and replacing the IP.

#### Example hosts file:
<br />
#[local]
<br />
#localhost ansible_connection=local
<br />
[remotehost]
<br />
192.168.1.5


