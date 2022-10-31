
# Network Engineer. Basic. ДЗ №6 по лекции №13.

## Лабораторная работа. Доступ к сетевым устройствам по протоколу SSH

### Исходные данные

| Устройство  | Интерфейс   | IP-адрес|Маска подсети|Шлюз по умолчанию|
| :------------ |:---------------:| :---------------:| :---------------:| :-----:|
| R1      | G0/0/1.10 | 192.168.10.1 |255.255.255.0 |-|
| R1      | G0/0/1.20 | 192.168.20.1 |255.255.255.0 |-|
| R1      | G0/0/1.30 | 192.168.30.1 |255.255.255.0 |-|
| R1      | G0/0/1.1000 | - |- |-|
| S1      | VLAN 10 | 192.168.10.11 |255.255.255.0 |192.168.10.1 |
| S2      | VLAN 10 | 192.168.10.12 |255.255.255.0 |192.168.10.1 |
| PC-A      | NIC       |  192.168.20.3 |255.255.255.0 |192.168.20.1 |
| PC-B      | NIC       |  192.168.30.3 |255.255.255.0 |192.168.30.1 |
>Таблица адресации

| VLAN | Имя   | Назначенный интерфейс|
| :------------ |:---------------| :---------------|
| 10     | Control | S1: VLAN10; S2: VLAN 10|
| 20     | Sales | S1: F0/6|
| 30    | Operations| S2: F0/18|
| 999     | Control | S1: F0/2-4, F0/7-24, G0/1-2; S2: F0/2-17, F0/19-24, G0/1-2|
| 1000  | Собственная | -|
>Таблица VLAN

## Задание

##### [Часть 1. Создание сети и настройка основных параметров устройства](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-1)

##### [Часть 2. Создание сетей VLAN и назначение портов коммутатора](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B5%D0%B9-vlan-%D0%B8-%D0%BD%D0%B0%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0-1)

##### [Часть 3. Настройка транка 802.1Q между коммутаторами](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-3-%D0%BA%D0%BE%D0%BD%D1%84%D0%B8%D0%B3%D1%83%D1%80%D0%B0%D1%86%D0%B8%D1%8F-%D0%BC%D0%B0%D0%B3%D0%B8%D1%81%D1%82%D1%80%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BA%D0%B0%D0%BD%D0%B0%D0%BB%D0%B0-%D1%81%D1%82%D0%B0%D0%BD%D0%B4%D0%B0%D1%80%D1%82%D0%B0-8021q-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0%D0%BC%D0%B8)

##### [Часть 4. Настройка маршрутизации между сетями VLAN](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-4-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BC%D0%B0%D1%80%D1%88%D1%80%D1%83%D1%82%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D0%B8-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-%D1%81%D0%B5%D1%82%D1%8F%D0%BC%D0%B8-vlan-1)

##### [Часть 5. Проверка, что маршрутизация между VLAN работает](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-5-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D1%87%D1%82%D0%BE-%D0%BC%D0%B0%D1%80%D1%88%D1%80%D1%83%D1%82%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-vlan-%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%D0%B5%D1%82-1)


### Часть 1. Создание сети и настройка основных параметров устройства

![](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/Screenshot_1.jpg)
> Сеть согласно топологии в ПО СРТ

#### Выполняется настройка базовых параметров на маршрутизаторе

R1(config)#**line console 0**

R1(config-line)#**password cisco**

R1(config-line)#**login**

R1(config-line)#

R1(config-line)#**exi**

g.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.

R1(config)#**line vty 0 15**

R1(config-line)#**password cisco**

R1(config-line)#**login**

R1(config-line)#**exi**

R1(config)#**service password-encryption**

R1(config)#**banner motd #**

Enter TEXT message. End with the character '#'.

_________________________________________

_______________GO AWAY___________________

_________________________________________#

R1(config)#**copy run start**

##### Настройте на маршрутизаторе время.

R1#**sh clock**

*0:8:9.519 UTC Mon Mar 1 1993

R1(config)#**clock timezone Moscow 3**

R1(config)#**clo**

R1(config)#**clock ?**

timezone Configure time zone

R1(config)#**clock set ?**

% Unrecognized command

R1(config)#**clock set 16:12:00 28 oct 2022**

**Вне CPT данная команда дожна работать, иначе можно забрать время с ntp сервера**

#### Выполняется настройка базовых параметров каждого коммутатора

Switch>**en**

Switch#**conf t**

Enter configuration commands, one per line.  End with CNTL/Z.

Switch(config)#**hostname S1**

S1(config)#**no ip domain-l**

S1(config)#**no ip domain-lookup **

S1(config)#**enable secret class**

S1(config)#**line cons 0**

S1(config-line)#**password cisco**

S1(config-line)#**login**

S1(config-line)#

S1(config-line)#**exi**

S1(config)#**line vty 0 15**

S1(config-line)#**password cisco**

S1(config-line)#**login**

S1(config-line)#**exi**

S1(config)#**serv**

S1(config)#**service pas**

S1(config)#**service password-encryption**

S1(config)#**banner motd #**

Enter TEXT message.  End with the character '#'.

________________________

____GO AWAY FROM MY SWITCH______
___________________________________________

#

S1(config)#**clock set 16:21:00 28 oct 2022**

^

% Invalid input detected at '^' marker.

S1(config)#**ex**

S1#

%SYS-5-CONFIG_I: Configured from console by console

S1#**copy run start**

Destination filename [startup-config]? 

Building configuration...

[OK]

__По аналогии настраивается второй коммутатор__

#### Производится настройка узлов ПК 

![](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/Screenshot_2.jpg)

![](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/Screenshot_3.jpg)

### Часть 2. Создание сетей VLAN и назначение портов коммутатора

#### Осуществеляется создание сети VLAN на коммутаторах

S1(config)#vlan 10

S1(config-vlan)#

S1(config-vlan)#name control

##### Настройка интерфейса управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 

S1(config)#int vlan 10

S1(config-if)#ip add

S1(config-if)#ip address 192.168.10.11 255.255.255.0

S1(config-if)#

S1(config-if)#ex

S1(config)#ip de

S1(config)#ip default-gateway 192.168.10.1

##### В соответствии с заданием, необходимо назначить все неиспользуемые порты коммутатора VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивировать их.

S1(config)#int range f 0/2-4, f 0/7-24, g 0/1, g 0/2

S1(config-if-range)#switchport acce

S1(config-if-range)#switchport access vlan 999

S1(config-if-range)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down

S1(config-if-range)#

#### Назначьте сети VLAN соответствующим интерфейсам коммутатора.

Необходимо назначить используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настроить их для режима статического доступа.

S1(config)#int range f 0/1, f 0/5, f 0/6

S1(config-if-range)#switchport mode access

S1(config-if-range)#switchport access vlan 10

S1(config-if-range)#

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up

%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (10), with S2 FastEthernet0/1 (1).

S1(config-if-range)#end

S1#

%SYS-5-CONFIG_I: Configured from console by console

S1#

S1#

S1(config-if)#end

Осуществляется проверка, что VLAN назначены на правильные интерфейсы.

S1#sh vlan

VLAN Name Status Ports

---- -------------------------------- --------- -------------------------------

1 default active Fa0/2, Fa0/3, Fa0/4, Fa0/7

Fa0/8, Fa0/9, Fa0/10, Fa0/11

Fa0/12, Fa0/13, Fa0/14, Fa0/15

Fa0/16, Fa0/17, Fa0/18, Fa0/19

Fa0/20, Fa0/21, Fa0/22, Fa0/23

Fa0/24, Gig0/1, Gig0/2

10 control active Fa0/1, Fa0/5, Fa0/6

### Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами

#### Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.

##### Настройка статического транкинга на интерфейсе F0/1 для обоих коммутаторов.

S1(config)#int f 0/1

S1(config-if)#sw

S1(config-if)#switchport m t

Откройте окно конфигурации

##### Установите native VLAN 1000 на обоих коммутаторах.

S1(config)#vlan 1000

S1(config-vlan)#name native

S1(config-vlan)# ex

S1(config)#int f0/1

S1(config-if)#sw

S1(config-if)#switchport tr

S1(config-if)#switchport trunk n

S1(config-if)#switchport trunk native vlan 1000

S1(config-if)#%SPANTREE-2-RECV_PVID_ERR: Received BPDU with inconsistent peer vlan id 1 on FastEthernet0/1 VLAN1000.
%SPANTREE-2-BLOCK_PVID_LOCAL: Blocking FastEthernet0/1 on VLAN1000. Inconsistent local vlan.

S1(config)#do sh int trunk

Port Mode Encapsulation Status Native vlan

Fa0/1 on 802.1q trunking 1000

Port Vlans allowed on trunk

Fa0/1 1-1005

Port Vlans allowed and active in management domain

Fa0/1 1,10,1000

Port Vlans in spanning tree forwarding state and not pruned

Fa0/1 1,10,1000

##### Укажите, что VLAN 10, 20, 30 и 1000 могут проходить по транку.

**через транк по умолчанию разрешены все VLAN**

#####Проверьте транки, native VLAN и разрешенные VLAN через транк.

S1(config)#do sh int tr

Port Mode Encapsulation Status Native vlan

Fa0/1 on 802.1q trunking 1000

Port Vlans allowed on trunk

Fa0/1 1-1005

Port Vlans allowed and active in management domain

Fa0/1 1,10,20,30,999,1000

Port Vlans in spanning tree forwarding state and not pruned

Fa0/1 1,10,20,30,999,1000

#### Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

##### Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.

S1(config)#int f 0/5

S1(config-if)#sw tr native vlan 1000

S1(config-if)#ex

S1(config)#ex

S1#

%SYS-5-CONFIG_I: Configured from console by console

S1#

S1#sh vlan

VLAN Name Status Ports

---- -------------------------------- --------- -------------------------------

1 default active 

10 control active Fa0/6

20 Sales active 

30 Operations active 

999 Parking_Lot active Fa0/2, Fa0/3, Fa0/4, Fa0/7

Fa0/8, Fa0/9, Fa0/10, Fa0/11

Fa0/12, Fa0/13, Fa0/14, Fa0/15

Fa0/16, Fa0/17, Fa0/18, Fa0/19

Fa0/20, Fa0/21, Fa0/22, Fa0/23

Fa0/24, Gig0/1, Gig0/2

1000 native active 

1002 fddi-default active 

1003 token-ring-default active 

1004 fddinet-default active 

1005 trnet-default active 

VLAN Type SAID MTU Parent RingNo BridgeNo Stp BrdgMode Trans1 Trans2

---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

1 enet 100001 1500 - - - - - 0 0

10 enet 100010 1500 - - - - - 0 0

20 enet 100020 1500 - - - - - 0 0

30 enet 100030 1500 - - - - - 0 0

999 enet 100999 1500 - - - - - 0 0

1000 enet 101000 1500 - - - - - 0 0

1002 fddi 101002 1500 - - - - - 0 0 

1003 tr 101003 1500 - - - - - 0 0 

1004 fdnet 101004 1500 - - - ieee - 0 0 

1005 trnet 101005 1500 - - - ibm - 0 0 

VLAN Type SAID MTU Parent RingNo BridgeNo Stp BrdgMode Trans1 Trans2

---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs

------------------------------------------------------------------------------

Primary Secondary Type Ports

------- --------- ----------------- ------------------------------------------

S1#sh int trunk

Port Mode Encapsulation Status Native vlan

Fa0/1 on 802.1q trunking 1000

Fa0/5 on 802.1q trunking 1000


Port Vlans allowed on trunk

Fa0/1 1-1005

Fa0/5 1-1005



Port Vlans allowed and active in management domain

Fa0/1 1,10,20,30,999,1000

Fa0/5 1,10,20,30,999,1000



Port Vlans in spanning tree forwarding state and not pruned

Fa0/1 1,10,20,30,999,1000

Fa0/5 1,10,20,30,999,1000

S1#wr

Building configuration...

[OK]

S1#

##### Вопрос:

Что произойдет, если G0/0/1 на R1 будет отключен?

__не будет связи между вланами__

__также у меня не настраивается транк на fa05 S1 до прописывания команды “no shutdown” для интерефейса G0/0/1 R1!__

### Часть 4. Настройка маршрутизации между сетями VLAN

#### Настройте маршрутизатор.

R1(config)#

R1(config)#int g

R1(config)#int gigabitEthernet 0/0/1

R1(config-if)#no shut

#### Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.

R1(config)#int g 0/0/1

R1(config-if)# 

R1(config-if)#

R1(config-if)#

R1(config-if)#

R1(config-if)#int g 0/0/1.10

R1(config-subif)#

*Mar 07, 01:47:54.4747: %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.10, changed state to up

*Mar 07, 01:47:54.4747: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.10, changed state to up

R1(config-subif)#en

R1(config-subif)#encapsulation dot1q 10

R1(config-subif)#ip addr 192.168.10.1 255.255.255.0

R1(config-subif)#

R1(config-subif)#no shut

R1(config-subif)#ex

R1(config)#int g 0/0/1.20

R1(config-subif)#

*Mar 07, 01:49:32.4949: %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.20, changed state to up

*Mar 07, 01:49:32.4949: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.20, changed state to up

R1(config-subif)#en

R1(config-subif)#encapsulation 

R1(config-subif)#encapsulation dot1Q 20

R1(config-subif)#ip addr 192.168.20.1 255.255.255.0

R1(config-subif)#no shut

R1(config-subif)#

R1(config-subif)#ex

R1(config)#int g 0/0/1.30

R1(config-subif)#

*Mar 07, 01:50:10.5050: %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.30, changed state to up

*Mar 07, 01:50:10.5050: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.30, changed state to up

R1(config-subif)#

R1(config-subif)#en

R1(config-subif)#encapsulation 

R1(config-subif)#encapsulation dot1Q 30

R1(config-subif)#ip addr 192.168.30.1 255.255.255.0

R1(config-subif)#no shut

R1(config-subif)#ex

R1(config)#int g0/0/1.1000

R1(config-subif)#

*Mar 07, 01:54:21.5454: %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

*Mar 07, 01:54:21.5454: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up

R1(config-subif)#en

R1(config-subif)#encapsulation 

R1(config-subif)#encapsulation dot1Q 1000 native



##### Убедитесь, что вспомогательные интерфейсы работают

R1#sh int

GigabitEthernet0/0/1.10 is up, line protocol is up (connected)

Hardware is PQUICC_FEC, address is 000a.f327.ac02 (bia 000a.f327.ac02)

Internet address is 192.168.10.1/24

MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec, 

reliability 255/255, txload 1/255, rxload 1/255

Encapsulation 802.1Q Virtual LAN, Vlan ID 10

ARP type: ARPA, ARP Timeout 04:00:00, 

Last clearing of "show interface" counters never

GigabitEthernet0/0/1.20 is up, line protocol is up (connected)

Hardware is PQUICC_FEC, address is 000a.f327.ac02 (bia 000a.f327.ac02)

Internet address is 192.168.20.1/24

MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec, 

reliability 255/255, txload 1/255, rxload 1/255

Encapsulation 802.1Q Virtual LAN, Vlan ID 20

ARP type: ARPA, ARP Timeout 04:00:00, 

Last clearing of "show interface" counters never

GigabitEthernet0/0/1.30 is up, line protocol is up (connected)

Hardware is PQUICC_FEC, address is 000a.f327.ac02 (bia 000a.f327.ac02)

Internet address is 192.168.30.1/24

MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec, 

reliability 255/255, txload 1/255, rxload 1/255

Encapsulation 802.1Q Virtual LAN, Vlan ID 30

ARP type: ARPA, ARP Timeout 04:00:00, 

Last clearing of "show interface" counters never

GigabitEthernet0/0/1.1000 is up, line protocol is up (connected)

Hardware is PQUICC_FEC, address is 000a.f327.ac02 (bia 000a.f327.ac02)

MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec, 

reliability 255/255, txload 1/255, rxload 1/255

Encapsulation 802.1Q Virtual LAN, Vlan ID 1000

ARP type: ARPA, ARP Timeout 04:00:00,

### Часть 5. Проверка, что маршрутизация между VLAN работает

#### Выполните следующие тесты с PC-A. Все должно быть успешно.

Примечание. Возможно, вам придется отключить брандмауэр ПК для работы ping

##### Отправьте эхо-запрос с PC-A на шлюз по умолчанию.

![](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/Screenshot_4.jpg)

##### Отправьте эхо-запрос с PC-A на PC-B.

![](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/Screenshot_5.jpg)

##### Отправьте команду ping с компьютера PC-A на коммутатор S2.

![](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/Screenshot_6.jpg)

#### Пройдите следующий тест с PC-B

##### В окне командной строки на PC-B выполните команду tracert на адрес PC-A.

![](https://github.com/yksie/Network-engineer/blob/main/lab06(lec13)/Screenshot_7.jpg)

#### Вопрос:

Какие промежуточные IP-адреса отображаются в результатах?

1 – ip шлюза со стороны vlan 30

2 – ip PC-A









