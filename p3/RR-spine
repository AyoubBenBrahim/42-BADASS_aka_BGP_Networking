```
vtysh
conf t
!
interface eth0
 ip address 10.1.1.1/30
 ip ospf area 0
!
interface eth1
 ip address 10.1.1.5/30
 ip ospf area 0
!
interface eth2
 ip address 10.1.1.9/30
 ip ospf area 0
!
interface lo
 ip address 1.1.1.1/32
 ip ospf area 0
!
router bgp 1
 neighbor ibgp peer-group
 neighbor ibgp remote-as 1
 neighbor ibgp update-source lo
 bgp listen range 1.1.1.0/29 peer-group ibgp
  !
 address-family l2vpn evpn
  neighbor ibgp activate
  neighbor ibgp route-reflector-client
 exit-address-family
!
router ospf
!
#
exit
exit
exit
ip a sh dev lo
#
```
======

### commentary

`vtysh`: This command starts the VTYSH command-line interface, which is used to configure and monitor various network protocols on a device.

`conf t`: This command enters configuration mode, which allows the user to make changes to the device's configuration.

`interface eth0`: This command enters the configuration for the Ethernet interface "eth0".

`ip address 10.1.1.1/30`: This command assigns the IP address 10.1.1.1 with a subnet mask of /30 to the interface "eth0". This means that the device will be able to communicate with other devices on the 10.1.1.0/30 network.

`ip ospf area 0`: This command enables the OSPF (Open Shortest Path First) protocol on the interface "eth0" and assigns it to area 0. OSPF is a routing protocol used to distribute routing information within a single autonomous system (AS).

`interface eth1`: This command enters the configuration for the Ethernet interface "eth1".

`ip address 10.1.1.5/30`: This command assigns the IP address 10.1.1.5 with a subnet mask of /30 to the interface "eth1". This means that the device will be able to communicate with other devices on the 10.1.1.4/30 network.

`interface eth2`: This command enters the configuration for the Ethernet interface "eth2".

`ip address 10.1.1.9/30`: This command assigns the IP address 10.1.1.9 with a subnet mask of /30 to the interface "eth2". This means that the device will be able to communicate with other devices on the 10.1.1.8/30 network.

`interface lo`: This command enters the configuration for the loopback interface "lo".

`ip address 1.1.1.1/32`: This command assigns the IP address 1.1.1.1 with a subnet mask of /32 to the loopback interface "lo". This is a virtual interface that is always up and can be used for testing and management purposes.

`router bgp 1`: This command enters the BGP (Border Gateway Protocol) configuration mode and sets the local AS (Autonomous System) number to 1. BGP is a routing protocol used to distribute routing information between different autonomous systems.

`neighbor ibgp peer-group`: This command creates a peer group called "ibgp" for BGP internal peers.

`neighbor ibgp remote-as 1`: This command configures the remote AS number for the "ibgp" peer group to 1.

`neighbor ibgp update-source lo`: This command configures the loopback interface as the source for BGP updates for the "ibgp" peer group.

`bgp listen range 1.1.1.0/29 peer-group ibgp`: This command configures BGP to listen for incoming connections on the IP range 1.1.1.0/29 for the "ibgp" peer group.

`address-family l2vpn evpn`: This command enters the address family configuration mode for L2VPN EVPN (Ethernet Virtual Private Network).

`neighbor ibgp activate` This command activates the ibgp neighbor.

`neighbor ibgp route-reflector-client` This command configures the ibgp peer as a route reflector client.

`exit-address-family`
==


neighbor 1.1.1.1 route-reflector-client

(the type of information BGP can distribute)
configures the router as a BGP route reflector and the specified peer as its client. 
Without this, no EVPN routes will be distributed to the leaves.

    =====


    BGP Dynamic Neighbors are a way to bring up  BGP neighbors without specifically defining the neighbors remote IP address.
    Using the BGP Listen Range command you specify a range of IP addresses typically on your Hub site that you trust to become BGP neigbors with you.

    R1#sh run int f0/0

    interface FastEthernet0/0
     ip address 172.16.1.1 255.255.255.0
    R1#sh run | sec router bgp
    router bgp 65000
     bgp listen range 172.16.1.0/24 peer-group DYNAMIC
     neighbor DYNAMIC peer-group
     neighbor DYNAMIC remote-as 65000
    R1#


https://ipwithease.com/dynamic-bgp-peering/


BGP Dynamic Neighbors / BGP Listen Range

The BGP dynamic neighbors feature is supported in some implementations where one end, the listening end, is passive.
It is just told what IP subnet to accept connections from, and is associated with a peer group that controls the characteristics of the peering session.

router bgp 65011
      ...
      neighbor SERVERS peer-group
      neighbor SERVERS remote-as 64512
      bgp listen range 172.16.1.0/24 peer-group SERVERS
      ...
The listen command notifies BGP to accept any BGP connection that is received on the 172.16.1.0/24 subnet and treat the configuration for this peering session to be taken from the peer-group template, SERVERS.

Cloud Native Data Center Networking p 330

========

RR to avoid full-mesh topology(all leafs connected)
RR take the VTEP information and redistribute/Reflect it to the other VTEPs
https://youtu.be/9Hq8W9PZYus?t=2753

To simplify repetition when configuring multiple neighbors, most routing suites support a form of templating called peer group.
The user creates a peer group with a name and then proceeds to configure the desired attributes for neighbor connection (such as remote-as, connection timers, and the use of BFD). In the next step, the operator assigns each real neighbor to the created peer group, thus avoiding the need to type the same boring stuff over and over again.

Cloud Native Data Center Networking p 307




1/ defines a peer-group called ISL.
2/ assign attributes that all members of the peer-group share.
3/ adds this neighbor to the peer-group already defined.



1 / Enable EVPN between BGP Neighbors
    To enable EVPN between BGP neighbors, add the address family evpn to the existing neighbor address-family activation command.

 address-family ==> address families, they are a way for BGP to carry information for different protocols. 

Advertise All VNIs


====

VxLAN Address Learning


One is learning through the data plane, and the other is learning through the control plane.

Data plane learning is the typical flood-and-learn (FL) style. It’s in the original VxLAN specification and is very similar to the way Ethernet learns addresses.

Control plane learning is more recent and sophisticated. This method uses MP-BGP to share MAC address and VTEP information. This feels similar to how BGP learns routes.

https://networkdirection.net/articles/routingandswitching/vxlanoverview/vxlanaddresslearning/


BUM is an acronym, standing for Broadcast, Unknown Unicast, Multicast. It is significant, as it is any traffic that has more than one destination.

Consider how Ethernet would handle Unknown Unicast for example. If traffic needs to go to an IP, but the MAC address is unknown, an ARP request will be flooded to the network segment. The owner of the IP will respond, and the rest will ignore the request.

VxLAN is similar in some ways but uses a few tricks to make it more efficient.

One method is to use multicast. Each VNI is mapped to a multicast group. Each group can have one or more VNI mapped to it.

When a VTEP comes online, it uses IGMP to join the groups that it needs for the VNI’s that it owns.

Now when the switch needs to send BUM traffic, it can send it to the multicast group. Any VTEP that’s part of the group will receive the traffic. Other destinations are not in the group, and will not get the traffic, limiting the scope of flooding. An ARP request, for example, will not go to the entire network, just to the parts it needs to go to.

This is a very efficient way to handle BUM traffic, and it scales well. It does require a multicast infrastructure, which may make it a little more tricky to implement.

https://networkdirection.net/articles/routingandswitching/vxlanoverview/vxlanaddresslearning/

VxLAN Control Plane (EVPN)
BGP-EVPN
BGP operates in the control plane. A normal BGP deployment will share IP reachability information (routes). When integrated with VxLAN, it can also share MAC and VTEP reachability information.

It does this with the EVPN (Ethernet VPN) address family. If you’re not familiar with address families, they are a way for BGP to carry information for different protocols. There are address families for IPv4, IPv6, L3VPN (MPLS), and others.

As all the addresses are learned proactively, there’s no need for flooding.

All switches in the VxLAN topology need to run BGP EVPN. They don’t all need to be running VTEPs. An example is the spine switches in the spine/leaf topology.

The switches peer with each other. The standard BGP rules apply here. This means that you need an iBGP full mesh or route reflectors. The spine switches are good candidates for route-reflectors.

When a host comes online, it announces its MAC address. This can also happen at other times with GARP messages. The local switch will add the MAC into the local BGP database. This is then sent to its peers as a BGP update.

When VTEPs are learned through BGP, they are dynamically added to a whitelist. This prevents rogue VTEP injection. BGP neighbour authentication can be used to prevent rogue peers.

https://networkdirection.net/articles/routingandswitching/vxlanoverview/vxlanaddresslearning/


VXLAN allows us to create layer 2 networks over layer 3 infrastructure. The VXLAN base use case is to connect two or more layer three network domains and make them look like a common layer two domain. In the Data center environment this allows virtual machines on different networks to communicate as if they were in the same layer 2 subnet.

VXLAN bridging is the concept of using the VXLAN protocol to provide layer 2 connectivity across the layer 3 infrastructure. VxLAN itself doesn’t have any control plane and rely on flood and learn mechanism similar to classic switches or VPLS services.

With the usage of EVPN, mac address information can be distributed with MP-iBGP updates. Using route reflectors at the Spine layer is recommended for the scalability. (Spine-Leaf Architecture is the baseline for the VxLAN-EVPN networks.)

http://www.fabricplane.com/vxlan-evpn-bridging/



