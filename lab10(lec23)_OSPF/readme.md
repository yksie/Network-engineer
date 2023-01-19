# Network Engineer. Basic. ДЗ №10 по лекции №23.

## Лабораторная работа. Настройка протокола OSPFv2 для одной области


| Устройство  | Интерфейс   | IP-адрес|Маска подсети|
| :------------ |:---------------:| :---------------:| :---------------:|
| R1      | G0/0/1    | 10.53.0.1   |255.255.255.0 |
| R1      | Loopback 1| 172.16.1.1  |255.255.255.0 |
| R2      | G0/0/1    | 10.53.0.2   |255.255.255.0 |
| R2      | Loopback 1| 192.168.1.1 |255.255.255.0 |

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_0.jpg)

## Задание

##### [Часть 1. Создание сети и настройка основных параметров устройства](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-1)

##### [Часть 2. Выбор корневого моста](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D0%B2%D1%8B%D0%B1%D0%BE%D1%80-%D0%BA%D0%BE%D1%80%D0%BD%D0%B5%D0%B2%D0%BE%D0%B3%D0%BE-%D0%BC%D0%BE%D1%81%D1%82%D0%B0-1)

##### [Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-3-%D0%BD%D0%B0%D0%B1%D0%BB%D1%8E%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B7%D0%B0-%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%81%D0%BE%D0%BC-%D0%B2%D1%8B%D0%B1%D0%BE%D1%80%D0%B0-%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%D0%BE%D0%BC-stp-%D0%BF%D0%BE%D1%80%D1%82%D0%B0-%D0%B8%D1%81%D1%85%D0%BE%D0%B4%D1%8F-%D0%B8%D0%B7-%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B8-%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2-1)

##### [Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-4-%D0%BD%D0%B0%D0%B1%D0%BB%D1%8E%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B7%D0%B0-%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%81%D0%BE%D0%BC-%D0%B2%D1%8B%D0%B1%D0%BE%D1%80%D0%B0-%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%D0%BE%D0%BC-stp-%D0%BF%D0%BE%D1%80%D1%82%D0%B0-%D0%B8%D1%81%D1%85%D0%BE%D0%B4%D1%8F-%D0%B8%D0%B7-%D0%BF%D1%80%D0%B8%D0%BE%D1%80%D0%B8%D1%82%D0%B5%D1%82%D0%B0-%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2-1)

##### [Вопросы для повторения](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B-%D0%B4%D0%BB%D1%8F-%D0%BF%D0%BE%D0%B2%D1%82%D0%BE%D1%80%D0%B5%D0%BD%D0%B8%D1%8F-1)



### Часть 1. Создание сети и настройка основных параметров устройства

#### Создайте сеть согласно топологии.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_1.jpg)

#### Настройте базовые параметры каждого коммутатора.

##### Отключите поиск DNS.

S1>**en**

S1#**conf t**

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#**no ip dom**

S1(config)#**no ip domain-lookup**
