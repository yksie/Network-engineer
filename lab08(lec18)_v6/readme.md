
# Network Engineer. Basic. ДЗ №8 по лекции №18.

## Лабораторная работа. Реализация DHCPv6

![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_10.jpg)
> Сеть согласно топологии


| Устройство  | Интерфейс   | IPv6-адрес|
| :------------ |:---------------:| :---------------:|
| R1      | G0/0/0      | 2001:db8:acad:2::1/64  |
| R1      | G0/0/0      | fe80::1                |
| R1      | G0/0/1      | 2001:db8:acad:1::1/64  |
| R1      | G0/0/1      | fe80::1                |
| R2      | G0/0/0      | 2001:db8:acad:2::2/64  |
| R2      | G0/0/0      | fe80::1                |
| R2      | G0/0/1      | 2001:db8:acad:3::1/64  |
| R2      | G0/0/1      | fe80::1                |
| PC-A    | NIC         | DHCP                   |
| PC-B    | NIC         | DHCP                   |

> Таблица адресации



## Задание

##### [Часть 1. Создание сети и настройка основных параметров устройства]()

##### [Часть 2. Проверка назначения адреса SLAAC от R1]()

##### [Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1]()

##### [Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1]()

##### [Часть 5. Настройка и проверка DHCPv6 Relay на R2]()



### Часть 1. Создание сети и настройка основных параметров устройства


#### Произвести базовую настройку коммутаторов (отключить DNS, назначить class в качестве зашифрованного пароля привилегированного режима EXEC, назначить cisco в качестве пароля консоли и включить вход в систему по паролю, назначить cisco в качестве пароля VTY и включить вход в систему по паролю, зашифровать открытые пароли, создайте баннер с предупреждением о запрете несанкционированного доступа к устройству, сохранить текущую конфигурацию в файл загрузочной конфигурации, установить часы на маршрутизаторе на сегодняшнее время и дату, отключить все неиспользуемые порты)
___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_11.jpg)
___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_12.jpg)
___

#### Произвести базовую настройку маршрутизаторов (отключить DNS, назначить class в качестве зашифрованного пароля привилегированного режима EXEC, назначить cisco в качестве пароля консоли и включить вход в систему по паролю, назначить cisco в качестве пароля VTY и включить вход в систему по паролю, зашифровать открытые пароли, создайте баннер с предупреждением о запрете несанкционированного доступа к устройству, сохранить текущую конфигурацию в файл загрузочной конфигурации, установить часы на маршрутизаторе на сегодняшнее время и дату)

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

R1(config)#**ipv6 unicast-routing**  
R2(config)#**ipv6 unicast-routing**



#### Настройка интерфейсов и маршрутизации для обоих маршрутизаторов.

Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше.
___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_13.jpg)
___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_14.jpg)
___
#### Настройте маршрут по умолчанию на каждом маршрутизаторе, который указывает на IP-адрес G0/0/0 на другом маршрутизаторе.

R1(config)#ipv6 route 2001:db8:acad:3::1/64 2001:db8:acad:2::2  
R2(config)#ipv6 route 2001:db8:acad:1::1/64 2001:db8:acad:2::1  

Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1

Сохраните текущую конфигурацию в файл загрузочной конфигурации.

___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_15.jpg)
___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_16.jpg)
___


### Часть 2. Проверка назначения адреса SLAAC от R1

В части 2 осуществляется проверка, что узел PC-A получает адрес IPv6 с помощью метода SLAAC.  
Включите PC-A и убедитесь, что сетевой адаптер настроен для автоматической настройки IPv6.  
Через несколько минут результаты команды ipconfig должны показать, что PC-A присвоил себе адрес из сети 2001:db8:1::/64.  


___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_17.jpg)
___

IPV6 addr 2001:DB8:ACAD:1:20C:CFFF:FE48:D599  

Вопрос:

Откуда взялась часть адреса с идентификатором хоста?  
48:D5:99 - три последние октета МАС-адреса PC-A  


### Часть 3. Настройка и проверка сервера DHCPv6 на R1

В части 3 выполняется настройка и проверка состояния DHCP-сервера на R1. Цель состоит в том, чтобы предоставить PC-A информацию о DNS-сервере и домене.

#### Настройте R1 для предоставления DHCPv6 без состояния для PC-A.

Создайте пул DHCP IPv6 на R1 с именем R1-STATELESS. В составе этого пула назначьте адрес DNS-сервера как 2001:db8:acad: :1, а имя домена — как stateless.com.

R1(config)# ipv6 dhcp pool R1-STATELESS  
R1(config-dhcp)# dns-server 2001:db8:acad::254  
R1(config-dhcp)# domain-name STATELESS.com  

Настройте интерфейс G0/0/1 на R1, чтобы предоставить флаг конфигурации OTHER для локальной сети R1 и укажите только что созданный пул DHCP в качестве ресурса DHCP для этого интерфейса.

R1(config)# interface g0/0/1  
R1(config-if)# ipv6 nd other-config-flag   
R1(config-if)# ipv6 dhcp server R1-STATELESS  

![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_18_0.jpg)

Сохраните текущую конфигурацию в файл загрузочной конфигурации.

___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_18.jpg)
___

Перезапустите PC-A.

Проверьте вывод ipconfig /all и обратите внимание на изменения.

C:\Users\Student> ipconfig /all

___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_19.jpg)
___

### Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R2

__(сервер будем делать на R2 из-за ограниченного функционала СРТ)__

В части 4 настраивается R1 для ответа на запросы DHCPv6 от локальной сети на R2.

#### Создайте пул DHCPv6 на R1 для сети 2001:db8:acad:3:aaa::/80. Это предоставит адреса локальной сети, подключенной к интерфейсу G0/0/1 на R2. В составе пула задайте DNS-сервер 2001:db8:acad: :254 и задайте доменное имя STATEFUL.com

R1(config)# ipv6 dhcp pool R2-STATEFUL  
R1(config-dhcp)# address prefix 2001:db8:acad:3:aaa::/80  
R1(config-dhcp)# dns-server 2001:db8:acad::254  
R1(config-dhcp)# domain-name STATEFUL.com  
Назначьте только что созданный пул DHCPv6 интерфейсу g0/0/0 на R1.  
R1(config)# interface g0/0/0  
R1(config-if)# ipv6 dhcp server R2-STATEFUL  

___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_20.jpg)
___

R2 (конфигурация) # интерфейс g0/0/1  
R2(config-if)# ipv6 nd managed-config-flag  

___
![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_20_1.jpg)
___

![](https://github.com/yksie/Network-engineer/blob/main/lab08(lec18)_v6/Screenshot_21.jpg)
___
