# Configuration IPv6
## Configuration des routeurs
R1
```shell
conf t
interface FastEthernet0/0
 no shutdown
 description Vers SW1 (LAN1)
 ipv6 address fda3:92dc:1963:1::1/64
 ipv6 enable
exit

interface FastEthernet1/0
 no shutdown
 description Vers R3
 ipv6 address fda3:92dc:1963:13::1/64
 ipv6 enable
exit

interface FastEthernet2/0
 no shutdown
 description Vers R2
 ipv6 address fda3:92dc:1963:12::1/64
 ipv6 enable
exit

ipv6 unicast-routing
ipv6 route fda3:92dc:1963:2::/64 fda3:92dc:1963:12::2
ipv6 route fda3:92dc:1963:23::/64 fda3:92dc:1963:12::2

do wr
```
R2
```shell
conf t
interface FastEthernet0/0
 no shutdown
 description Vers R3
 ipv6 address fda3:92dc:1963:23::1/64
 ipv6 enable
exit

interface FastEthernet1/0
 no shutdown
 description Vers SW2 (LAN2)
 ipv6 address fda3:92dc:1963:2::1/64
 ipv6 enable
exit

interface FastEthernet2/0
 no shutdown
 description Vers R1
 ipv6 address fda3:92dc:1963:12::2/64
 ipv6 enable
exit

ipv6 unicast-routing
ipv6 route fda3:92dc:1963:1::/64 fda3:92dc:1963:12::1
ipv6 route fda3:92dc:1963:13::/64 fda3:92dc:1963:12::1

do wr
```
R3
```shell
conf t
interface FastEthernet0/0
 no shutdown
 description Vers R2
 ipv6 address fda3:92dc:1963:23::2/64
 ipv6 enable
exit

interface FastEthernet1/0
 no shutdown
 description Vers R1
 ipv6 address fda3:92dc:1963:13::2/64
 ipv6 enable
 ip address 192.168.3.1 255.255.255.0
exit

ipv6 unicast-routing
ipv6 route fda3:92dc:1963:1::/64 fda3:92dc:1963:13::1
ipv6 route fda3:92dc:1963:2::/64 fda3:92dc:1963:23::1

do wr
```

## Configuration des vpcs

VPC1
```shell
ip fda3:92dc:1963:1::10/64 fda3:92dc:1963:1::1
```
VPC2
```shell
ipv6 fda3:92dc:1963:1::40/64 fda3:92dc:1963:1::1
```
VPC3
```shell
ipv6 fda3:92dc:1963:2::20/64 fda3:92dc:1963:2::1
```
VPC4
```shell
ipv6 fda3:92dc:1963:2::30/64 fda3:92dc:1963:2::1
```

## Test de connectivité

Ping depuis VPC1 vers VPC2
```shell
PC1> ping fda3:92dc:1963:2::20

fda3:92dc:1963:2::20 icmp6_seq=1 ttl=60 time=62.178 ms
fda3:92dc:1963:2::20 icmp6_seq=2 ttl=60 time=62.908 ms
fda3:92dc:1963:2::20 icmp6_seq=3 ttl=60 time=60.443 ms
fda3:92dc:1963:2::20 icmp6_seq=4 ttl=60 time=54.737 ms
```
Trace route depuis VPC1 vers VPC2
```shell
PC1> trace fda3:92dc:1963:2::20

trace to fda3:92dc:1963:2::20, 64 hops max
 1 fda3:92dc:1963:1::1   31.026 ms  15.275 ms  15.958 ms
 2 fda3:92dc:1963:12::2   45.891 ms  46.082 ms  48.573 ms
 3 fda3:92dc:1963:2::20   76.739 ms  62.628 ms  61.679 ms
```
Table de routage IPv6 sur R1
```shell
R1#show ipv6 route
IPv6 Routing Table - default - 9 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, R - RIP, H - NHRP, I1 - ISIS L1
       I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary, D - EIGRP
       EX - EIGRP external, ND - ND Default, NDp - ND Prefix, DCE - Destination
       NDr - Redirect, O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1
       OE2 - OSPF ext 2, ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, l - LISP
C   FDA3:92DC:1963:1::/64 [0/0]
     via FastEthernet0/0, directly connected
L   FDA3:92DC:1963:1::1/128 [0/0]
     via FastEthernet0/0, receive
S   FDA3:92DC:1963:2::/64 [1/0]
     via FDA3:92DC:1963:12::2
C   FDA3:92DC:1963:12::/64 [0/0]
     via FastEthernet2/0, directly connected
L   FDA3:92DC:1963:12::1/128 [0/0]
     via FastEthernet2/0, receive
C   FDA3:92DC:1963:13::/64 [0/0]
     via FastEthernet1/0, directly connected
L   FDA3:92DC:1963:13::1/128 [0/0]
     via FastEthernet1/0, receive
S   FDA3:92DC:1963:23::/64 [1/0]
     via FDA3:92DC:1963:12::2
L   FF00::/8 [0/0]
     via Null0, receive
```

## Différences entre IPv4 et IPv6

La configuration IPv6 est similaire à celle d’IPv4 avec quelques différences, comme l’absence de NAT et l’utilisation d’un adressage beaucoup plus long. Le routage doit explicitement etre indiquer avec  `ipv6 unicast-routing`.
