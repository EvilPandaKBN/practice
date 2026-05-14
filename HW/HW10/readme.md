# Лабораторная работа. Настройка протокола OSPFv2 для одной области
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW10/Screen/topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|
| ------------ | ------------ | ------------ |------------ |
| R1|G0/0/1|10.53.0.1|255.255.255.0|
| R1|Loopback1|172.16.1.1|255.255.255.0|
| R2|G0/0/1|10.53.0.2|255.255.255.0|
| R2|Loopback1|192.168.1.1|255.255.255.0|
## Цели
Часть 1. Создание сети и настройка основных параметров устройства
Часть 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области
Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области
## Общие сведения и сценарий
Вам было поручено настроить сеть небольшой компании с помощью OSPFv2. R1 будет размещать интернет-соединение (имитируемое интерфейсом Loopback 1) и делиться информацией о маршруте по умолчанию до  R2. После первоначальной настройки организация попросила оптимизировать конфигурацию, чтобы уменьшить трафик протокола и гарантировать, что R1 продолжает контролировать маршрутизацию.<br>
Примечание. Статическая маршрутизация, используемая в данной лаборатории, заключается в оценке возможности настройки и настройки OSPFv2 в конфигурации для одной области. Этот подход, используемый в данной лаборатории, может не отражать рекомендации по работе с сетевыми сетями. <br>
Примечание: Маршрутизаторы, используемые в практических лабораторных работах CCNA, - это Cisco 4221 с Cisco IOS XE Release 16.9.4 (образ universalk9). В лабораторных работах используются коммутаторы Cisco Catalyst 2960 с Cisco IOS версии 15.2(2) (образ lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. Правильные идентификаторы интерфейса см. в сводной таблице по интерфейсам маршрутизаторов в конце лабораторной работы.<br>
## Необходимые ресурсы
•	2 маршрутизатора (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
•	2 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.2(2) (образ lanbasek9) или аналогичная модель)<br>
•	1 ПК (под управлением Windows с программой эмуляции терминала, например, Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Инструкции
## Часть 1. Создание сети и настройка основных параметров устройства
### Шаг 1. Создайте сеть согласно топологии.
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.
### Шаг 2. Произведите базовую настройку маршрутизаторов.
a.	Назначьте маршрутизатору имя устройства.<br>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации<br>

    R1#show startup-config 
    Using 776 bytes
    !
    version 16.6.4
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname R1
    !
    !
    !
    enable password 7 0822404F1A0A
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
    !
    !
    !
    no ip domain-lookup
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
    no ip address
    duplex auto
    speed auto
    shutdown
    !
    interface GigabitEthernet0/0/2
    no ip address
    duplex auto
    speed auto
    shutdown
    !
    interface Vlan1
    no ip address
    shutdown
    !
    ip classless
    !
    ip flow-export version 9
    !
    !
    !
    banner motd ^CWarning!!!^C
    !
    !
    !
    !
    !
    line con 0
    password 7 0822455D0A16
    login
    !
    line aux 0
    !
    line vty 0 4
    password 7 0822455D0A16
    login
    !
    !
    !
    End
<br>
    
    R2(config)#do sh start
    Using 776 bytes
    !
    version 16.6.4
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname R2
    !
    !
    !
    enable password 7 0822404F1A0A
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
    !
    !
    !
    no ip domain-lookup
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
    no ip address
    duplex auto
    speed auto
    shutdown
    !
    interface GigabitEthernet0/0/2
    no ip address
    duplex auto
    speed auto
    shutdown
    !
    interface Vlan1
    no ip address
    shutdown
    !
    ip classless
    !
    ip flow-export version 9
    !
    !
    !
    banner motd ^CWarning!!!^C
    !
    !
    !
    !
    !
    line con 0
    password 7 0822455D0A16
    login
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
### Шаг 3. Настройте базовые параметры каждого коммутатора.
a.	Назначьте коммутатору имя устройства.<br>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>

    S1#show startup-config 
    Using 1208 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname S1
    !
    enable password 7 0822404F1A0A
    !
    !
    !
    no ip domain-lookup
    !
    !
    !
    spanning-tree mode pvst
    spanning-tree extend system-id
    !
    interface FastEthernet0/1
    !
    interface FastEthernet0/2
    !
    interface FastEthernet0/3
    !
    interface FastEthernet0/4
    !
    interface FastEthernet0/5
    !
    interface FastEthernet0/6
    !
    interface FastEthernet0/7
    !
    interface FastEthernet0/8
    !
    interface FastEthernet0/9
    !
    interface FastEthernet0/10
    !
    interface FastEthernet0/11
    !
    interface FastEthernet0/12
    !
    interface FastEthernet0/13
    !
    interface FastEthernet0/14
    !
    interface FastEthernet0/15
    !
    interface FastEthernet0/16
    !
    interface FastEthernet0/17
    !
    interface FastEthernet0/18
    !
    interface FastEthernet0/19
    !
    interface FastEthernet0/20
    !
    interface FastEthernet0/21
    !
    interface FastEthernet0/22
    !
    interface FastEthernet0/23
    !
    interface FastEthernet0/24
    !
    interface GigabitEthernet0/1
    !
    interface GigabitEthernet0/2
    !
    interface Vlan1
    no ip address
    shutdown
    !
    banner motd ^CWarning!!!^C
    !
    !
    !
    line con 0
    password 7 0822455D0A16
    login
    !
    line vty 0 4
    password 7 0822455D0A16
    login
    line vty 5 15
    login
    !
    !
    !
    !
    End
<br>
    
    S2(config)#do sh star
    Using 1208 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname S2
    !
    enable password 7 0822404F1A0A
    !
    !
    !
    no ip domain-lookup
    !
    !
    !
    spanning-tree mode pvst
    spanning-tree extend system-id
    !
    interface FastEthernet0/1
    !
    interface FastEthernet0/2
    !
    interface FastEthernet0/3
    !
    interface FastEthernet0/4
    !
    interface FastEthernet0/5
    !
    interface FastEthernet0/6
    !
    interface FastEthernet0/7
    !
    interface FastEthernet0/8
    !
    interface FastEthernet0/9
    !
    interface FastEthernet0/10
    !
    interface FastEthernet0/11
    !
    interface FastEthernet0/12
    !
    interface FastEthernet0/13
    !
    interface FastEthernet0/14
    !
    interface FastEthernet0/15
    !
    interface FastEthernet0/16
    !
    interface FastEthernet0/17
    !
    interface FastEthernet0/18
    !
    interface FastEthernet0/19
    !
    interface FastEthernet0/20
    !
    interface FastEthernet0/21
    !
    interface FastEthernet0/22
    !
    interface FastEthernet0/23
    !
    interface FastEthernet0/24
    !
    interface GigabitEthernet0/1
    !
    interface GigabitEthernet0/2
    !
    interface Vlan1
    no ip address
    shutdown
    !
    banner motd ^CWarning!!!^C
    !
    !
    !
    line con 0
    password 7 0822455D0A16
    login
    !
    line vty 0 4
    password 7 0822455D0A16
    login
    line vty 5 15
    login
    !
    !
    !
    !
    end
## Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области
### Шаг 1. Настройте адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.
a.	Настройте адреса интерфейсов на каждом маршрутизаторе, как показано в таблице адресации выше.
Откройте окно конфигурации<br>
b.	Перейдите в режим конфигурации маршрутизатора OSPF, используя идентификатор процесса 56.<br>
c.	Настройте статический идентификатор маршрутизатора для каждого маршрутизатора (1.1.1.1 для R1, 2.2.2.2 для R2).<br>
d.	Настройте инструкцию сети для сети между R1 и R2, поместив ее в область 0.<br>

    R1(config)#interface g 0/0/1
    R1(config-if)#ip os
    R1(config-if)#ip ospf 56 area 0
    
    R2(config)#interface gigabitEthernet 0/0/1
    R2(config-if)#ip os
    R2(config-if)#ip ospf 56 area 0
e.	Только на R2 добавьте конфигурацию, необходимую для объявления сети Loopback 1 в область OSPF 0.

    R2(config)#interface lo
    R2(config)#interface loopback 1
    R2(config-if)#ip osp
    R2(config-if)#ip ospf 56 are
    R2(config-if)#ip ospf 56 area 0
f.	Убедитесь, что OSPFv2 работает между маршрутизаторами. Выполните команду, чтобы убедиться, что R1 и R2 сформировали смежность.

    R2(config-if)#do show ip ospf neighbor
    Neighbor ID Pri State Dead Time Address Interface
    1.1.1.1 1 FULL/BDR 00:00:36 10.53.0.1 GigabitEthernet0/0/1
#### Вопрос <br>
Какой маршрутизатор является DR? Какой маршрутизатор является BDR? Каковы критерии отбора?
#### Ответ <br>
R2 является DR,R1 –BDR. У R2 более высокий Router ID (2.2.2.2)
g.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание, что поведение OSPF по умолчанию заключается в объявлении интерфейса обратной связи в качестве маршрута узла с использованием 32-битной маски
    
    R1(config-if)#do show ip route ospf
    192.168.1.0/32 is subnetted, 1 subnets
    O 192.168.1.1 [110/2] via 10.53.0.2, 00:04:00, GigabitEthernet0/0/1
h.	Запустите Ping до  адреса интерфейса R2 Loopback 1 из R1. Выполнение команды ping должно быть успешным.
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW10/Screen/R1-R2L.png)
## Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области
### Шаг 1. Реализация различных оптимизаций на каждом маршрутизаторе.
