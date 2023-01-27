
Forcing my thoughts into an ordered sequence is a torment for me. [...] I squander untold effort making an arrangement of my thoughts.

Ludwig Wittgenstein - Culture and Value

Give me six hours to chop down a tree and I will spend the first four sharpening the axe. - Abraham Lincoln

*******************************************************************

OSI model --> Hussein Nasser's ytube channel

*******************************************************************


7 Application = providing services to the user and for interfacing with the other layers

      ---------

6 Presentation = Encrypts if necessary, zip

      ---------

5 Session = session id

pres + session = convert that thing we want to transmit into serialized info

   establish conx, Dialogue
   
      ---------
                
4 trans = segment, manage ports(src/dest)
  
  Responsible for ensuring that data is delivered reliably and in order, and for managing the flow of data between devices

  segmentation and reassembly
            
  speed matching mechanism
            
TCP/UDP


            
layer 5 and 6 are useless, we skip them most of the time

      ---------
     
3 net/ip protocol = Packets / add ip(src/dest) + other headers, routing

      ---------

2 Data Link = moving frames(HDLC/PPP) - Hop to Hop data delivery

  Responsible for creating a reliable link between two devices on a network

  MAC + LLC
              
  - error control/ flow control(speed matching)
              
  Frames / deal with mac address / (error detection..) small pieces ARP(Mac->IP) | deals with mac adrs
              
  corrects errors that may occur in the physical layer
              
  Sliding Window Protocol (https://youtu.be/LnbvhoxHn8M)
              
  High-level Data Link Control (HDLC) is a group of communication protocols of the data link layer for transmitting data between network 
              
  points or nodes. Since it is a data link protocol, data is organized into frames. A frame is transmitted via the network to the 
              
  destination that verifies its successful arrival.

  Multiple Access Protocols = avoid talking simultaneously aka collision 
              
  Random access protocol (CSMA/CD..) = any station has the right to acess and send data
                                          
  CSMA = carrier(medium/chanel) sense protocol aka "listen before talk"
  
      --------- 

1 Physical = frame into ones and zeros (electricity doesn't has direction aka broadcast) 

  converts the digital bits into electrical, radio, or optical signals.

this data(binary) will be sent to everyone

then send them up to make sense

*******************************************************************

Data link receives all the frames checks adres and rejects them if not the right host

each layer towards up removes its added headers..

switch goes only to layer 2

hub to layer 3

router 3

Hub = im not smart enough so broadcast the frame to all, machines accept it or drops it (its for me or not)

the network become very busy

switch works smarter(looks at the content) + remembers (tables)

*******************************************************************

protocol data unit (PDU) is a single unit of information transmitted among peer entities of a computer network.

(packet, segment, datagram)

PDU (Protocol Data Unit) is a generic term used to refer to the packets in different layers of the OSI model.

In Open Systems Interconnection (OSI) terminology, a service data unit (SDU) is a unit of data that has been passed down from an OSI layer or sublayer 
to a lower layer. This unit of data (SDU) has not yet been encapsulated into a protocol data unit (PDU) by the lower layer. That SDU is then 
encapsulated into the lower layer's PDU and the process continues until reaching the PHY, physical, or lowest layer of the OSI stack.
At the receiving device the data is passed from the lowest to the highest layer as a series of SDUs while being successively stripped from each layer's 
header or footer until reaching the topmost layer, where the last of the data is consumed

Packet:

data bytes have a specific format in the OSI networking model since each layer has its specific unit

The basic unit of communication between a source and a destination in a network is a packet.

Data sent through the network is divided into packets

Fragment:

Packets can often be larger than the MTU, so each packet is also divided into smaller pieces of data called fragments.

each network has its own MTU capacity. 

If the packet length is bigger than the MTU, the network layer checks the Don’t Fragment (DF) flag associated with the packet. If the DF flag is 1, we discard the packet. Otherwise, the network layer decides the size of the fragments, 

create the header, encapsulate the fragments within the header, and send them to the next layer:

Frame:

The main difference between a packet and a frame is the association with the OSI layers.

While a packet is the unit of data used in the network layer, 

a frame is the unit of data used in the OSI model's data link layer. 

Frames are not much different from packets. They contain more information about the data. 

The process of framing consists of adding some extra bytes into the packet turning it into the frame

Packet + extra info/headers = Frame


Datagram:

While TCP uses packets in connection-oriented protocols, datagrams are used in UDP

*******************************************************************

Application = Data

Transprt = Segments/Datagrams  : ACK, packet segmentation(MTU)

Internet = Packets = addressing , routing

Data Link = Frames

Physical = bit

(https://youtu.be/-hkF4OyxzNQ)

*******************************************************************

VLAN is a layer 2 concept (frames)

 VLAN tagging
 
*******************************************************************

A switching loop or bridge loop occurs in computer networks when there is more than one layer 2 path between two endpoints

(e.g. multiple connections between two network switches or two ports on the same switch connected to each other).

The loop creates broadcast storms as broadcasts and multicasts are forwarded by switches out every port, 

the switch or switches will repeatedly rebroadcast the broadcast messages flooding the network.

Since the layer-2 header does not include a time to live (TTL) field, if a frame is sent into a looped topology,it can loop forever

Routing loops are tempered by a time to live (TTL) field in layer-3 packet header; Packets will circulate the routing loop until their TTL value 
expires.

 **No TTL concept exists at layer 2 and packets in a switching loop will circulate until dropped**
 **Layer 2 frame does not have TTL**
 
 from Reddit [answer](https://www.reddit.com/r/networking/comments/2rnxzh/why_ethernet_frames_dont_have_ttl/)
 ```
 A TTL type field doesn't "solve" the loop problem anyway,
 it will stop a particular frame from circling forever but it doesn't fix everything.

Imagine a triangle network of just 3 switches: A joined to B and C, B joined to A and C, C joined to A and B.

A broadcast goes into switch A, which floods it to B and C. Now there are 2 frames.

B sees the broadcast and floods to A and C. C sees the broadcast and floods it to A and B. Now there are 4.

Even with a modest TTL type value of 16 that one frame would grow exponentially to 216=65536 frames.
Every endpoint on the network would be drowning in broadcast / multicast / unknown unicast traffic. More nodes / links would make it worse.

TTL is good at layer 3 as multi-hop traffic doesn't flood (multicast uses other mechanisms to prevent loops)
 ```
 
Broadcast Storm

A packet that induces such a storm is occasionally nicknamed a **Chernobyl packet**.

a broadcast storm can be instigated for the purpose of a denial of service (DOS) 

==> Spanning Tree Protocol (STP) prevents broadcast loops, blocks the redundant link(will be used when active links or ports goes down)

  (https://youtu.be/GSKoQ8ZR8rw?list=PLBlnK6fEyqRgMCUAG0XRw78UA8qnv6jEx)

Layer 2 loops


Fragmentation is a phenomenon in which storage space is used inefficiently, reducing capacity or performance and often both.

Segmentation is the process of dividing the computer’s primary memory into segments or sections.
a process in which free memory space is broken into little pieces. In this, memory blocks cannot be allocated to processes due to their small size and  such blocks remain unused. It usually occurs in dynamic memory allocation system when many of free blocks are too small to satisfy any request

*******************************************************************

ip = location of a person (router friendly)

MAC = name of the person (switch friendly)

(https://youtu.be/FkiTOMn-XGw?list=PLBlnK6fEyqRgMCUAG0XRw78UA8qnv6jEx)

reaching the city (find the network)

reaching the appartment (find the host)

reaching the right person (the port)

ip adr says who are u in the network

subnet mask says who are ur neighbors in the network

*******************************************************************

Switch => three operations :

  Learn
        
  Flood
        
  Forward
        
*******************************************************************

 internet is nothing more than a series of routers

RIP = routing information protocol

RIP uses the nbr of hopes(routers) between networks (max 15 router)

OSPF calculates the cost of a route/ based on bandwidth (link-state protocols) = bandwidth-based metrics

RIP cares only about nbr of routers in the path/ uses hopes to determine the best route = distance based

a value of each route = metric

OSPF cost link = 100 000 000 / bandwidth

100 Mbps ==> 100 000 000 / 100 000 000 bps = 1

right protocol/route is chosen based on administrative distances(rank)..

*******************************************************************

AS

 autonomous system (AS) is a collection of connected Internet Protocol (IP) routing prefixes under the control of one or more network operators on behalf of a single administrative entity or domain, that presents a common and clearly defined routing policy to the Internet

  In simpler terms, an AS can be thought of as a connected group of IP networks managed by a single administrative entity, such as a university, government, commercial organization or other type of internet service provider (ISP).

  AS is sometimes described as a set of routers that are managed by a single administrative entity. 

  An AS has many different sub-networks with combined routing logic and common routing policies.

  Autonomous systems exchange routing information based on their routing policies

  Each autonomous system is assigned a globally unique number called an Autonomous System Number (ASN). The number serves as an identifier for the AS 
  
  and is used when exchanging routing information with other autonomous systems.

  (IANA) manages the ASN system and coordinates the distribution of ASNs across five global regions. 

  BGP can direct packets along the most efficient routes to their target AS.

  Networks within an AS communicate routing information to each other using one or more IGPs - interior gateway protocols.
  
  In contrast, communications between autonomous systems rely on a single EGP Exterior Gateway Protocol -- Border Gateway Protocol BGP -- which makes it possible to share routing information and efficiently route data packets between AS
  
*******************************************************************

To use an analogy, an AS is like a city with many streets.

A Network Prefix (/n) is one street, 

and an IP address is one particular house.

Network Packets are like cars traveling from one house to another,

and BGP is like a navigation app that helps them take the best possible route.

https://www.imperva.com/learn/ddos/border-gateway-protocol-bgp/

let's say you're in city A and you want to drive to city Z. To get there, you'll need to take a route through different cities, or networks. Without BGP, your route might take you through many cities that are not the most direct or efficient. But with BGP, a navigation system can check its BGP table and see the best route to get you directly from city A to city Z, avoiding any unnecessary stops.

BGP also helps with dealing with any issues that might come up on the way. For example, if there is a traffic jam or road construction on the way, BGP can find an alternative route to ensure you still get to your destination quickly.

Additionally, BGP also allows different road-map service providers to share information about their networks, so that all the road-map service providers have a complete map of the road network and can make the best routing decisions possible.

*******************************************************************

  QoS ==> quality of service 
  
  (ToS) type of service  field = a packet would be placed in a prioritized outgoing queue

*******************************************************************

  An ISP (internet service provider) is a company that provides individuals and organizations access to the internet and other related services. An ISP has the equipment and the telecommunication line access required to have a point of presence on the internet for the geographic area served.

ISPs make it possible for customers to access the internet while also providing additional services such as email,

*******************************************************************

Intra / Inter

IGP / EGP

Intra-, which comes from the Latin intra (meaning “within”)

inter, meaning "among, between”

IGP / intra-autonomous system routing protocols (OSPF - RIP)

EGP/ inter-AS routing protocols (BGP)

https://sabercomlogica.com/en/intra-as-and-inter-as-routing/

OSPF EIGRP BGP RIP IS-IS

https://youtu.be/KjNYEzEBRD8

*******************************************************************

best simple definition i come across

**BGP = the glue that holds internet together**

*******************************************************************

the ISP uses an IGP internally to build reachability between its routers. It then implements iBGP on top of this to actually create a coherent routing policy within its autonomous system and then eBGP to carefully expose network information to- and receive information from- other autonomous systems.

https://www.quora.com/What-is-the-need-of-iBGP-when-we-have-other-routing-protocols-like-EIGP-OSPF

*******************************************************************

IS-IS vs OSPF

Both IS-IS and Open Shortest Path First(OSPF) are link state protocols, and both use the same Dijkstra algorithm for computing the best path through the network

IS-IS differs from OSPF in the way that "areas" are defined and routed between.

IS-IS routers are designated as being:

Level 1 (intra-area);

Level 2 (inter area);

or Level 1–2 (both).

Routing information is exchanged between Level 1 routers and other Level 1 routers of the same area, and Level 2 routers can only form relationships and exchange information with other Level 2 routers.

Level 1–2 routers exchange information with both levels and are used to connect the inter area routers with the intra area routers.


In OSPF, areas are delineated on the interface such that an area border router (ABR) is actually in two or more areas at once, effectively creating the borders between areas inside the ABR, whereas in IS-IS area borders are in between routers, designated as Level 2 or Level 1–2. The result is that an IS-IS router is only ever a part of a single area.


ISIS runs on top of data link layer(encapsulated in Data link layer), whereas OSPF runs on top of IP


 Administrative Distance of OSPF is 110 and ISIS is 115.

*******************************************************************

 So, BGP performs “path vector” routing by advertising a vector of paths (lists of ASes) rather than just distances (hop counts.)

 Path-vector routing protocol : It is different from the distance vector routing and link state routing. 
 A path vector protocol guarantees loop free paths through the network by recording each hop the routing advertisement traverses through the network.

 Each entry in the routing table contains the destination network, the next router and the path to reach the destination. 

 Updates that have looped through the network and returned to the same node are easily detected and discarded.

 advertise the reachability of networks
 
 It has three phases:
 
  Initiation
    
  Sharing
    
  Updating

*******************************************************************

  Why use iBGP inside an Autonomous System, if IGP protocols fulfill the need for internal communication:
    
  iBGP isn't really used for internal routing, it is used by all your eBGP routers to share their routes.
    
  EBGP exchanges internet (i.e. global) prefixes/routes from other Autonomous systems (EBGP peers). To reach those prefixes, someone should 
    
  distribute/advertise across the internal network, IBGP comes to the rescue. IBGP advertises these global prefixes to the routers within the 
    
  Autonomous System (AS). Why not IGP does the same? As IGP is quite chatty (i.e. in fact not designed for handling a large number of routes),

  https://networkengineering.stackexchange.com/questions/3909/why-use-ibgp-inside-an-autonomous-system-if-igp-protocols-fulfill-the-need-for

*******************************************************************

Multiprotocol Label Switching (MPLS) => a packet forwarding technology ==>label Switching =>faster lookup => ip header is still there

An MPLS-capable router is often referred to as a label- switched router

MPLS-capable router need not extract the destination IP address and perform a lookup of the longest prefix match in the forwarding table.

In a nutshell, packets in an MPLS network are prefixed with an MPLS header (called a label stack). 

The label stack is the only thing examined by the MPLS switches

Therefore, no traditional routing table lookups are required, which in theory makes this **a much faster solution than more traditional IP-based 
solutions.**

- Network Warrior

*******************************************************************

```
do sh ip route rip
int fa1/0
interface fastEthernet0/0
ip add ......
router
router ospf 1
int fa1/0 ==> enable ospf on this interface
ip ospf
ip ospf 1 area 0 (interface belongs to)
```
******

redistribution of routes

===
```
int fa 1/0
ip add 40.1.1.1 255.....
router bgp 1 (==> ASN)
neighbor 40.1.1.2 255.255.255.252 remote-as 2
do sh ip bgp summ

ip address 10.10.1.65 255.255.255.224

show ip route
show ip ospf interface

show ip ospf neighbor
(This command gives you a list of OSPF neighbors that are directly connected to the router)
```

*******************************************************************

OSPF

 Link-state advertisements 

 how updates are sent between areas. 
  
These updates are called link-state advertisements , or LSAs. An LSA is an OSPF multicast that describes a routing change or routing update to other routers or areas. 

Each network segment needs a designated router, known as the DR

it is the job of the DR to multicast the update to all routers within its area. 
 
A backup designated router (BDR) must also be selected. This router becomes the DR if the DR becomes unavailable. 

You configure OSPF much like you configure other routing protocols: use the router command to establish the protocol and a process number, and use the network command to tell the router which networks it's responsible for. It's easy to become confused: with OSPF the network command takes a wildcard mask, not a subnet mask. 
```
router ospf  99
  network 10.10.1.0 0.0.0.255 area 0
```
Here, we establish an OSPF routing process with the process ID 99. This routing process is responsible for 
although they are frequently called AS numbers, the numbers associated with EIGRP and OSPF routing processes are really just process IDs

the network 10.10.1.0/24, which belongs to area 0. Therefore, this router is part of the OSPF backbone area. Do not confuse the process ID (99) with the area ID (0).

ASBR (autonomous system border router)


192.168.2.0 0.0.0.255

that wildcard masks look like subnet masks, but they aren't;

Wildcard = 255 - Subnet

*******************************************************************

Route Summarization

you should notice that there are four routes pointing to 172.16.1.1 for the various 10.x.x.x networks. 

We can simplify the routing table

*******************************************************************

BGP

We don't provide a network mask, as BGP assumes the old classful addressing scheme when a mask isn't provided explicitly.
```
router bgp 500
network 10.10.2.0 mask 255.255.224.0
```
Next, we need to define our routing peers with the neighbor command. This step also defines whether we are using iBGP or eBGP. If our neighbor router has the same AS number, we are using iBGP. If our neighbor has a different AS number, we are configuring eBGP. In this case, we configured a neighbor with a different AS number, meaning that we are using eBGP. 
```
router bgp 500
neighbor 192.168.1.5 remote-as 400
```
By default, BGP summarizes routes on class boundaries.

Configuring "no auto-summary" disables automatic summarization.

When an iBGP router advertises a route, it advertises the next hop of the route as it learned it. The next-hop-self keyword (used with the neighbor command)
 
tells the router to rewrite the route's next hop as itself.

*******************************************************************

 Circuit Switching vs. Packet Switching vs Message Switching
 
https://youtu.be/-HlJ4psu5aU

*******************************************************************

VXLAN

Extensible: able to be extended or stretched; extendable.

*******************************************************************

Network virtualization

*******************************************************************

VXLAN is the most commonly used protocol to create overlay networks that sit on top of the physical network, 

*******************************************************************

What Problem Does VXLAN Solve?

Data centers have rapidly increased their server virtualization over the past decade

VXLAN is a technology that allows you to segment your networks (as VLANs do) but also solves the scaling limitation of VLANs and provides benefits that VLANs can

*******************************************************************

How Does VXLAN Work?

VXLAN allows us to create layer 2 networks over layer 3 infrastructure. 

VXLAN tunneling protocol that encapsulates Layer 2 Ethernet frames in Layer 3 UDP packets, enables you to create virtualized Layer 2 subnets, or segments, that span physical Layer 3 networks. Each Layer 2 subnet is uniquely identified by a VXLAN network identifier (VNI) that segments traffic.

The entity that performs the encapsulation and decapsulation of packets is called a VXLAN tunnel endpoint (VTEP).

*******************************************************************

 Overlay Network 

overlay = cover/layer

something laid as a covering over something else.

An overlay network is a computer network based on another network. 

An overlay network separates network services from the underlying infrastructure by encapsulating a packet into another packet.

Using nested networks, multiple layers of network abstraction can be created to run virtual network layers

Network overlays are a method of using software virtualization to create additional layers of network abstraction 

endpoints designated by an identification tag

We need overlay networks due to the limitations of the underlay network.

*******************************************************************

An underlay network, as the name implies, is the underlying physical infrastructure of overlay networks.

On an underlay network, devices such as switches, routers, load balancers, and firewalls can be interconnected.

The underlay network can be a Layer 2 or Layer 3 network. A typical example of Layer 2 underlay network is an Ethernet network, on which VLANs are created.

Overlay Network (VXLAN, NVGRE, SST, GRE, NVO3, and EVPN)

To get rid of the limitations of underlay networks, virtual overlay networks can be created over underlay networks using network virtualization technologies.

Tunnels are established between interconnected overlay devices.

When sending a data packet, a device adds a new IP header and a tunnel header to the data packet and shields the inner IP header. The data packet is then forwarded based on the new IP header. When the data packet is received by another device, the device removes the outer IP header and tunnel header to obtain the original data packet. 

 extra overheads

 The scalability is high. For example, a VLAN supports a maximum of 4096 discriminators, whereas a VXLAN provides a maximum of 16 million discriminators.

https://info.support.huawei.com/info-finder/encyclopedia/en/Overlay+network.html

*******************************************************************

Virtual eXtensible Local Area Network (VXLAN) is one of the Network Virtualization over Layer 3 technologies / 
an extension to Virtual Local Area Network (VLAN)

 VXLAN encapsulates a Layer 2 Ethernet frame into a UDP packet and transmits the packet over a Layer 3 network.
 
 VXLAN network identifier (VNI) that is similar to a VLAN ID to a VXLAN header. 

 VXLAN is essentially a tunneling technology.

  It establishes a logical tunnel on the IP network between the source and destination network devices to encapsulate user-side packets and forward them through the tunnel. 

VXLAN has become the mainstream technology for constructing data center networks

*******************************************************************

What Are the Differences Between VXLAN and VLAN?
-----

VLAN is as a traditional network isolation technology. In accordance with standards, a maximum of about 4k VLANs are available, which cannot meet the tenant isolation requirements of large data centers. In addition, each VLAN is a small and fixed Layer 2 virtual network, which does not support large-scale dynamic VM migration.

VXLAN overcomes the preceding disadvantages of VLAN. VXLAN uses the 24-bit VNI field (as shown in Figure 1-5) to identify up to 16M tenants, compared to a maximum of 4k tenants in VLAN. 

during VXLAN packet encapsulation, a 24-bit VNI is added to a VXLAN packet

A VNI identifies a tenant.

  https://support.huawei.com/enterprise/en/doc/EDOC1100086966

  VTEP ==> VXLAN Tunnel Endpoint 
  
  VNI  ==> VXLAN Network Identifier 

*******************************************************************

 spine-leaf architecture / Clos topology for data center networks

more spins =  more bandwidth + low cost

Data Center = horizontal(east west) traffic

a two-layer network topology composed of spine and leaf switches.

  Three-Tier data center networks were the generally recommended data center network design in the past. They worked very well when most of the traffic moved North-South (from outside the data center in) or vice versa. A packet flows to the core, is routed to the correct distribution switch, then forwarded on to the access switch where the server was connected; 

  A spine-leaf architecture is a data center network topology that consists of two switching layers—a spine and a leaf. The leaf layer consists of access switches that aggregate traffic from servers and connect directly to the spine or network core. Spine switches interconnect all leaf switches 

  https://forum.huawei.com/enterprise/en/spine-leaf-vs-three-tier-network-architecture/thread/782313-100723

  The spine-leaf architecture consists of only two layers of switches: spine and leaf switches. The spine layer consists of switches that perform routing and work as the core of the network. The leaf layer involves access switches that connect to servers, storage devices, and other end-users. This structure helps data center networks reduce hop count and reduce network latency.

  https://community.fs.com/blog/leaf-spine-with-fs-com-switches.html

The spine simply ensures that VTEP-to-VTEP communication is done efficiently between different leafs by using the underlay IP network

*******************************************************************

  the difference between a subinterface and a loopbackinterface?

  There is no physicality to a Loopback (Lo).  There's no physical port, there's no cable, and it doesn't actually go anywhere.  I think of a Lo as a placeholder for an IP address, and since a Lo never goes down, the IP address is always available.

  Sub-ints are subdivisions of an actual physical connection.  It's associated with an actual port, an actual cable, and it actually does go somewhere, and carries traffic.

A loop back interface is a software interface which can be used to emulate a physical interface. By default, router doesn’t have any loopback interfaces (loopback interfaces are not enabled by default), but they can easily be created. 

  Loop back interface is a virtual interface that sits directly under the IOS. IOS crash or hardware failure will affect this interface.

  The loopback interface is used by a local system to test if its own network interface is working. A message is sent from the local system to the loopback interface address (default IP address 127.0.0.1 with IPv4). If the loopback interface receives the message then the system knows its network interface is working properly. The loopback interface is never used to send data to another machine.

  IP datagrams sent by a host to a 127.x.x.x loopback address are not passed down to the data link layer for transmission. Instead, they “loop back” to the source device at the IP level. In essence, this represents a “short-circuiting” of the normal protocol stack; data is sent by a device's layer three IP implementation and then immediately received by it.

The purpose of the loopback range is testing of the TCP/IP protocol implementation on a host. Since the lower layers are short-circuited, sending to a loopback address allows the higher layers (IP and above) to be effectively tested without the chance of problems at the lower layers manifesting themselves.

===> "You can also configure the loopback address as the Router ID for routing protocols like OSPF and BGP."

https://study-ccna.com/loopback-interface-loopback-address/

Loopback interface’s IP Address determines a router’s OSPF Router ID. A loopback interface is always up and allows Border Gateway Protocol (BGP) neighborship between two routers to stay up even if one of the outbound physical interface connected between the routers is down. 

https://www.omnisecu.com/cisco-certified-network-associate-ccna/what-is-loopback-interface-in-a-router.php

What is OSPF Router ID
https://www.omnisecu.com/cisco-certified-network-associate-ccna/what-is-ospf-router-id-and-how-to-configure-ospf-router-id.php

Service providers typically choose between OSPF and IS-IS as their IGP.

Each router stores the information in their link-state database (LSDB). Routers send LSAs to their neighbours who forward them to their neighbours until every router in the network area has received them –  this process is called flooding.

*******************************************************************

Ethernet VPN (EVPN)

VXLAN = L2 frame encapsulated in UDP + ID for VPNs
VXLAN Header

EVPN = a succesor of VPLS (Virtual Private LAN Service)
MPLS

VPLS = a way to provide Ethernet-based multipoint to multipoint communication over MPLS networks.

EVPN is just a successor of MPLS based L2VPN/L3VPN and that VXLAN can work on itself by just manual setup or multicast flood-and-learn. 

 MPLS = IPVPN
 
 MPLS = L2.5
 
 VPLS = L2 VPN

a WAN behaves like LAN

 virtual private LAN service (VPLS) ensures that geographically isolated user sites can communicate over metropolitan area networks (MANs) and wide-area networks (WANs) as if they were on the same local area network (LAN). 

 MPLS network is a Layer 2 switched network that allows users to communicate with each other similarly to communication over a LAN.

 VPLS becomes increasingly important for enterprises setting up branches in different regions. 

VPLS is a specific MPLS-based Ethernet technology that supports P2MP communication.

VPLS integrates the advantages of both Ethernet and MPLS to provide a comprehensive multipoint communication solution. By emulating traditional LAN functions, VPLS enables users on different LANs to communicate with each other over MPLS networks as if they were on the same LAN.

*******************************************************************


 P2MP vs P2P (Point-to-Point and Point-to-MultiPoint)
 ------>
 in the context of telecommunication/connection


P2P = links two endpoints 

P2MP = For businesses with two or more locations, allows three or more separate points to work 

In the context of telecommunication, a point to point communication (also known as P2P) is an established connection between two nodes that can be used to communicate back and forth. 

A multicast routing protocol is a mechanism for constructing a loop-free shortest path from a source host that sends data to the multiple destinations that receives the data. 

*******************************************************************

As an MPLS-based point-to-multipoint (P2MP) Layer 2 Virtual Private Network (L2VPN) service provided over a public network, the virtual private LAN service (VPLS) ensures that geographically isolated user sites can communicate over metropolitan area networks (MANs) and wide-area networks (WANs) as if they were on the same local area network (LAN). VPLS is also called the transparent LAN service (TLS).

Users located in different geographical regions communicate with each other over different provider edge (PE) devices. An MPLS network is a Layer 2 switched network that allows users to communicate with each other similarly to communication over a LAN.

https://forum.huawei.com/enterprise/en/vpls-vs-mpls-what-s-the-difference/thread/776591-861

VPLS

securely and seamlessly connect multiple LANs over the Internet, making them appear as if they were all on the same LAN. 

creating a virtualized Ethernet

VPLS creates a simulated 'pseudo-LAN' environment. 

see the image

https://www.fiber-optic-tutorial.com/vpls-vs-mpls-whats-difference.html

*******************************************************************

VXLAN to discover neighbors:

  ==> Flood and learn = to build MAC tables (consumes a lot) 
  
  ==> BGP EVPN (VTEP switches share info with BGP)
  
  https://youtu.be/RSbbIMwgUys

*******************************************************************

  why using VXLAN and not VLAN
  
  
  2^12 vs 2^24 =  broadcast domains

  VXLAN is a tunneling encapsulation protocol that uses mac in UDP encapsulation.

  STP and you have multiple redundancy links between the switches, only one would be sanctionable 
  cannot use half of the link efficiently,

  VXLAN maps VLAN ID into the VXLAN VNID. Instead of just carrying the VLANs within the same layer2 domain, you can stretch the VLAN from one location to another over layer3 or underlay network. 

  https://getlabsdone.com/what-is-vxlan-overlay-and-underlay-networks/

  The traditional network devices cannot read the VXLAN packet, to use VXLAN you need to use VTEP known as vxlan tunnel endpoint device. VTEP’s can encapsulate and decapsulate the VXLAN packets; there are two types of VTEPs. Hardware-based and software-based. VTEP’s are usually configured with IP addresses.


  To make it simpler, let me put it this way:

Have you ever worked in virtualization such as VMware or KVM?

In virtualization, the host where you have the VM’s resides is called the Host machine (the hypervisor), and the VM’s are called Guest VM’s. 

Similarly, The physical networks are called the Underlay network, and the network that runs on top of the physical networks is called the overlay network.

*******************************************************************

RR

iBGP can utilize the function of a route reflector (RR). An RR is allowed to receive iBGP information and then “reflect” the information to any of its RR clients—that is, all known iBGP neighbors. 


MAC over IP/UDP overlay

The transport network, termed the underlay, needs to account for the additional bytes incurred by the overlay headers. In the case of VXLAN, typically, the transport network needs to be provisioned for an additional 50 bytes in the maximum transmission unit (MTU). 

Increase MTU (9000 MTU) to avoid fragmentation = In computer networking, jumbo frames are Ethernet frames with more than 1500 bytes 

/30  is the most common prefix size for P2P IP networks. A typical /30 prefix provides one network ID address, two usable IP addresses, and one broadcast ID address. While using this prefix size is an efficient way of assigning IP addresses to P2P networks, note that 50% of the addresses (the network ID address and broadcast ID address) are unusable.

Alternatively, a more efficient option from an IP address assignment perspective would be to use a /31 prefix length. The IP subnet of a /31 prefix consists of only two IP addresses, both capable of being used in the P2P network. With the /30 prefix, the additional two nonusable IP addresses (network ID and broadcast ID) are overloaded due to usage with the two usable IP addresses. However, a /31 prefix provides only two usable IP addresses, without network ID and broadcast ID addresses, and therefore overloading does not occur. By using /31 prefixes instead of /30 prefixes in the underlay network, all the IP addresses in the area of P2P networks are usable.

Consider the match for a network with 4 spines and a 40-leaf underlay with a /31 prefix: 4 spines × 40 leafs = 160 P2P links

= 160 links × 2 (/31) + 40 leafs + 4 spines

= 320 IP addresses for P2P links + 44 IP addresses for loopback interfaces

= 364 IP addresses aggregate required => /23 prefix (512 IP addresses)

-- Building Data Centers with VXLAN BGP EVPN_ A Cisco NX-OS Perspective-Cisco Press (2017)

BGP as underlay routing protocol requires greater design and configuration time in a spine–leaf underlay compared to SPF-based routing protocols. 

BGP, as a hard-state protocol, sends out updates only when there is a change in network reachability. Hence, it does not incur the periodic update overhead that link-state protocols like OSPF have in terms of periodic updates

when using eBGP in the underlay, the overlay also needs to be configured in eBGP.

With eBGP, the next-hop attribute is always set, which means the neighbor of a route becomes the next hop. In the spine–leaf topology, this would be inappropriate, as the spine would generally not host a VTEP interface and would thereby lack the capability to encapsulate/decapsulate VXLAN traffic. The EVPN advertisement sent out by a leaf, with regard to its locally attached hosts or prefixes, needs to be relayed by the spine to other leafs without a change in the next-hop attribute, which thereby allows VXLAN tunnels to be established end-to-end from leaf to leaf. This is achieved by setting a specific configuration in eBGP where the next-hop attribute is set to unchanged. 

-- Building Data Centers with VXLAN BGP EVPN_ A Cisco NX-OS Perspective-Cisco Press (2017)

*******************************************************************

virtual network ID

VLAN = 12 bits

MPLS = 20 bits

VXLAN-GRE = 24 bit

p 133 - chap 6: Network virtualization

*******************************************************************

The main reason MPLS was developed in the 1990s was to reduce the amount of IP routing lookups.
The idea behind MPLS is to “switch” based on labels with lookups that use exact matching instead of the compute-intensive longest prefix IP routing lookups.

VXLAN extends the layer 2 accrross an ip fabric

Overlay =  the original frame comming from a end ponit  

Ethernet VPN (EVPN) is a technology for carrying layer 2 Ethernet traffic as a virtual private network

EVPN is a Control plane: routing info,  defines what to do with incoming packets

MP-BGP EVPN carries info: MAC, IP, network prefix, VNID, VTEP IP + distributes MAC, IP info avoiding flooding

https://youtu.be/9Hq8W9PZYus?t=2472


EVPN is a solution that provides the control plane for network virtualization. In the simplest of terms, EVPN is a technology that connects L2 network segments separated by an L3 network. EVPN accomplishes this by building the L2 network as a virtual Layer 2 net‐ work overlay over the Layer 3 network. It uses BGP as its control protocol, and in the data center it uses VXLAN for packet encapsulation.

It uses BGP as the control protocol to exchange reachability information for virtual networks

VXLAN-EVPN adopted by Data Centers in 2017

VXLAN needs to know which VTEPs2 are interested in which virtual networks, so that multidestination frames such as Broadcast, Unknown unicast, and Multicast (BUM) traffic can be delivered only to the interested endpoints.

EVPN goes even further and also provides support for the following:

• ARP/ND suppression

• Routing

• Multihomed nodes

• L3 multicast

Cloud Native Data Center Networking p 335

*******************************************************************

 VTEP, as a memory refresher, is the edge of the network virtual overlay, the place where the non-virtual network meets the virtual network; the place where packets are encapsulated on their way into the overlay and decapsulated on their way out of the overlay. 

 OSPF (or IS-IS) is used to build the routed underlay, and iBGP is used to exchange virtual network information.

 Cloud Native Data Center Networking p 337

 When packets can be encapsulated post-routing or packets can be routed post-decapsulation, this functionality is often referred to as Routing In and Out of Tunnels (RIOT).

 - p 132

iBGP peering is between leaves for the use case of NVO3 in a Clos topology. This means that each leaf sets up an iBGP peering session with every other leaf. This full-mesh iBGP peering quickly becomes unscalable and fragile. To overcome this problem, two separate solutions were invented: BGP confederations and Route Reflectors (RRs). Of the two, RR is by far the most popular solution and the way people consider deploying iBGP

 Cloud Native Data Center Networking p 338


RR follows a hub-and-spoke model, in which all the iBGP speakers connect to a group of central RR servers. The job of the RRs is to compute the best path for a route and advertise that to each of the RR clients.

 Cloud Native Data Center Networking p 339

The spines in a Clos topology act as RRs. The spines are part of the underlay and have no knowledge of the virtual networks. 

The spines are used as iBGP RRs. 

  - Cloud Native Data Center Networking p 340

EVPN is considered to be a kind of an L2 VPN.

The spines are only part of the underlay. 

*******************************************************************

1994: BGP

1996: VPN

1999: MPLS (L3VPN)

2005: VPLS  (https://www.researchgate.net/publication/352384676_A_survey_of_Virtual_Private_LAN_Services_VPLS_Past_present_and_future)

2011: VXLAN

2017: EVPN

*******************************************************************

 most VXLAN networks use an IPv4 underlay exclusively.

  Cloud Native Data Center Networking p 258

  Bridging operates at Layer 2 of the OSI reference model while routing operates at Layer 3. A set of logical ports configured for bridging can be said to constitute a bridging domain.

  Configure interfaces as a bridge to deploy security devices in a topology without reconfiguration of the IP routing scheme.

  Bridge interfaces connect two different interfaces (bridge ports). Bridging two interfaces causes every Ethernet frame that is received on one bridge port to be transmitted to the other port. Thus, the two bridge ports participate in the same Broadcast domain (different from router port behavior). 

  they behave in most respects as if they were a single network. 
  
*******************************************************************

  VXLAN is an overlay network technology. Overlay network can be defined as any logical network that is created on top of the existing physical networks. VXLAN creates Layer 2 logical networks on top of the IP network. 

VXLAN encapsulates original MAC frames in to a UDP header (shown below), and all vSphere hosts participating in VXLAN acts as tunnel end points. They are called Virtual Tunnel Endpoints (VTEPs).

https://blogs.vmware.com/vsphere/2013/04/vxlan-series-different-components-part-1.html

*******************************************************************

a unique IP address range is assigned as Multicast group IP address. It is a Class D address range from 224.0.0.0 to 239.255.255.255. Each address in this range designates a multicast group. Some of the addresses are reserved.

Any node (computer/user) in the network can join a multicast group using Internet Group Management Protocol (IGMP).

IGMP-PIM

The network devices use these protocols to learn about which nodes have joined which multicast groups and where the nodes are in the network.

The layer 2 switches monitor the IGMP query and report messages to find out which switch ports are subscribed to which multicast group. This functionality of a layer 2 switch is called IGMP snooping.

https://blogs.vmware.com/vsphere/2013/05/vxlan-series-multicast-basics-part-2.html

*******************************************************************

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

*******************************************************************

VxLAN Address Learning

 how the destination MAC addresses are learned? What about the VTEPs? How do the switches know the VTEP IP

 In short, there are two ways to do this. One is learning through the data plane, and the other is learning through the control plane.


When frames from one of these VNI’s arrives at the Ingress VTEP, the VTEP encapsulates it with UDP and IP headers.

The encapsulated packet is sent over the IP network to the Egress VTEP. When it arrives, the VTEP removes the IP and UDP headers, and delivers the frame as normal.

 https://networkdirection.net/articles/routingandswitching/vxlanoverview/

*******************************************************************

L2vpn VS L3vpn

L3VPN

SP devices forward customer’s packet based on L3 info (eg.IP). SP is involved in customer IP routing.SP and customer may be present in different LAN.

L2VPN

SP devices forward customer’s packet based on L2 info (eg. MAC,VlanID).Spis not involved in customer IP routing.SP and customer are present in same LAN.

 L3VPN (VPLS..) , L3VPN (MPLS..)
 
 
 *******************************************************************
 THE FOLLWING IS A CONVERSATION WITH chatGPT
 *******************************************************************
 
 
 
 
 
**explain VXLAN to a layman in a nutshell**
 
VXLAN (Virtual Extensible LAN) is a technology that allows you to create a virtual network on top of your existing physical network infrastructure. Think of it like creating a virtual room in your house, within your existing house.

This virtual network can be used to segment and isolate different types of traffic, such as different departments within a company, or different types of applications. This allows for better security and network management.

VXLAN also allows for more flexibility in network design, as it can span multiple physical locations, allowing you to connect remote offices or even cloud services to your main network.

It's a bit like using a software to create a virtual LAN, so you can have multiple networks in your existing network infrastructure without having to physically rewire it. It is beneficial for large enterprise networks, data centers, and cloud service providers.


**explore more on the virtual room example**

Sure, let's say you have a large house with many rooms and you want to create a new space for a specific purpose, like a home office or a game room. With VXLAN, you can create a virtual room within your existing house without having to build any new walls or make any major changes to your house's infrastructure.

This virtual room can be accessed only by the people you invite, just like how VXLAN segments and isolates different types of traffic on a network. This allows for better security and network management, as you can ensure that sensitive information or specific applications are only accessible by authorized users.

Just like how you can move the furniture around in a virtual room, VXLAN allows you to move virtual networks around your physical infrastructure. This allows you to create a virtual room in one location and move it to another location if needed, without having to rebuild the room. This flexibility allows for better scalability and disaster recovery.

It's worth noting that VXLAN is a Layer 2 (Data Link Layer) protocol, that means it is used to create virtual networks based on MAC addresses, and it can provide flexibility in network design and it is not limited by physical location of devices.



**why a bridge is needed to configure vxlan**


A bridge is needed to configure VXLAN because it acts as the intermediary between the virtual and physical networks. In VXLAN, the virtual network is called the VXLAN Tunnel Endpoint (VTEP), and the physical network is called the underlay. The bridge connects these two networks by encapsulating the traffic from the virtual network and forwarding it to the physical network, and vice versa.

The bridge allows for the creation of a VXLAN segment, which is a virtual Layer 2 (Data Link Layer) network. This segment is created by encapsulating the original Layer 2 frames with VXLAN headers and forwarding them over the physical network. The bridge is responsible for terminating the VXLAN headers and forwarding the original frames to the correct destination.

Additionally, the bridge also provides features such as VLAN tagging and filtering, which allows for better network segmentation and security. It also provides multicast support, which allows for efficient traffic forwarding to multiple destinations.

In short, a bridge is needed in VXLAN to connect the virtual and physical networks, encapsulate and forward traffic, and provide features such as segmentation, security, and multicast support.


**what may be a bridge in the example of virtual room above**

In the example of the virtual room, the bridge could be thought of as the virtual door or portal that connects the virtual room to the rest of the house. The virtual room is the VXLAN segment or virtual network, and the rest of the house is the physical network or underlay.

Just like how the bridge in VXLAN is responsible for encapsulating and forwarding the traffic between the virtual and physical networks, the virtual door in this example is responsible for allowing people to enter and exit the virtual room, and for controlling who can access the room.

The bridge also plays a role in security, like how the virtual door can be locked to prevent unauthorized access. And also plays role in segmentation, like how the virtual room can be isolated from the rest of the house, just like how VXLAN network is isolated from the rest of the network infrastructure.

It's worth noting that the virtual door is not a physical thing but a software construct that simulates the functionality of a door and it is responsible for handling the traffic between the virtual room and the rest of the house.



**what an equivalent of EVPN in the example** 

In the example of the virtual room, the equivalent of EVPN (Ethernet Virtual Private Network) could be thought of as the virtual key or access card that allows people to enter and exit the virtual room.

Just like how EVPN is used to connect multiple VXLAN segments together, the virtual key or access card allows people to move between different virtual rooms within the house. It allows for the creation of a larger, virtual network that spans multiple rooms.

EVPN is a control plane protocol that allows for efficient and secure communication between different VXLAN segments, it also allows for automatic discovery of VXLAN endpoints, and it provides a common addressing scheme for the virtual network.

Similarly, the virtual key or access card allows for efficient and secure communication between different virtual rooms, it also allows for automatic discovery of virtual rooms, and it provides a common access scheme for the virtual rooms.

It's worth noting that, just like how EVPN is a software construct that simulates the functionality of a key or access card, it allows to connect multiple VXLAN segments together and handle the communication between them.



**whats the relation VXLAN EVPN BGP, and include them in the example of virtual room**


VXLAN, EVPN, and BGP are all technologies that are used to build and manage virtual networks. They are all related in the sense that they are often used together to create a highly scalable and resilient virtual network infrastructure.

In the example of the virtual room, VXLAN is the technology that is used to create the virtual room itself. It allows for the creation of a virtual Layer 2 network that is isolated from the rest of the house.

EVPN is the technology that is used to connect multiple virtual rooms together. It allows for the creation of a virtual network that spans multiple rooms, and it provides a common access scheme for the virtual rooms.

BGP is the routing protocol that is used to manage the virtual network. It allows for the automatic discovery of virtual rooms and the efficient communication between them. It also allows for the virtual network to be connected to the rest of the house, so that people can move seamlessly between the virtual and physical rooms.

To be more specific, BGP is used to distribute the information of virtual rooms across the network, so that all routers in the house know where each virtual room is located and how to reach it. This is important for allowing people to move seamlessly between virtual and physical rooms.

EVPN is used to establish a communication between different virtual rooms, it allows to create a virtual network that spans multiple virtual rooms, and it provides a common addressing scheme for the virtual network.

And VXLAN is used to create the virtual room itself, it allows to create a virtual Layer 2 network that is isolated from the rest of the house, this isolation allows to create a more secure and efficient network.

In summary, VXLAN, EVPN, and BGP work together to create a virtual network infrastructure that is highly scalable, resilient, and efficient. They allow for the creation of virtual rooms, the connection of multiple virtual rooms together, and the efficient communication between them.




**correct me if im wrong, follwing the exampl, a bridge connects the physical room to the virtual room, and evpn connects virtual rooms togeter**


You are correct that a bridge is used to connect the physical room to the virtual room in the example. The bridge allows communication between the physical and virtual networks, it allows people to move seamlessly between the physical and virtual rooms.

EVPN is used to connect multiple virtual rooms together, it creates a virtual network that spans multiple virtual rooms, and it provides a common addressing scheme for the virtual network.

BGP is used to distribute the information of virtual rooms across the network, so that all routers in the house know where each virtual room is located and how to reach it. This is important for allowing people to move seamlessly between virtual and physical rooms.

In summary, a bridge is used to connect the physical room to the virtual room, allowing communication between the two networks and allowing people to move seamlessly between the physical and virtual rooms. EVPN is used to connect multiple virtual rooms together, creating a virtual network that spans multiple virtual rooms and providing a common addressing scheme. BGP is used to distribute information about the virtual rooms across the network, allowing for efficient communication between virtual and physical rooms.
