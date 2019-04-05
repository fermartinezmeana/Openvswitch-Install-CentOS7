### Installation using Ansible
In this section there are all the necessary files for an automated installation and configuration of openvswitch using Ansible. 

### Installation options

#### Hosts

You can install openvswitch in localhost or in a remote host. By default the installation is in localhost, if you want install openvswitch in a remote host you need to:
<br />
- Modify the file site.yml and replace the line `hosts: local` by `hosts: remotehost`.
<br />
- Modify the file hosts commenting [local] and localhost lines, uncommenting remotehost and its IP lines and replacing the IP with yours.


