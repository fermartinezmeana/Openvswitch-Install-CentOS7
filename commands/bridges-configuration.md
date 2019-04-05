### **Creating external and internal bridge:**
[root@fer ~]# ovs-vsctl add-br `bridge_ext_name`
<br />
[root@fer ~]# ovs-vsctl add-br `bridge_int_name`
<br />

**Example**
<br />
[root@fer ~]# ovs-vsctl add-br br-ex
<br />
[root@fer ~]# ovs-vsctl add-br br-in
<br />
<br />

### **Add the network interface to the external bridge (Needed for internet connection):**

Use command `ip link show` to know your network interface

[root@fer ~]# ovs-vsctl add-port `bridge_ext_name` `interface_name`
<br />

**Example:**
<br />
[root@fer ~]# ovs-vsctl add-port br-ex eth0
<br />
<br />

### Configuration of external bridge and network interface:
You need know your ip address, netmask and gateway. Use commands `ip a` and `ip r` to know them

[root@fer ~]# ip addr flush dev `interface_name`
<br />
[root@fer ~]# ip addr add `IP_XX` dev `bridge_ext_name`
<br />
[root@fer ~]# ip link set dev `bridge_ext_name` up
<br />

**Example:**
<br />
[root@fer ~]# ip addr flush dev eth0
<br />
[root@fer ~]# ip addr add 192.168.1.5 dev br-ex
<br />
[root@fer ~]# ip link set dev br-ex up
<br />
<br />

### Configuring internal bridge

[root@fer ~]# ip addr add `IP_YY` dev `bridge_int_name`
<br />
[root@fer ~]# ip link set dev `bridge_int_name` up
<br />

**Example:**
<br />
[root@fer ~]# ip addr add 10.0.0.100 dev br-in
<br />
[root@fer ~]# ip link set dev br-in up
<br />
<br />

### Persistent configuration for exterior bridge:

Clear the file /etc/sysconfig/network-scripts/ifcfg-`bridge_ext_name` and add:

[root@fer ~]# vi /etc/sysconfig/network-scripts/ifcfg-`bridge_ext_name`
<br />
TYPE=OVSBridge
<br />
BOOTPROTO=static
<br />
DEVICE=`bridge_ext_name`
<br />
DEVICETYPE=ovs
<br />
ONBOOT=yes
<br />
IPADDR=`XX.XX.XX.XX`
<br />
NETMASK=`XXX.XXX.XXX.X`
<br />
GATEWAY=`XX.XX.XX.XX`
<br />

**Example:**
<br />
[root@fer ~]# vi /etc/sysconfig/network-scripts/ifcfg-br-ex
<br />
TYPE=OVSBridge
<br />
BOOTPROTO=static
<br />
DEVICE=br-ex
<br />
DEVICETYPE=ovs
<br />
ONBOOT=yes
<br />
IPADDR=192.168.1.5
<br />
NETMASK=255.255.255.0
<br />
GATEWAY=192.168.1.1
<br />
<br />

### Peristent configuration for network interface:

Clear the file /etc/sysconfig/network-scripts/ifcfg-`interface_name` and add

[root@fer ~]# vi /etc/sysconfig/network-scripts/ifcfg-`interface_name`
<br />
DEVICE=`interface_name`
<br />
TYPE=OVSPort
<br />
DEVICETYPE=ovs
<br />
OVS_BRIDGE=`bridge_ext_name`
<br />
ONBOOT=yes
<br />

**Example:**
<br />
[root@fer ~]# vi /etc/sysconfig/network-scripts/ifcfg-eth0
<br />
DEVICE=eth0
<br />
TYPE=OVSPort
<br />
DEVICETYPE=ovs
<br />
OVS_BRIDGE=br-ex
<br />
ONBOOT=yes
<br />
<br />

### Persistent configuration for interior bridge:
Clear the file /etc/sysconfig/network-scripts/ifcfg-`bridge_int_name` and add:

[root@fer ~]# vi /etc/sysconfig/network-scripts/ifcfg-`bridge_int_name`
<br />
DEVICE=`bridge_int_name`
<br />
DEVICETYPE=ovs
<br />
TYPE=OVSBridge
<br />
BOOTPROTO=static
<br />
IPADDR=`YY.YY.YY.YY`
<br />
NETMASK=`YYY.YYY.YYY.Y`
<br />
ONBOOT=yes
<br />

**Example**
<br />
DEVICE=br-in
<br />
DEVICETYPE=ovs
<br />
TYPE=OVSBridge
<br />
BOOTPROTO=static
<br />
IPADDR=10.0.0.100
<br />
NETMASK=255.255.255.0
<br />
ONBOOT=yes
<br />
<br />

### Restart the network:

[root@fer ~]# systemctl restart network
