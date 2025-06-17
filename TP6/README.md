# Configuration OSPF Multi-Zones
## Configuration de base des routeurs

R0
```shell
enable
conf t
hostname R0
interface GigabitEthernet0/0
 ip address 10.10.0.1 255.255.255.252
 no shutdown
interface GigabitEthernet1/0
 ip address 10.100.0.1 255.255.255.252
 no shutdown
interface GigabitEthernet2/0
 ip address 10.10.0.5 255.255.255.252
 no shutdown
interface GigabitEthernet3/0
 ip address 10.100.0.5 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.1
 network 10.10.0.0 0.0.0.255 area 0

router eigrp 100
 network 10.100.0.0 0.0.0.255
 no auto-summary
do wr
```
R1
```shell
enable
conf t
hostname R1
interface GigabitEthernet0/0
 ip address 10.10.0.9 255.255.255.252
 no shutdown
interface GigabitEthernet1/0
 ip address 10.10.0.13 255.255.255.252
 no shutdown
interface GigabitEthernet2/0
 ip address 10.10.0.2 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.2
 network 10.10.0.0 0.0.0.255 area 0
do wr
```
R2
```shell
enable
conf t
hostname R2
interface GigabitEthernet0/0
 ip address 10.10.0.6 255.255.255.252
 no shutdown
interface GigabitEthernet1/0
 ip address 10.1.10.1 255.255.255.252
 no shutdown
interface GigabitEthernet2/0
 ip address 10.2.10.1 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.3
 network 10.10.0.0 0.0.0.255 area 0
 network 10.1.10.0 0.0.0.3 area 1
 network 10.2.10.0 0.0.0.3 area 2
 area 2 stub
do wr
```
R3
```shell
enable
conf t
hostname R3
interface GigabitEthernet0/0
 ip address 10.10.0.10 255.255.255.252
 no shutdown
interface GigabitEthernet2/0
 ip address 10.2.10.2 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.4
 network 10.10.0.0 0.0.0.255 area 0
 network 10.2.10.0 0.0.0.3 area 2
 area 2 stub
do wr
```
R4
```shell
enable
conf t
hostname R4
interface GigabitEthernet0/0
 ip address 10.10.0.17 255.255.255.252
 no shutdown
interface GigabitEthernet1/0
 ip address 10.10.0.14 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.5
 network 10.10.0.0 0.0.0.255 area 0
do wr
```
R5
```shell
enable
conf t
hostname R5
interface GigabitEthernet0/0
 ip address 10.10.0.18 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.6
 network 10.10.0.0 0.0.0.255 area 0
do wr
```
R6
```shell
enable
conf t
hostname R6
interface GigabitEthernet0/0
 ip address 10.1.10.5 255.255.255.252
 no shutdown
interface GigabitEthernet1/0
 ip address 10.1.10.2 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.7
 network 10.1.10.0 0.0.0.3 area 1
 network 10.1.10.4 0.0.0.3 area 1
do wr
```
R7
```shell
enable
conf t
hostname R7
interface GigabitEthernet0/0
 ip address 10.1.10.6 255.255.255.252
 no shutdown

router ospf 1
 router-id 0.0.0.8
 network 10.1.10.4 0.0.0.3 area 1
do wr
```
R8
```shell
enable
conf t
hostname R8
interface GigabitEthernet1/0
 ip address 10.100.0.2 255.255.255.252
 no shutdown
interface GigabitEthernet2/0
 ip address 10.100.0.10 255.255.255.252
 no shutdown

router eigrp 100
 network 10.100.0.0 0.0.0.255
 no auto-summary
do wr
```
R9
```shell
enable
conf t
hostname R9
interface GigabitEthernet0/0
 ip address 10.100.0.6 255.255.255.252
 no shutdown
interface GigabitEthernet2/0
 ip address 10.100.0.9 255.255.255.252
 no shutdown

router eigrp 100
 network 10.100.0.0 0.0.0.255
 no auto-summary
do wr
```
## Configuration EIGRP et Redistribution
R0
```shell
enable
conf t
router ospf 1
 redistribute eigrp 100 subnets metric-type 1
router eigrp 100
 redistribute ospf 1 metric 1500 100 255 1 1500
do wr
```
## Test de connectivité

R0
```shell
R0#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 9 subnets, 2 masks
C        10.10.0.0/30 is directly connected, GigabitEthernet0/0
L        10.10.0.1/32 is directly connected, GigabitEthernet0/0
C        10.10.0.4/30 is directly connected, GigabitEthernet2/0
L        10.10.0.5/32 is directly connected, GigabitEthernet2/0
C        10.100.0.0/30 is directly connected, GigabitEthernet1/0
L        10.100.0.1/32 is directly connected, GigabitEthernet1/0
C        10.100.0.4/30 is directly connected, GigabitEthernet3/0
L        10.100.0.5/32 is directly connected, GigabitEthernet3/0
D        10.100.0.8/30 [90/3072] via 10.100.0.6, 00:06:27, GigabitEthernet3/0
                       [90/3072] via 10.100.0.2, 00:06:27, GigabitEthernet1/0
R0#
*Jun 17 13:00:40.767: %SYS-5-CONFIG_I: Configured from console by console
R0#ping 10.10.0.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.0.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/3/12 ms
```	
