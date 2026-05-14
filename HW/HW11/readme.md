# Лабораторная работа. Настройка и проверка расширенных списков контроля доступа.
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|Шлюз по умолчанию|
| ------------ | ------------ | ------------ |------------ |------------ |
| R1|G0/0/1|-|-|-|
| R1|G0/0/1.10|10.20.0.1|255.255.255.0|-|
| R1|G0/0/1.20|10.30.0.1|255.255.255.0|-|
| R1|G0/0/1.30|10.40.0.1|255.255.255.0|-|
| R1|G0/0/1.1000|-|-|-|
| R1|Loopback1|172.16.1.1|255.255.255.0|-|
| R2|G0/0/1|10.20.0.4|255.255.255.0|-|
| S1|Vlan 20|10.20.0.2|255.255.255.0|10.20.0.1|
| S2|Vlan 20|10.20.0.3|255.255.255.0|10.20.0.1|
| PC-A|NIC|10.30.0.10|255.255.255.0|10.30.0.1|
| PC-B|NIC|10.40.0.10|255.255.255.0|10.40.0.1|
## Таблица VLAN
|VLAN|Имя|Назначенный интерфейс|
| ------------ | ------------ | ------------ |
| 20| Management | S2: F0/5|
| 30| Operations | S1: F0/6 |
| 40| Sales | S2: F0/18 |
| 999| Parking_Lot | S1: F0/2-4, F0/7-24, G0/1-2 |
| 999| Parking_Lot | S2: F0/2-17, F0/19-24, G0/1-2 |
| 1000| Собственная | - |
## Задачи
### Часть 1. Создание сети и настройка основных параметров устройства
### Часть 2. Настройка и проверка списков расширенного контроля доступа
## Общие сведения и сценарий
Вам было поручено настроить списки контроля доступа в сети небольшой компании. ACL являются одним из самых простых и прямых средств управления трафиком уровня 3. R1 будет размещать интернет-соединение (смоделированное интерфейсом Loopback 1) и предоставлять информацию о маршруте по умолчанию для R2. После завершения первоначальной настройки компания имеет некоторые конкретные требования к безопасности дорожного движения, которые вы несете ответственность за реализацию.<br>
Примечание: Маршрутизаторы, используемые в практических лабораторных работах CCNA, - это Cisco 4221 с Cisco IOS XE Release 16.9.4 (образ universalk9). В лабораторных работах используются коммутаторы Cisco Catalyst 2960 с Cisco IOS версии 15.2(2) (образ lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. Правильные идентификаторы интерфейса см. в сводной таблице по интерфейсам маршрутизаторов в конце лабораторной работы.<br>
## Необходимые ресурсы
•	2 маршрутизатора (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
•	2 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.2(2) (образ lanbasek9) или аналогичная модель)<br>
•	2 ПК (ОС Windows с программой эмуляции терминалов, такой как Tera Term)<br>
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
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
    
    R1#sh startup-config 
    Using 776 bytes
    !
    version 16.6.4
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname S1
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
    
    R2# sh start
    Using 756 bytes
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
a.	Присвойте коммутатору имя устройства.<br>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
    
    S1#sh start
    Using 1164 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    no service password-encryption
    !
    hostname S1
    !
    enable password class
    !
    !
    !
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
    password cisco
    login
    !
    line vty 0 4
    password cisco
    login
    line vty 5 15
    login
    !
    !
    !
    !
    End

<br>

    S2#sh start
    Using 1188 bytes
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

## Часть 2. Настройка сетей VLAN на коммутаторах.
### Шаг 1. Создайте сети VLAN на коммутаторах.
a.	Создайте необходимые VLAN и назовите их на каждом коммутаторе из приведенной выше таблицы.
    
    S1(config-vlan)#do sh vlan bri
    
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/2, Fa0/3, Fa0/4
    Fa0/5, Fa0/6, Fa0/7, Fa0/8
    Fa0/9, Fa0/10, Fa0/11, Fa0/12
    Fa0/13, Fa0/14, Fa0/15, Fa0/16
    Fa0/17, Fa0/18, Fa0/19, Fa0/20
    Fa0/21, Fa0/22, Fa0/23, Fa0/24
    Gig0/1, Gig0/2
    20 Management active 
    30 Operations active 
    40 Sales active 
    999 ParkingLot active 
    1000 Native active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active

<br>
    
    S2(config)#do sh vlan bri
    
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/2, Fa0/3, Fa0/4
    Fa0/5, Fa0/6, Fa0/7, Fa0/8
    Fa0/9, Fa0/10, Fa0/11, Fa0/12
    Fa0/13, Fa0/14, Fa0/15, Fa0/16
    Fa0/17, Fa0/18, Fa0/19, Fa0/20
    Fa0/21, Fa0/22, Fa0/23, Fa0/24
    Gig0/1, Gig0/2
    20 Management active 
    30 Operations active 
    40 Sales active 
    999 ParkingLot active 
    1000 Native active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active
b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP
адресе в таблице адресации. 

    S1(config)#interface vlan 20
    S1(config-if)#
    %LINK-5-CHANGED: Interface Vlan20, changed state to up
    S1(config-if)#ip ad
    S1(config-if)#ip address 10.20.0.2 255.255.255.0
    S1(config-if)#exit
    S1(config)#ip def
    S1(config)#ip default-gateway 10.20.0.1

<br>
    
    S2(config)#int vlan 20
    S2(config-if)#
    %LINK-5-CHANGED: Interface Vlan20, changed state to up
    S2(config-if)#ip ad
    S2(config-if)#ip address 10.20.0.3 255.255.255.0
    S2(config-if)#exit
    S2(config)#ip defa
    S2(config)#ip default-gateway 10.20.0.1

### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.
a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для
режима статического доступа.<br>
b.	Выполните команду show vlan brief, чтобы убедиться, что сети VLAN назначены правильным интерфейсам.

    S1(config)#do sh vlan bri
    
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/5
    20 Management active 
    30 Operations active Fa0/6
    40 Sales active 
    999 ParkingLot active Fa0/2, Fa0/3, Fa0/4, Fa0/7
    Fa0/8, Fa0/9, Fa0/10, Fa0/11
    Fa0/12, Fa0/13, Fa0/14, Fa0/15
    Fa0/16, Fa0/17, Fa0/18, Fa0/19
    Fa0/20, Fa0/21, Fa0/22, Fa0/23
    Fa0/24, Gig0/1, Gig0/2
    1000 Native active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active

<br>
    
    S2(config)#do sh vlan br
    
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/5
    20 Management active 
    30 Operations active 
    40 Sales active Fa0/18
    999 ParkingLot active Fa0/2, Fa0/3, Fa0/4, Fa0/6
    Fa0/7, Fa0/8, Fa0/9, Fa0/10
    Fa0/11, Fa0/12, Fa0/13, Fa0/14
    Fa0/15, Fa0/16, Fa0/17, Fa0/19
    Fa0/20, Fa0/21, Fa0/22, Fa0/23
    Fa0/24, Gig0/1, Gig0/2
    1000 Native active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active
## Часть 3. ·Настройте транки (магистральные каналы).
### Шаг 1. Вручную настройте магистральный интерфейс F0/1.
a.	Измените режим порта коммутатора на интерфейсе F0/1, чтобы принудительно создать магистральную связь.
Не забудьте сделать это на обоих коммутаторах.<br>
b.	В рамках конфигурации транка установите для native vlan значение 1000 на обоих коммутаторах. При
настройке двух интерфейсов для разных собственных VLAN сообщения об ошибках могут отображаться временно
<br>
c.	В качестве другой части конфигурации транка укажите, что VLAN 20, 30, 40 и 1000 разрешены в транке
<br>
d.	Выполните команду show interfaces trunk для проверки портов магистрали, собственной VLAN и
разрешенных VLAN через магистраль.<br>

    S1(config)#do sh int trun
    Port Mode Encapsulation Status Native vlan
    Fa0/1 on 802.1q trunking 1000
    
    Port Vlans allowed on trunk
    Fa0/1 20,30,40,1000
    
    Port Vlans allowed and active in management domain
    Fa0/1 20,30,40,1000
    
    Port Vlans in spanning tree forwarding state and not pruned
    Fa0/1 20,30,40,1000
<br>
    
    S2(config)#do sh int trun
    Port Mode Encapsulation Status Native vlan
    Fa0/1 on 802.1q trunking 1000
    
    Port Vlans allowed on trunk
    Fa0/1 20,30,40,1000
    
    Port Vlans allowed and active in management domain
    Fa0/1 20,30,40,1000
    
    Port Vlans in spanning tree forwarding state and not pruned
    Fa0/1 none
### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.
a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.<br>
b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
c.	Используйте команду show interfaces trunk для проверки настроек транка.<br>
    
    S1(config)#do sh int trun
    Port Mode Encapsulation Status Native vlan
    Fa0/1 on 802.1q trunking 1000
    Fa0/5 on 802.1q trunking 1000
    
    Port Vlans allowed on trunk
    Fa0/1 20,30,40,1000
    Fa0/5 20,30,40,1000
    
    Port Vlans allowed and active in management domain
    Fa0/1 20,30,40,1000
    Fa0/5 20,30,40,1000
    
    Port Vlans in spanning tree forwarding state and not pruned
    Fa0/1 20,30,40,1000
    Fa0/5 none
### Часть 4. Настройте маршрутизацию.
a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.<br>
b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы
используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейс для собственной VLAN не имеет назначенного IP
адреса. Включите описание для каждого подинтерфейса.<br>
c.	Настройте интерфейс Loopback 1 на R1 с адресацией из приведенной выше таблицы.<br>
d.	С помощью команды show ip interface brief проверьте конфигурацию подынтерфейса.<br>

    R1(config)#do sh ip int bri
    Interface IP-Address OK? Method Status Protocol 
    GigabitEthernet0/0/0 unassigned YES unset administratively down down 
    GigabitEthernet0/0/1 unassigned YES unset up up 
    GigabitEthernet0/0/1.2010.20.0.1 YES manual up up 
    GigabitEthernet0/0/1.3010.30.0.1 YES manual up up 
    GigabitEthernet0/0/1.4010.40.0.1 YES manual up up 
    GigabitEthernet0/0/1.1000unassigned YES unset up up 
    GigabitEthernet0/0/2 unassigned YES unset administratively down down 
    Loopback1 172.16.1.1 YES manual up up 
    Vlan1 unassigned YES unset administratively down down
### Шаг 2. Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с
адресом следующего перехода 10.20.0.1
    
    R2(config)#interface g0/0/1
    R2(config-if)#ip ad
    R2(config-if)#ip address 10.20.0.4 255.255.255.0
    R2(config-if)#no sh
    
    R2(config-if)#
    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up
    
    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
    
    R2(config-if)#exit
    R2(config)#ip route 0.0.0.0 0.0.0.0 10.20.0.1
### Часть 5. Настройте удаленный доступ
a.	Создайте локального пользователя с именем пользователя SSHadmin и зашифрованным паролем $cisco123!<br>
b.	Используйте ccna-lab.com в качестве доменного имени.<br>
c.	Генерируйте криптоключи с помощью 1024 битного модуля.<br>
d.	Настройте первые пять линий VTY на каждом устройстве, чтобы поддерживать только SSH-соединения и с локальной аутентификацией.<br>

    R1(config)#username SSHadmin secret $cisco123!
    R1(config)#ip domain-name ccna-lab.com 
    R1(config)#cry
    R1(config)#crypto k
    R1(config)#crypto key ge
    R1(config)#crypto key generate r
    R1(config)#crypto key generate rsa 
    The name for the keys will be: R1.ccna-lab.com
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.
    
    How many bits in the modulus [512]: 1024
    % Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
    
    R1(config)#ip ssh
    *Mar 1 1:14:20.78: %SSH-5-ENABLED: SSH 1.99 has been enabled
    R1(config)#ip ssh ver
    R1(config)#ip ssh version 2
    R1(config)#line vty 0 4
    R1(config-line)#tra
    R1(config-line)#transport in
    R1(config-line)#transport input
    R1(config-line)#transport input ssh
    R1(config-line)#login lo
    R1(config-line)#login local 
    R1(config-line)#exit
<br>
    
    S1(config)#username SSHadmin secret $cisco123!
    S1(config)#ip domain-name ccna-lab.com
    S1(config)#crypto key generate rsa
    The name for the keys will be: S1.ccna-lab.com
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.
    
    How many bits in the modulus [512]: 1024
    % Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
    
    S1(config)#ip ssh v
    *Mar 1 1:16:32.621: %SSH-5-ENABLED: SSH 1.99 has been enabled
    S1(config)#ip ssh version 2
    S1(config)#line vty 0 4
    S1(config-line)#transport input ssh
    S1(config-line)#login local	
<br>

    S2(config)#username SSHadmin secret $cisco123!
    S2(config)#ip domain-name ccna-lab.com
    S2(config)#crypto key generate rsa
    The name for the keys will be: S2.ccna-lab.com
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.
    
    How many bits in the modulus [512]: 1024
    % Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

    S2(config)#
    *Mar 1 1:18:17.259: %SSH-5-ENABLED: SSH 1.99 has been enabled
    S2(config)#ip ss
    S2(config)#ip ssh ve
    S2(config)#ip ssh version 2
    S2(config)#line vty 0 4
    S2(config-line)#transport input ssh
    S2(config-line)#login local
    S2(config-line)#exit
<br>

    R2(config)#username SSHadmin secret $cisco123!
    R2(config)#ip domain-name ccna-lab.com
    R2(config)#crypto key generate rsa
    The name for the keys will be: R2.ccna-lab.com
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.
    
    How many bits in the modulus [512]: 1024
    % Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
    
    R2(config)#
    *Mar 1 1:21:0.774: %SSH-5-ENABLED: SSH 1.99 has been enabled
    R2(config)#ip ssh version 2
    R2(config)#line vty 0 4
    R2(config-line)#transport input ssh
    R2(config-line)#login local
    R2(config-line)#exit
    
### Шаг 2. Включите защищенные веб-службы с проверкой подлинности на R1.
a.	Включите сервер HTTPS на R1.<br>
R1(config)# ip http secure-server <br>
b.	Настройте R1 для проверки подлинности пользователей, пытающихся подключиться к веб-серверу.<br>
R1(config)# ip http authentication local<br>
не удалось проделать этот шаг, из-за отсутствия необходимого функционала в cpt
## Часть 6. Проверка подключения
### Шаг 1. Настройте узлы ПК.
Адреса ПК можно посмотреть в таблице адресации.
### Шаг 2. Выполните следующие тесты. Эхозапрос должен пройти успешно.
PC-A	Ping	10.40.0.10
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-A-10.40.0.10.png)
PC-A	Ping	10.20.0.1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-A-10.20.0.1.png)
PC-B	Ping	10.30.0.10
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-B-10.30.0.10.png)
PC-B	Ping	10.20.0.1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-B-10.20.0.1.png)
PC-B	Ping	172.16.1.1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-B-172.16.1.1.png)
PC-B	HTTPS	10.20.0.6<br>
Из-за отсутствия возможности включения HTTPS сервера на R1 в схему был добавлен сервер с адресом 10.20.0.6
## новая топология
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/NewTopology.png)
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/HTTPS-PC-B-10.20.0.6.png)
PC-B	SSH	10.20.0.1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/SSH-PC-B-172.16.1.1.png)
PC-B	SSH	172.16.1.1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/SSH-PC-B-172.16.1.1.png)
## Часть 7. Настройка и проверка списков контроля доступа (ACL)
При проверке базового подключения компания требует реализации следующих политик безопасности:
Политика1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен).<br> 
Политика 2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).<br> 
Политика3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. <br> 
Политика 4: Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам. <br> 
### Шаг 1. Проанализируйте требования к сети и политике безопасности для планирования реализации ACL.
### Шаг 2. Разработка и применение расширенных списков доступа, которые будут соответствовать требованиям политики безопасности.
Поскольку 1, 2 и 3 политика касаются сети Sales, мы можем устроить это одним access-list, применив его на интерфейс роутера R1 GigabitEthernet0/0/1.40

    R1#sh ip access-lists 
    Extended IP access list SALES_POLICY
    10 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22
    20 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq www
    30 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443
    40 deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq www
    50 deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq 443
    60 deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq www
    70 deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq 443
    80 deny tcp 10.40.0.0 0.0.0.255 host 10.20.0.1 eq www
    90 deny tcp 10.40.0.0 0.0.0.255 host 10.20.0.1 eq 443
    100 deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 echo
    110 deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255 echo
    120 permit ip any any
А политику 4 применяется к входному интерфейсу R1  GigabitEthernet0/0/1.30

    Extended IP access list OPERATIONS_POLICY
    10 deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255 echo
    20 permit ip any any
    Откройте окно конфигурации
    !
    interface GigabitEthernet0/0/1.30
    description Operations
    encapsulation dot1Q 30
    ip address 10.30.0.1 255.255.255.0
    ip access-group OPERATIONS_POLICY in
    !
    interface GigabitEthernet0/0/1.40
    description Sales
    encapsulation dot1Q 40
    ip address 10.40.0.1 255.255.255.0
    ip access-group SALES_POLICY in
    !
### Шаг 3. Убедитесь, что политики безопасности применяются развернутыми списками доступа.
PC-A	Ping	10.40.0.10	Сбой
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-A-10.40.0.10N.png)
PC-A	Ping	10.20.0.1	Успех
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-A-10.20.0.1Y.png)
PC-B	Ping	10.30.0.10	Сбой
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-B-10.20.0.1N.png)
PC-B	Ping	10.20.0.1	Сбой
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/Ping-PC-B-10.20.0.1N.png)
PC-B	Ping	172.16.1.1	Успех
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/SSH-PC-B-172.16.1.1Y.png)
PC-B	HTTPS	10.20.0.6	Сбой
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/HTTPS-PC-B-10.20.0.6N.png)
PC-B	SSH	10.20.0.4	Сбой
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/SSH-PC-B-10.20.0.4N.png)
PC-B	SSH	172.16.1.1	Успех
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW11/screen/SSH-PC-B-172.16.1.1Y.png)
