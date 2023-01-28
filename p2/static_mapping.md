static mapping

### R1:

```
ip link add br0 type bridge
ip addr add 10.1.1.1/24 dev eth0
ip link add name vxlan10 type vxlan id 10 dev eth0 remote 10.1.1.2 local 10.1.1.1 dstport 4789
ip addr add 20.1.1.1/24 dev vxlan10
brctl addif br0 eth1
brctl addif br0 vxlan10
ip link set dev br0 up
ip link set dev vxlan10 up
```
---

### R2:

```
ip link add br0 type bridge
ip addr add 10.1.1.2/24 dev eth0
ip link add name vxlan10 type vxlan id 10 dev eth0 remote 10.1.1.1 local 10.1.1.2 dstport 4789
ip addr add 20.1.1.2/24 dev vxlan10
brctl addif br0 eth1
brctl addif br0 vxlan10
ip link set dev br0 up
ip link set dev vxlan10 up 
```

### with some commentary:

`ip link add br0 type bridge` - This command creates a new bridge interface named "br0" and sets its type as "bridge". A bridge interface is used to connect multiple network interfaces together, allowing them to share the same network segment.

`ip addr add 10.1.1.2/24 dev eth0` - This command assigns the IP address "10.1.1.2" with a netmask of "/24" to the network interface "eth0". This interface will be used as the local endpoint for communication.

`ip link add name vxlan10 type vxlan id 10 dev eth0 remote 10.1.1.1 local 10.1.1.2 dstport 4789` - This command creates a new VXLAN interface named "vxlan10" with an identifier of "10". The interface is associated with the "eth0" device, and the remote endpoint of the VXLAN tunnel is set to "10.1.1.1" and the local endpoint is set to "10.1.1.2" and the destination port is set to "4789". VXLAN is a network virtualization technology used to extend Layer 2 networks over Layer 3 networks.

`ipaddr add 20.1.1.2/24 dev vxlan10` - This command assigns the IP address "20.1.1.2" with a netmask of "/24" to the VXLAN interface "vxlan10".

`brctl addif br0 eth1` - This command adds the interface "eth1" to the bridge "br0". This allows traffic to flow between the interfaces that are part of the bridge.

`brctl addif br0 vxlan10` - This command adds the VXLAN interface "vxlan10" to the bridge "br0". This allows traffic to flow between the VXLAN interface and other interfaces that are part of the bridge.

`ip link set dev br0 up` - This command brings the bridge interface "br0" up, making it active and able to pass traffic.

`ip link set dev vxlan10 up` - This command brings the VXLAN interface "vxlan10" up, making it active and able to pass traffic.


### Alpine machines:

```
ip addr add 30.1.1.1/24  dev eth1
----
ip addr add 30.1.1.2/24  dev eth1
```


configured VXLAN interfaces, attached them to a bridge, and mapped VLANs to VNIs.
