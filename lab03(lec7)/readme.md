
# ДЗ №3 по лекции №7.

## Лабораторная работа. Расчет подсетей IPv4 


| Дано: |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |



| Устройство  | Интерфейс   | IP-адрес| Маска подсети |
| :------------ |:---------------:|:---------------:|-----:|
| S1      | VLAN 1 | 192.168.1.11 | 255.255.255.0 |
| S2      | VLAN 1 | 192.168.1.12 | 255.255.255.0 |
| PC-A      | NIC       |  192.168.1.1 | 255.255.255.0 |
| PC-B      | NIC       |  192.168.1.2 | 255.255.255.0 |

## Задание

##### [Часть 1. Создание сети и настройка сети.](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D1%81%D0%B5%D1%82%D0%B8-1)

##### [Часть 2. Настройка базовых параметров сетевых устройств](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D0%B8%D0%B7%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D1%8B-%D0%BC%D0%B0%D1%81-%D0%B0%D0%B4%D1%80%D0%B5%D1%81%D0%BE%D0%B2-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0)

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
