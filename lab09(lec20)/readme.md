
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


![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_4.jpg) 
![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_5.jpg) 
![](https://github.com/yksie/Network-engineer/blob/main/lab09(lec20)/Screenshot_6.jpg) 



### Часть 2. Настройка сетей VLAN





### Часть 3. Настройки безопасности коммутатора








