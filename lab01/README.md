# Network Engineer. Basic. ДЗ по лекции №2.

## Лабораторная работа. Базовая настройка коммутатора


| Устройство  | Интерфейс   | IP-адрес/префикс|
| :------------ |:---------------:| -----:|
| S1      | VLAN 1 | 192.168.0.10/18 |
| PC-A      | NIC       |  192.168.0.11/18 |

## Задание

##### [Часть 1. Создание сети и проверка настроек коммутатора по умолчанию.](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-1-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%82%D0%B8-%D0%B8-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BA-%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D1%82%D0%B0%D1%82%D0%BE%D1%80%D0%B0-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E-1)

##### [Часть 2. Настройка базовых параметров сетевых устройств](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-2-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2-1)

##### [Часть 3. Проверка сетевых подключений](https://github.com/yksie/Network-engineer/edit/main/lab01/README.md#%D1%87%D0%B0%D1%81%D1%82%D1%8C-3-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B9-1)




### Часть 1. Создание сети и проверка настроек коммутатора по умолчанию.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/01.png?raw=true)
> S1 с отключенным 6-ым портом

Почему нужно использовать консольное подключение для первоначальной настройки коммутатора? Почему нельзя подключиться к коммутатору через Telnet или SSH?

__Потому что для подключения по SSH нужен IP коммутатора.__

![](https://github.com/yksie/Network-engineer/blob/main/lab01/02.png?raw=true)
Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?

__24__

Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?

__2__

Каков диапазон значений, отображаемых в vty-линиях?

__0-4 and 5-15__

![](https://github.com/yksie/Network-engineer/blob/main/lab01/03.png?raw=true)

Почему появляется это сообщение?

__Потому что ее еще не задали, не создали, не сохранили, новый коммутатор.__

![](https://github.com/yksie/Network-engineer/blob/main/lab01/04.png?raw=true)
![](https://github.com/yksie/Network-engineer/blob/main/lab01/05.png?raw=true)

Назначен ли IP-адрес сети VLAN 1?

__Нет__

Какой MAC-адрес имеет SVI? Возможны различные варианты ответов.
Назначен ли IP-адрес сети VLAN 1?

__Нет__

Какой MAC-адрес имеет SVI? Возможны различные варианты ответов.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/06.png?raw=true)

__Sh interfaces vlan 1
00e0.a345.c634__

![](https://github.com/yksie/Network-engineer/blob/main/lab01/07.png?raw=true)

Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/08.png?raw=true)
Какие выходные данные вы видите?

__Интерфейсы засветились зеленым цветом__

Под управлением какой версии ОС Cisco IOS работает коммутатор?

__Switch#sh ver
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)__

Как называется файл образа системы?

__System image file is "flash:c2960-lanbasek9-mz.150-2.SE4.bin"__

Какой базовый MAC-адрес назначен коммутатору?

__Base ethernet MAC Address : 00:17:59:A7:51:80__

Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.

__Switch# show interface f0/6__

Вопрос:
Интерфейс включен или выключен?

__Включен__

Что нужно сделать, чтобы включить интерфейс?

__Int f0/6 no shutdown__

Какой MAC-адрес у интерфейса?

__0001.c975.2006__

Какие настройки скорости и дуплекса заданы в интерфейсе?

__Full-duplex, 100Mb/s__

![](https://github.com/yksie/Network-engineer/blob/main/lab01/09.png?raw=true)

Какое имя присвоено сети VLAN 1 по умолчанию?

__default__

Какие порты расположены в сети VLAN 1?

__Fa0/1, Fa0/2, Fa0/3, Fa0/4
Fa0/5, Fa0/6, Fa0/7, Fa0/8
Fa0/9, Fa0/10, Fa0/11, Fa0/12
Fa0/13, Fa0/14, Fa0/15, Fa0/16
Fa0/17, Fa0/18, Fa0/19, Fa0/20
Fa0/21, Fa0/22, Fa0/23, Fa0/24
Gig0/1, Gig0/2__

Активна ли сеть VLAN 1?

__active__

К какому типу сетей VLAN принадлежит VLAN по умолчанию?

__Enet__

Выполните одну из следующих команд, чтобы изучить содержимое флеш-каталога.
Switch# show flash 
Switch# dir flash: 

![](https://github.com/yksie/Network-engineer/blob/main/lab01/10.png?raw=true)

В конце имени файла указано расширение, например .bin. Каталоги не имеют расширения файла.
Вопрос:
Какое имя присвоено образу Cisco IOS?

__2960-lanbasek9-mz.150-2.SE4.bin__


### Часть 2. Настройка базовых параметров сетевых устройств

В режиме глобальной конфигурации скопируйте следующие базовые параметры конфигурации и вставьте их в файл на коммутаторе S1. 

__no ip domain-lookup
hostname S1
service password-encryption
enable secret class
banner motd #
Unauthorized access is strictly prohibited. #__

Назначьте IP-адрес интерфейсу SVI на коммутаторе. Благодаря этому вы получите возможность удаленного управления коммутатором.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/11.png?raw=true)

Доступ через порт консоли также следует ограничить с помощью пароля. Используйте cisco в качестве пароля для входа в консоль в этом задании. Конфигурация по умолчанию разрешает все консольные подключения без пароля. Чтобы консольные сообщения не прерывали выполнение команд, используйте параметр logging synchronous.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/12.png?raw=true)

__S1(config)# line con 0
S1(config-line)# logging synchronous__

![](https://github.com/yksie/Network-engineer/blob/main/lab01/13.png?raw=true)

Настройте каналы виртуального соединения для удаленного управления (vty), чтобы коммутатор разрешил доступ через Telnet. Если не настроить пароль VTY, будет невозможно подключиться к коммутатору по протоколу Telnet.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/14.png?raw=true)

Для чего нужна команда login?

__Команда навешивает замок (команда пассворд создает только ключ)__

Настройте IP-адрес на компьютере PC-A.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/15.png?raw=true)
![](https://github.com/yksie/Network-engineer/blob/main/lab01/16.png?raw=true)


Используйте консольное подключение на компьютере PC-A для отображения и проверки конфигурации коммутатора. Команда show run позволяет постранично отобразить всю текущую конфигурацию. Для пролистывания используйте клавишу пробела.
Пример конфигурации приведен ниже. Параметры, которые вы настроили, выделены желтым. Другие параметры конфигурации — значения IOS по умолчанию.
Откройте окно конфигурации
S1# show run
Building configuration...

Current configuration : 2206 bytes
!
version 15.2
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption __да__
!

hostname S1 __да__
!

boot-start-marker
boot-end-marker
!

enable secret 5 $1$mtvC$6NC.1VKr3p6bj7YGE.jNg0 __$1$mERr$9cTjUIEqNGurQiFU.ZeCi1__
!

no aaa new-model
system mtu routing 1500 
!
!

no ip domain-lookup __да__
!

<output omitted>
!
  
interface FastEthernet0/24
 switchport access vlan 99 __нет такого__
!
  
interface GigabitEthernet0/1
 switchport access vlan 99 __нет такого__
!
  
interface GigabitEthernet0/2
 switchport access vlan 99 __нет такого__
!
  
interface Vlan1 да 
 ip address 192.168.1.2 255.255.255.0 __ip address 192.168.0.10 255.255.192.0__
!
  
ip default-gateway 192.168.1.1 __нет такого__
ip http server
ip http secure-server
!
  
banner motd ^C да
Unauthorized access is strictly prohibited. ^C __да__
!
  
line con 0
 password 7 00071A150754 __password 7 0822455D0A16__
 logging synchronous да
 login да
line vty 0 4
 password 7 121A0C041104 __password 7 0822455D0A16__
 login да
line vty 5 15
 password 7 121A0C041104 __password 7 0822455D0A16__
 login да
!
  
end


Проверьте параметры VLAN 1.

__S1# show interface vlan 1__

![](https://github.com/yksie/Network-engineer/blob/main/lab01/17.png?raw=true)

Какова полоса пропускания этого интерфейса?
  
__100000 Kb/s (100 Mb/s)__
  
В каком состоянии находится VLAN 1?
  
__Vlan1 is administratively down__
  
В каком состоянии находится канальный протокол?
  
__Line protocol Is down__

В командной строке компьютера PC-A с помощью утилиты ping проверьте связь сначала с адресом PC-A.
  
C:\> ping 192.168.1.10 
  
IP PC – 192.168.0.11
  
IP SW – 192.168.0.10

![](https://github.com/yksie/Network-engineer/blob/main/lab01/18.png?raw=true)

Из командной строки компьютера PC-A отправьте эхо-запрос на административный адрес интерфейса SVI коммутатора S1.
  
C:\> ping 192.168.1.2
  
Поскольку компьютеру PC-A нужно преобразовать МАС-адрес коммутатора S1 с помощью ARP, время ожидания передачи первого пакета может истечь. Если эхо-запрос не удается, найдите и устраните неполадки базовых настроек устройства. Проверьте как физические кабели, так и логическую адресацию.

![](https://github.com/yksie/Network-engineer/blob/main/lab01/19.png?raw=true)
![](https://github.com/yksie/Network-engineer/blob/main/lab01/20.png?raw=true)
  


### Часть 3. Проверка сетевых подключений

![](https://github.com/yksie/Network-engineer/blob/main/lab01/21.png?raw=true)


### 	Вопросы для повторения
  
1.	Зачем необходимо настраивать пароль VTY для коммутатора?
  
__В целях безопасности__
  
2.	Что нужно сделать, чтобы пароли не отправлялись в незашифрованном виде?
  
__Использовать ssh__
  
__Использовать enable secret/enable password-encryption__
  







