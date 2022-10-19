# ДЗ №4 по лекции №8.

## Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/0.jpg)



## Задачи

##### [Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D1%81%D0%B5%D1%82%D0%B8-1)

##### [Часть 2. Ручная настройка IPv6-адресов](https://github.com/yksie/Network-engineer/blob/main/lab02(lec4)/readme.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D0%B8%D0%B7%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D1%8B-%D0%BC%D0%B0%D1%81-%D0%B0%D0%B4%D1%80%D0%B5%D1%81%D0%BE%D0%B2-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0)

##### [Часть 3. Проверка сквозного подключения](https:

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/1.jpg)

> Собранная сеть в CPT



Шаблон по умолчанию менеджера базы данных 2960 Switch Database Manager (SDM) не поддерживает IPv6. Перед назначением IPv6-адреса SVI VLAN 1 может понадобиться выполнение команды sdm prefer dual-ipv4-and-ipv6 default для включения IPv6-адресации.

Шаблон default bias, который по умолчанию используется диспетчером SDM (диспетчер базы данных коммутатора), не предоставляет возможностей адресации IPv6. Убедитесь, что SDM использует шаблон dual-ipv4-and-ipv6 или lanbase-routing. Новый шаблон будет использоваться после перезагрузки. 

S1# **show sdm prefer**

The current template is "default" template.

Чтобы установить шаблон dual-ipv4-and-ipv6 в качестве шаблона SDM по умолчанию, выполните следующие действия:

S1# **configure terminal**

S1(config)# **sdm prefer dual-ipv4-and-ipv6 default**

S1(config)# **end**

S1# **reload**

S1#**sh sdm prefer**

The current template is "dual-ipv4-and-ipv6 default" template.

### Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора

##### Настройка маршрутизатора

Router(config)#**hostname R1**

##### Настройка коммутатора

S1# **configure terminal**

Switch(config)# **hostname S1**

S1(config)# **sdm prefer dual-ipv4-and-ipv6 default**

S1(config)# **end**

S1# **reload**


### Часть 2. Ручная настройка IPv6-адресов

##### Назначение IPv6-адреса интерфейсам Ethernet на R1

Настройка глобальных IPv6-адресов, указанных в таблице адресации обоим интерфейсам Ethernet на R1.

R1(config)#**int gig**

R1(config)#**int gigabitEthernet 0/0/0**

R1(config-if)#**ipv6 address 2001:db8:acad:a ::1/64**

R1(config-if)#**no shut**

R1(config-if)#

%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

R1(config)#**int gi**

R1(config)#**int gigabitEthernet 0/0/1**

R1(config-if)#**ipv6 address 2001:db8:acad:1::1/64**

R1(config-if)#

R1(config-if)#**no shut**

R1(config-if)#

%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

R1#**sh ipv6 int br**

GigabitEthernet0/0/0 [up/up]

FE80::2E0:B0FF:FE0E:8401

2001:DB8:ACAD:A::1

GigabitEthernet0/0/1 [up/up]

FE80::2E0:B0FF:FE0E:8402

2001:DB8:ACAD:1::1

Vlan1 [administratively down/down]

Проверка, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.

R1#**sh ipv6 int br**

GigabitEthernet0/0/0 [up/up]

FE80::2E0:B0FF:FE0E:8401

2001:DB8:ACAD:A::1

GigabitEthernet0/0/1 [up/up]

FE80::2E0:B0FF:FE0E:8402

2001:DB8:ACAD:1::1

Vlan1 [administratively down/down]

Unassigned

Настройка локальных адресов канала на каждом интерфейсе Ethernet на R1.

R1>

R1>**en**

R1#**conf t**

Enter configuration commands, one per line. End with CNTL/Z.

R1(config)#**int g**

R1(config)#**int gigabitEthernet 0**

R1(config)#**int gigabitEthernet 0/0/0**

R1(config-if)#**ipv6 addr**

R1(config-if)#**ipv6 address fe80::1 l**

R1(config-if)#**ipv6 address fe80::1 link-local**

R1(config-if)#**exi**

R1(config)#**int g**

R1(config)#**int gigabitEthernet 0/0/1**

R1(config-if)#**ipv6 address fe80::1 link-local**

Проверка, что локальный адрес связи изменен на fe80::1.  

R1(config-if)#**do sh ipv6 int br**

GigabitEthernet0/0/0 [up/up]

FE80::1

2001:DB8:ACAD:A::1

GigabitEthernet0/0/1 [up/up]

FE80::1

2001:DB8:ACAD:1::1

Vlan1 [administratively down/down]

unassigned

R1(config-if)#

##### Какие группы многоадресной рассылки назначены интерфейсу G0/0?

R1(config)#do sh ipv6 int g0/0/0

GigabitEthernet0/0/0 is up, line protocol is up

IPv6 is enabled, link-local address is FE80::1

No Virtual link-local address(es):

Global unicast address(es):

2001:DB8:ACAD:A::1, subnet is 2001:DB8:ACAD:A::/64

Joined group address(es):

Группы многоадресной рассылки:

__FF02::1__

__FF02::1:FF00:1__

##### Активируйте IPv6-маршрутизацию на R1.

В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.

FastEthernet0 Connection:(default port)

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   
   Link-local IPv6 Address.........: FE80::2D0:97FF:FE09:DC6C
   
   IPv6 Address....................: ::
   
   IPv4 Address....................: 0.0.0.0
   
   Subnet Mask.....................: 0.0.0.0
   
   Default Gateway.................: ::
   
   0.0.0.0с:

Назначен ли глобальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?

__нет__

Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing

R1(config)#**ipv6 unicast-routing**

Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.

C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   
   Link-local IPv6 Address.........: FE80::2D0:97FF:FE09:DC6C
   
   IPv6 Address....................: 2001:DB8:ACAD:A:2D0:97FF:FE09:DC6C
   
   IPv4 Address....................: 0.0.0.0
   
   Subnet Mask.....................: 0.0.0.0
   
   Default Gateway.................: FE80::1
   
   0.0.0.0 

Почему PC-B получил глобальный префикс маршрутизации и идентификатор подсети, которые вы настроили на R1?

__IPv6 на PC должен стоять в автоматическом режиме.__

__PC-B осуществил запрос параметров в сети.__

__PC-B получил первые 64 бит от R1.__

__PC-B получил последние 64 бит от Link-local PC-B.__

__PC-B получил шлюз по умолчанию от R1__

##### Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.

Назначьте адрес IPv6 для S1. Также назначьте этому интерфейсу локальный адрес канала.

S1#**sh ipv6 int br**

FastEthernet0/1 [up/up]

FastEthernet0/2 [up/up]

FastEthernet0/3 [down/down]

FastEthernet0/4 [down/down]

-

-

-

FastEthernet0/24 [down/down]

GigabitEthernet0/1 [down/down]

GigabitEthernet0/2 [down/down]

Vlan1 [up/up]

FE80::206:2AFF:FEBB:7A60

S1#**en**

S1#**conf t**

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#

S1(config)#**int vlan 1**

S1(config-if)#**ipv6 add**

S1(config-if)#**ipv6 address 2001:db8:acad:1::b/64**

S1(config-if)#**no shut**

S1(config-if)#**exit**

S1(config)#**exit**

S1#

%SYS-5-CONFIG_I: Configured from console by console

S1#

S1#**sh ipv6 int br**

FastEthernet0/1 [up/up]

FastEthernet0/2 [up/up]

-

-

-

FastEthernet0/24 [down/down]

GigabitEthernet0/1 [down/down]

GigabitEthernet0/2 [down/down]

Vlan1 [up/up]

FE80::206:2AFF:FEBB:7A60

2001:DB8:ACAD:1::B

S1#

Проверьте правильность назначения IPv6-адресов интерфейсу управления с помощью команды show ipv6 interface vlan1.

S1#

S1#**sh ipv6 int vlan 1**

Vlan1 is up, line protocol is up

IPv6 is enabled, link-local address is FE80::206:2AFF:FEBB:7A60

No Virtual link-local address(es):

Global unicast address(es):

2001:DB8:ACAD:1::B, subnet is 2001:DB8:ACAD:1::/64

Joined group address(es):

FF02::1

FF02::1:FF00:B

FF02::1:FFBB:7A60

MTU is 1500 bytes

ICMP error messages limited to one every 100 milliseconds

ICMP redirects are enabled

ICMP unreachables are sent

Output features: Check hwidb

ND DAD is enabled, number of DAD attempts: 1

ND reachable time is 30000 milliseconds

ND NS retransmit interval is 1000 milliseconds

##### Назначьте компьютерам статические IPv6-адреса.

Откройте окно Свойства Ethernet для каждого ПК и назначьте адресацию IPv6.

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/2.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/3.jpg)

Убедитесь, что оба компьютера имеют правильную информацию адреса IPv6. Каждый компьютер должен иметь два глобальных адреса IPv6: один статический и один SLACC

PC-A:

C:\>**ipconfig**

FastEthernet0 Connection:(default port)


Connection-specific DNS Suffix..: 

Link-local IPv6 Address.........: FE80::2

IPv6 Address....................: 2001:DB8:ACAD:1::3

IPv4 Address....................: 0.0.0.0

Subnet Mask.....................: 0.0.0.0

Default Gateway.................: ::

0.0.0.0

PC-B:

C:\>**ipconfig**

FastEthernet0 Connection:(default port)


Connection-specific DNS Suffix..: 

Link-local IPv6 Address.........: FE80::2D0:97FF:FE09:DC6C

IPv6 Address....................: 2001:DB8:ACAD:A::3

IPv4 Address....................: 0.0.0.0

Subnet Mask.....................: 0.0.0.0

Default Gateway.................: ::

0.0.0.0

### Часть 3. Проверка сквозного подключения

С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1:

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/4.jpg)

Отправьте эхо-запрос на интерфейс управления S1 с PC-A:

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/5.jpg)

Введите команду tracert на PC-A, чтобы проверить наличие сквозного подключения к PC-B:

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/6.jpg) 

С PC-B отправьте эхо-запрос на PC-A:

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/7.jpg)

С PC-B отправьте эхо-запрос на локальный адрес канала G0/0 на R1:

![](https://github.com/yksie/Network-engineer/blob/main/lab04(lec08)/8.jpg)

##### Вопросы для повторения

1.	Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот же локальный адрес канала — FE80::1?

Каждый интерфейс маршрутизатора относится к отдельной сети. Пакеты с локальным адресом канала никогда не выходят за пределы локальной сети, а значит, для обоих интерфейсов можно указывать один и тот же локальный адрес канала.

2.	Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64?

первые 64 бит - префикс

из них:

48 – префикс 

16 – идентификатор подсети

вторые 64 бит - идентификатор интерфейса

IPv6 адрес - 2001:db8:acad:0000:0000:0000:aaaa:1234

Смотрим на четвертый хекстет.

Ответ: 0000




