# ДЗ №2 по лекции №4.

## Лабораторная работа. Канальный уровень. Ethernet. 

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/1.png)
> Топология сети


| Устройство  | Интерфейс   | IP-адрес| Маска подсети |
| :------------ |:---------------:|:---------------:|-----:|
| S1      | VLAN 1 | 192.168.1.11 | 255.255.255.0 |
| S2      | VLAN 1 | 192.168.1.12 | 255.255.255.0 |
| PC-A      | NIC       |  192.168.1.1 | 255.255.255.0 |
| PC-B      | NIC       |  192.168.1.2 | 255.255.255.0 |

## Задание

##### [Часть 1. Создание сети и настройка сети.](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BA-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E-1)

##### [Часть 2. Настройка базовых параметров сетевых устройств](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2-1)

##### [Часть 3. Проверка сетевых подключений](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-3-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B9-1)




### Часть 1. Создание сети и настройка сети.

##### Необходимо настроить IP-адреса устройств, переименовать устройства, назначить пароли для консоли, vty и доступа к привелегированному режиму EXEC

Switch>ena
Switch#conf t

Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#int vlan 1

Switch(config-if)#ip addr 192.168.1.11 255.255.255.0

Switch(config-if)#no shutdown

Switch#sh ip int vlan 1

Vlan1 is up, line protocol is up

Internet address is 192.168.1.11/24

Broadcast address is 255.255.255.255

Switch#conf t

Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#line vty 0 15

Switch(config-line)#password cisco

Switch(config-line)#login

Switch(config-line)#end

Switch#

Switch#conf t

Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#hostname S1

S1(config)#

S1(config)#line ?

<0-16> First Line number

console Primary terminal line

vty Virtual terminal

S1(config)#line console ?

<0-0> First Line number

S1(config)#line console 0

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#end

S1#

S1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#

S1(config)#enable secret class

S1(config)#

S1(config)#exit

S1#

S1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#

S1(config)#enable secret class

S1(config)#

S1(config)#exit

S1#

По аналогии настраиваем второй коммутатор

Switch>ena

Switch#conf t

Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#int vlan 1

Switch(config-if)#ip addr 192.168.1.12 255.255.255.0

Switch(config-if)#no shutdown

Switch(config-if)#

%LINK-5-CHANGED: Interface Vlan1, changed state to up

Switch#sh int vlan 1

Vlan1 is up, line protocol is up

Hardware is CPU Interface, address is 00e0.a344.dc73 (bia 00e0.a344.dc73)

Internet address is 192.168.1.12/24


### Часть 2. Изучение таблицы МАС-адресов коммутатора

##### Необходимо настроить IP-адреса устройств, переименовать уст


