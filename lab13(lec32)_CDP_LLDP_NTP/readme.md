# Network Engineer. Basic. ДЗ №13 по лекции №32.

## Лабораторная работа. Настройка протоколов CDP, LLDP и NTP.

![](https://github.com/yksie/Network-engineer/blob/main/lab13(lec32)_CDP_LLDP_NTP/Screenshot_1.jpg)


## Задание

##### Часть 1. Создание сети и настройка основных параметров устройства
##### Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP
##### Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP
##### Часть 4. Настройка и проверка NTP

___

### Часть 1. Создание сети и настройка основных параметров устройства

![](https://github.com/yksie/Network-engineer/blob/main/lab13(lec32)_CDP_LLDP_NTP/Screenshot_2.jpg)

Произведено создание сети и настройка основных параметров устройств, настройка сетей VLAN1 на коммутаторах

**Произведите базовую настройку маршрутизаторов**

en  
conf t  
no ip domain-lookup   
enable secret class  
service password-encryption   
line console 0  
password cisco  
login  
logging synchronous   
exit  
line vty 0 15  
password cisco  
login  
exit  
banner motd #  
GO AWAY!!!#  
ex  
copy run start  
conf t  
int g 0/0/0  
ip addr 10.22.0.1 255.255.255.0  
int lo1
ip addr 172.16.1.1 255.255.255.0  
ex  
copy run start  

**Настройте базовые параметры каждого коммутатора.**

en  
conf t  
no ip domain-lookup  
enable secret class  
service password-encryption   
line console 0  
password cisco  
login  
logging synchronous   
exit  
line vty 0 15  
password cisco  
login  
exit  
banner motd #  
GO AWAY!!!#  
ex  
copy run start  
conf t  
int vlan 1  
ip addr 10.22.0.2 255.255.255.0  
no shut  
ex  
ex  
copy run start  

### Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP

На устройствах Cisco протокол CDP включен по умолчанию. Воспользуйтесь CDP, чтобы обнаружить порты, к которым подключены кабели.

На R1 используйте соответствующую команду show cdp, чтобы определить, сколько интерфейсов включено CDP, сколько из них включено и сколько отключено.
 
R1#**sh cdp int**  
Vlan1 is administratively down, line protocol is down  
Sending CDP packets every 60 seconds  
Holdtime is 180 seconds  
GigabitEthernet0/0/0 is administratively down, line protocol is down  
Sending CDP packets every 60 seconds  
Holdtime is 180 seconds  
GigabitEthernet0/0/1 is up, line protocol is up  
Sending CDP packets every 60 seconds  
Holdtime is 180 seconds  
GigabitEthernet0/0/2 is administratively down, line protocol is down  
Sending CDP packets every 60 seconds  
Holdtime is 180 seconds  
Вопрос:    
Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны?  
**4, G0/0/1**  
На R1 используйте соответствующую команду show cdp, чтобы определить версию IOS, используемую на S1.  

R1#**sh cdp entry S1**  

Device ID: S1  
Platform: cisco 2960, Capabilities: Switch  
Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5  
Holdtime: 172  

Version :  
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)  
Technical Support: http://www.cisco.com/techsupport  
Copyright (c) 1986-2013 by Cisco Systems, Inc.  
Compiled Wed 26-Jun-13 02:49 by mnguyen  

advertisement version: 2  
Duplex: full  
Вопрос:  
Какая версия IOS используется на  S1?  
**15.0(2)SE4, RELEASE SOFTWARE (fc1)**  

На S1 используйте соответствующую команду show cdp, чтобы определить, сколько пакетов CDP было выданных.

S1# **show cdp traffic – этой команды нет в СРТ**  
CDP counters :   
        Total packets output: 179, Input: 148   
        Hdr syntax: 0, Chksum error: 0, Encaps failed: 0   
        No memory: 0, Invalid packet: 0,   
        CDP version 1 advertisements output: 0, Input: 0   
        CDP version 2 advertisements output: 179, Input: 148  
 
Настройте SVI для VLAN 1 на S1 и S2, используя IP-адреса, указанные в таблице адресации выше. Настройте шлюз по умолчанию для каждого коммутатора на основе таблицы адресов.  

**ip default-gateway 10.22.0.1**  
 
На R1 выполните команду show cdp entry S1 .   
Вопрос:  
Какие дополнительные сведения доступны теперь?  
**Появились данные:**  
**ip SVI Switch;**  
**VTP Management Domain: '';**  
**Native VLAN: 1.**  

 
R1 # **show cdp entry  S1**  

Device ID: S1  
Entry address(es):   
IP address : 10.22.0.2  
Platform: cisco 2960, Capabilities: Switch  
Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5  
Holdtime: 126  

Version :
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)  
Technical Support: http://www.cisco.com/techsupport  
Copyright (c) 1986-2013 by Cisco Systems, Inc.  
Compiled Wed 26-Jun-13 02:49 by mnguyen    

advertisement version: 2  
Duplex: full  

Отключить CDP глобально на всех устройствах.   
  
R1(config)#**no cdp run**  
R1(config)#ex**  
%SYS-5-CONFIG_I: Configured from console by console  

R1#**sh cdp entry S1**  
% CDP is not enabled  
R1#  

S1, S2 аналогично  


### Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP  

На устройствах Cisco протокол LLDP может быть включен по умолчанию. Воспользуйтесь LLDP, чтобы обнаружить порты, к которым подключены кабели.
Откройте окно конфигурации  
Введите соответствующую команду lldp, чтобы включить LLDP на всех устройствах в топологии.  
R1(config)#**lldp run**  
S1(config)#**lldp run**  
S2(config)#**lldp run**  
На S1 выполните соответствующую команду lldp, чтобы предоставить подробную информацию о S2.   
S1# **show lldp entry S2 – в cpt не реализовано, но ответ на реальном оборудовании:**  

Capability codes:  
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device  
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other  

Local Intf: Fa0/1    
Chassis id: c025.5cd7.ef00   
Port id: Fa0/1   
Port Description: FastEthernet0/1  
System Name: S2  

System Description:  
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.2(4)E8, RELEASE SOFTWARE (fc3)   
Technical Support: http://www.cisco.com/techsupport  
Copyright (c) 1986-2019 by Cisco Systems, Inc.  
Compiled Fri 15-Mar-19 17:28 by prod_rel_team   

Time remaining: 109 seconds   
System Capabilities: B  
Enabled Capabilities: B  
Management Addresses:  
    IP: 10.22.0.3   
Auto Negotiation - supported, enabled  
Physical media capabilities:  
    100base-TX(FD)  
    100base-TX(HD)  
    10base-T(FD)  
    10base-T(HD)  
Media Attachment Unit type: 16  
Vlan ID: 1  


Total entries displayed: 1  
Вопрос:  
Что такое chassis ID  для коммутатора S2?  
**MAC- адрес Switch*    

Соединитесь через консоль на всех устройствах и используйте команды LLDP, необходимые для отображения топологии физической сети только из выходных данных команды show.

R1#**sh lldp neighbors**  
Capability codes:  
(R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device  
(W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other  
Device ID Local Intf Hold-time Capability Port ID  
S1 Gig0/0/1 120 B Fa0/5  
Total entries displayed: 1  

S1#**sh lldp neighbors**   
Capability codes:  
(R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device  
(W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other  
Device ID Local Intf Hold-time Capability Port ID  
R1 Fa0/5 120 R Gig0/0/1  
S2 Fa0/1 120 B Fa0/1  
Total entries displayed: 2  

S2#**sh lldp neighbors**   
Capability codes:  
(R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device  
(W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other  
Device ID Local Intf Hold-time Capability Port ID  
S1 Fa0/1 120 B Fa0/1  
Total entries displayed: 1  


### Часть 4. Настройка и проверка NTP  

В части 4 необходимо настроить маршрутизатор R1 в качестве сервера NTP, а маршрутизатор R2 в качестве клиента NTP маршрутизатора R1. Необходимо выполнить синхронизацию времени для Syslog и отладочных функций. Если время не синхронизировано, сложно определить, какое сетевое событие стало причиной данного сообщения.  

Введите команду show clock для отображения текущего времени на R1. Запишите отображаемые сведения о текущем времени в следующей таблице.

R1#**sh clock**  
*0:41:52.287 UTC Mon Mar 1 1993  
Дата	Время	Часовой пояс	Источник времени  

С помощью команды clock set установите время на маршрутизаторе R1. Введенное время должно быть в формате UTC.   

 R1#**clock set 11:45:00 Feb 6 2023**  
 
Настройте главный сервер NTP.  

Настройте R1 в качестве хозяина NTP с уровнем слоя 4.  

R1(config)#**ntp master 4**  
R1(config)#**ntp update-calendar**  
		
Настройте S1 и S2 в качестве клиентов NTP. Используйте соответствующие команды NTP для получения времени от интерфейса G0/0/1 R1, а также для периодического обновления календаря или аппаратных часов коммутатора.  

S1(config)#**ntp server 10.22.0.1**  
S2(config)#**ntp server 10.22.0.1**  

**после нескольких минут ожидания:**    
S1#**sh clock detail**    
12:50:53.35 UTC Mon Feb 6 2023  
Time source is NTP  

Проверьте настройку NTP.

Используйте соответствующую команду show , чтобы убедиться, что S1 и S2 синхронизированы с R1.

S1#**sh ntp status**  
Clock is synchronized, stratum 16, reference is 10.22.0.1  
nominal freq is 250.0000 Hz, actual freq is 249.9990 Hz, precision is 2**24  
reference time is 1FA9006B.0000022A (1:31:23.554 UTC Tue Jan 14 2053)  
clock offset is 0.00 msec, root delay is 0.00 msec  
root dispersion is 10.20 msec, peer dispersion is 0.12 msec.  
loopfilter state is 'CTRL' (Normal Controlled Loop), drift is - 0.000001193 s/s system poll interval is 4, last update was 10 sec ago.  

S1#**sh ntp associations**

address ref clock st when poll reach delay offset disp  
*~10.22.0.1 127.127.1.1 4 0 16 17 0.00 0.00 0.12  
sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured  
  
Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время.

![](https://github.com/yksie/Network-engineer/blob/main/lab13(lec32)_CDP_LLDP_NTP/Screenshot_3.jpg)
