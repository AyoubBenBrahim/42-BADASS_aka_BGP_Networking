```
#shell
ip link add br0 type bridge
ip link add vxlan0 type vxlan id 10 dstport 4789
ip link set eth1 master br0
ip link set vxlan0 master br0
ip link set vxlan0 up
ip link set br0 up


#vtysh
vtysh
conf t
!
interface eth0
 ip address 10.1.1.2/30
 ip ospf area 0
!
interface lo
 ip address 1.1.1.2/32 
 ip ospf area 0
!
router bgp 1
 neighbor 1.1.1.1 remote-as 1
 neighbor 1.1.1.1 update-source lo
 !
 address-family l2vpn evpn
  neighbor 1.1.1.1 activate
  advertise-all-vni
exit-address-family
!
router ospf
!
exit 
exit
exit
ip a sh dev lo
#
```
=====
### script commentary

The first block of code is using the ip command to create and configure a Linux bridge and a VXLAN interface.

`ip link add br0 type bridge` creates a bridge interface named "br0".

`ip link add vxlan0 type vxlan id 10 dstport 4789` creates a VXLAN interface named "vxlan0" with VXLAN identifier (VNI) 10 and destination port 4789.

`ip link set eth1 master br0` adds the ethernet interface "eth1" to the bridge "br0"

`ip link set vxlan0 master br0` adds the VXLAN interface "vxlan0" to the bridge "br0"

`ip link set vxlan0 up` brings up the VXLAN interface "vxlan0"

`ip link set br0 up` brings up the bridge "br0"


The second block of code is using the vtysh command.

`vtysh` enters the vtysh command-line interface

`conf t` enters the configuration mode

`interface eth0` enters the configuration mode for the ethernet interface "eth0"

`ip address 10.1.1.2/30` assigns the IP address 10.1.1.2 with a subnet mask of 255.255.255.252 to the interface eth0

`ip ospf area 0` assigns the OSPF area 0 to the interface eth0

`interface lo` enters the configuration mode for the loopback interface

`ip address 1.1.1.2/32` assigns the IP address 1.1.1.2 with a subnet mask of 255.255.255.255 to the loopback interface

`ip ospf area 0` assigns the OSPF area 0 to the loopback interface

`router bgp 1` enters the configuration mode for BGP with autonomous system number 1

`neighbor 1.1.1.1 remote-as 1` specifies the BGP neighbor with IP address 1.1.1.1 and its autonomous system number is 1

`neighbor 1.1.1.1 update-source lo` specifies that the source IP address of the BGP updates will be the loopback interface

`address-family l2vpn evpn` enters the address family configuration mode for L2VPN EVPN

`neighbor 1.1.1.1 activate` activates the BGP neighbor specified

`advertise-all-vni` Advertise all VXLAN VNIs to the BGP peer

`exit-address-family` exits the address family configuration mode

`router ospf` enters the configuration mode for OSPF

`exit` exits the configuration mode

`exit` exits vtysh

`ip a sh dev lo` command shows the IP address of the loopback interface.

==

neighbor 1.1.1.1 activate  ==> activate address- family for the RR

---


When EVPN is enabled on a VTEP, all locally defined VNIs on that switch and other information (such as MAC addresses) are advertised to EVPN peers.

1/ configured VXLAN interfaces, attached them to a bridge, and mapped VLANs to VNIs.
2/ Enable EVPN between BGP Neighbors
        add the address family evpn to the existing neighbor address-family 

            address-family l2vpn evpn
            neighbor 1.1.1.1 activate

3/ advertise-all-vni (This configuration is only needed on leaf switches that are VTEPs )
        enable the BGP control plane for all VNIs configured on the switch.


https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-41/Network-Virtualization/Ethernet-Virtual-Private-Network-EVPN/Basic-Configuration/


==

ip addr show dev em1	Display information only for device em1
link: Manage and display the state of all network interfaces

ip link add name eth0.110 link eth0 type vlan id 110 ==> Add VLAN 110 on the fly to the interface eth0, naming it eth1.110.

ip COMMAND CHEAT SHEET
https://github.com/yuriskinfo/cheat-sheets/blob/master/cheat-sheets/Linux-ip-route-reference-by-examples.adoc
https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf


neighbor 1.1.1.1 update-source loopback
 If multiple paths exist between the iBGP routers, using a loopback interface as the neighbor address can add stability to the network. With this command, stability can be achieved by providing the loopback interface address as the source address of the TCP/IP session.

"There are two paths between A and B. And that justifies the use of update-source in the neighbor command"


https://shortest.link/55Mo

Its not needed at all by default. You only use it when you want your router to use a specific IP address when exchanging BGP updates with another router.

A common configuration is to have a loopback interface on your router and then to use under your BGP config

router bgp 100

neighbor x.x.x.x loopback 10

But you can have an iBGP between a loopback on R1 and the physical interface on R2.


 the purpose of the the update-source command in BGP.

  is used to specify that we will use the loopback interfaces as the source for the IBGP session.

R4(config-router)# neighbor 2.2.2.2 update-source loopback 0

=====

address-family l2vpn evpn

Selects the L2VPN EVPN address family.

Specifies address family to use and changes to the configuration context for the specified family


  address-family l2vpn evpn
The spine activates the l2vpn evpn address family to signal to its peers that it can process EVPN routes.

====

 neighbor 1.1.1.1 activate

  enables the address-family capability and exchange of information specific to an address family with a BGP neighbor.

===

neighbor 1.1.1.1 remote-as 1

Creates a peer, initiates the connection to the peer, and adds an entry to the BGP neighbor table. Specifies a neighbor with an autonomous system (AS) number that identifies the neighbor as internal to the local autonomous system. 

Neighbor <peer group name> remote-as < as-number>
	Mettez à jour la table de voisins BGP IPv4 avec l’adresse IPv4 du voisin dans le système autonome spécifié.
====

ARP suppression 




Enable EVPN in an iBGP Environment with an OSPF Underlay

You can use EVPN with an OSPF or static route underlay. This is a more complex configuration than using eBGP. In this case, iBGP advertises EVPN routes directly between VTEPs and the spines are unaware of EVPN or BGP.


In a centralized routing deployment, you must configure layer 3 interfaces even if the switch is configured only for layer 2 (you are not using VXLAN routing). To avoid unnecessary layer 3 information from being installed, configure the ip forward off or ip6 forward off options as appropriate on the VLANs. 
https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-42/Network-Virtualization/Ethernet-Virtual-Private-Network-EVPN/EVPN-Enhancements/


 exchange EVPN routes



Configuration de BGP

 https://docs.citrix.com/fr-fr/citrix-adc/current-release/networking/ip-routing/configuring-dynamic-routes/configuring-bgp.html



 The neighbor command must be repeated to configure various parameters such as advertisement interval, activation, and so on.


