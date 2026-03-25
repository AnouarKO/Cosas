# COMANDAS REPASO EX9 Y EX10

Solo comandas necesarias.

## Ex9 - Parte 1

```sh
ip addr add 192.168.1.X/24 dev eth0
ip route add default via 192.168.1.1
ip addr flush dev eth0
ip route flush dev eth0
ip route add 224.0.0.0/4 dev eth0
msend -g 229.1.1.2 -p 5000 -text "mensaje"
mreceive -g 229.1.1.2 -p 5000
```

```rsc
/ip address add address=192.168.1.1/24 interface=ether1
/interface print
/ip address print
```

## Ex9 - Parte 2

```rsc
/interface bridge add name=loopback
/ip address add address=10.0.0.X/32 interface=loopback network=10.0.0.X
/ip address add address=10.0.12.1/30 interface=ether1
/ip address add address=10.0.13.1/30 interface=ether2
/ip address add address=10.0.23.1/30 interface=ether2
/ip address add address=192.168.2.1/24 interface=ether3
/ip address add address=192.168.3.1/24 interface=ether3
/routing ospf instance add name=core version=2 router-id=10.0.0.X redistribute=connected
/routing ospf area add name=backbone area-id=0.0.0.0 instance=core
/routing ospf interface-template add interfaces=loopback,ether1,ether2 area=backbone
/routing ospf interface-template add interfaces=loopback,ether1,ether2,ether3 area=backbone
/routing ospf neighbor print
/ip route print
/routing pimsm instance add name=pimsm-instance-1
/routing pimsm interface-template add instance=pimsm-instance-1 interfaces=loopback,ether1,ether2
/routing pimsm interface-template add instance=pimsm-instance-1 interfaces=loopback,ether1,ether2,ether3
/routing pimsm neighbor print
/routing pimsm uib-g print
/routing pimsm uib-sg print
/routing pimsm static-rp add address=10.0.0.1 instance=pimsm-instance-1
```

```sh
ip addr add 192.168.2.2/24 dev eth0
ip route add default via 192.168.2.1
ip addr add 192.168.3.2/24 dev eth0
ip route add default via 192.168.3.1
msend -g 229.1.1.2 -p 5000 -text "desde R2"
mreceive -g 229.1.1.2 -p 5000
```

## Ex10

```rsc
/ip address add address=10.0.0.2/30 interface=ether1
/ip route add dst-address=0.0.0.0/0 gateway=10.0.0.1
/ip address add address=10.0.0.1/30 interface=ether1
/ip address add address=83.2.35.14/30 interface=ether2
/ip route add dst-address=0.0.0.0/0 gateway=83.2.35.13
/ip address add address=83.2.35.13/30 interface=ether1
/ip address add address=67.11.5.25/30 interface=ether2
/ip address add address=67.11.5.26/30 interface=ether1
/ip address add address=11.0.0.25/30 interface=ether2
/ip route add dst-address=0.0.0.0/0 gateway=67.11.5.25
/ip address add address=11.0.0.26/30 interface=ether1
/ip route add dst-address=0.0.0.0/0 gateway=11.0.0.25
/interface gre add name=gre-r2-r3 local-address=83.2.35.14 remote-address=67.11.5.26
/interface gre add name=gre-r3-r2 local-address=67.11.5.26 remote-address=83.2.35.14
/ip address add address=172.16.0.1/30 interface=gre-r2-r3
/ip address add address=172.16.0.2/30 interface=gre-r3-r2
/ip route add dst-address=11.0.0.24/30 gateway=172.16.0.2
/ip route add dst-address=10.0.0.0/30 gateway=172.16.0.1
/interface eoip add name=eoip-r1-r4 tunnel-id=42 remote-address=11.0.0.26
/interface eoip add name=eoip-r4-r1 tunnel-id=42 remote-address=10.0.0.2
/interface bridge add name=br-lan
/interface bridge port add bridge=br-lan interface=ether2
/interface bridge port add bridge=br-lan interface=eoip-r1-r4
/interface bridge port add bridge=br-lan interface=eoip-r4-r1
```

```sh
ip addr add 192.168.42.2/24 dev eth0
ip addr add 192.168.42.3/24 dev eth0
arp
```
