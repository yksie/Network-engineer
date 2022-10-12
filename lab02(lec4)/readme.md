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

##### Откройте командную строку на PC-A и PC-B и введите команду ipconfig /all.

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/2.png)

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/3.png)

Назовите физические адреса адаптера Ethernet. 

MAC-адрес компьютера PC-A: __0060.70AD.2436__

MAC-адрес компьютера PC-B: __00D0.D39E.CB57__

Закройте окно командной строки.

a.	Подключитесь к коммутаторам S1 и S2 через консоль и введите команду show interface F0/1 на каждом коммутаторе.

Откройте окно конфигурации

Вопросы:

Назовите адреса оборудования во второй строке выходных данных команды (или зашитый адрес — bia).

МАС-адрес коммутатора S1 Fast Ethernet 0/1: __000c.cf77.5601__

МАС-адрес коммутатора S2 Fast Ethernet 0/1: __000b.be93.5801__

Закройте окно настройки.

##### Просмотрите таблицу МАС-адресов коммутатора.

Подключитесь к коммутатору S2 через консоль и просмотрите таблицу МАС-адресов до и после тестирования сетевой связи с помощью эхо-запросов.

a.	Подключитесь к коммутатору S2 через консоль и войдите в привилегированный режим EXEC.

Откройте окно конфигурации

b.	В привилегированном режиме EXEC введите команду show mac address-table и нажмите клавишу ввода.

S2# show mac address-table

Даже если сетевая коммуникация в сети не происходила (т. е. если команда ping не отправлялась), коммутатор может узнать МАС-адреса при подключении к ПК и другим коммутаторам.

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/4.png)

Записаны ли в таблице МАС-адресов какие-либо МАС-адреса?

__Есть. Соседний коммутатор.__

Если вы не записали МАС-адреса сетевых устройств в шаге 1, как можно определить, каким устройствам принадлежат МАС-адреса, используя только выходные данные команды show mac address-table? Работает ли это решение в любой ситуации?

__По первой половине адреса (три байта)__

##### Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.
В привилегированном режиме EXEC введите команду clear mac address-table dynamic и нажмите клавишу Enter.

S2# clear mac address-table dynamic

Снова быстро введите команду show mac address-table.

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/5.png)

Указаны ли в таблице МАС-адресов адреса для VLAN 1? Указаны ли другие МАС-адреса?

Через 10 секунд введите команду show mac address-table и нажмите клавишу ввода. Появились ли в таблице МАС-адресов новые адреса?

__Нет, все также, баг CPT (в жизни пропадет)__

##### С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.

На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/6.png)

Не считая адресов многоадресной и широковещательной рассылки, сколько пар IP- и МАС-адресов устройств было получено через протокол ARP?

__0__

Из командной строки PC-B отправьте эхо-запросы на компьютер PC-A, а также коммутаторы S1 и S2.

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/7.png)

От всех ли устройств получены ответы? Если нет, проверьте кабели и IP-конфигурации.

__От всех были получены ответы__

Подключившись через консоль к коммутатору S2, введите команду show mac address-table

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/8.png)

Добавил ли коммутатор в таблицу МАС-адресов дополнительные МАС-адреса? Если да, то какие адреса и устройства?

__Верхний остался. Это S2
Вторая строчка - Это S1
Два нижних – два РС.__


На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.

![](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/9.png)

Появились ли в ARP-кэше компьютера PC-B дополнительные записи для всех сетевых устройств, которым были отправлены эхо-запросы?

__Да__

Закройте командную строку.

##### Вопрос для повторения

В сетях Ethernet данные передаются на устройства по соответствующим МАС-адресам. Для этого коммутаторы и компьютеры динамически создают ARP-кэш и таблицы МАС-адресов. Если компьютеров в сети немного, эта процедура выглядит достаточно простой. Какие сложности могут возникнуть в крупных сетях?

__Засорение широковещательным трафиком.__



