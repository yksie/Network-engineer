# Network Engineer. Basic. ДЗ №5 по лекции №11.

## Лабораторная работа. Доступ к сетевым устройствам по протоколу SSH


| Устройство  | Интерфейс   | IP-адрес|Маска подсети|Шлюз по умолчанию|
| :------------ |:---------------:| :---------------:| :---------------:| :-----:|
| R1      | G0/0/1 | 192.168.1.11 |255.255.255.0 |-|
| S1      | VLAN 1 | 192.168.1.11 |255.255.255.0 |192.168.1.1 |
| PC-A      | NIC       |  192.168.1.3 |255.255.255.0 |192.168.1.1 |

## Задание

##### [Часть 1. Настройка основных параметров устройства](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BA-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E-1)

##### [Часть 2. Настройка маршрутизатора для доступа по протоколу SSH](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2-1)

##### [Часть 3. Настройка коммутатора для доступа по протоколу SSH](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-3-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B9-1)

##### [Часть 4. SSH через интерфейс командной строки (CLI) коммутатора](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-3-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B9-1)



### Часть 1. Настройка основных параметров устройства

![](https://github.com/yksie/Network-engineer/blob/main/lab05(lec11)/Screenshot_1.jpg)
> Сеть согласно топологии в ПО СРТ

#### Выполните настройку топологии сети и основные параметры, такие как IP-адреса интерфейсов, доступ к устройствам и пароли на маршрутизаторе.

##### Выполните инициализацию и перезагрузку маршрутизатора и коммутатора.

R1#sh ip int br

Interface IP-Address OK? Method Status Protocol 

GigabitEthernet0/0/0 unassigned YES unset administratively down down 

GigabitEthernet0/0/1 192.168.1.1 YES manual up up 

Vlan1 unassigned YES unset up down

R1#

R1#reload

System configuration has been modified. Save? [yes/no]:y

Building configuration...

[OK]

Proceed with reload? [confirm]

Initializing Hardware ...

S1#sh ip int br

Interface              IP-Address      OK? Method Status                Protocol 

FastEthernet0/1        unassigned      YES manual down                  down 

FastEthernet0/2        unassigned      YES manual down                  down 

FastEthernet0/3        unassigned      YES manual down                  down 

FastEthernet0/4        unassigned      YES manual down                  down 

FastEthernet0/5        unassigned      YES manual up                    up 

FastEthernet0/6        unassigned      YES manual up                    up 

FastEthernet0/7        unassigned      YES manual down                  down 

FastEthernet0/8        unassigned      YES manual down                  down 

-

-

-

FastEthernet0/24       unassigned      YES manual down                  down 

GigabitEthernet0/1     unassigned      YES manual down                  down 

GigabitEthernet0/2     unassigned      YES manual down                  down 

Vlan1                  192.168.1.11    YES manual up                    up



S1#reload

System configuration has been modified. Save? [yes/no]:y

Building configuration...

[OK]

Proceed with reload? [confirm]

##### Настройте маршрутизатор

a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.

b.	Войдите в режим конфигурации.

c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные 
команды таким образом, как будто они являются именами узлов.

d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

g.	Зашифруйте открытые пароли.

h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.

i.	Настройте и активируйте на маршрутизаторе интерфейс G0/0/1, используя информацию, приведенную в таблице адресации.

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

R1#sh run

Building configuration...

Current configuration : 817 bytes

!

version 15.4

no service timestamps log datetime msec

no service timestamps debug datetime msec

service password-encryption

!

hostname R1

!

!

!

enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1

enable password 7 0822455D0A16

!

!
!
ip cef

no ipv6 cef
!
!
!
!
!

spanning-tree mode pvst

!
!
!
!

interface GigabitEthernet0/0/0

no ip address

duplex auto

speed auto

shutdown

!

interface GigabitEthernet0/0/1

ip address 192.168.1.1 255.255.255.0

duplex auto

speed auto

!

interface Vlan1

no ip address

!

ip classless

!

ip flow-export version 9

!

!

!

banner motd ^C

Warning!!!

Warning!!!

Warning!!!

Private territory of 6th homework

Strongly recoomend to leave the area

Warning!!!

Warning!!!

Warning!!!

^C

!

!

!

!

!

line con 0

!

line aux 0

!

line vty 0 4

password 7 0822455D0A16

login

!

!

!

end

##### Настройте компьютер PC-A.

a.	Настройте для PC-A IP-адрес и маску подсети.

b.	Настройте для PC-A шлюз по умолчанию.

![](https://github.com/yksie/Network-engineer/blob/main/lab05(lec11)/Screenshot_2.jpg)

##### Проверьте подключение к сети.

Пошлите с PC-A команду Ping на маршрутизатор R1. Если эхо-запрос с помощью команды ping не проходит, 
найдите и устраните неполадки подключения.

![](https://github.com/yksie/Network-engineer/blob/main/lab05(lec11)/Screenshot_3.jpg)



### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH



#### Настройте аутентификацию устройств.

R1(config)#ip domain-n

R1(config)#ip domain-name ?

WORD Default domain name

R1(config)#ip domain-name domain

#### Создайте ключ шифрования с указанием его длины.

R1(config)#crypto key g

R1(config)#crypto key generate 

R1(config)#crypto key generate rsa 

The name for the keys will be: R1.domain

Choose the size of the key modulus in the range of 360 to 2048 for your
General Purpose Keys. Choosing a key modulus greater than 512 may take
a few minutes.

How many bits in the modulus [512]: 1024

% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

R1(config)#

*Mar 7 5:1:0.485: %SSH-5-ENABLED: SSH 1.99 has been enabled

R1(config)#ip ssh ?

authentication-retries Specify number of authentication retries

time-out Specify SSH time-out interval

version Specify protocol version to be supported

R1(config)#ip ssh ve

R1(config)#ip ssh version ?

<1-2> Protocol version

R1(config)#ip ssh version 2

#### Создайте имя пользователя в локальной базе учетных записей.

Настройте имя пользователя, используя admin в качестве имени пользователя и Adm1nP @55 в качестве пароля.

R1(config)#username admin secret Adm1nP @55

#### Активируйте протокол SSH на линиях VTY.

##### Активируйте протоколы Telnet и SSH на входящих линиях VTY с помощью команды transport input.

R1(config)#line vty 0 4

R1(config-line)#log

R1(config-line)#logi

R1(config-line)#login lo

R1(config-line)#login local 

R1(config-line)#tra

R1(config-line)#transport in

R1(config-line)#transport input ?

all All protocols

none No protocols

ssh TCP/IP SSH protocol

telnet TCP/IP Telnet protocol

R1(config-line)#transport input ssh

##### Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.

#### Cохраните текущую конфигурацию в файл загрузочной конфигурации.

R1#copy run start

Destination filename [startup-config]? 

Building configuration...

[OK]

R1#

#### Установите соединение с маршрутизатором по протоколу SSH.

##### Запустите Tera Term с PC-A.

![](https://github.com/yksie/Network-engineer/blob/main/lab05(lec11)/Screenshot_4.jpg)

##### Установите SSH-подключение к R1. Use the username admin and password Adm1nP@55. У вас должно получиться установить SSH-подключение к R1.

![](https://github.com/yksie/Network-engineer/blob/main/lab05(lec11)/Screenshot_5.jpg)

### Часть 3. Настройка коммутатора для доступа по протоколу SSH

В части 3 вам предстоит настроить коммутатор для приема подключений по протоколу SSH, а затем установить SSH-подключение с помощью программы Tera Term.
Шаг 1. Настройте основные параметры коммутатора.
Откройте окно конфигурации
a.	Подключитесь к коммутатору с помощью консольного подключения и активируйте привилегированный режим EXEC.
b.	Войдите в режим конфигурации.
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
g.	Зашифруйте открытые пароли.
h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.
i.	Настройте и активируйте на коммутаторе интерфейс VLAN 1, используя информацию, приведенную в таблице адресации.
j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

Warning!!!
Warning!!!
Warning!!!
Private territory of 5th homework
Strongly recoomend to leave the area
Warning!!!
Warning!!!
Warning!!!


R1>
R1>en
Password: 
R1#sh run
Building configuration...

Current configuration : 817 bytes
!
version 15.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname R1
!
!
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
enable password 7 0822455D0A16
!
!
!
!
!
!
ip cef
no ipv6 cef
!
!
!
!
!
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface GigabitEthernet0/0/0
no ip address
duplex auto
speed auto
shutdown
!
interface GigabitEthernet0/0/1
ip address 192.168.1.1 255.255.255.0
duplex auto
speed auto
!
interface Vlan1
no ip address
!
ip classless
!
ip flow-export version 9
!
!
!
banner motd ^C
Warning!!!
Warning!!!
Warning!!!
Private territory of 6th homework
Strongly recoomend to leave the area
Warning!!!
Warning!!!
Warning!!!
^C
!
!
!
!
!
line con 0
!
line aux 0
!
line vty 0 4
password 7 0822455D0A16
login
!
!
!
end

Шаг 2. Настройте коммутатор для соединения по протоколу SSH.
Для настройки протокола SSH на коммутаторе используйте те же команды, которые применялись для аналогичной настройки маршрутизатора в части 2.
a.	Настройте имя устройства, как указано в таблице адресации.
b.	Задайте домен для устройства.
c.	Создайте ключ шифрования с указанием его длины.
d.	Создайте имя пользователя в локальной базе учетных записей.
e.	Активируйте протоколы Telnet и SSH на линиях VTY.
f.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.

S1>
S1>en
S1#
S1#conf t
Enter configuration commands, one per line. End with CNTL/Z.
S1(config)#
S1(config)#ip domain-n
S1(config)#ip domain-name domainS
S1(config)#crypto key g
S1(config)#crypto key generate 
S1(config)#crypto key generate rsa 
The name for the keys will be: S1.domainS
Choose the size of the key modulus in the range of 360 to 2048 for your
General Purpose Keys. Choosing a key modulus greater than 512 may take
a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

S1(config)#
*Mar 7 6:8:1.346: %SSH-5-ENABLED: SSH 1.99 has been enabled
S1(config)#ip ssh vers 2

S1(config)#username admin secret admin
S1(config)#line vty 0 4
S1(config-line)#login local
S1(config-line)#transport inpu
S1(config-line)#transport input ?
all All protocols
none No protocols
ssh TCP/IP SSH protocol
telnet TCP/IP Telnet protocol
S1(config-line)#transport input ssh
S1(config-line)#
S1(config-line)#^Z
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#
S1#copy run start
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#
S1#

Шаг 3. Установите соединение с коммутатором по протоколу SSH.
Запустите программу Tera Term на PC-A, затем установите подключение по протоколу SSH к интерфейсу SVI коммутатора S1.
Scr6





