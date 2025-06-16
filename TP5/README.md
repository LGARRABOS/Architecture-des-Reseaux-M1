# Configuration RIPng et EIGRP
## Topologie IPv6
### Configuration des routeurs

R1
```
enable
configure terminal
hostname R1
ipv6 unicast-routing

interface GigabitEthernet0/0
description Link to R2
ipv6 address fe80::1 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet1/0
description Link to R4
ipv6 address fe80::2 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet2/0
description Link to R3
ipv6 address fe80::3 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet3/0
description LAN1
ipv6 address 2001:db8:A::1/64
ipv6 rip RIPNG enable
no shutdown

ipv6 router rip RIPNG

do write memory
```
R2
```
enable
configure terminal
hostname R2
ipv6 unicast-routing

interface GigabitEthernet0/0
description Link to R1
ipv6 address fe80::4 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet1/0
description Link to R3
ipv6 address fe80::5 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet2/0
description Link to R4
ipv6 address fe80::6 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet3/0
description LAN2
ipv6 address 2001:db8:B::1/64
ipv6 rip RIPNG enable
no shutdown

ipv6 router rip RIPNG
do write memory
```
R3
```
enable
configure terminal
hostname R3
ipv6 unicast-routing

interface GigabitEthernet0/0
description Link to R4
ipv6 address fe80::7 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet1/0
description Link to R2
ipv6 address fe80::8 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet2/0
description Link to R1
ipv6 address fe80::9 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet3/0
description Link to R5
ipv6 address fe80::A link-local
ipv6 rip RIPNG enable
no shutdown

ipv6 router rip RIPNG
do write memory
```
R4
```
enable
configure terminal
hostname R4
ipv6 unicast-routing

interface GigabitEthernet0/0
description Link to R3
ipv6 address fe80::B link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet1/0
description Link to R1
ipv6 address fe80::C link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet2/0
description Link to R2
ipv6 address fe80::D link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet3/0
description Link to R5
ipv6 address fe80::E link-local
ipv6 rip RIPNG enable
no shutdown

ipv6 router rip RIPNG
do write memory
```
R5
```
enable
configure terminal
hostname R5
ipv6 unicast-routing

interface GigabitEthernet1/0
description Link to R4
ipv6 address fe80::F link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet2/0
description Link to R3
ipv6 address fe80::10 link-local
ipv6 rip RIPNG enable
no shutdown

interface GigabitEthernet0/0
description LAN3
ipv6 address 2001:db8:C::1/64
ipv6 rip RIPNG enable
no shutdown

ipv6 router rip RIPNG
do write memory
```
### Test de connectivité avec R1
Visualisation de la table de routage IPv6 sur R1 et test de connectivité vers R5.
```
R1#show ipv6 route
IPv6 Routing Table - default - 5 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, R - RIP, H - NHRP, I1 - ISIS L1
       I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary, D - EIGRP
       EX - EIGRP external, ND - ND Default, NDp - ND Prefix, DCE - Destination
       NDr - Redirect, O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1
       OE2 - OSPF ext 2, ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, l - LISP
C   2001:DB8:A::/64 [0/0]
     via GigabitEthernet3/0, directly connected
L   2001:DB8:A::1/128 [0/0]
     via GigabitEthernet3/0, receive
R   2001:DB8:B::/64 [120/2]
     via FE80::4, GigabitEthernet0/0
R   2001:DB8:C::/64 [120/3]
     via FE80::9, GigabitEthernet2/0
     via FE80::C, GigabitEthernet1/0
L   FF00::/8 [0/0]
     via Null0, receive
```
```
R1#ping 2001:db8:C::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:DB8:C::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 40/53/68 ms
```
## Topologie IPv4
### Configuration des routeurs
Je remet les configurations comme si les routeurs n'avaient pas été configurés.
R1
```
enable
configure terminal
hostname R1

interface GigabitEthernet0/0
ip address 10.0.0.1 255.255.255.252
no shutdown

interface GigabitEthernet1/0
ip address 10.0.0.17 255.255.255.252
no shutdown

interface GigabitEthernet2/0
ip address 10.0.0.5 255.255.255.252
no shutdown

interface GigabitEthernet3/0
ip address 192.168.10.1 255.255.255.0
no shutdown

router eigrp 100
network 10.0.0.0
network 192.168.10.0
no auto-summary
do write memory
```
R2
```
enable
configure terminal
hostname R2

interface GigabitEthernet0/0
ip address 10.0.0.2 255.255.255.252
no shutdown

interface GigabitEthernet1/0
ip address 10.0.0.9 255.255.255.252
no shutdown

interface GigabitEthernet2/0
ip address 10.0.0.13 255.255.255.252
no shutdown

interface GigabitEthernet3/0
ip address 192.168.20.1 255.255.255.0
no shutdown

router eigrp 100
network 10.0.0.0
network 192.168.20.0
no auto-summary
do write memory
```
R3
```
enable
configure terminal
hostname R3

interface GigabitEthernet0/0
ip address 10.0.0.10 255.255.255.252
no shutdown

interface GigabitEthernet1/0
ip address 10.0.0.9 255.255.255.252
no shutdown

interface GigabitEthernet2/0
ip address 10.0.0.6 255.255.255.252
no shutdown

interface GigabitEthernet3/0
ip address 10.0.0.25 255.255.255.252
no shutdown

router eigrp 100
network 10.0.0.0
no auto-summary
do write memory
```
R4
```
enable
configure terminal
hostname R4

interface GigabitEthernet0/0
ip address 10.0.0.11 255.255.255.252
no shutdown

interface GigabitEthernet1/0
ip address 10.0.0.18 255.255.255.252
no shutdown

interface GigabitEthernet2/0
ip address 10.0.0.14 255.255.255.252
no shutdown

interface GigabitEthernet3/0
ip address 10.0.0.29 255.255.255.252
no shutdown

router eigrp 100
network 10.0.0.0
no auto-summary
do write memory
```
R5
```
enable
configure terminal
hostname R5

interface GigabitEthernet1/0
ip address 10.0.0.30 255.255.255.252
no shutdown

interface GigabitEthernet2/0
ip address 10.0.0.26 255.255.255.252
no shutdown

interface GigabitEthernet0/0
ip address 192.168.30.1 255.255.255.0
no shutdown

router eigrp 100
network 10.0.0.0
network 192.168.30.0
no auto-summary
do write memory
```
### Test de connectivité avec R1
Visualisation de la table de routage IPv4 sur R1 et test de connectivité vers R5.
```
R1#
R1#show ip eigrp neighbors
*Jun 16 14:24:31.419: %SYS-5-CONFIG_I: Configured from console by console
R1#show ip eigrp neighbors
EIGRP-IPv4 Neighbors for AS(100)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
2   10.0.0.18               Gi1/0                    14 00:00:52   36   216  0  10
1   10.0.0.6                Gi2/0                    10 00:01:09   39   234  0  10
0   10.0.0.2                Gi0/0                    14 00:01:32   25   150  0  10
```
```
R1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 10 subnets, 2 masks
C        10.0.0.0/30 is directly connected, GigabitEthernet0/0
L        10.0.0.1/32 is directly connected, GigabitEthernet0/0
C        10.0.0.4/30 is directly connected, GigabitEthernet2/0
L        10.0.0.5/32 is directly connected, GigabitEthernet2/0
D        10.0.0.8/30 [90/3072] via 10.0.0.6, 00:01:19, GigabitEthernet2/0
                     [90/3072] via 10.0.0.2, 00:01:19, GigabitEthernet0/0
D        10.0.0.12/30 [90/3072] via 10.0.0.18, 00:01:19, GigabitEthernet1/0
                      [90/3072] via 10.0.0.2, 00:01:19, GigabitEthernet0/0
C        10.0.0.16/30 is directly connected, GigabitEthernet1/0
L        10.0.0.17/32 is directly connected, GigabitEthernet1/0
D        10.0.0.24/30 [90/3072] via 10.0.0.6, 00:00:48, GigabitEthernet2/0
D        10.0.0.28/30 [90/3072] via 10.0.0.18, 00:00:48, GigabitEthernet1/0
      192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.10.0/24 is directly connected, GigabitEthernet3/0
L        192.168.10.1/32 is directly connected, GigabitEthernet3/0
D     192.168.20.0/24 [90/3072] via 10.0.0.2, 00:01:19, GigabitEthernet0/0
D     192.168.30.0/24 [90/3328] via 10.0.0.18, 00:00:48, GigabitEthernet1/0
                      [90/3328] via 10.0.0.6, 00:00:48, GigabitEthernet2/0
```
```
R1#ping 192.168.30.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.30.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 44/52/60 ms
```

## Comparaison des protocoles de routage
RIPng met plus de temps à s’adapter lorsqu’un lien tombe. En cas de panne, les routes mettent plusieurs secondes à être mises à jour sur l’ensemble des routeurs. Cela s’explique par son fonctionnement basé sur des mises à jour régulières toutes les 30 secondes.

EIGRP, en revanche, réagit beaucoup plus vite. Lorsqu’un lien est coupé, le protocole calcule immédiatement un nouveau chemin sans attendre une mise à jour globale. Cela permet de maintenir la connectivité presque sans interruption.