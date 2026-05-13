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
