# Network Engineer. Basic. ДЗ №11 по лекции №26.

## Лабораторная работа. Настройка и проверка расширенных списков контроля доступа.

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_1.jpg)

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_2.jpg)

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_3.jpg)



## Задание


##### Часть 1. Создание сети и настройка основных параметров устройства

##### Часть 2. Настройка и проверка списков расширенного контроля доступа


___

### Часть 1. Создание сети и настройка основных параметров устройства

Произведено создание сети и настройка основных параметров устройства, настройка сетей VLAN на коммутаторах, настройка магистральных каналов

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_6.jpg)

Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_7.jpg)

#### Настройте маршрутизацию.

##### Настройка маршрутизации между сетями VLAN на R1.

Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейс для собственной VLAN не имеет назначенного IP-адреса. Включите описание для каждого подинтерфейса.  
Настройте интерфейс Loopback 1 на R1 с адресацией из приведенной выше таблицы.  
С помощью команды show ip interface brief проверьте конфигурацию подынтерфейса.  

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_8.jpg)

#### Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1  

R2(config)#**ip route 0.0.0.0 0.0.0.0 10.20.0.1**

#### Настройте удаленный доступ

##### Настройте все сетевые устройства для базовой поддержки SSH.  
Откройте окно конфигурации  
Создайте локального пользователя с именем пользователя SSHadmin и зашифрованным паролем $cisco123!  
Используйте ccna-lab.com в качестве доменного имени.  
Генерируйте криптоключи с помощью 1024 битного модуля.  
Настройте первые пять линий VTY на каждом устройстве, чтобы поддерживать только SSH-соединения и с локальной аутентификацией.  

S1(config)#**ip domain-name ccna-lab.com**  
S1(config)#**crypto key generate rsa**  
The name for the keys will be: S1.ccna-lab.com  
Choose the size of the key modulus in the range of 360 to 2048 for your  
General Purpose Keys. Choosing a key modulus greater than 512 may take  
a few minutes.  

How many bits in the modulus [512]: **1024**  
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]  

S1(config)#  
S1(config)#ip ssh version 2  
*Mar 2 19:26:52.473: %SSH-5-ENABLED: SSH 2 has been enabled  

S1(config)#**username SSHadmin secret $cisco123!**   
S1(config)#**line vty 0 4**   
S1(config-line)#**login local**   
S1(config-line)#**transport input ssh**   

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_9.jpg)  
![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_10.jpg)  
![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_11.jpg)  


##### Включите защищенные веб-службы с проверкой подлинности на R1 (не реализовано в СРТ)  
Включите сервер HTTPS на R1.  
R1(config)# **ip http secure-server**   
Настройте R1 для проверки подлинности пользователей, пытающихся подключиться к веб-серверу.  
R1(config)# **ip http authentication local**  

#### Проверка подключения  
Выполните следующие тесты. Эхозапрос должен пройти успешно  
Примечание. Возможно, вам придется отключить брандмауэр ПК для работы ping  


| От      | Протокол    |    Назначение   |  Результат|
| :------ |:-----------:| :--------------:|   -------------:| 
| R1      | G0/0/1    | 10.53.0.1   |   Успех| 
| R1      | Loopback 1| 172.16.1.1  |   Успех| 
| PC-A| 	Ping	|       10.40.0.10  |   Успех| 
| PC-A| 	Ping	|       10.20.0.1   |   Успех| 
| PC-B| 	Ping	|       10.30.0.10  |   Успех| 
| PC-B| 	Ping	| 10.20.0.1|  Успех| 
| PC-B| 	Ping	| 172.16.1.1|  Успех| 
| PC-B| 	SSH 	| 10.20.0.1 |  Успех| 
| PC-B| 	SSH	  | 172.16.1.1 |  Успех| 

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_12.jpg)  

### Часть 2. Настройка и проверка списков контроля доступа (ACL)   
При проверке базового подключения компания требует реализации следующих политик безопасности:  

Политика 1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен). 

Политика 2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).

Политика 3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. 

R1(config)#**ip access-list extended Policy1-3**  
R1(config-ext-nacl)#**deny __tcp__ 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255 __eq 22__**  
R1(config-ext-nacl)#**deny __tcp__ 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 __eq 80__**  
R1(config-ext-nacl)#**deny __tcp__ 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 __eq 443__**  
R1(config-ext-nacl)#**deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 echo**  
R1(config-ext-nacl)#**deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255 echo**  
__R1(config-ext-nacl)#**deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq 80**__  
__R1(config-ext-nacl)#**deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq 443**__  
__R1(config-ext-nacl)#**deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq 80**__   
__R1(config-ext-nacl)#**deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq 443**__   
R1(config-ext-nacl)#**200 permit ip any any**  
R1(config-ext-nacl)#**int g0/0/1.40**  
R1(config-subif)#**ip access-group Policy1-3 in**  


![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_13.jpg)

![](https://github.com/yksie/Network-engineer/blob/main/lab11(lec26)_ACL/Screenshot_14.jpg)


Политика 4: Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам.  

R1(config)#**ip access-list extended Policy4**  
R1(config-ext-nacl)#**deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255 echo**  
R1(config-ext-nacl)#**200 permit ip any any**  
R1(config-ext-nacl)#**int g0/0/1.30**  
R1(config-subif)#**ip access-group Policy4 in**  

  


#### Убедитесь, что политики безопасности применяются развернутыми списками доступа.

Выполнены следующие тесты. Результаты показаны в таблице:


| От      | Протокол    |    Назначение   |Результат|
| :------------ |:---------------:| :---------------:| :---------------:|
| PC-A	| Ping	| 10.40.0.10	| Сбой | 
| PC-A	| Ping	| 10.20.0.1 	| Успех| 
| PC-B	| Ping	| 10.30.0.10	| Сбой | 
| PC-B	| Ping	| 10.20.0.1	  | Сбой | 
| PC-B	| Ping	| 172.16.1.1	| Успех| 
| PC-B	| SSH  	| 10.20.0.4	  | Сбой | 
| PC-B	| SSH 	| 172.16.1.1	| Успех| 

Фактические результаты совпали с ожидаемыми.
