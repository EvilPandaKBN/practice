# Лабораторная работа - Конфигурация безопасности коммутатора 
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|
| ------------ | ------------ | ------------ |------------ |
| R1|G0/0/1|192.168.10.1|255.255.255.0 |
| R1|Loopback0 |192.168.20.1|255.255.255.0 |
| S1|Vlan 10|192.168.10.201|255.255.255.0 |
| S2|Vlan 10|192.168.10.202|255.255.255.0 |
| PC-A|NIC|DHCP|255.255.255.0 |
| PC-B|NIC|DHCP|255.255.255.0 |
## Цели
Часть 1. Настройка основного сетевого устройства<br>
•	Создайте сеть.<br>
•	Настройте маршрутизатор R1.<br>
•	Настройка и проверка основных параметров коммутатора<br>
Часть 2. Настройка сетей VLAN<br>
•	Сконфигруриуйте VLAN 10.<br>
•	Сконфигруриуйте SVI для VLAN 10.<br>
•	Настройте VLAN 333 с именем Native на S1 и S2.<br>
•	Настройте VLAN 999 с именем ParkingLot на S1 и S2.<br>
Часть 3: Настройки безопасности коммутатора.<br>
•	Реализация магистральных соединений 802.1Q.<br>
•	Настройка портов доступа<br>
•	Безопасность неиспользуемых портов коммутатора<br>
•	Документирование и реализация функций безопасности порта.<br>
•	Реализовать безопасность DHCP snooping .<br>
•	Реализация PortFast и BPDU Guard<br>
•	Проверка сквозной связанности.<br>
## Общие сведения/сценарий
Это комплексная лабораторная работа, нацеленная на повторение ранее изученных функций безопасности уровня 2.
Примечание: Маршрутизаторы, используемые в практических лабораторных работах CCNA, - это Cisco 4221 с Cisco IOS XE Release 16.9.3 (образ universalk9). В лабораторных работах используются коммутаторы Cisco Catalyst 2960 с Cisco IOS версии 15.0(2) (образ lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. Правильные идентификаторы интерфейса см. в сводной таблице по интерфейсам маршрутизаторов в конце лабораторной работы.<br>
Примечание: Убедитесь, что все настройки коммутатора удалены и загрузочная конфигурация отсутствует. Если вы не уверены, обратитесь к инструктору.<br>
## Необходимые ресурсы
•	1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.3 или аналогичным)<br>
•	2 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.0(2) (образ lanbasek9) или аналогичная модель)<br>
•	2 ПК (ОС Windows с программой эмуляции терминалов, такой как Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Инструкции
## Часть 1. Настройка основного сетевого устройства
### Шаг 1. Создайте сеть.
a.	Создайте сеть согласно топологии.<br>
b.	Инициализация устройств
### Шаг 2. Настройте маршрутизатор R1.
a.	Загрузите следующий конфигурационный скрипт на R1.

    enable
    configure terminal
    hostname R1
    no ip domain lookup
    ip dhcp excluded-address 192.168.10.1 192.168.10.9
    ip dhcp excluded-address 192.168.10.201 192.168.10.202
    ip dhcp relay information trust-all
    !
    ip dhcp pool Students
     network 192.168.10.0 255.255.255.0
     default-router 192.168.10.1
     domain-name CCNA2.Lab-11.6.1
    !
    interface Loopback0
     ip address 10.10.1.1 255.255.255.0
    !
    interface GigabitEthernet0/0/1
     description Link to S1
     ip address 192.168.10.1 255.255.255.0
     no shutdown
    !
    line con 0
     logging synchronous
     exec-timeout 0 0

 b.	Проверьте текущую конфигурацию на R1, используя следующую команду:
 R1# show ip interface brief

    R1#show ip int brief 
    Interface IP-Address OK? Method Status Protocol 
    GigabitEthernet0/0/0 unassigned YES unset administratively down down 
    GigabitEthernet0/0/1 192.168.10.1 YES manual up up 
    Loopback0 10.10.1.1 YES manual up up 
    Vlan1 unassigned YES unset administratively down down

c.	Убедитесь, что IP-адресация и интерфейсы находятся в состоянии up / up (при необходимости устраните неполадки).
### Шаг 3. Настройка и проверка основных параметров коммутатора
a.	Настройте имя хоста для коммутаторов S1 и S2.<br>
b.	Запретите нежелательный поиск в DNS.<br>
c.	Настройте описания интерфейса для портов, которые используются в S1 и S2.<br>
d.	Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.<br>
## Часть 2. Настройка сетей VLAN на коммутаторах.
### Шаг 1. Сконфигруриуйте VLAN 10.
Добавьте VLAN 10 на S1 и S2 и назовите VLAN - Management.
### Шаг 2. Сконфигруриуйте SVI для VLAN 10.
Настройте IP-адрес в соответствии с таблицей адресации для SVI для VLAN 10 на S1 и S2. Включите интерфейсы SVI и предоставьте описание для интерфейса.
### Шаг 3. Настройте VLAN 333 с именем Native на S1 и S2.
### Шаг 4. Настройте VLAN 999 с именем ParkingLot на S1 и S2.

    S1#show running-config 
    Building configuration...
    
    Current configuration : 1273 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    no service password-encryption
    !
    hostname S1
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
    description to S2
    !
    interface FastEthernet0/2
    !
    interface FastEthernet0/3
    !
    interface FastEthernet0/4
    !
    interface FastEthernet0/5
    description to R1
    !
    interface FastEthernet0/6
    description to pc-a
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
    interface Vlan10
    description Management
    ip address 192.168.10.201 255.255.255.0
    !
    ip default-gateway 192.168.10.1
    !
    !
    !
    !
    line con 0
    !
    line vty 0 4
    login
    line vty 5 15
    login
    !
    !
    !
    !
    End
<br>

    S1#show vlan brief 
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/2, Fa0/3, Fa0/4
    Fa0/5, Fa0/6, Fa0/7, Fa0/8
    Fa0/9, Fa0/10, Fa0/11, Fa0/12
    Fa0/13, Fa0/14, Fa0/15, Fa0/16
    Fa0/17, Fa0/18, Fa0/19, Fa0/20
    Fa0/21, Fa0/22, Fa0/23, Fa0/24
    Gig0/1, Gig0/2
    10 Management active 
    333 Native active 
    999 parkingLot active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active

<br>
    
    S2#show running-config 
    Building configuration...
    
    Current configuration : 1254 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    no service password-encryption
    !
    hostname S2
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
    description to S1
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
    description to PC-B
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
    interface Vlan10
    description Management
    ip address 192.168.10.202 255.255.255.0
    !
    ip default-gateway 192.168.10.1
    !
    !
    !
    !
    line con 0
    !
    line vty 0 4
    login
    line vty 5 15
    login
    !
    !
    !
    !
    End

<br>

    S2#show vlan brief 
    
    VLAN Name Status Ports
    ---- -------------------------------- --------- -------------------------------
    1 default active Fa0/1, Fa0/2, Fa0/3, Fa0/4
    Fa0/5, Fa0/6, Fa0/7, Fa0/8
    Fa0/9, Fa0/10, Fa0/11, Fa0/12
    Fa0/13, Fa0/14, Fa0/15, Fa0/16
    Fa0/17, Fa0/18, Fa0/19, Fa0/20
    Fa0/21, Fa0/22, Fa0/23, Fa0/24
    Gig0/1, Gig0/2
    10 Management active 
    333 Native active 
    999 ParkingLot active 
    1002 fddi-default active 
    1003 token-ring-default active 
    1004 fddinet-default active 
    1005 trnet-default active

## Часть 3. Настройки безопасности коммутатора.
### Шаг 1. Релизация магистральных соединений 802.1Q.
a.	Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN.<br>
b.	Убедитесь, что режим транкинга успешно настроен на всех коммутаторах.

    S1#show interfaces trunk 
    Port        Mode         Encapsulation  Status        Native vlan
    Fa0/1       on           802.1q         trunking      333
    
    Port        Vlans allowed on trunk
    Fa0/1       1-1005
    
    Port        Vlans allowed and active in management domain
    Fa0/1       1,10,333,999
    
    Port        Vlans in spanning tree forwarding state and not pruned
    Fa0/1       1,10,333,999
    S2#show interfaces trunk 
    Port        Mode         Encapsulation  Status        Native vlan
    Fa0/1       on           802.1q         trunking      333
    
    Port        Vlans allowed on trunk
    Fa0/1       1-1005
    
    Port        Vlans allowed and active in management domain
    Fa0/1       1,10,333,999
    
    Port        Vlans in spanning tree forwarding state and not pruned
    Fa0/1       1,10,333,999
c.	Отключить согласование DTP F0/1 на S1 и S2.<br>
d.	Проверьте с помощью команды show interfaces.

    S1#sh interfaces f0/1 switchport | include Negoti
    Negotiation of Trunking: Off
    S1#sh interfaces f0/1 switchport | include Negoti
    Negotiation of Trunking: Off
### Шаг 2. Настройка портов доступа
a.	На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10.<br>
b.	На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10.
### Шаг 3. Безопасность неиспользуемых портов коммутатора
a.	На S1 и S2 переместите неиспользуемые порты из VLAN 1 в VLAN 999 и отключите неиспользуемые порты.<br>
b.	Убедитесь, что неиспользуемые порты отключены и связаны с VLAN 999, введя команду  show.

    S1(config)#do show int sta
    Port Name Status Vlan Duplex Speed Type
    Fa0/1 to S2 connected trunk auto auto 10/100BaseTX
    Fa0/2 disabled 999 auto auto 10/100BaseTX
    Fa0/3 disabled 999 auto auto 10/100BaseTX
    Fa0/4 disabled 999 auto auto 10/100BaseTX
    Fa0/5 to R1 connected 10 auto auto 10/100BaseTX
    Fa0/6 to pc-a connected 10 auto auto 10/100BaseTX
    Fa0/7 disabled 999 auto auto 10/100BaseTX
    Fa0/8 disabled 999 auto auto 10/100BaseTX
    Fa0/9 disabled 999 auto auto 10/100BaseTX
    Fa0/10 disabled 999 auto auto 10/100BaseTX
    Fa0/11 disabled 999 auto auto 10/100BaseTX
    Fa0/12 disabled 999 auto auto 10/100BaseTX
    Fa0/13 disabled 999 auto auto 10/100BaseTX
    Fa0/14 disabled 999 auto auto 10/100BaseTX
    Fa0/15 disabled 999 auto auto 10/100BaseTX
    Fa0/16 disabled 999 auto auto 10/100BaseTX
    Fa0/17 disabled 999 auto auto 10/100BaseTX
    Fa0/18 disabled 999 auto auto 10/100BaseTX
    Fa0/19 disabled 999 auto auto 10/100BaseTX
    Fa0/20 disabled 999 auto auto 10/100BaseTX
    Fa0/21 disabled 999 auto auto 10/100BaseTX
    Fa0/22 disabled 999 auto auto 10/100BaseTX
    Fa0/23 disabled 999 auto auto 10/100BaseTX
    Fa0/24 disabled 999 auto auto 10/100BaseTX
    Gig0/1 disabled 999 auto auto 10/100BaseTX
    Gig0/2 disabled 999 auto auto 10/100BaseTX 
<br>

    S2(config)#do sh int sta
    Port Name Status Vlan Duplex Speed Type
    Fa0/1 to S1 connected trunk auto auto 10/100BaseTX
    Fa0/2 disabled 999 auto auto 10/100BaseTX
    Fa0/3 disabled 999 auto auto 10/100BaseTX
    Fa0/4 disabled 999 auto auto 10/100BaseTX
    Fa0/5 disabled 999 auto auto 10/100BaseTX
    Fa0/6 disabled 999 auto auto 10/100BaseTX
    Fa0/7 disabled 999 auto auto 10/100BaseTX
    Fa0/8 disabled 999 auto auto 10/100BaseTX
    Fa0/9 disabled 999 auto auto 10/100BaseTX
    Fa0/10 disabled 999 auto auto 10/100BaseTX
    Fa0/11 disabled 999 auto auto 10/100BaseTX
    Fa0/12 disabled 999 auto auto 10/100BaseTX
    Fa0/13 disabled 999 auto auto 10/100BaseTX
    Fa0/14 disabled 999 auto auto 10/100BaseTX
    Fa0/15 disabled 999 auto auto 10/100BaseTX
    Fa0/16 disabled 999 auto auto 10/100BaseTX
    Fa0/17 disabled 999 auto auto 10/100BaseTX
    Fa0/18 to PC-B connected 10 auto auto 10/100BaseTX
    Fa0/19 disabled 999 auto auto 10/100BaseTX
    Fa0/20 disabled 999 auto auto 10/100BaseTX
    Fa0/21 disabled 999 auto auto 10/100BaseTX
    Fa0/22 disabled 999 auto auto 10/100BaseTX
    Fa0/23 disabled 999 auto auto 10/100BaseTX
    Fa0/24 disabled 999 auto auto 10/100BaseTX
    Gig0/1 disabled 999 auto auto 10/100BaseTX
    Gig0/2 disabled 999 auto auto 10/100BaseTX 
    Gi0/1 disabled 999 auto auto 10/100/1000BaseTX
    Gi0/2 disabled 999 auto auto 10/100/1000BaseTX
### Шаг 4. Документирование и реализация функций безопасности порта.
Интерфейсы F0/6 на S1 и F0/18 на S2 настроены как порты доступа. На этом шаге вы также настроите безопасность портов на этих двух портах доступа.<br>
a.	На S1, введите команду show port-security interface f0/6  для отображения настроек по умолчанию безопасности порта для интерфейса F0/6. Запишите свои ответы ниже.
| Функция| Настройка по умолчанию|
| ------------ | ------------ |
|Защита портов|Disabled|
| Максимальное количество записей MAC-адресов|1|
|Режим проверки на нарушение безопасности|Shutdown|
| Aging Time|0|
|Aging Type|Absolute|
|Secure Static Address Aging|Disabled|
|Sticky MAC Address|0|
b.	На S1 включите защиту порта на F0 / 6 со следующими настройками:<br>
o	Максимальное количество записей MAC-адресов: 3<br>
o	Режим безопасности: restrict<br>
o	Aging time: 60 мин.<br>
o	Aging type: неактивный<br>
c.	Verify port security on S1 F0/6.

    S1#show port-security interface f0/6
    Port Security              : Enabled
    Port Status                : Secure-up
    Violation Mode             : Restrict
    Aging Time                 : 60 mins
    Aging Type                 : Absolute
    SecureStatic Address Aging : Disabled
    Maximum MAC Addresses      : 3
    Total MAC Addresses        : 0
    Configured MAC Addresses   : 0
    Sticky MAC Addresses       : 0
    Last Source Address:Vlan   : 0000.0000.0000:0
    Security Violation Count   : 0
    
    S1#show port-security address 
    Secure Mac Address Table
    -----------------------------------------------------------------------------
    Vlan Mac Address Type Ports Remaining Age
    (mins)
    ---- ----------- ---- ----- -------------
    10 00E0.F7A8.C77A DynamicConfigured FastEthernet0/6 -
    -----------------------------------------------------------------------------
    Total Addresses in System (excluding one mac per port) : 0
    Max Addresses limit in System (excluding one mac per port) : 1024

Не удалось установить Aging Type, на данной модели коммутатора нет такой команды

        S1(config-if)#switchport port-security aging ?
        time Port-security aging time
        S1(config-if)#switchport port-security aging



Включите безопасность порта для F0 / 18 на S2. Настройте каждый активный порт доступа таким образом, чтобы он автоматически добавлял адреса МАС, изученные на этом порту, в текущую конфигурацию.
d.	Настройте следующие параметры безопасности порта на S2 F / 18:<br>
o	Максимальное количество записей MAC-адресов: 2<br>
o	Тип безопасности: Protect<br>
o	Aging time: 60 мин.<br>
e.	Проверка функции безопасности портов на S2 F0/18.

        S2#show port-security interface f0/18
        Port Security              : Enabled
        Port Status                : Secure-up
        Violation Mode             : Protect
        Aging Time                 : 60 mins
        Aging Type                 : Absolute
        SecureStatic Address Aging : Disabled
        Maximum MAC Addresses      : 2
        Total MAC Addresses        : 0
        Configured MAC Addresses   : 0
        Sticky MAC Addresses       : 0
        Last Source Address:Vlan   : 0000.0000.0000:0
        Security Violation Count   : 0
        
### Шаг 5. Реализовать безопасность DHCP snooping.
a.	На S2 включите DHCP snooping и настройте DHCP snooping во VLAN 10.<br>
b.	Настройте магистральные порты на S2 как доверенные порты.<br>
c.	Ограничьте ненадежный порт Fa0/18 на S2 пятью DHCP-пакетами в секунду.<br>
d.	Проверка DHCP Snooping на S2.<br>

        S2(config)#do show ip dhcp snooping
        Switch DHCP snooping is enabled
        DHCP snooping is configured on following VLANs:
        10
        Insertion of option 82 is enabled
        Option 82 on untrusted port is not allowed
        Verification of hwaddr field is enabled
        Interface                  Trusted    Rate limit (pps)
        -----------------------    -------    ----------------
        FastEthernet0/1            yes        unlimited       
        FastEthernet0/18           no         5     
e.	В командной строке на PC-B освободите, а затем обновите IP-адрес.
      
        C:\Users\Student> ipconfig /release
        C:\Users\Student> ipconfig /renew
f.	Проверьте привязку отслеживания DHCP с помощью команды show ip dhcp snooping binding.
            
            S2(config)#do show ip dhcp snooping binding
            MacAddress IpAddress Lease(sec) Type VLAN Interface
            ------------------ --------------- ---------- ------------- ---- -----------------
            00:60:5C:57:47:DD 192.168.10.11 0 dhcp-snooping 10 FastEthernet0/18

### Шаг 6. Реализация PortFast и BPDU Guard
a.	Настройте PortFast на всех портах доступа, которые используются на обоих коммутаторах.<br>
b.	Включите защиту BPDU на портах доступа VLAN 10 S1 и S2, подключенных к PC-A и PC-B.<br>
c.	Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.<br>

S1#show spanning-tree interface f0/6 detail



        Port 6 (FastEthernet0/6) of VLAN0010 is designated forwarding
        Port path cost 19, Port priority 128, Port Identifier 128.6
        Designated root has priority 32778, address 0001.C951.9722
        Designated bridge has priority 32778, address 0003.E474.5577
        Designated port id is 128.6, designated path cost 19
        Timers: message age 16, forward delay 0, hold 0
        Number of transitions to forwarding state: 1
        The port is in the portfast mode
        Link type is point-to-point by default
        !
        interface FastEthernet0/6
        description to pc-a
        switchport access vlan 10
        switchport mode access
        switchport port-security
        switchport port-security maximum 3
        switchport port-security violation restrict 
        switchport port-security aging time 60
        spanning-tree portfast
        spanning-tree bpduguard enable
        !
Шаг 7. Проверьте наличие сквозного подключения.
Проверьте PING свзяь между всеми устройствами в таблице IP-адресации. В случае сбоя проверки связи может потребоваться отключить брандмауэр на хостах.<br>
PC-A-R1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/screen/PC-A-R1.png)
PC-A-S1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/screen/PC-A-S1.png)
PC-A-S2
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/screen/PC-A-S2.png)
PC-A-PC-B
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/screen/PC-A-PC-B.png)
PC-B-R1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/screen/PC-B-R1.png)
PC-B-S1
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/screen/PC-B-S1.png)
PC-B-S2
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW9/screen/PC-B-S2.png)
S1-R1

        S1#ping 192.168.10.1
        
        Type escape sequence to abort.
        Sending 5, 100-byte ICMP Echos to 192.168.10.1, timeout is 2 seconds:
        .!!!!
        Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/0 ms
S1-S2

        S1#ping 192.168.10.202
        Type escape sequence to abort.
        Sending 5, 100-byte ICMP Echos to 192.168.10.202, timeout is 2 seconds:
        ..!!!
        Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms
S2-R1

        S2#ping 192.168.10.1

        Type escape sequence to abort.
        Sending 5, 100-byte ICMP Echos to 192.168.10.1, timeout is 2 seconds:
        .!!!!
        Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/2 ms
#### Вопрос <br>
С точки зрения безопасности порта на S2, почему нет значения таймера для оставшегося возраста в минутах,
когда было сконфигурировано динамическое обучение - sticky?
#### Ответ <br>
В этом суть sticky, что изученный адрес остается в текущей конфигурации, и при записи в стартовую
остаётся там, как статический, а таймер возраста не применяется к статическим
#### Вопрос <br>
Что касается безопасности порта на S2, если вы загружаете скрипт текущей конфигурации на S2, почему порту
18 на PC-B никогда не получит IP-адрес через DHCP?
#### Ответ <br>
При загрузке скрипта уже пррописан mac  устройства (из той схемы на которой создавался скрипт), а режим
Protect будет отбрасывать пакеты он неизвестных адресов
#### Вопрос <br>
Что касается безопасности порта, в чем разница между типом абсолютного устаревания и типом устаревание по
неактивности?
#### Ответ <br>
В устаревании абсолютном мак адрес удаляется спустя определенное время после первого изучения, и если устройство активно, то коммутатору придется изучить его адрес повторно. А при устаревании по активности таймер начнет отсчитывать время, после того, как устройство перестанет проявлять активность (если устройство будет активно постоянно, его адрес не будет удален)

