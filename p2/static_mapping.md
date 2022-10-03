static mapping

### Router 1:
 
`ip link add br0 type bridge` : **lorem lepsum**
`ip addr add 10.1.1.1/24 dev eth0` : **lorem lepsum**
`ip link add name vxlan10 type vxlan id 10 dev eth0 remote 10.1.1.2 local 10.1.1.1 dstport 4789` : **lorem lepsum**
`ip addr add 20.1.1.1/24 dev vxlan10` : **lorem lepsum**
`brctl addif br0 eth1` : **lorem lepsum**
`brctl addif br0 vxlan10` : **lorem lepsum**
`ip link set dev br0 up` : **lorem lepsum**
`ip link set dev vxlan10 up ` : **lorem lepsum**
- to copy:
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
```
# ip addr show eth0


## VTEP IP

# ip -d link show vxlan10

## (make interfaces into one bridge domain) (frames comming from eth1 can be encapsulated into vxlan10)



# ip -d link show vxlan10 

## (see if they are in the same bridge domain)
ip link show vxlan10 
ip link show eth1
```


-------------------

### Router 2:

ip link add br0 type bridge
##
ip addr add 10.1.1.2/24 dev eth0
# ip addr show eth0
##
ip link add name vxlan10 type vxlan id 10 dev eth0 remote 10.1.1.1 local 10.1.1.2 dstport 4789
## VTEP IP
ip addr add 20.1.1.2/24 dev vxlan10
# ip -d link show vxlan10
##
## (make interfaces into one bridge domain) (frames comming from eth1 can be encapsulated into vxlan10)
brctl addif br0 eth1
brctl addif br0 vxlan10
##
ip link set dev br0 up
ip link set dev vxlan10 up 
 # ip -d link show vxlan10 
##
## (see if they are in the same bridge domain)
# ip link show vxlan10 
# ip link show eth1
#

-------------------
 alpine machines

ip addr add 30.1.1.1/24  dev eth1
----
ip addr add 30.1.1.2/24  dev eth1

**********************************





configured VXLAN interfaces, attached them to a bridge, and mapped VLANs to VNIs.