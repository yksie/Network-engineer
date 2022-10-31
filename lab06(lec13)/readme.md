
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

##### [Часть 1. Создание сети и настройка основных параметров устройства]

##### [Часть 2. Создание сетей VLAN и назначение портов коммутатора]

##### [Часть 3. Настройка транка 802.1Q между коммутаторами]

##### [Часть 4. Настройка маршрутизации между сетями VLAN]

##### [Часть 5. Проверка, что маршрутизация между VLAN работает](https://github.com/yksie/Network-engineer/blob/main/lab05(lec11)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-4-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%D0%B0-ssh-%D1%81-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%D0%BC-%D0%B8%D0%BD%D1%82%D0%B5%D1%80%D1%84%D0%B5%D0%B9%D1%81%D0%B0-%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%BD%D0%BE%D0%B9-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B8-cli-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0)


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

По аналогии настраивается второй коммутатор
