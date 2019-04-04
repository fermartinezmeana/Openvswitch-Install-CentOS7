### **Creating external and internal bridge:**

ovs-vsctl add-br bridge_ext_name

ovs-vsctl add-br bridge_int_name
<br />
_Example_

ovs-vsctl add-br br-ex
ovs-vsctl add-br br-in



**Adding the network interface to external bridge (Needed for internet connection):**

**Use command `ip link show` to know your network interface**

ovs-vsctl add-port bridge_ext_name interface_name

## Example:

ovs-vsctl add-port br-ex eht0



## Configuring external bridge and network interface:
## You need to know your ip address, the netmask and the gateway. Use commands "ip a" and "ip r"
## When using command "ip r" you have a line that looks like: default via XXXXX
## The number XXXXX is the Gateway
## The next line looks like XXXXX/ZZ
## ZZ is the netmask      /16 means 255.255.0.0  /24 means 255.255.255.0  /21 means 255.255.248.0  ...

ip addr flush dev interface_name
ip addr add IP_XX dev bridge_ext_name
ip link set dev bridge_ext_name up

## Example:

ip addr flush dev eth0
ip addr add 192.168.1.5 dev br-ex
ip link set dev br-ex up


## Configuring internal bridge

ip addr add IP_YY dev bridge_int_name
ip link set dev bridge_int_name up

## Example:

ip addr add 10.0.0.100 dev br-in
ip link set dev br-in up



## Persistent configuration for exterior bridge:

vi /etc/sysconfig/network-scripts/ifcfg-bridge_ext_name
TYPE=OVSBridge
BOOTPROTO=static
DEVICE=bridge_ext_name
DEVICETYPE=ovs
ONBOOT=yes
IPADDR=XX.XX.XX.XX
NETMASK=XXX.XXX.XXX.X
GATEWAY=XX.XX.XX.XX

# Example:

vi /etc/sysconfig/network-scripts/ifcfg-br-ex
TYPE=OVSBridge
BOOTPROTO=static
DEVICE=br-ex
DEVICETYPE=ovs
ONBOOT=yes
IPADDR=192.168.1.5
NETMASK=255.255.255.0
GATEWAY=192.168.1.1


# Peristent configuration for network interface:

vi /etc/sysconfig/network-scripts/ifcfg-interface_name
DEVICE=interface_name
TYPE=OVSPort
DEVICETYPE=ovs
OVS_BRIDGE=bridge_ext_name
ONBOOT=yes

# Example

vi /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
TYPE=OVSPort
DEVICETYPE=ovs
OVS_BRIDGE=br-ex
ONBOOT=yes


## Persistent configuration for interior bridge:

vi /etc/sysconfig/network-scripts/ifcfg-bridge_int_name
DEVICE=bridge_int_name
DEVICETYPE=ovs
TYPE=OVSBridge
BOOTPROTO=static
IPADDR=XX.XX.XX.XX
NETMASK=XXX.XXX.XXX.X
ONBOOT=yes

# Example

DEVICE=br-in
DEVICETYPE=ovs
TYPE=OVSBridge
BOOTPROTO=static
IPADDR=10.0.0.100
NETMASK=255.255.255.0
ONBOOT=yes



# Restart the network:

systemctl restart network
