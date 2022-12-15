
# Network Engineer. Basic. ДЗ №8 по лекции №18.

## Лабораторная работа. Реализация DHCPv4


| Устройство  | Интерфейс   | IP-адрес|Маска подсети|Шлюз по умолчанию |
| :------------ |:---------------:| :---------------:|:---------------:| :---------------:|
| R1      | G0/0/0      | 10.0.0.1     |255.255.255.252 |-   |
| R1      | G0/0/1      | -            |-               |-   |
| R1      | G0/0/1.100  | 192.168.1.1  |255.255.255.192 |-   |
| R1      | G0/0/1.200  | 192.168.1.65 |255.255.255.224 |-   |
| R1      | G0/0/1.1000 | -            |-               |-   |
| R2      | G0/0        | 10.0.0.2     |255.255.255.252 |-   |
| R2      | G0/0/1      | 192.168.1.97 |255.255.255.240 |-   |
| S1      | VLAN 200    | 192.168.1.66 |255.255.255.224 |    |
| S2      | VLAN 1      | 192.168.1.98 |255.255.255.240 |    |
| PC-A    | NIC         | DHCP         |DHCP            |DHCP|
| PC-B    | NIC         | DHCP         |DHCP            |DHCP|

> Таблица адресации

| VLAN  | Имя   | Назначенный интерфейс|
| :------------ |:---------------:| :---------------:|
| 1     | Нет         | S2: F0/18    |
| 100   | Клиенты     | S1: F0/6     |
| 200   | Управление  | S1: VLAN 200 |
| 999   | Parking_Lot | S1: F0/1-4, F0/7-24,G0/1-2|
| 1000  | Собственная | -            |

>Таблица VLAN

![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_1.jpg)
> Сеть согласно топологии в ПО СРТ


## Задание

##### [Часть 1. Создание сети и настройка основных параметров устройства](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-1)

##### [Часть 2. Настройка и проверка двух серверов DHCPv4 на R1](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D0%B2%D1%8B%D0%B1%D0%BE%D1%80-%D0%BA%D0%BE%D1%80%D0%BD%D0%B5%D0%B2%D0%BE%D0%B3%D0%BE-%D0%BC%D0%BE%D1%81%D1%82%D0%B0-1)

##### [Часть 3. Настройка и проверка DHCP-ретрансляции на R2](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-3-%D0%BD%D0%B0%D0%B1%D0%BB%D1%8E%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B7%D0%B0-%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%81%D0%BE%D0%BC-%D0%B2%D1%8B%D0%B1%D0%BE%D1%80%D0%B0-%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%D0%BE%D0%BC-stp-%D0%BF%D0%BE%D1%80%D1%82%D0%B0-%D0%B8%D1%81%D1%85%D0%BE%D0%B4%D1%8F-%D0%B8%D0%B7-%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B8-%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2-1)

##### [Вопросы для повторения](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/readme.md#%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B-%D0%B4%D0%BB%D1%8F-%D0%BF%D0%BE%D0%B2%D1%82%D0%BE%D1%80%D0%B5%D0%BD%D0%B8%D1%8F-1)



### Часть 1. Создание сети и настройка основных параметров устройства

#### Создайте сеть согласно топологии.

![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_1.jpg)

#### Разбить подсеть 192.168.1.0/24 на:

a.	Одна подсеть «Подсеть A», поддерживающая 58 хостов (клиентская VLAN на R1).

Подсеть A: 192.168.1.1 255.255.255.192 (R1 G0/0/1.100)
 
b.	Одна подсеть «Подсеть B», поддерживающая 28 хостов (управляющая VLAN на R1). 

Подсеть B: 192.168.1.65 255.255.255.224 (R1 G0/0/1.200)

IP-address S1 VLAN 200 - 192.168.1.66 255.255.255.224

Шлюз по умолчанию для S1 VLAN 200 - 192.168.1.65

c.	Одна подсеть «Подсеть C», поддерживающая 12 узлов (клиентская сеть на R2).

Подсеть C: 192.168.1.97 255.255.255.240

#### Произвести базовую настройку коммутаторов (отключить DNS, назначить class в качестве зашифрованного пароля привилегированного режима EXEC, назначить cisco в качестве пароля консоли и включить вход в систему по паролю, назначить cisco в качестве пароля VTY и включить вход в систему по паролю, зашифровать открытые пароли, создайте баннер с предупреждением о запрете несанкционированного доступа к устройству, сохранить текущую конфигурацию в файл загрузочной конфигурации, установить часы на маршрутизаторе на сегодняшнее время и дату)

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
__________GO AWAY!!!_____#  
ex  
clock set 17:17:17 21 nov 2022  
copy run start  
__R2 настраивается аналогично.__  



#### Настройка маршрутизации между сетями VLAN на маршрутизаторе R1



R1(config)#**int g 0/0/1.100**  
R1(config-subif)#  
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.100, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.100, changed state to up

R1(config-subif)#
R1(config-subif)#**encapsulation dot1Q 100**  
R1(config-subif)#**ip addr 192.168.1.1 255.255.255.192**  
R1(config-subif)#**int g 0/0/1.200**  
R1(config-subif)#  
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.200, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.200, changed state to up

R1(config-subif)#**encapsulation dot1Q 200**  
R1(config-subif)#**ip addr 192.168.1.65 255.255.255.192**  
R1(config-subif)#  
R1(config-subif)#**int g 0/0/1.1000**  
R1(config-subif)#  
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up

R1(config-subif)#  



#### Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов



a.	Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.

R2(config-if)#i**p addr 192.168.1.129 255.255.255.192**  

b.	Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации.

R2(config)#**int g 0/0/0**  
R2(config-if)#**ip addr 10.0.0.2 255.255.255.252**  
R1(config-subif)#**int g 0/0/0**  
R1(config-if)#**ip addr 10.0.0.1 255.255.255.252**  

c.	Настройте маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом маршрутизаторе.

R2(config)#**ip routing**  
R2(config)#**ip route 10.0.0.1 255.255.255.252 g 0/0/0**  
%Default route without gateway, if not a point-to-point interface, may impact performance  
%Inconsistent address and mask

R1(config)#**ip routing**  
R1(config)#**ip route 10.0.0.2 255.255.255.252 g 0/0/0**  
%Default route without gateway, if not a point-to-point interface, may impact performance  
%Inconsistent address and mask  

d.	Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.

![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_2.jpg)



#### Произвести базовую настройку коммутаторов (отключить DNS, назначить class в качестве зашифрованного пароля привилегированного режима EXEC, назначить cisco в качестве пароля консоли и включить вход в систему по паролю, назначить cisco в качестве пароля VTY и включить вход в систему по паролю, зашифровать открытые пароли, создайте баннер с предупреждением о запрете несанкционированного доступа к устройству, сохранить текущую конфигурацию в файл загрузочной конфигурации, установить часы на маршрутизаторе на сегодняшнее время и дату)

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
__________GO AWAY!!!_____#  
ex  
clock set 17:17:17 21 nov 2022  
copy run start  
__S2 настраивается аналогично.__ 

#### Создайте сети VLAN на коммутаторе S1

Примечание. S2 настроен только с базовыми настройками.

a.	Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.

b.	Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.

c.	Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2
Switch(config)#**int vlan 1**  
Switch(config-if)#**ip addr 192.168.1.2 255.255.255.252**  
Switch(config-if)#**ex**  
Switch(config)#**ip def**  
Switch(config)#**ip default-gateway 192.168.1.2**  

d.	Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.

S1(config)#**int r f0/1-4, f0/7-24, g0/1-2**  
S1(config-if-range)#**sw a v 999**  
% Access VLAN does not exist. Creating vlan 999  
S1(config-if-range)#**shut**  

Примечание. Команда interface range полезна для выполнения этой задачи с минимальным количеством команд.

#### Назначьте сети VLAN соответствующим интерфейсам коммутатора.  

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.  
Откройте окно конфигурации  
b.	Убедитесь, что VLAN назначены на правильные интерфейсы.  
	S2(config)#**int f 0/5**  
	S2(config-if)#**sw m tr**  
Вопрос:
Почему интерфейс F0/5 указан в VLAN 1?

__Так по заданию. ПО умолчанию настроен Vlan1__

#### Вручную настройте интерфейс S1 F0/5 в качестве транка 802.1Q.

a. Измените режим порта коммутатора, чтобы принудительно создать магистральный канал.  
S1(config)#**int f 0/5**  
S1(config-if)#**sw m tr**  
a.	В рамках конфигурации транка  установите для native  VLAN значение 1000.  
b.	В качестве другой части конфигурации магистрали укажите, что VLAN 100, 200 и 1000 могут проходить по транку.  
c.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.  
	S1(config-if)#**sw tr n v 1000**  
	S1(config-if)#**sw tr allow v 100,200,1000**  
	S1(config-if)#**do copy run start**  
	Destination filename [startup-config]?   
	Building configuration...  
	[OK]  
	S1(config-if)#  
	Проверьте состояние транка.  
	S1(config-if)#**do sh int tr**  
	Port Mode Encapsulation Status Native vlan  
	Fa0/5 on 802.1q trunking 1000  
	
	Port Vlans allowed on trunk  
	Fa0/5 100,200,1000  
	
	Port Vlans allowed and active in management domain  
	Fa0/5 100  
	
	Port Vlans in spanning tree forwarding state and not pruned  
	Fa0/5 100  
Вопрос:  
Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?  
__192.168.1.67__



### Часть 2. Настройка и проверка двух серверов DHCPv4 на R1

В части 2 необходимо настроить и проверить сервер DHCPv4 на R1. Сервер DHCPv4 будет обслуживать две подсети, подсеть A и подсеть C.

#### Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. Ниже приведен только пул DHCP для подсети A

Исключите первые пять используемых адресов из каждого пула адресов.  
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5  
R1(config)#ip dhcp excluded-address 192.168.1.65 192.168.1.69  
R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.101  
Откройте окно конфигурации  
Создайте пул DHCP (используйте уникальное имя для каждого пула).  
Укажите сеть, поддерживающую этот DHCP-сервер.  
В качестве имени домена укажите CCNA-lab.com.  
Настройте соответствующий шлюз по умолчанию для каждого пула DHCP.  
Настройте время аренды на 2 дня 12 часов и 30 минут. (не реализовано в CPT)  
Затем настройте второй пул DHCPv4, используя имя пула R2_Client_LAN и вычислите сеть, маршрутизатор по умолчанию, и используйте то же имя домена и время аренды, что и предыдущий пул DHCP.  

R1(config)#**ip dhc**  
R1(config)#**ip dhcp pool POOL100**  
R1(dhcp-config)#**net**  
R1(dhcp-config)#**network 192.168.1.0 255.255.255.192**  
R1(dhcp-config)#**def**  
R1(dhcp-config)#**default-router 192.168.1.1**  
R1(dhcp-config)#**ex**  

R1(config)#**ip dhcp pool POOLRight**  (моя версия названия R2_Client_LAN)
R1(dhcp-config)#**netw**  
R1(dhcp-config)#**network 192.168.1.96 255.255.255.240**  
R1(dhcp-config)#**def**  
R1(dhcp-config)#**default-router 192.168.1.97**  
R1(dhcp-config)#**ex**


#### Сохраните конфигурацию.  
Сохраните текущую конфигурацию в файл загрузочной конфигурации.  
Закройте окно настройки.  
Проверка конфигурации сервера DHCPv4  
Чтобы просмотреть сведения о пуле, выполните команду show ip dhcp pool  
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_3.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_4.jpg)
Выполните команду show ip dhcp binding для проверки установленных назначений адресов DHCP.
Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.  (__не реализовано в СРТ__)
Попытка получить IP-адрес от DHCP на PC-A  

Из командной строки компьютера PC-A выполните команду ipconfig /all.
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_5.jpg)  
После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.  
Проверьте подключение с помощью пинга IP-адреса интерфейса R0 G0/0/1.  
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_6.jpg)




### Часть 3. Настройка и проверка DHCP-ретрансляции на R2

В части 3 настраивается R2 для ретрансляции DHCP-запросов из локальной сети на интерфейсе G0/0/1 на DHCP-сервер (R1). 

#### Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1  
Настройте команду ip helper-address на G0/0/1, указав IP-адрес G0/0/0 R1.  
Откройте окно конфигурации  
Сохраните конфигурацию.  
R2(config)#**ip route 0.0.0.0 0.0.0.0 10.0.0.1**  
R2(config)#
R2(config)#**int g0/0/1**  
R2(config-if)#**ip help**  
R2(config-if)#**ip helper-address 10.0.0.1**  
R2(config-if)#**ex**  
#### Попытка получить IP-адрес от DHCP на PC-B  
Из командной строки компьютера PC-B выполните команду ipconfig /all.  
После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.  
Проверьте подключение с помощью пинга IP-адреса интерфейса R1 G0/0/1.  
Выполните show ip dhcp binding для R1 для проверки назначений адресов в DHCP.  
Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.  
	
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_7.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v4/Screenshot_8.jpg)




