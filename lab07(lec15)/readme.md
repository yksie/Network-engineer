
# Network Engineer. Basic. ДЗ №7 по лекции №15.

## Лабораторная работа. Развертывание коммутируемой сети с резервными каналами.


| Устройство  | Интерфейс   | IP-адрес|Маска подсети|
| :------------ |:---------------:| :---------------:| :---------------:|
| S1      | VLAN 1 | 192.168.1.1 |255.255.255.0 |
| S2      | VLAN 1 | 192.168.1.2 |255.255.255.0 |
| S3      | VLAN 1 | 192.168.1.3 |255.255.255.0 |

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_0.jpg)

## Задание

##### [Часть 1. Создание сети и настройка основных параметров устройства]()

##### [Часть 2. Выбор корневого моста]

##### [Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов]

##### [Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов]



### Часть 1. Создание сети и настройка основных параметров устройства

#### Создайте сеть согласно топологии.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_1.jpg)

#### Настройте базовые параметры каждого коммутатора.

##### Отключите поиск DNS.

S1>en

S1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#no ip dom

S1(config)#no ip domain-lookup

##### Присвойте имена устройствам в соответствии с топологией.
##### Назначьте class в качестве зашифрованного пароля доступа к привилегированному режиму.
##### Назначьте cisco в качестве паролей консоли и VTY и активируйте вход для консоли и VTY каналов.
##### Настройте logging synchronous для консольного канала.
##### Настройте баннерное сообщение дня (MOTD) для предупреждения пользователей о запрете несанкционированного доступа.
##### Задайте IP-адрес, указанный в таблице адресации для VLAN 1 на всех коммутаторах.
##### Скопируйте текущую конфигурацию в файл загрузочной конфигурации.

S1>

S1>en

S1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#

S1(config)#enable secret class

S1(config)#service pass

S1(config)#service password-encryption 

S1(config)#line 

S1(config)#line c

S1(config)#line console 0

S1(config-line)#password cisco

S1(config-line)#exit

S1(config)#line vty 0 15

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#exit

S1(config)#line con

S1(config)#line console 0

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#password cisco

S1(config-line)#logging synchronous 

S1(config-line)#exit

S1(config)#ban

S1(config)#banner motd #

Enter TEXT message. End with the character '#'.

__________GO AWAY!!!_____#

S1(config)#int vlan 1

S1(config-if)#ip addr 192.168.1.1 255.255.255.0

S1(config-if)#no shut

S1(config-if)#end

S1#

%SYS-5-CONFIG_I: Configured from console by console

S1#

S1#copy run start

Destination filename [startup-config]? 

Building configuration...

[OK]

S1#

__S2 и S3 настраиваем аналогично.__

#### Проверьте связь.
Проверьте способность компьютеров обмениваться эхо-запросами.

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?	__да__

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?	__да__

Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?	__да__


### Часть 2. Выбор корневого моста

#### Отключите все порты на коммутаторах.

S1(config)#int range f 0/1-4

S1(config-if-range)#shut

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
S1(config)#int range f0/1-4

S1(config-if-range)#sw mode tr

#### Включите порты F0/2 и F0/4 на всех коммутаторах.
S1(config)#int f0/2

S1(config-if)#no shut

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to down

S1(config-if)#ex

S1(config)#int f0/4

S1(config-if)#no shut

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to down

S1(config-if)#

#### Отобразите данные протокола spanning-tree.

S1# show spanning-tree

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_3.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_4.jpg)
![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_5.jpg)
 
![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_6.jpg)

#### Oтветьте на следующие вопросы.

Какой коммутатор является корневым мостом? __S1__

Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?

__Поскольку приоритет идентификатора моста одинаков (Priority), а также одинаков VLAN, коммутатор в качестве корневого моста для STP выбирается по наименьшему MAC-адресу__

Какие порты на коммутаторе являются корневыми портами? 

__S2 F0/2, S3 F0/4__

Какие порты на коммутаторе являются назначенными портами?

__S1 F0/2, S0/4; S2 F0/4__

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

схема после перестроения дерева


Шаг 3:	Просмотрите изменения протокола spanning-tree.
Повторно выполните команду show spanning-tree на обоих коммутаторах некорневого моста. Обратите внимание, что ранее заблокированный порт (S1 – F0/4) теперь является назначенным портом, и протокол spanning-tree теперь блокирует порт на другом коммутаторе некорневого моста (S3 – F0/4).
должен стать Alternative порт S2-F0/4
потому что path cost до S3 меньше, чем path cost до S2

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_8.jpg)

S1# show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0cd9.96d2.4000
             Cost        18
             Port        2 (FastEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0cd9.96e8.8a00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Fa0/2               Root FWD 18        128.2    P2p 
Fa0/4               Desg FWD 19        128.4    P2p

S3# show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0cd9.96d2.4000
             Cost        19
             Port        2 (FastEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0cd9.96e8.7400
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Fa0/2               Root FWD 19        128.2    P2p 
Fa0/4               Altn BLK 19        128.4    P2p
Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе?
____ потому что path cost до S3 меньше, чем path cost до S2_________________________
_______________________________________________________________________________________
Шаг 4:	Удалите изменения стоимости порта.
a.	Выполните команду no spanning-tree cost 18 режима конфигурации интерфейса, чтобы удалить запись стоимости, созданную ранее.
S1(config)# interface f0/2
S1(config-if)# no spanning-tree cost 18
S3(config)# interface f0/4
S3(config-if)# no spanning-tree vlan 1 cost 18

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_9.jpg) 

схема после перестроения

b.	Повторно выполните команду show spanning-tree, чтобы подтвердить, что протокол STP сбросил порт на коммутаторе некорневого моста, вернув исходные настройки порта. Протоколу STP требуется примерно 30 секунд, чтобы завершить процесс перевода порта.

![](https://github.com/yksie/Network-engineer/blob/main/lab07(lec15)/Screenshot_10.jpg)





### Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов







