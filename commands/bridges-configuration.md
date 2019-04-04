### **Creating external and internal bridge:**
ovs-vsctl add-br `bridge_ext_name`
<br />
ovs-vsctl add-br `bridge_int_name`

<br />

**Example**

ovs-vsctl add-br br-ex

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

ip addr add `IP_XX` dev `bridge_ext_name`

ip link set dev `bridge_ext_name` up

<br />

**Example:**

ip addr flush dev eth0

ip addr add 192.168.1.5 dev br-ex

ip link set dev br-ex up
<br />
<br />
<br />

### Configuring internal bridge

ip addr add `IP_YY` dev `bridge_int_name`

ip link set dev `bridge_int_name` up

<br />

**Example:**

ip addr add 10.0.0.100 dev br-in

ip link set dev br-in up
<br />
<br />
<br />

### Persistent configuration for exterior bridge:

vi /etc/sysconfig/network-scripts/ifcfg-bridge_ext_name

TYPE=OVSBridge

BOOTPROTO=static

DEVICE=bridge_ext_name

DEVICETYPE=ovs

ONBOOT=yes

IPADDR=XX.XX.XX.XX

NETMASK=XXX.XXX.XXX.X

GATEWAY=XX.XX.XX.XX

<br />

**Example:**

vi /etc/sysconfig/network-scripts/ifcfg-br-ex

TYPE=OVSBridge

BOOTPROTO=static

DEVICE=br-ex

DEVICETYPE=ovs

ONBOOT=yes

IPADDR=192.168.1.5

NETMASK=255.255.255.0

GATEWAY=192.168.1.1
<br />
<br />
<br />

### Peristent configuration for network interface:

vi /etc/sysconfig/network-scripts/ifcfg-interface_name

DEVICE=interface_name

TYPE=OVSPort

DEVICETYPE=ovs

OVS_BRIDGE=bridge_ext_name

ONBOOT=yes

<br />

**Example:**

vi /etc/sysconfig/network-scripts/ifcfg-eth0

DEVICE=eth0

TYPE=OVSPort

DEVICETYPE=ovs

OVS_BRIDGE=br-ex

ONBOOT=yes
<br />
<br />
<br />

### Persistent configuration for interior bridge:

vi /etc/sysconfig/network-scripts/ifcfg-bridge_int_name

DEVICE=bridge_int_name

DEVICETYPE=ovs

TYPE=OVSBridge

BOOTPROTO=static

IPADDR=XX.XX.XX.XX

NETMASK=XXX.XXX.XXX.X

ONBOOT=yes

<br />

**Example**

DEVICE=br-in

DEVICETYPE=ovs

TYPE=OVSBridge

BOOTPROTO=static

IPADDR=10.0.0.100

NETMASK=255.255.255.0

ONBOOT=yes
<br />
<br />
<br />

### Restart the network:

systemctl restart network
