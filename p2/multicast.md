
### R1
```
ip link add br0 type bridge
ip link set dev br0 up
###
ip addr add 10.1.1.1/24 dev eth0
###
ip link add name vxlan10 type vxlan id 10 dev eth0 group 239.1.1.1 dstport 4789

ip addr add 20.1.1.1/24 dev vxlan10 
###
brctl addif br0 eth1
brctl addif br0 vxlan10
###
ip link set dev vxlan10 up
###
ip -d link show vxlan10

brctl show 
``` 

-------

### R2
```
ip link add br0 type bridge
ip link set dev br0 up
###
ip addr add 10.1.1.2/24 dev eth0
###
ip link add name vxlan10 type vxlan id 10 dev eth0 group 239.1.1.1 dstport 4789
ip addr add 20.1.1.2/24 dev vxlan10
###
brctl addif br0 eth1
brctl addif br0 vxlan10
###
ip link set dev vxlan10 up
###
ip -d link show vxlan10
```

---

 alpine machines
```
ip addr add 30.1.1.1/24  dev eth1
    ----
ip addr add 30.1.1.2/24  dev eth1
```
====

`ip link add br0 type bridge`: this command creates a new bridge device named "br0" and sets its type as "bridge".

`ip link set dev br0 up`: this command brings up the "br0" bridge device, making it active and ready to use.

`ip addr add 10.1.1.1/24 dev eth0`: this command assigns the IP address 10.1.1.1 with a netmask of /24 to the interface "eth0"

`ip link add name vxlan10 type vxlan id 10 dev eth0 group 239.1.1.1 dstport 4789`: this command creates a new VXLAN interface named "vxlan10" with a VXLAN ID of 10, using the interface "eth0" as the local endpoint and the multicast group 239.1.1.1 as the destination. "dstport 4789" is the destination port for the VXLAN tunnel.

`ipaddr add 20.1.1.1/24 dev vxlan10`: this command assigns the IP address 20.1.1.1 with a netmask of /24 to the VXLAN interface "vxlan10".

`brctl addif br0 eth1`: this command adds the interface "eth1" to the bridge "br0"

`brctl addif br0 vxlan10`: this command adds the VXLAN interface "vxlan10" to the bridge "br0"

`ip link set dev vxlan10 up`: this command brings up the "vxlan10" interface, making it active and ready to use.

`ip -d link show vxlan10`: this command shows detailed information about the VXLAN interface "vxlan10"

`brctl show`: this command shows the status of all the bridges on the system.

====

Multicast to handl BUM traffic


Multicasting can be used to push data to multiple hosts simultaneously. 
Characteristics of multicasting are as follows:

■ sending one stream of traffic to each requesting broadcast domain.

■ The IP address that defines a multicast group is a Class D address (224.0.0.0 to 239.255.255.255).

■ A multicast address identifies a group of hosts sharing the same address.

■ Multicast addresses are not assigned to a device, rather, a device proceeds to listen for and receive traffic destined to a multicast group that it has joined by some process.

■ Multicasting is UDP-based.

■ IGMP is required to be used in host computers that wish to participate in multicasting.
    Internet Group Management Protocol (IGMP)

(IGMP) is a protocol that allows several devices to share one IP address so they can all receive the same data.
Specifically, IGMP allows devices to join a multicasting group.


====

How VTEPs Learns and Creates Forwarding Table
  Each VTEP has its own forwarding table, which is initially empty

  We will take an example of virtual machine on Host 1 trying to communicate with the virtual machine on the Host 2. First, an ARP request is sent from the virtual machine MAC1 to find the MAC address of the virtual machine on Host 2. The ARP request is a broadcast packet.

1/  Virtual machine on Host1 sends ARP packet with Destination MAC as “FFFFFFFFFFF”
2/  VTEP on Host 1 encapsulates the Ethernet broadcast packet into a UDP header with Multicast address “239.1.1.100” as the     
      destination IP address and VTEP address “10.20.10.10” as the Source IP address.
3/  The physical network delivers the multicast packet to the hosts that joined the multicast group address “239.1.1.10”.
4/  The VTEP on Host 2 receives the encapsulated packet. Based on the outer and inner header, it makes an entry in the forwarding table 
      that shows the mapping of the virtual machine MAC address and the VTEP. In this example, the virtual machine MAC1 running on Host 1 is associated with VTEP IP “10.20.10.10”. VTEP also checks the segment ID or VXLAN logical network ID (5001) in the external header to decide if the packet has to be delivered on the host or not.
5/  The packet is de-encapsulated and delivered to the virtual machine connected on that logical network VXLAN 5001.

https://blogs.vmware.com/vsphere/2013/05/vxlan-series-how-vtep-learns-and-creates-forwarding-table-part-5.html

====

VxLAN Address Learning

BUM is an acronym, standing for Broadcast, Unknown Unicast, Multicast. It is significant, as it is any traffic that has more than one destination.

Consider how Ethernet would handle Unknown Unicast for example. If traffic needs to go to an IP, but the MAC address is unknown, an ARP request will be flooded to the network segment. The owner of the IP will respond, and the rest will ignore the request.

VxLAN is similar in some ways but uses a few tricks to make it more efficient.
