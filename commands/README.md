## Installation using CLI
In this folder there are all the necessary commands to install openvswitch. 

The version installed is 2.11.0. You can change it replacing that number in all the corresponding commands
<br />
<br />

#### Example:
If you want to install the version 2.10.1 then:

Replace this: 
<br />
[ovs@fer ~]# wget http://openvswitch.org/releases/openvswitch-2.11.0.tar.gz
<br />
[ovs@fer ~]# cp openvswitch-2.11.0.tar.gz ~/rpmbuild/SOURCES/ 
<br />
...

By this: 
<br />
[ovs@fer ~]# wget http://openvswitch.org/releases/openvswitch-2.10.1.tar.gz 
<br />
[ovs@fer ~]# cp openvswitch-2.10.1.tar.gz ~/rpmbuild/SOURCES/ 
<br />
...
