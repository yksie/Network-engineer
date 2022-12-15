
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

a.	Измените режим порта коммутатора, чтобы принудительно создать магистральный канал.  
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
a.	Проверьте состояние транка.  
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








### Часть 2. Выбор корневого моста

#### Отключите все порты на коммутаторах.

S1(config)#**int range f 0/1-4**

S1(config-if-range)#**shut**

S1(config-if-range)#

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down

__По аналогии отключаем порты на S2 и S3.__

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_2.jpg)

#### Настройте подключенные порты в качестве транковых.
S1(config)#**int range f0/1-4**

S1(config-if-range)#**sw mode tr**

#### Включите порты F0/2 и F0/4 на всех коммутаторах.
S1(config)#**int f0/2**

S1(config-if)#**no shut**

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to down

S1(config-if)#**ex**

S1(config)#**int f0/4**

S1(config-if)#**no shut**

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to down

S1(config-if)#

#### Отобразите данные протокола spanning-tree.

S1#**show spanning-tree**

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_3.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_4.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_5.jpg)
 
![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_6.jpg)

#### Oтветьте на следующие вопросы.

Какой коммутатор является корневым мостом? __S1__

Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?

__Поскольку приоритет идентификатора моста одинаков (Priority), а также одинаков VLAN, коммутатор в качестве корневого моста для STP выбирается по наименьшему MAC-адресу__

Какие порты на коммутаторе являются корневыми портами? 

__S2 F0/2; S3 F0/4__

Какие порты на коммутаторе являются назначенными портами?

__S1 F0/2; S1 F0/4; S2 F0/4__

Какой порт отображается в качестве альтернативного и в настоящее время заблокирован? 

__S3 F0/2__

Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?

__Цена пути одинаковая, значит по МАС-адресам. У S3 бОльший МАС, значит с его стороны порт становится Alternative.__


### Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов

#### Определите коммутатор с заблокированным портом.

##### Выполните команду show spanning-tree на обоих коммутаторах некорневого моста.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_4.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_5.jpg)

#### Измените стоимость порта.

##### Помимо заблокированного порта, единственным активным портом на этом коммутаторе является порт, выделенный в качестве порта корневого моста. Уменьшите стоимость этого порта корневого моста до 18, выполнив команду spanning-tree cost 18 режима конфигурации интерфейса.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_9.jpg)

>схема до изменения path cost

S3(config)# interface f0/4

S3(config-if)# spanning-tree vlan 1 cost 18

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_7.jpg) 

>схема после перестроения дерева


#### Просмотрите изменения протокола spanning-tree.

##### Повторно выполните команду show spanning-tree на обоих коммутаторах некорневого моста. Обратите внимание, что ранее заблокированный порт (S1 – F0/4) теперь является назначенным портом, и протокол spanning-tree теперь блокирует порт на другом коммутаторе некорневого моста (S3 – F0/4).

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_8.jpg)

##### Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе?

__path cost до S3 меньше, чем path cost до S2__

#### Удалите изменения стоимости порта.
##### Выполните команду no spanning-tree cost 18 режима конфигурации интерфейса, чтобы удалить запись стоимости, созданную ранее.

S3(config)#**interface f0/4**

S3(config-if)#**no spanning-tree vlan 1 cost 18**

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_9.jpg) 

>схема после перестроения

##### Повторно выполните команду show spanning-tree, чтобы подтвердить, что протокол STP сбросил порт на коммутаторе некорневого моста, вернув исходные настройки порта. Протоколу STP требуется примерно 30 секунд, чтобы завершить процесс перевода порта.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_10.jpg)

### Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

##### Включите порты F0/1 и F0/3 на всех коммутаторах.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_11.jpg) 

__Аналогично для S1 и S2__

##### Выполните команду show spanning-tree на коммутаторах некорневого моста. Обратите внимание, что порт корневого моста переместился на порт с меньшим номером, связанный с коммутатором корневого моста, и заблокировал предыдущий порт корневого моста.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_12.jpg) 

Какой порт выбран протоколом STP в качестве порта корневого моста на каждом коммутаторе некорневого моста? 

__S2-f0/1, S3-f0/3__

Почему протокол STP выбрал эти порты в качестве портов корневого моста на этих коммутаторах?

__1. Они направлены в сторону корневого моста__

__2. При равных приоритетах, смотрится номер порта со стороны корневого моста (подключение осуществляется через порт с наименьшим номером)__


### Вопросы для повторения

1.	Какое значение протокол STP использует первым после выбора корневого моста, чтобы определить выбор порта?

__path cost (в CPT со стороны – противоположной корневому мосту)__

2.	Если первое значение на двух портах одинаково, какое следующее значение будет использовать протокол STP при выборе порта?

__port priority (со стороны корневго моста)__

3.	Если оба значения на двух портах равны, каким будет следующее значение, которое использует протокол STP при выборе порта?

__port number со стороны корневого моста (подключение осуществляется через порт с наименьшим номером)__







