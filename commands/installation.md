### Install dependencies:
[root@fer ~]#  yum install gcc gcc-c++ autoconf automake libtool openssl-devel python-devel desktop-file-utils groff graphviz checkpolicy selinux-policy-devel python-sphinx python-twisted-core python-zope-interface python-six libcap-ng-devel unbound unbound-devel wget rpm-build -y

### Create and login user ovs:
[root@fer ~]# useradd ovs
[root@fer ~]# su - ovs

### Donwload Openvswitch:
[root@fer ~]# wget http://openvswitch.org/releases/openvswitch-2.11.0.tar.gz

### Build rpm package:
[root@fer ~]# mkdir -p ~/rpmbuild/SOURCES
[root@fer ~]# cp openvswitch-2.11.0.tar.gz ~/rpmbuild/SOURCES/
[root@fer ~]# tar xfz openvswitch-2.11.0.tar.gz
[root@fer ~]# rpmbuild -bb --nocheck openvswitch-2.11.0/rhel/openvswitch-fedora.spec

### Change to root user and install Openvswitch
[root@fer ~]# exit
[root@fer ~]# yum localinstall /home/ovs/rpmbuild/RPMS/x86_64/openvswitch-2.11.0-1.el7.x86_64.rpm -y

### Start and enable service:
[root@fer ~]# systemctl start openvswitch.service
[root@fer ~]# systemctl enable openvswitch.service

### Checking installation:
[root@fer ~]# ovs-vsctl show
