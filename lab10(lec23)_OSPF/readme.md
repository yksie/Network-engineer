# Network Engineer. Basic. ДЗ №10 по лекции №23.

## Лабораторная работа. Настройка протокола OSPFv2 для одной области


| Устройство  | Интерфейс   | IP-адрес|Маска подсети|
| :------------ |:---------------:| :---------------:| :---------------:|
| R1      | G0/0/1    | 10.53.0.1   |255.255.255.0 |
| R1      | Loopback 1| 172.16.1.1  |255.255.255.0 |
| R2      | G0/0/1    | 10.53.0.2   |255.255.255.0 |
| R2      | Loopback 1| 192.168.1.1 |255.255.255.0 |

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_1.jpg)

## Задание

##### [Часть 1. Создание сети и настройка основных параметров устройства]()

##### [Часть 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области]()

##### [Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области]()


### Часть 1. Создание сети и настройка основных параметров устройства

#### Создайте сеть согласно топологии.  
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_2.jpg)


### Часть 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области

#### Настройте адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.  
##### Настройте адреса интерфейсов на каждом маршрутизаторе, как показано в таблице адресации выше.  
Откройте окно конфигурации  
Router(config)#**host R1**  
R1(config)#**int loopback 1**  
R1(config-if)#**ip addr 172.16.1.1 255.255.255.0**  
R1(config-if)#**ex**  
R1(config)#**int g 0/0/1**  
R1(config-if)#**ip addr 10.53.0.1 255.255.255.0**  
##### Перейдите в режим конфигурации маршрутизатора OSPF, используя идентификатор процесса 56.  

R1(config)#**router ospf 56**  

##### Настройте статический идентификатор маршрутизатора для каждого маршрутизатора (1.1.1.1 для R1, 2.2.2.2 для R2).

R1(config-router)#**router-id 1.1.1.1**  

#### Настройте инструкцию сети для сети между R1 и R2, поместив ее в область 0.

R1(config)#**int g0/0/1**  
R1(config-if)#**ip ospf 56 area 0**  
R1(config-if)#**no shut**  

##### Только на R2 добавьте конфигурацию, необходимую для объявления сети Loopback 1 в область OSPF 0.
R2(config)#**int loo 1**  
R2(config-if)#**ip ospf 56 area 0**  
R2(config-if)#**router ospf 56**  
R2(config-router)#**passive-interface loopback 1**  

##### Убедитесь, что OSPFv2 работает между маршрутизаторами. Выполните команду, чтобы убедиться, что R1 и R2 сформировали смежность.

___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_3.jpg)

___


Вопрос:  
Какой маршрутизатор является DR=1? Какой маршрутизатор является BDR=2? Каковы критерии отбора?  
___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_5.jpg)

___

__DR выбирается по приоритету. При равных приоритетах – выбирается по Router-ID.__  
##### На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание, что поведение OSPF по умолчанию заключается в объявлении интерфейса обратной связи в качестве маршрута узла с использованием 32-битной маски.

##### Запустите Ping до  адреса интерфейса R2 Loopback 1 из R1. Выполнение команды ping должно быть успешным.
___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_4.jpg)

___
### Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области

#### Реализация различных оптимизаций на каждом маршрутизаторе.
Откройте окно конфигурации  
##### На R1 настройте приоритет OSPF интерфейса G0/0/1 на 50, чтобы убедиться, что R1 является назначенным маршрутизатором.  
R1(config-if)#**ip ospf priority 50**
##### Настройте таймеры OSPF на G0/0/1 каждого маршрутизатора для таймера приветствия, составляющего 30 секунд.  

**interface g0/0/1**  
 **ip ospf hello-interval 30**  
 **ip ospf dead-interval 120**  


##### На R1 настройте статический маршрут по умолчанию, который использует интерфейс Loopback 1 в качестве интерфейса выхода. Затем распространите маршрут по умолчанию в OSPF. Обратите внимание на сообщение консоли после установки маршрута по умолчанию.  
R1(config)#**ip route 0.0.0.0 0.0.0.0 loo 1**  
R1(config)#**router ospf 56**  
R1(config-router)#**default-information originate**  

##### Добавьте конфигурацию, необходимую для OSPF для обработки R2 Loopback 1 как сети точка-точка. Это приводит к тому, что OSPF объявляет Loopback 1 использует маску подсети интерфейса.  
  
___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_7.jpg)

___

##### Только на R2 добавьте конфигурацию, необходимую для предотвращения отправки объявлений OSPF в сеть Loopback 1.  
R2(config)#**router ospf 56**  
R2(config-router)#**passive-interface loo 1**  

R2(config)#**int loo 1**  
R2(config-if)#**ip ospf network**   
R2(config-if)#**ip ospf network point-to-point**  

##### Измените базовую пропускную способность для маршрутизаторов. После этой настройки перезапустите OSPF с помощью команды clear ip ospf process . Обратите внимание на сообщение консоли после установки новой опорной полосы пропускания.  

___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_8.jpg)

___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_9.jpg)

___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_10.jpg)

___

![](https://github.com/yksie/Network-engineer/blob/main/lab10(lec23)_OSPF/Screenshot_11.jpg)

___

#### Убедитесь, что оптимизация OSPFv2 реализовалась.  
##### Выполните команду show ip ospf interface g0/0/1 на R1 и убедитесь, что приоритет интерфейса установлен равным 50, а временные интервалы — Hello 30, Dead 120, а тип сети по умолчанию — Broadcast  

R1#**sh ip ospf int g 0/0/1**  

GigabitEthernet0/0/1 is up, line protocol is up  
Internet address is 10.53.0.1/24, Area 0  
Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 100  
Transmit Delay is 1 sec, State DR, Priority 50  
Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1  
No backup designated router on this network  
Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5  
Hello due in 00:00:13  
Index 2/2, flood queue length 0  
Next 0x0(0)/0x0(0)  
Last flood scan length is 1, maximum is 1  
Last flood scan time is 0 msec, maximum is 0 msec  
Neighbor Count is 0, Adjacent neighbor count is 1  
Abjacent with neighbor 2.2.2.2  
Suppress hello for 0 neighbor(s)  
##### На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание на разницу в метрике между этим выходным и предыдущим выходным. Также обратите внимание, что маска теперь составляет 24 бита, в отличие от 32 битов, ранее объявленных.  

R1#**sh ip route ospf**  
O 		192.168.1.0 [110/101] via 10.53.0.2, 00:00:20, GigabitEthernet0/0/1  



##### Введите команду show ip route ospf на маршрутизаторе R2. Единственная информация о маршруте OSPF должна быть распространяемый по умолчанию маршрут R1.  

R2#**sh ip route ospf**  
172.16.0.0/32 is subnetted, 1 subnets  
O 		172.16.1.1 [110/101] via 10.53.0.1, 00:00:20, GigabitEthernet0/0/1  
O*E2 	0.0.0.0 [110/1] via 10.53.0.1, 00:00:20, GigabitEthernet0/0/1  


##### Запустите Ping до адреса интерфейса R1 Loopback 1 из R2. Выполнение команды ping должно быть успешным.  

R1#**ping 172.16.1.1**  

Type escape sequence to abort.  
Sending 5, 100-byte ICMP Echos to 172.16.1.1, timeout is 2 seconds:  
!!!!!  
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/5/9 ms  





