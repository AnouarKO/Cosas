# COMANDAS REPASO EX9 Y EX10

Solo comandas usadas y comentario corto.

## Ex9 - Parte 1

| Comanda | Comentario |
|---|---|
| `ip addr add 192.168.1.X/24 dev eth0` | Pone IP manual en un `ipterm`. |
| `ip route add default via 192.168.1.1` | Pone el router como gateway. |
| `ip addr flush dev eth0` | Borra las IPs de `eth0`. |
| `ip route flush dev eth0` | Borra rutas asociadas a `eth0`. |
| `ip route` | Muestra la tabla de rutas del `ipterm`. |
| `ping 192.168.1.1` | Comprueba si el `ipterm` llega al router. |
| `/ip address add address=192.168.1.1/24 interface=ether1` | Pone la IP LAN del MikroTik. |
| `/interface print` | Muestra interfaces; `R` significa `running`. |
| `/ip address print` | Muestra las IPs configuradas en el router. |
| `ip route add 224.0.0.0/4 dev eth0` | Ruta multicast que en nuestro entorno hizo falta para `msend`. |
| `msend -g 229.1.1.2 -p 5000 -text "mensaje"` | Envia trafico multicast al grupo `229.1.1.2`. |
| `mreceive -g 229.1.1.2 -p 5000` | Escucha ese grupo multicast. |
| `udp.dstport == 5000 && ip.dst == 229.1.1.2` | Filtro Wireshark para ver solo el UDP multicast. |
| `igmp || udp.dstport == 5000` | Filtro Wireshark para ver IGMP y el flujo multicast. |

## Ex9 - Parte 2

| Comanda | Comentario |
|---|---|
| `/interface bridge add name=loopback` | Crea la interfaz virtual `loopback`. |
| `/ip address add address=10.0.0.X/32 interface=loopback network=10.0.0.X` | Pone la IP fija `/32` de referencia del router. |
| `/ip address add address=10.0.12.1/30 interface=ether1` | IP de un enlace entre routers. |
| `/ip address add address=10.0.13.1/30 interface=ether2` | IP de un enlace entre routers. |
| `/ip address add address=10.0.23.1/30 interface=ether2` | IP de un enlace entre routers. |
| `/ip address add address=192.168.2.1/24 interface=ether3` | IP LAN de `R2` para su `ipterm`. |
| `/ip address add address=192.168.3.1/24 interface=ether3` | IP LAN de `R3` para su `ipterm`. |
| `/routing ospf instance add name=core version=2 router-id=10.0.0.X redistribute=connected` | Crea OSPF y anuncia redes conectadas. |
| `/routing ospf area add name=backbone area-id=0.0.0.0 instance=core` | Crea el area backbone. |
| `/routing ospf interface-template add interfaces=loopback,ether1,ether2 area=backbone` | Mete interfaces en OSPF. |
| `/routing ospf interface-template add interfaces=loopback,ether1,ether2,ether3 area=backbone` | Mete interfaces y LAN en OSPF. |
| `/routing ospf neighbor print` | Comprueba vecinos OSPF; deben acabar en `Full`. |
| `/ip route print` | Comprueba rutas aprendidas por OSPF. |
| `ip addr add 192.168.2.2/24 dev eth0` | IP del `ipterm` conectado a `R2`. |
| `ip route add default via 192.168.2.1` | Gateway del `ipterm` de `R2`. |
| `ip addr add 192.168.3.2/24 dev eth0` | IP del `ipterm` conectado a `R3`. |
| `ip route add default via 192.168.3.1` | Gateway del `ipterm` de `R3`. |
| `ping 192.168.3.2` | Comprueba conectividad entre hosts de R2 y R3. |
| `ping 10.0.0.1` | Comprueba llegada a loopbacks por OSPF. |
| `/routing pimsm instance add name=pimsm-instance-1` | Crea la instancia PIM-SM. |
| `/routing pimsm interface-template add instance=pimsm-instance-1 interfaces=loopback,ether1,ether2` | Activa PIM en R1. |
| `/routing pimsm interface-template add instance=pimsm-instance-1 interfaces=loopback,ether1,ether2,ether3` | Activa PIM en R2/R3. |
| `/routing pimsm neighbor print` | Comprueba vecinos PIM. |
| `/routing pimsm uib-g print` | Muestra entradas `(*,G)` del grupo multicast. |
| `/routing pimsm uib-sg print` | Muestra entradas `(S,G)` con fuente concreta. |
| `/routing pimsm static-rp add address=10.0.0.1 instance=pimsm-instance-1` | Pone a R1 como RP estatico. |
| `msend -g 229.1.1.2 -p 5000 -text "desde R2"` | Emisor multicast en el host de R2. |
| `mreceive -g 229.1.1.2 -p 5000` | Receptor multicast en el host de R3. |

## Notas cortas

| Idea | Comentario |
|---|---|
| `229.1.1.2` | Grupo multicast, no maquina concreta. |
| `5000` | Puerto UDP del ejercicio. |
| `IGMP Join` | Sale al arrancar `mreceive`. |
| `IGMP Leave` | Sale al parar `mreceive`. |
| `OSPF` | Hace el routing unicast base entre routers. |
| `PIM-SM` | Hace el routing multicast entre routers. |
| `RP` | Punto de encuentro del multicast en sparse mode. |

## Pendiente

- Ex10 aun no anadido.
