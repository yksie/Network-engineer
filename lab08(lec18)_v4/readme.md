
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





![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)/Screenshot_1.jpg)
> Сеть согласно топологии в ПО СРТ


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

##### Присвойте имена устройствам в соответствии с топологией.
##### Назначьте class в качестве зашифрованного пароля доступа к привилегированному режиму.
##### Назначьте cisco в качестве паролей консоли и VTY и активируйте вход для консоли и VTY каналов.
##### Настройте logging synchronous для консольного канала.
##### Настройте баннерное сообщение дня (MOTD) для предупреждения пользователей о запрете несанкционированного доступа.
##### Задайте IP-адрес, указанный в таблице адресации для VLAN 1 на всех коммутаторах.
##### Скопируйте текущую конфигурацию в файл загрузочной конфигурации.

S1>

S1>**en**

S1#**conf t**

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#

S1(config)#**enable secret class**

S1(config)#**service pass**

S1(config)#**service password-encryption**

S1(config)#**line**

S1(config)#**line c**

S1(config)#**line console 0**

S1(config-line)#**password cisco**

S1(config-line)#**exit**

S1(config)#**line vty 0 15**

S1(config-line)#**password cisco**

S1(config-line)#**login**

S1(config-line)#**exit**

S1(config)#**line con**

S1(config)#**line console 0**

S1(config-line)#**password cisco**

S1(config-line)#**login**

S1(config-line)#**password cisco**

S1(config-line)#**logging synchronous**

S1(config-line)#**exit**

S1(config)#**ban**

S1(config)#**banner motd #**

Enter TEXT message. End with the character '#'.

__________GO AWAY!!!_____#

S1(config)#**int vlan 1**

S1(config-if)#**ip addr 192.168.1.1 255.255.255.0**

S1(config-if)#**no shut**

S1(config-if)#**end**

S1#

%SYS-5-CONFIG_I: Configured from console by console

S1#

S1#**copy run start**

Destination filename [startup-config]? 

Building configuration...

[OK]

S1#

__S2 и S3 настраиваются аналогично.__

#### Проверьте связь.
Проверьте способность компьютеров обмениваться эхо-запросами.

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?	__да__

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?	__да__

Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?	__да__


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







