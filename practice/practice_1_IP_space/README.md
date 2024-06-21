# Проектирование адресного пространства

## Цели

- 1 - Соберете топологию CLOS, как на схеме 

  ![alt text](https://github.com/Deselerrano/-design_data_center/blob/main/pictures/task_topology.png?raw=true)

- 2 - Распределите адресное пространство для Underlay сети

- 3 - План работы, адресное пространство, схема сети, настройки - зафиксированы в документации

### Название хостов

Формируются по следующей форме: 
	
#### <№_пода>-<Назначение_хотса>-<№_хоста>

В названии назначения хостов следующие:

- lf - leaf
- sp - spine
- srv - server

### Распределение IP

IP адреса распределены следующим образом:

#### 10.<№_пода>.<Назначение_хоста>.№_хоста>

####Нумерация для назначения хостов следующие:

- 20 - leaf
- 10 - spine
- 100-110 - server

Для линков между свитчами/серверами используется

- 192.168.0-100.X/31

Для пирлинков:

- 192.168.200-250.X/31

Итоговая схема будет выглядеть следующим образом

 
![alt text](https://github.com/Deselerrano/-design_data_center/blob/main/pictures/edited_task_topology_1.png?raw=true)

Перейдем же к настройке!

IP адресация будет следующей:

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- |
| pd01-sp-001   | Lo1           | 10.1.10.1        | Loopback                         |
| pd01-sp-001   | Eth1          | 192.168.1.1      | pd01-lf-001 Eth1                 |
| pd01-sp-001   | Eth2          | 192.168.1.3      | pd01-lf-002 Eth1                 |
| pd01-sp-001   | Eth3          | 192.168.1.5      | pd01-lf-003 Eth1                 |
| pd01-sp-001   | Eth4          | 192.168.1.7      | pd01-lf-004 Eth1                 |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- |
| pd01-sp-002   | Lo1           | 10.1.10.2        | Loopback                         |
| pd01-sp-002   | Eth1          | 192.168.2.1      | pd01-lf-001 Eth2                 |
| pd01-sp-002   | Eth2          | 192.168.2.3      | pd01-lf-002 Eth2                 |
| pd01-sp-002   | Eth3          | 192.168.2.5      | pd01-lf-003 Eth2                 |
| pd01-sp-002   | Eth4          | 192.168.2.7      | pd01-lf-004 Eth2                 |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- |
| pd01-lf-001   | Lo1           | 10.1.20.1        | Loopback                         |
| pd01-lf-001   | Eth1          | 192.168.1.0      | pd01-sp-001 Eth1                 |
| pd01-lf-001   | Eth2          | 192.168.2.0      | pd01-sp-002 Eth1                 |
| pd01-lf-001   | Eth3          | 192.168.200.1    | pd01-lf-002 Eth3                 |
| pd01-lf-001   | Eth4          | 192.168.200.3    | pd01-lf-002 Eth4                 |
| pd01-lf-001   | Eth5          | 192.168.10.1     | pd01-srv-001 eth0                |
| pd01-lf-001   | Eth6          | 192.168.10.5     | pd01-srv-002 eth0                |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- | 
| pd01-lf-002   | Lo1           | 10.1.20.2        | Loopback                         |
| pd01-lf-002   | Eth1          | 192.168.1.2      | pd01-sp-001 Eth2                 |
| pd01-lf-002   | Eth2          | 192.168.2.2      | pd01-sp-002 Eth2                 |
| pd01-lf-002   | Eth3          | 192.168.200.0    | pd01-lf-001 Eth3                 | 
| pd01-lf-002   | Eth4          | 192.168.200.2    | pd01-lf-001 Eth3                 | 
| pd01-lf-002   | Eth5          | 192.168.20.1     | pd01-srv-001 eth1                |
| pd01-lf-002   | Eth6          | 192.168.20.5     | pd01-srv-002 eth1                |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- | 
| pd01-lf-003   | Lo1           | 10.1.20.3        | Loopback                         |
| pd01-lf-003   | Eth1          | 192.168.1.4      | pd01-sp-001 Eth3                 |
| pd01-lf-003   | Eth2          | 192.168.2.4      | pd01-sp-002 Eth3                 |
| pd01-lf-003   | Eth3          | 192.168.201.1    | pd01-lf-004 Eth3                 |
| pd01-lf-003   | Eth4          | 192.168.201.3    | pd01-lf-004 Eth4                 |
| pd01-lf-003   | Eth5          | 192.168.30.1     | pd01-srv-001 eth0                |
| pd01-lf-003   | Eth6          | 192.168.30.5     | pd01-srv-002 eth0                |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- | 
| pd01-lf-004   | Lo1           | 10.1.20.4        | Loopback                         |
| pd01-lf-004   | Eth1          | 192.168.1.6      | pd01-sp-001 Eth4                 |
| pd01-lf-004   | Eth2          | 192.168.2.6      | pd01-sp-002 Eth4                 |
| pd01-lf-004   | Eth3          | 192.168.201.0    | pd01-lf-003 Eth3                 |
| pd01-lf-004   | Eth4          | 192.168.201.2    | pd01-lf-003 Eth4                 |
| pd01-lf-004   | Eth5          | 192.168.40.1     | pd01-srv-001 eth1                |
| pd01-lf-004   | Eth6          | 192.168.40.5     | pd01-srv-002 eth1                |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- |
| pd01-srv-001  | eth0          | 192.168.10.2     | pd01-lf-001 Eth5                 |
| pd01-srv-001  | eth1          | 192.168.20.2     | pd01-lf-002 Eth5                 |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- | 
| pd01-srv-002  | eth0          | 192.168.10.6     | pd01-lf-001 Eth6                 |
| pd01-srv-002  | eth1          | 192.168.20.6     | pd01-lf-002 Eth6                 |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- | 
| pd01-srv-003  | eth0          | 192.168.30.2     | pd01-lf-003 Eth5                 |
| pd01-srv-003  | eth1          | 192.168.40.2     | pd01-lf-004 Eth5                 |

| Устройство    | Интерфейс     | IP аддрес        | Описание                         |
| ------------- | ------------- | ---------------- | -------------------------------- |
| pd01-srv-004  | eth0          | 192.168.30.6     | pd01-lf-003 Eth6                 |
| pd01-srv-004  | eth1          | 192.168.40.6     | pd01-lf-004 Eth6                 | 

Так же назначим всем hostname, проставим заданные нами адреса на интерфейсы и отобразим Loopback адреса на схеме:

![alt text](https://github.com/Deselerrano/-design_data_center/blob/main/pictures/ip_in_topology_1.png?raw=true)

```
pd01-sp-001>show ip interface brief
                                                                        Address
Interface       IP Address         Status      Protocol          MTU    Owner
--------------- ------------------ ----------- ------------- ---------- -------
Ethernet1       192.168.1.1/31     up          up               1500
Ethernet2       192.168.1.3/31     up          up               1500
Ethernet3       192.168.1.5/31     up          up               1500
Ethernet4       192.168.1.7/31     up          up               1500
Loopback0       10.1.10.1/32       up          up              65535
Management1     unassigned         up          up               1500

pd01-sp-001>show interfaces description
Interface                      Status         Protocol           Description
Et1                            up             up                 pd01-lf-001 Eth1
Et2                            up             up                 pd01-lf-002 Eth1
Et3                            up             up                 pd01-lf-003 Eth1
Et4                            up             up                 pd01-lf-004 Eth1
Et5                            up             up
Et6                            up             up
Et7                            up             up
Lo0                            up             up
Ma1                            up             up
pd01-sp-001>
```

```
pd01-sp-002(config)#show interfaces description
Interface                      Status         Protocol           Description
Et1                            up             up                 pd01-lf-001 Eth2
Et2                            up             up                 pd01-lf-002 Eth2
Et3                            up             up                 pd01-lf-003 Eth2
Et4                            up             up                 pd01-lf-004 Eth2
Et5                            up             up
Et6                            up             up
Et7                            up             up
Et8                            up             up
Lo1                            up             up
Ma1                            up             up
pd01-sp-002(config)#show ip interface brief
                                                                        Address
Interface       IP Address         Status      Protocol          MTU    Owner
--------------- ------------------ ----------- ------------- ---------- -------
Ethernet1       192.168.2.1/31     up          up               1500
Ethernet2       192.168.2.3/31     up          up               1500
Ethernet3       192.168.2.5/31     up          up               1500
Ethernet4       192.168.2.7/31     up          up               1500
Loopback1       10.1.10.2/32       up          up              65535
Management1     unassigned         up          up               1500

pd01-sp-002(config)#
```

```
pd01-lf-001#show interfaces description
Interface                      Status         Protocol           Description
Et1                            up             up                 pd01-sp-001 Eth1
Et2                            up             up                 pd01-sp-002 Eth1
Et3                            up             up                 pd01-lf-002 Eth3
Et4                            up             up                 pd01-lf-002 Eth4
Et5                            up             up                 pd01-srv-001 eth0
Et6                            up             up                 pd01-srv-002 eth0
Et7                            up             up
Et8                            up             up
Lo1                            up             up
Ma1                            up             up
pd01-lf-001#show ip interface brief
                                                                        Address
Interface       IP Address           Status     Protocol         MTU    Owner
--------------- -------------------- ---------- ------------ ---------- -------
Ethernet1       192.168.1.0/31       up         up              1500
Ethernet2       192.168.2.0/31       up         up              1500
Ethernet3       192.168.200.1/31     up         up              1500
Ethernet4       192.168.200.3/31     up         up              1500
Ethernet5       192.168.10.1/30      up         up              1500
Ethernet6       192.168.10.5/30      up         up              1500
Loopback1       10.1.20.1/32          up         up             65535
Management1     unassigned           up         up              1500
```

```
pd01-lf-002#show interfaces description
Interface                      Status         Protocol           Description
Et1                            up             up                 pd01-sp-001 Eth2
Et2                            up             up                 pd01-sp-002 Eth2
Et3                            up             up                 pd01-lf-001 Eth3
Et4                            up             up                 pd01-lf-001 Eth4
Et5                            up             up                 pd01-srv-001 eth1
Et6                            up             up                 pd01-srv-002 eth1
Et7                            up             up
Et8                            up             up
Lo1                            up             up
Ma1                            up             up
pd01-lf-002#show ip interface brief
                                                                        Address
Interface       IP Address           Status     Protocol         MTU    Owner
--------------- -------------------- ---------- ------------ ---------- -------
Ethernet1       192.168.1.2/31       up         up              1500
Ethernet2       192.168.2.2/31       up         up              1500
Ethernet3       192.168.200.0/31     up         up              1500
Ethernet4       192.168.200.2/31     up         up              1500
Ethernet5       192.168.20.1/30      up         up              1500
Ethernet6       192.168.20.5/30      up         up              1500
Loopback1       10.1.20.2/32          up         up             65535
Management1     unassigned           up         up              1500
```

```
pd01-lf-003#show interfaces description
Interface                      Status         Protocol           Description
Et1                            up             up                 pd01-sp-001 Eth3
Et2                            up             up                 pd01-sp-002 Eth3
Et3                            up             up                 pd01-lf-004 Eth3
Et4                            up             up                 pd01-lf-004 Eth4
Et5                            up             up                 pd01-srv-003 eth0
Et6                            up             up                 pd01-srv-004 eth0
Et7                            up             up
Et8                            up             up
Lo1                            up             up
Ma1                            up             up
pd01-lf-003#show ip interface brief
                                                                        Address
Interface       IP Address           Status     Protocol         MTU    Owner
--------------- -------------------- ---------- ------------ ---------- -------
Ethernet1       192.168.1.4/31       up         up              1500
Ethernet2       192.168.2.4/31       up         up              1500
Ethernet3       192.168.201.1/31     up         up              1500
Ethernet4       192.168.201.3/31     up         up              1500
Ethernet5       192.168.30.1/30      up         up              1500
Ethernet6       192.168.30.5/30      up         up              1500
Loopback1       10.1.20.3/32          up         up             65535
Management1     unassigned           up         up              1500
```

```
pd01-lf-004#show interfaces description
Interface                      Status         Protocol           Description
Et1                            up             up                 pd01-sp-001 Eth4
Et2                            up             up                 pd01-sp-002 Eth4
Et3                            up             up                 pd01-lf-003 Eth3
Et4                            up             up                 pd01-lf-003 Eth4
Et5                            up             up                 pd01-srv-003 eth1
Et6                            up             up                 pd01-srv-004 eth1
Et7                            up             up
Et8                            up             up
Lo1                            up             up
Ma1                            up             up
pd01-lf-004#show ip interface brief
                                                                        Address
Interface       IP Address           Status     Protocol         MTU    Owner
--------------- -------------------- ---------- ------------ ---------- -------
Ethernet1       192.168.1.6/31       up         up              1500
Ethernet2       192.168.2.6/31       up         up              1500
Ethernet3       192.168.201.0/31     up         up              1500
Ethernet4       192.168.201.2/31     up         up              1500
Ethernet5       192.168.40.1/30      up         up              1500
Ethernet6       192.168.40.5/30      up         up              1500
Loopback1       10.1.20.4/32         up         up             65535
Management1     unassigned           up         up              1500
```

Сервера будут сконфигурированны позже, когда этого будет требовать задание




