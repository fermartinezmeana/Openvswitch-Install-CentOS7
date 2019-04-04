### **Creating external and internal bridge:**
ovs-vsctl add-br `bridge_ext_name`
<br />
ovs-vsctl add-br `bridge_int_name`

<br />

**Example**

ovs-vsctl add-br br-ex
<br />
ovs-vsctl add-br br-in
<br />
<br />
<br />

### **Add the network interface to the external bridge (Needed for internet connection):**

#Use command `ip link show` to know your network interface

ovs-vsctl add-port `bridge_ext_name` `interface_name`

<br />

**Example:**

ovs-vsctl add-port br-ex eth0
<br />
<br />
<br />

### Configuration of external bridge and network interface:
#You need know your ip address, netmask and gateway. Use commands `ip a` and `ip r` to know them

ip addr flush dev `interface_name`
<br />
ip addr add `IP_XX` dev `bridge_ext_name`
<br />
ip link set dev `bridge_ext_name` up

<br />

**Example:**

ip addr flush dev eth0
<br />
ip addr add 192.168.1.5 dev br-ex
<br />
ip link set dev br-ex up
<br />
<br />
<br />

### Configuring internal bridge

ip addr add `IP_YY` dev `bridge_int_name`
<br />
ip link set dev `bridge_int_name` up

<br />

**Example:**

ip addr add 10.0.0.100 dev br-in
<br />
ip link set dev br-in up
<br />
<br />
<br />

### Persistent configuration for exterior bridge:

vi /etc/sysconfig/network-scripts/ifcfg-bridge_ext_name

TYPE=OVSBridge
<br />
BOOTPROTO=static
<br />
DEVICE=bridge_ext_name
<br />
DEVICETYPE=ovs
<br />
ONBOOT=yes
<br />
IPADDR=XX.XX.XX.XX
<br />
NETMASK=XXX.XXX.XXX.X
<br />
GATEWAY=XX.XX.XX.XX

<br />

**Example:**

vi /etc/sysconfig/network-scripts/ifcfg-br-ex

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
<br />

### Peristent configuration for network interface:

vi /etc/sysconfig/network-scripts/ifcfg-interface_name
<br />
DEVICE=interface_name
<br />
TYPE=OVSPort
<br />
DEVICETYPE=ovs
<br />
OVS_BRIDGE=bridge_ext_name
<br />
ONBOOT=yes

<br />

**Example:**

vi /etc/sysconfig/network-scripts/ifcfg-eth0

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
<br />

### Persistent configuration for interior bridge:

vi /etc/sysconfig/network-scripts/ifcfg-bridge_int_name

DEVICE=bridge_int_name
<br />
DEVICETYPE=ovs
<br />
TYPE=OVSBridge
<br />
BOOTPROTO=static
<br />
IPADDR=XX.XX.XX.XX
<br />
NETMASK=XXX.XXX.XXX.X
<br />
ONBOOT=yes

<br />

**Example**

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
<br />

### Restart the network:

systemctl restart network
