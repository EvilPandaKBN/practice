# Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW6/screen/topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|Шлюз по умолчанию|
| ------------ | ------------ | ------------ |------------ |------------ |
| R1|G0/0/1.10|192.168.10.1|255.255.255.0|-|
| R1|G0/0/1.20|192.168.20.1|255.255.255.0|-|
| R1|G0/0/1.30|192.168.30.1|255.255.255.0|-|
| R1|G0/0/1000|-|-|-|
| S1|Vlan 10|192.168.10.11|255.255.255.0|192.168.10.1|
| S2|Vlan 10|192.168.10.12|255.255.255.0|192.168.10.1|
| PC-A|NIC|192.168.20.3|255.255.255.0|192.168.20.1|
| PC-B|NIC|192.168.30.3|255.255.255.0|192.168.30.1|
## Таблица VLAN
|VLAN|Имя|Назначенный интерфейс|
| ------------ | ------------ | ------------ |
| 10| Управление | S1: VLAN 10 S2: VLAN 10 |
| 20| Sales | S1: F0/6 |
| 30| Operations | S2: F0/18 |
| 999| Parking_Lot | S1: F0/2-4, F0/7-24, G0/1-2 |
| 999| Parking_Lot | S2: F0/2-17, F0/19-24, G0/1-2 |
| 1000| Собственная | - |
## Задачи
#### Часть 1. Создание сети и настройка основных параметров устройства
#### Часть 2. Создание сетей VLAN и назначение портов коммутатора
#### Часть 3. Настройка транка 802.1Q между коммутаторами.
#### Часть 4. Настройка маршрутизации между сетями VLAN
#### Часть 5. Проверка, что маршрутизация между VLAN работает
## Общие сведения/сценарий
В целях повышения производительности сети большие широковещательные домены 2-го уровня делят на домены меньшего размера. Для этого современные коммутаторы используют виртуальные локальные сети (VLAN). VLAN также можно использовать в качестве меры безопасности, отделяя конфиденциальный трафик данных от остальной части сети. Сети VLAN облегчают процесс проектирования сети, обеспечивающей помощь в достижении целей организации. Для связи между VLAN требуется устройство, работающее на уровне 3 модели OSI. Добавление маршрутизации между VLAN позволяет организации разделять и разделять широковещательные домены, одновременно позволяя им обмениваться данными друг с другом.<br>
Транковые каналы сети VLAN используются для распространения сетей VLAN по различным устройствам. Транковые каналы разрешают передачу трафика из множества сетей VLAN через один канал, не нанося вред идентификации и сегментации сети VLAN. Особый вид маршрутизации между VLAN, называемый «Router-on-a-Stick», использует магистраль от маршрутизатора к коммутатору, чтобы все VLAN могли переходить к маршрутизатору.<br>
В этой лабораторной работе вы создадите VLAN на обоих коммутаторах в топологии, назначите VLAN для коммутации портов доступа, убедитесь, что VLAN работают должным образом, создадите транки VLAN между двумя коммутаторами и между S1 и R1, и настройте маршрутизацию между VLAN на R1 для разрешения связи между хостами в разных VLAN независимо от подсети, в которой находится хост.<br>
Примечание: Маршрутизаторы, используемые в практических лабораторных работах CCNA, - это Cisco 4221 с Cisco IOS XE Release 16.9.4 (образ universalk9). В лабораторных работах используются коммутаторы Cisco Catalyst 2960 с Cisco IOS версии 15.2(2) (образ lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. Правильные идентификаторы интерфейса см. в сводной таблице по интерфейсам маршрутизаторов в конце лабораторной работы.<br>
Примечание. Убедитесь, что у всех маршрутизаторов и коммутаторов была удалена начальная конфигурация. Если вы не уверены в этом, обратитесь к инструктору.<br>
## Необходимые ресурсы
•	1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
•	2 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.2(2) (образ lanbasek9) или аналогичная модель)<br>
•	2 ПК (ОС Windows с программой эмуляции терминалов, такой как Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Инструкции
## Часть 1. Создание сети и настройка основных параметров устройства
В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.
### Шаг 1. Создайте сеть согласно топологии.
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.
### Шаг 2. Настройте базовые параметры для маршрутизатора.
a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.<br>
b.	Войдите в режим конфигурации.<br>
c.	Назначьте маршрутизатору имя устройства.<br>
d.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
e.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
f.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
g.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.<br>
h.	Зашифруйте открытые пароли.<br>
i.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>

    R1#show startup-config 
    Using 734 bytes
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
    banner motd ^C
    WARNING! Unauthorized access is strictly prohibited
    ^C
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
e.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Настройте на коммутаторах время.<br>
i.	Сохранение текущей конфигурации в качестве начальной.<br>

    S1# show startup-config 
    Using 1274 bytes
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
    banner motd ^C
    WARNING! Unauthorized access is strictly prohibited
    ^C
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
<br>

    S2#show startup-config 
    Using 1251 bytes
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
    banner motd ^C
    WARNING! Unauthorized access is strictly prohibited
    ^C
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
### Шаг 4. Настройте узлы ПК.
Адреса ПК можно посмотреть в таблице адресации.
## Часть 2. Создание сетей VLAN и назначение портов коммутатора
Во второй части вы создадите VLAN, как указано в таблице выше, на обоих коммутаторах. Затем вы назначите VLAN соответствующему интерфейсу и проверите настройки конфигурации. Выполните следующие задачи на каждом коммутаторе.
### Шаг 1. Создайте сети VLAN на коммутаторах.
a.	Создайте и назовите необходимые VLAN на каждом коммутаторе из таблицы выше.

    S1(config)#do show vlan brief
    
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/2, Fa0/3, Fa0/4
    Fa0/5, Fa0/6, Fa0/7, Fa0/8
    Fa0/9, Fa0/10, Fa0/11, Fa0/12
    Fa0/13, Fa0/14, Fa0/15, Fa0/16
    Fa0/17, Fa0/18, Fa0/19, Fa0/20
    Fa0/21, Fa0/22, Fa0/23, Fa0/24
    Gig0/1, Gig0/2
    10 management active 
    20 sales active 
    30 operations active 
    999 parking_lot active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active
<br>

    S2(config)#do show vlan brief
    
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/2, Fa0/3, Fa0/4
    Fa0/5, Fa0/6, Fa0/7, Fa0/8
    Fa0/9, Fa0/10, Fa0/11, Fa0/12
    Fa0/13, Fa0/14, Fa0/15, Fa0/16
    Fa0/17, Fa0/18, Fa0/19, Fa0/20
    Fa0/21, Fa0/22, Fa0/23, Fa0/24
    Gig0/1, Gig0/2
    10 management active 
    20 sales active 
    30 operations active 
    999 parking_lot active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active
b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации

    S1#sh running-config
    !
    interface Vlan10
    ip address 192.168.10.11 255.255.255.0
    !
    ip default-gateway 192.168.10.1
    !
<br>
    
    S2#sh running-config
    !
    interface Vlan10
    ip address 192.168.10.12 255.255.255.0
    !
    ip default-gateway 192.168.10.1
    !
c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их.

    S1(config)#do sh ru
    Building configuration...
    
    Current configuration : 2793 bytes
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
    clock timezone MSK 3
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
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/3
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/4
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/5
    !
    interface FastEthernet0/6
    !
    interface FastEthernet0/7
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/8
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/9
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/10
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/11
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/12
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/13
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/14
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/15
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/16
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/17
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/18
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/19
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/20
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/21
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/22
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/23
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/24
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface GigabitEthernet0/1
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface GigabitEthernet0/2
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface Vlan1
    no ip address
    shutdown
    !
    interface Vlan10
    ip address 192.168.10.11 255.255.255.0
    !
    ip default-gateway 192.168.10.1
    !
    banner motd ^C
    WARNING! Unauthorized access is strictly prohibited
    ^C
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

    S2(config-if-range)#do sh r
    Building configuration...
    
    Current configuration : 2894 bytes
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
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/3
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/4
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/5
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/6
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/7
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/8
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/9
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/10
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/11
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/12
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/13
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/14
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/15
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/16
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/17
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/18
    !
    interface FastEthernet0/19
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/20
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/21
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/22
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/23
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface FastEthernet0/24
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface GigabitEthernet0/1
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface GigabitEthernet0/2
    switchport access vlan 999
    switchport mode access
    shutdown
    !
    interface Vlan1
    no ip address
    shutdown
    !
    interface Vlan10
    ip address 192.168.10.12 255.255.255.0
    !
    ip default-gateway 192.168.10.1
    !
    banner motd ^C
    WARNING! Unauthorized access is strictly prohibited
    ^C
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

### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.
a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.<br>
b.	Убедитесь, что VLAN назначены на правильные интерфейсы.

    S1#show running-config
    !
    interface FastEthernet0/6
    switchport access vlan 20
    switchport mode access
    !
    S2#show running-config
    !
    interface FastEthernet0/18
    switchport access vlan 30
    switchport mode access
    !
## Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами
### Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.
a.	Настройка статического транкинга на интерфейсе F0/1 для обоих коммутаторов.
Откройте окно конфигурации.<br>
b.	Установите native VLAN 1000 на обоих коммутаторах.<br>
c.	Укажите, что VLAN 10, 20, 30 и 1000 могут проходить по транку.<br>
d.	Проверьте транки, native VLAN и разрешенные VLAN через транк.<br>

    S1(config)#do sh r
    interface FastEthernet0/1
    switchport trunk native vlan 1000
    switchport trunk allowed vlan 10,20,30,1000
    switchport mode trunk
    !
    
    S2(config)#do sh r
    interface FastEthernet0/1
    switchport trunk native vlan 1000
    switchport trunk allowed vlan 10,20,30,1000
    switchport mode trunk
    !
### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.
a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.<br>
b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
c.	Проверка транкинга.<br>

    !
    interface FastEthernet0/5
    switchport trunk native vlan 1000
    switchport trunk allowed vlan 10,20,30,1000
    switchport mode trunk
    !
## Часть 4. Настройка маршрутизации между сетями VLAN
### Шаг 1. Настройте маршрутизатор.
a.	При необходимости активируйте интерфейс G0/0/1 на маршрутизаторе.<br>
b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.<br>
Убедитесь, что вспомогательные интерфейсы работают<br>

    !
    interface GigabitEthernet0/0/1
    no ip address
    duplex auto
    speed auto
    !
    interface GigabitEthernet0/0/1.10
    description management
    encapsulation dot1Q 10
    ip address 192.168.10.1 255.255.255.0
    !
    interface GigabitEthernet0/0/1.20
    description sales
    encapsulation dot1Q 20
    ip address 192.168.20.1 255.255.255.0
    !
    interface GigabitEthernet0/0/1.30
    description operations
    encapsulation dot1Q 30
    ip address 192.168.30.1 255.255.255.0
    !
    interface GigabitEthernet0/0/1.1000
    description native
    encapsulation dot1Q 1000
    no ip address
    !
    
## Часть 5. Проверьте, работает ли маршрутизация между VLAN
### Шаг 1. Выполните следующие тесты с PC-A. Все должно быть успешно.
a.	Отправьте эхо-запрос с PC-A на шлюз по умолчанию.
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW6/screen/PC-A%20-%20DEFAULT.png)
b.	Отправьте эхо-запрос с PC-A на PC-B.
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW6/screen/PC-A%20-PC-B.png)
c.	Отправьте команду ping с компьютера PC-A на коммутатор S2.
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW6/screen/PC-A-S2.png)
### Шаг 2. Пройдите следующий тест с PC-B
В окне командной строки на PC-B выполните команду tracert на адрес PC-A. 
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW6/screen/PC-B%20-PC-A.png)
#### Вопрос <br>
Какие промежуточные IP-адреса отображаются в результатах?
#### Ответ <br>
Отображается роутер
