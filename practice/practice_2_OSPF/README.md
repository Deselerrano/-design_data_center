# OSPF

## Цели

- 1 - Настроить OSPF для связанности всех устройств

### Настройка

Предварительно добавим на все устройства следующий конфиг:

```
ip icmp source-interface Loopback1
```
```
logging buffered 10000
logging level OSPF debuging
```
Далее перейдем к процессу настройки OSPF на pd01-lf-001:
```
router ospf 100                                    
   router-id 10.1.20.1
   passive-interface default                       
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet3
   no passive-interface Ethernet4
   network 192.168.1.0/31 area 0.0.0.0
   network 192.168.2.0/31 area 0.0.0.0
   network 192.168.10.0/30 area 0.0.0.0
   network 192.168.10.4/30 area 0.0.0.0
   network 192.168.200.0/31 area 0.0.0.0
   network 192.168.200.2/31 area 0.0.0.0
   log-adjacency-changes detail
```
Затем, на все интефейсы, которые будут учавствовать в процессе OSPF:
```
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet3
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet4
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Loopback1
   ip ospf area 0.0.0.0
```
По аналогии настроим на всех сетевых устройствах, дополнительно назначим IP на сервера для проверки связанности.
Отлично! Проверим логи:
```
Jul  7 02:33:49 pd01-lf-001 ConfigAgent: %SYS-5-CONFIG_STARTUP: Startup config saved from system:/running-config by admin on con0 (0.0.0.0).
Jul  7 02:34:41 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.0 change from <DOWN> to <DOWN>
Jul  7 02:34:41 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.0 change from <DOWN> to <INIT>
Jul  7 02:34:49 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.0 change from <INIT> to <2 WAYS>
Jul  7 02:34:49 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.0 change from <2 WAYS> to <EXCH START>
Jul  7 02:34:49 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.0 change from <EXCH START> to <EXCHANGE>
Jul  7 02:34:49 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.0 change from <EXCHANGE> to <LOADING>
Jul  7 02:34:49 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.0 change from <LOADING> to <FULL>
Jul  7 02:34:49 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_ADJACENCY_ESTABLISHED: NGB 10.1.20.2, interface 192.168.200.1 adjacency established
Jul  7 02:34:50 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.2 change from <DOWN> to <DOWN>
Jul  7 02:34:50 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.2 change from <DOWN> to <INIT>
Jul  7 02:34:50 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.2 change from <INIT> to <2 WAYS>
Jul  7 02:34:50 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.2 change from <2 WAYS> to <EXCH START>
Jul  7 02:34:55 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.2 change from <EXCH START> to <EXCHANGE>
Jul  7 02:34:55 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.20.2, intf 192.168.200.2 change from <EXCHANGE> to <FULL>
Jul  7 02:34:55 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_ADJACENCY_ESTABLISHED: NGB 10.1.20.2, interface 192.168.200.3 adjacency established
Jul  7 17:56:40 pd01-lf-001 ConfigAgent: %SYS-5-CONFIG_E: Enter configuration mode from console by admin on con0 (0.0.0.0)
Jul  7 17:56:50 pd01-lf-001 ConfigAgent: %SYS-5-CONFIG_I: Configured from console by admin on con0 (0.0.0.0)
Jul  7 17:56:54 pd01-lf-001 ConfigAgent: %SYS-5-CONFIG_STARTUP: Startup config saved from system:/running-config by admin on con0 (0.0.0.0).
Jul  7 18:07:56 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.1, intf 192.168.1.1 change from <DOWN> to <DOWN>
Jul  7 18:07:56 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.1, intf 192.168.1.1 change from <DOWN> to <INIT>
Jul  7 18:07:57 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.1, intf 192.168.1.1 change from <INIT> to <2 WAYS>
Jul  7 18:07:57 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.1, intf 192.168.1.1 change from <2 WAYS> to <EXCH START>
Jul  7 18:08:02 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.1, intf 192.168.1.1 change from <EXCH START> to <EXCHANGE>
Jul  7 18:08:02 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.1, intf 192.168.1.1 change from <EXCHANGE> to <FULL>
Jul  7 18:08:02 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_ADJACENCY_ESTABLISHED: NGB 10.1.10.1, interface 192.168.1.0 adjacency established
Jul  7 18:08:27 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.2, intf 192.168.2.1 change from <DOWN> to <DOWN>
Jul  7 18:08:27 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.2, intf 192.168.2.1 change from <DOWN> to <INIT>
Jul  7 18:08:27 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.2, intf 192.168.2.1 change from <INIT> to <2 WAYS>
Jul  7 18:08:27 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.2, intf 192.168.2.1 change from <2 WAYS> to <EXCH START>
Jul  7 18:08:27 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.2, intf 192.168.2.1 change from <EXCH START> to <EXCHANGE>
Jul  7 18:08:27 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_STATE_CHANGE: NGB 10.1.10.2, intf 192.168.2.1 change from <EXCHANGE> to <FULL>
Jul  7 18:08:27 pd01-lf-001 Rib: Instance 100: %OSPF-4-OSPF_ADJACENCY_ESTABLISHED: NGB 10.1.10.2, interface 192.168.2.0 adjacency established
```
Таблица маррутизации:
```
pd01-lf-001#show ip route ospf

VRF: default
Codes: C - connected, S - static, K - kernel,
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

 O        10.1.10.1/32 [110/20] via 192.168.1.1, Ethernet1
 O        10.1.10.2/32 [110/20] via 192.168.2.1, Ethernet2
 O        10.1.20.2/32 [110/20] via 192.168.200.0, Ethernet3
                                via 192.168.200.2, Ethernet4
 O        10.1.20.3/32 [110/30] via 192.168.1.1, Ethernet1
                                via 192.168.2.1, Ethernet2
 O        10.1.20.4/32 [110/30] via 192.168.1.1, Ethernet1
                                via 192.168.2.1, Ethernet2
 O        192.168.1.2/31 [110/20] via 192.168.1.1, Ethernet1
                                  via 192.168.200.0, Ethernet3
                                  via 192.168.200.2, Ethernet4
 O        192.168.1.4/31 [110/20] via 192.168.1.1, Ethernet1
 O        192.168.1.6/31 [110/20] via 192.168.1.1, Ethernet1
 O        192.168.2.2/31 [110/20] via 192.168.2.1, Ethernet2
                                  via 192.168.200.0, Ethernet3
                                  via 192.168.200.2, Ethernet4
 O        192.168.2.4/31 [110/20] via 192.168.2.1, Ethernet2
 O        192.168.2.6/31 [110/20] via 192.168.2.1, Ethernet2
 O        192.168.20.0/30 [110/20] via 192.168.200.0, Ethernet3
                                   via 192.168.200.2, Ethernet4
 O        192.168.20.4/30 [110/20] via 192.168.200.0, Ethernet3
                                   via 192.168.200.2, Ethernet4
 O        192.168.30.0/30 [110/30] via 192.168.1.1, Ethernet1
                                   via 192.168.2.1, Ethernet2
 O        192.168.30.4/30 [110/30] via 192.168.1.1, Ethernet1
                                   via 192.168.2.1, Ethernet2
 O        192.168.40.0/30 [110/30] via 192.168.1.1, Ethernet1
                                   via 192.168.2.1, Ethernet2
 O        192.168.40.4/30 [110/30] via 192.168.1.1, Ethernet1
                                   via 192.168.2.1, Ethernet2
 O        192.168.201.0/31 [110/30] via 192.168.1.1, Ethernet1
                                    via 192.168.2.1, Ethernet2
 O        192.168.201.2/31 [110/30] via 192.168.1.1, Ethernet1
                                    via 192.168.2.1, Ethernet2
```
И соседей:
```
pd01-lf-001#show ip ospf neighbor
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface
10.1.10.1       100      default  0   FULL                   00:00:33    192.168.1.1     Ethernet1
10.1.10.2       100      default  0   FULL                   00:00:34    192.168.2.1     Ethernet2
10.1.20.2       100      default  0   FULL                   00:00:29    192.168.200.0   Ethernet3
10.1.20.2       100      default  0   FULL                   00:00:37    192.168.200.2   Ethernet4
```
Теперь проверим, появилась ли связанность между pd01-srv-001 <==> pd01-srv-004:
```
[root@pd01-srv-001 ~]# tracepath 192.168.30.6
1?: [LOCALHOST]
1: 10.1.20.1                 2.262ms
1: 10.1.20.1                 1.868ms
2: 10.1.10.1                 4.651ms
3: 10.1.20.3                 6.827ms
Resume: pmtu 1500            3056.514ms H
```
Отлично! Попробуем включить пинг с лифа, отключить спайн через который идет трафик, после чего проверим, перестроится ли маршрут:
```
pd01-lf-001#trace 10.1.20.4
traceroute to 10.1.20.4 (10.1.20.4), 30 hops max, 60 byte packets
 1  10.1.10.2 (10.1.10.2)  3.347 ms  8.297 ms  9.014 ms
 2  10.1.20.4 (10.1.20.4)  16.166 ms  16.994 ms  22.438 ms
pd01-lf-001#ping 10.1.20.4 interval 1 repeat 100
PING 10.1.20.4 (10.1.20.4) 72(100) bytes of data.
80 bytes from 10.1.20.4: icmp_seq=1 ttl=63 time=7.41 ms
80 bytes from 10.1.20.4: icmp_seq=2 ttl=63 time=5.99 ms
80 bytes from 10.1.20.4: icmp_seq=3 ttl=63 time=5.76 ms
80 bytes from 10.1.20.4: icmp_seq=4 ttl=63 time=9.31 ms
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
ping: sendmsg: Network is unreachable
80 bytes from 10.1.20.4: icmp_seq=55 ttl=63 time=12.4 ms
80 bytes from 10.1.20.4: icmp_seq=56 ttl=63 time=5.56 ms
80 bytes from 10.1.20.4: icmp_seq=57 ttl=63 time=6.76 ms
80 bytes from 10.1.20.4: icmp_seq=58 ttl=63 time=6.07 ms
80 bytes from 10.1.20.4: icmp_seq=59 ttl=63 time=5.42 ms
^C
--- 10.1.20.4 ping statistics ---
59 packets transmitted, 9 received, 84% packet loss, time 59202ms
rtt min/avg/max/mdev = 5.422/7.192/12.420/2.175 ms
```
Шикарно, лабораторная работы выполнена успешно!
