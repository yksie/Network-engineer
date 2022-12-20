
# Network Engineer. Basic. ДЗ №9 по лекции №20.

## Лабораторная работа. Конфигурация безопасности коммутатора


| Устройство  | Интерфейс   | IP-адрес|Маска подсети|
| :------------ |:---------------:| :---------------:| :---------------:|
| R1      | G0/0/1    | 192.168.10.1   |255.255.255.0 |
| R1      | Loopback0 | 10.10.1.1      |255.255.255.0 |
| S1      | VLAN 10   | 192.168.10.201 |255.255.255.0 |
| S2      | VLAN 10   | 192.168.10.202 |255.255.255.0 |
| PC-A    | NIC       | DHCP           |255.255.255.0 |
| PC-B    | NIC       | DHCP           |255.255.255.0 |  
>Таблица адресации

___

![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_0.jpg)  
>Топология сети

## Задание

##### [Часть 1. Настройка основного сетевого устройства]()

##### [Часть 2. Настройка сетей VLAN]()

##### [Часть 3. Настройки безопасности коммутатора]()




### Часть 1. Настройка основного сетевого устройства

Создайте сеть  
![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_1.jpg)  
>Сеть в ПО CPT

#### Настройте маршрутизатор R1.  
Загрузите следующий конфигурационный скрипт на R1.  
Откройте окно конфигурации  
enable  
configure terminal  
hostname R1  
no ip domain lookup  
ip dhcp excluded-address 192.168.10.1 192.168.10.9  
ip dhcp excluded-address 192.168.10.201 192.168.10.202  

ip dhcp pool Students  
 network 192.168.10.0 255.255.255.0  
 default-router 192.168.10.1  
 domain-name CCNA2.Lab-11.6.1  
 
interface Loopback0  
 ip address 10.10.1.1 255.255.255.0  

interface GigabitEthernet0/0/1  
 description Link to S1  
 ip dhcp relay information trusted (-в CPT отсутствует) 
 ip address 192.168.10.1 255.255.255.0  
 no shutdown  

line con 0  
 logging synchronous  
 exec-timeout 0 0  
#### Проверьте текущую конфигурацию на R1, используя следующую команду:

R1# **show ip interface brief**

#### Убедитесь, что IP-адресация и интерфейсы находятся в состоянии up / up (при необходимости устраните неполадки)

![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_2.jpg) 

Закройте окно настройки.

#### Настройка и проверка основных параметров коммутатора

Настройте имя хоста для коммутаторов S1 и S2.  
**host S1**  
**host S2**  
Откройте окно конфигурации  
Запретите нежелательный поиск в DNS.  
**no ip domain-lookup**
Настройте описания интерфейса для портов, которые используются в S1 и S2.  

![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_3.jpg) 

Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.  
**ip default-gateway 192.168.10.1**   


### Часть 2. Настройка сетей VLAN


#### Сконфигруриуйте VLAN 10.  
Добавьте VLAN 10 на S1 и S2 и назовите VLAN - Management.
#### Сконфигруриуйте SVI для VLAN 10.  
Настройте IP-адрес в соответствии с таблицей адресации для SVI для VLAN 10 на S1 и S2. Включите интерфейсы SVI и предоставьте описание для интерфейса.  
![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_4.jpg) 
S1 настраивается аналогично
#### Настройте VLAN 333 с именем Native на S1 и S2.  
#### Настройте VLAN 999 с именем ParkingLot на S1 и S2.  
![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_5.jpg) 
__S2 аналогично__


### Часть 3. Настройки безопасности коммутатора


#### Релизация магистральных соединений 802.1Q.
Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN.  

**sw m tr**  
**sw tr nat v 333**  

Убедитесь, что режим транкинга успешно настроен на всех коммутаторах.  

S1# **show interface trunk**

Port Mode Encapsulation Status Native vlan  
Fa0/1 on 802.1q trunking 333

Port Vlans allowed on trunk
Fa0/1 1-1005 (в CPT 1005, в методичке 4095)  

Port Vlans allowed and active in management domain  
Fa0/1 1,10,333,999

Port Vlans in spanning tree forwarding state and not pruned  
Fa0/1 1,10,333,999

S2# **show interface trunk**

Port Mode Encapsulation Status Native vlan  
Fa0/1 on 802.1q trunking 333

Port Vlans allowed on trunk  
Fa0/1 1-1005

Port Vlans allowed and active in management domain  
Fa0/1 1,10,333,999

Port Vlans in spanning tree forwarding state and not pruned  
Fa0/1 1,10,333,999

Отключить согласование DTP F0/1 на S1 и S2. 
**sw nonegotiate**

Проверьте с помощью команды show interfaces.

S1# **show interfaces f0/1 switchport | include Negotiation**   
Negotiation of Trunking: Off  

S2# **show interfaces f0/1 switchport | include Negotiation**  
Negotiation of Trunking: Off


#### Настройка портов доступа

a.	На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10.
S1(config)#**int r f 0/5-6**  
S1(config-if-range)#**sw m a**  
S1(config-if-range)#**sw a v 10**  
b.	На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10.  
S2(config)#**int r f0/18**  
S2(config-if-range)#**sw m a**  
S2(config-if-range)#**sw a v 10**  

#### Безопасность неиспользуемых портов коммутатора

На S1 и S2 переместите неиспользуемые порты из VLAN 1 в VLAN 999 и отключите неиспользуемые порты.

S1(config)#**int r f 0/2-4, f0/7-24, g0/1-2**  
S1(config-if-range)#**sw a v 999**  
S1(config-if-range)#**shut**  
S1(config)# **do sh int sta**  

S2(config)#**int r f 0/2-17, f 0/19-24, g0/1-2**  
S2(config-if-range)#**sw a v 999**  
S2(config-if-range)#**shut**  

Убедитесь, что неиспользуемые порты отключены и связаны с VLAN 999, введя команду  show.

S1# **show interfaces status**
 
![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_6.jpg) 
 
___________________________


S2# **show interfaces status**

![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_7.jpg) 
 
___

#### Документирование и реализация функций безопасности порта.
 
Интерфейсы F0/6 на S1 и F0/18 на S2 настроены как порты доступа. На этом шаге вы также настроите безопасность портов на этих двух портах доступа.  
На S1, введите команду show port-security interface f0/6  для отображения настроек по умолчанию безопасности порта для интерфейса F0/6. Запишите свои ответы ниже.




Функция	Настройка по умолчанию
Защита портов	Disabled
Максимальное количество записей MAC-адресов	1
Режим проверки на нарушение безопасности	Shutdown
Aging Time	0 mins
Aging Type	Absolute
Secure Static Address Aging	Disabled
Sticky MAC Address	0

>Конфигурация безопасности порта по умолчанию


На S1 включите защиту порта на F0 / 6 со следующими настройками:
o	Максимальное количество записей MAC-адресов: 3
o	Режим безопасности: restrict
o	Aging time: 60 мин.
o	Aging type: неактивный как?

S1(config)#int f 0/6
S1(config-if)# sw port-security
S1(config-if)#sw port-security maximum 3
S1(config-if)#sw port-security violation res
S1(config-if)#sw po aging time 60

c.	Verify port security on S1 F0/6.
S1# show port-security interface f0/6
Port Security : Enabled
Port Status : Secure-up
Violation Mode : Restrict
Aging Time : 60 mins
Aging Type : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses : 3
Total MAC Addresses : 1
Configured MAC Addresses : 0
Sticky MAC Addresses : 0 (у меня 1)
Last Source Address:Vlan : 0009.7C4B.5624:10
Security Violation Count : 0

S1# show port-security address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan Mac Address Type Ports Remaining Age
                                                                   (mins)
---- ----------- ---- ----- -------------
  10 0009.7C4B.5624 SecureDynamic Fa0/6 60 (I)
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port) : 0
Max Addresses limit in System (excluding one mac per port) : 8192
d.	Включите безопасность порта для F0 / 18 на S2. Настройте каждый активный порт доступа таким образом, чтобы он автоматически добавлял адреса МАС, изученные на этом порту, в текущую конфигурацию.
e.	Настройте следующие параметры безопасности порта на S2 F / 18:
o	Максимальное количество записей MAC-адресов: 2
o	Тип безопасности: Protect
o	Aging time: 60 мин.

S2(config)#int f 0/18
S2(config-if)#sw port-security 
S2(config-if)#sw port-security aging time 60
S2(config-if)#sw po maximum 2
S2(config-if)#sw po violation pro


f.	Проверка функции безопасности портов на S2 F0/18.
S2# show port-security interface f0/18
Port Security : Enabled
Port Status : Secure-up
Violation Mode : Protect
Aging Time : 60 mins
Aging Type : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses : 2
Total MAC Addresses : 1
Configured MAC Addresses : 0
Sticky MAC Addresses : 1
Last Source Address:Vlan : 0007.EC04.1CA6:10
Security Violation Count : 0

S2# show port-security address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan Mac Address Type Ports Remaining Age
                                                                   (mins)
---- ----------- ---- ----- -------------
  10 0007.EC04.1CA6 SecureSticky Fa0/18 -
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port) : 0
Max Addresses limit in System (excluding one mac per port) : 1024
Шаг 5. Реализовать безопасность DHCP snooping.
a.	На S2 включите DHCP snooping и настройте DHCP snooping во VLAN 10.
b.	Настройте магистральные порты на S2 как доверенные порты.


S2(config)#ip dhcp snooping vlan 10
S2(config)#int f 0/1
S2(config-if)#ip dhcp snooping trust

c.	Ограничьте ненадежный порт Fa0/18 на S2 пятью DHCP-пакетами в секунду.


S2(config-if)#ip dhcp snooping limit rate 5


d.	Проверка DHCP Snooping на S2.
S2# show ip dhcp snooping
scr8
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10
DHCP snooping is operational on following VLANs:
10
DHCP snooping is configured on the following L3 Interfaces:
Insertion of option 82 is enabled
   circuit-id default format: vlan-mod-port
   remote-id: 0cd9.96d2.3f80 (MAC)
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Verification of giaddr field is enabled
DHCP snooping trust/rate is configured on the following Interfaces:

Interface Trusted Allow option Rate limit (pps)
----------------------- ------- ------------ ----------------
FastEthernet0/1 yes yes unlimited
  Custom circuit-ids:
FastEthernet0/18 no no 5
  Custom circuit-ids:
e.	В командной строке на PC-B освободите, а затем обновите IP-адрес.
C:\Users\Student> ipconfig /release
C:\Users\Student> ipconfig /renew
scr9
f.	Проверьте привязку отслеживания DHCP с помощью команды show ip dhcp snooping binding.
S2# show ip dhcp snooping binding 
MacIp адресAddress Lease(sec) Type VLAN Interface
------------------ --------------- ---------- ------------- ---- --------------------
00:50:56:90:D0:8E 192.168.10.11 86213 dhcp-snooping 10 FastEthernet0/18
Total number of bindings: 1
у меня нули:
 
Шаг 6. Реализация PortFast и BPDU Guard
 
 
a.	Настройте PortFast на всех портах доступа, которые используются на обоих коммутаторах.
ПРИМЕНЯЕТСЯ К КОНЕЧНЫМ УЗЛАМ
scr10
b.	Включите защиту BPDU на портах доступа VLAN 10 S1 и S2, подключенных к PC-A и PC-B.

ДЛЯ ТРАНКОВОГО F0/1 И F0/18
ПОКА ТРАНКОВЫЙ НЕ НАСТРОИЛ – ОТРУБАЛО ТРАНК

S2(config-if)#spa bpduguard ena
S2(config-if)#ex
S2(config)#spa portfast bpduguard default

c.	Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.
S1# show spanning-tree interface f0/6 detail
 Port 8 (FastEthernet0/6) of VLAN0010 is designated forwarding
   Port path cost 19, Port priority 128, Port Identifier 128.6.
   <output omitted for brevity>
   Number of transitions to forwarding state: 1
   The port is in the portfast mode
   Link type is point-to-point by default
   Bpdu guard is enabled
   BPDU: sent 128, received 0

но эта строчка есть в sh run
__________________________________________________
отрубает f0/1 S1

S1(config)#ip dhcp snooping vlan 10
S1(config)#int f 0/1
S1(config-if)#ip dhcp snooping trust

отключаю bpduguard на транке S1 – сразу отрубает транк


Шаг 7. Проверьте наличие сквозного ⁪подключения.
    
    
Проверьте PING связь между всеми устройствами в таблице IP-адресации. В случае сбоя проверки связи может потребоваться отключить брандмауэр на хостах.

scr11









