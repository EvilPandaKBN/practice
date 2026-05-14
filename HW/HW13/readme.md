# Лабораторная работа - Настройка протоколов CDP, LLDP и NTP
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW13/topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|Шлюз по умолчанию|
| ------------ | ------------ | ------------ |------------ |------------ |
| R1|G0/0/1|10.22.0.1|255.255.255.0|-|
| R1|Loopback1|172.16.1.1|255.255.255.0|-|
| S1|SVI VLAN 1 |10.22.0.2|255.255.255.0|10.22.0.1|
| S2|SVI VLAN 1 |10.22.0.3|255.255.255.0|10.22.0.1|
## Задачи
Часть 1. Создание сети и настройка основных параметров устройства
Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP
Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP
Часть 4. Настройка и проверка NTP
## Общие сведения/сценарий
Протокол Cisco Discovery Protocol (CDP) — собственный протокол Cisco для обнаружения сетевых ресурсов, функционирующий на канальном уровне. Он служит для обмена информацией, например именами устройств и версиями ПО IOS, с другими физически подключенными устройствами Cisco. Протокол Link Layer Discovery Protocol (LLDP) — это не зависящий от производителя протокол для обнаружения сетевых ресурсов, функционирующий на канальном уровне. В основном он используется сетевыми устройствами в локальной сети (LAN). Сетевые устройства сообщают соседям такие данные о себе, как идентификаторы и сведения о функциональных возможностях.<br>
Протокол сетевого времени (NTP) служит для синхронизации времени между распределенными серверами времени и клиентами. В качестве транспортного протокола NTP использует протокол UDP. Все операции обмена данными по протоколу NTP выполняются по времени в формате UTC.<br>
Сервер NTP обычно получает данные о времени из достоверного источника, такого как атомные часы, к которым подключен сервер. Затем он распределяет это время по сети. Протокол NTP чрезвычайно эффективен; для синхронизации времени на двух компьютерах с временной разницей в пределах миллисекунды требуется отправлять не более одного пакета в минуту.<br>
В этой лабораторной работе вам предстоит задокументировать порты, которые используются для подключения к другим коммутаторам по протоколам CDP и LLDP. Полученные результаты следует указать в диаграмме сетевой топологии. <br>
## Необходимые ресурсы
•	1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
•	2 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.2(2) (образ lanbasek9) или аналогичная модель)<br>
•	1 ПК (под управлением Windows с программой эмуляции терминала, например, Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией.<br>
## Часть 1. Создание сети и настройка основных параметров устройства
В первой части лабораторной работы вам предстоит создать топологию сети и настроить основные параметры для маршрутизатора и коммутаторов.
### Шаг 1. Создайте сеть согласно топологии.
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.
### Шаг 2. Настройте базовые параметры для маршрутизатора.
a.	Назначьте маршрутизатору имя устройства.<br>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Настройка интерфейсов, перечисленных в таблице выше<br>
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>

    R1#sh star
    Using 761 bytes
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
    interface Loopback1
    ip address 172.16.1.1 255.255.255.0
    !
    interface GigabitEthernet0/0/0
    no ip address
    duplex auto
    speed auto
    shutdown
    !
    interface GigabitEthernet0/0/1
    ip address 10.22.0.1 255.255.255.0
    duplex auto
    speed auto
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
g.	Создайте баннер, который предупреждает всех, кто обращается к устройству, видит баннерное сообщение «Только авторизованные пользователи!».<br>
h.	Отключите неиспользуемые интерфейсы<br>
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
   
    S1(config)#do sh start
    Using 1448 bytes
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
    shutdown
    !
    interface FastEthernet0/3
    shutdown
    !
    interface FastEthernet0/4
    shutdown
    !
    interface FastEthernet0/5
    !
    interface FastEthernet0/6
    shutdown
    !
    interface FastEthernet0/7
    shutdown
    !
    interface FastEthernet0/8
    shutdown
    !
    interface FastEthernet0/9
    shutdown
    !
    interface FastEthernet0/10
    shutdown
    !
    interface FastEthernet0/11
    shutdown
    !
    interface FastEthernet0/12
    shutdown
    !
    interface FastEthernet0/13
    shutdown
    !
    interface FastEthernet0/14
    shutdown
    !
    interface FastEthernet0/15
    shutdown
    !
    interface FastEthernet0/16
    shutdown
    !
    interface FastEthernet0/17
    shutdown
    !
    interface FastEthernet0/18
    shutdown
    !
    interface FastEthernet0/19
    shutdown
    !
    interface FastEthernet0/20
    shutdown
    !
    interface FastEthernet0/21
    shutdown
    !
    interface FastEthernet0/22
    shutdown
    !
    interface FastEthernet0/23
    shutdown
    !
    interface FastEthernet0/24
    shutdown
    !
    interface GigabitEthernet0/1
    shutdown
    !
    interface GigabitEthernet0/2
    shutdown
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

    S2#sh start
    Using 1458 bytes
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
    shutdown
    !
    interface FastEthernet0/3
    shutdown
    !
    interface FastEthernet0/4
    shutdown
    !
    interface FastEthernet0/5
    shutdown
    !
    interface FastEthernet0/6
    shutdown
    !
    interface FastEthernet0/7
    shutdown
    !
    interface FastEthernet0/8
    shutdown
    !
    interface FastEthernet0/9
    shutdown
    !
    interface FastEthernet0/10
    shutdown
    !
    interface FastEthernet0/11
    shutdown
    !
    interface FastEthernet0/12
    shutdown
    !
    interface FastEthernet0/13
    shutdown
    !
    interface FastEthernet0/14
    shutdown
    !
    interface FastEthernet0/15
    shutdown
    !
    interface FastEthernet0/16
    shutdown
    !
    interface FastEthernet0/17
    shutdown
    !
    interface FastEthernet0/18
    shutdown
    !
    interface FastEthernet0/19
    shutdown
    !
    interface FastEthernet0/20
    shutdown
    !
    interface FastEthernet0/21
    shutdown
    !
    interface FastEthernet0/22
    shutdown
    !
    interface FastEthernet0/23
    shutdown
    !
    interface FastEthernet0/24
    shutdown
    !
    interface GigabitEthernet0/1
    shutdown
    !
    interface GigabitEthernet0/2
    shutdown
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

## Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP
На устройствах Cisco протокол CDP включен по умолчанию. Воспользуйтесь CDP, чтобы обнаружить порты, к которым подключены кабели.<br>
a.	На R1 используйте соответствующую команду show cdp, чтобы определить, сколько интерфейсов включено CDP, сколько из них включено и сколько отключено.
    
    R1#show cdp interface 
    Vlan1 is administratively down, line protocol is down
    Sending CDP packets every 60 seconds
    Holdtime is 180 seconds
    GigabitEthernet0/0/0 is administratively down, line protocol is down
    Sending CDP packets every 60 seconds
    Holdtime is 180 seconds
    GigabitEthernet0/0/1 is up, line protocol is up
    Sending CDP packets every 60 seconds
    Holdtime is 180 seconds

#### Вопрос <br>
Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны?
#### Ответ <br>
3 интерфейса: Vlan1, GigabitEthernet0/0/0 и GigabitEthernet0/0/1. Активен  GigabitEthernet0/0/1 <br>
b.	На R1 используйте соответствующую команду show cdp, чтобы определить версию IOS, используемую на S1.

    R1# show cdp entry S1
    
    Device ID: S1
    Entry address(es): 
    Platform: cisco 2960, Capabilities: Switch
    Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5
    Holdtime: 157
    
    Version :
    Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2013 by Cisco Systems, Inc.
    Compiled Wed 26-Jun-13 02:49 by mnguyen
    
    advertisement version: 2
#### Вопрос <br>
Какая версия IOS используется на  S1?
#### Ответ <br>
Version 15.0(2)SE4
c.	На S1 используйте соответствующую команду show cdp, чтобы определить, сколько пакетов CDP было выданных. <br>
Не удалось проверить из-за отсуствия команды
d.	Настройте SVI для VLAN 1 на S1 и S2, используя IP-адреса, указанные в таблице адресации выше. Настройте шлюз по умолчанию для каждого коммутатора на основе таблицы адресов.<br>
e.	На R1 выполните команду show cdp entry S1 . 
    
    R1# show cdp entry S1
    
    Device ID: S1
    Entry address(es): 
    IP address : 10.22.0.2
    Platform: cisco 2960, Capabilities: Switch
    Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5
    Holdtime: 143
    
    Version :
    Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2013 by Cisco Systems, Inc.
    Compiled Wed 26-Jun-13 02:49 by mnguyen
    
    advertisement version: 2
    Duplex: full

#### Вопрос <br>
Какие дополнительные сведения доступны теперь?
#### Ответ <br>
Entry address(es):  IP address : 10.22.0.2 <br>
f.	Отключить CDP глобально на всех устройствах. 
## Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP
На устройствах Cisco протокол LLDP может быть включен по умолчанию. Воспользуйтесь LLDP, чтобы обнаружить порты, к которым подключены кабели.<br>
a.	Введите соответствующую команду lldp, чтобы включить LLDP на всех устройствах в топологии.<br>
b.	На S1 выполните соответствующую команду lldp, чтобы предоставить подробную информацию о S2.<br>
    
    S1#show lldp neighbors detail 
    ------------------------------------------------
    Chassis id: 0002.167D.7701
    Port id: Fa0/1
    Port Description: FastEthernet0/1
    System Name: S2
    System Description:
    Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2013 by Cisco Systems, Inc.
    Compiled Wed 26-Jun-13 02:49 by mnguyen
    Time remaining: 90 seconds
    System Capabilities: B
    Enabled Capabilities: B
    Management Addresses - not advertised
    Auto Negotiation - supported, enabled
    Physical media capabilities:
    100baseT(FD)
    100baseT(HD)
    1000baseT(HD)
    Media Attachment Unit type: 10
    Vlan ID: 1
### Вопрос <br>
Что такое chassis ID  для коммутатора S2?
#### Ответ <br>
Это мак адрес порта F0/1<br>
Соединитесь через консоль на всех устройствах и используйте команды LLDP, необходимые для отображения топологии физической сети только из выходных данных команды show.
    
    R1#show lldp neighbors 
    Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
    Device ID Local Intf Hold-time Capability Port ID
    S1 Gig0/0/1 120 B Fa0/5
<br>
    
    S1#show lldp neighbors 
    Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
    Device ID Local Intf Hold-time Capability Port ID
    S2 Fa0/1 120 B Fa0/1
    R1 Fa0/5 120 R Gig0/0/1
    
    Total entries displayed: 2
<br>

    S2(config)#do sh lldp nei
    Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
    Device ID Local Intf Hold-time Capability Port ID
    S1 Fa0/1 120 B Fa0/1
    
    Total entries displayed: 1
## Часть 4. Настройка NTP
### Шаг 1. Выведите на экран текущее время.
|Дата|Время|Часовой пояс|Источник времени|
| ------------ | ------------ | ------------ |------------ |
| 01.03.1993|0:30:32,980|UTC|Локальный|
### Шаг 2. Установите время.
С помощью команды clock set установите время на маршрутизаторе R1. Введенное время должно быть в формате
UTC. 
### Шаг 3. Настройте главный сервер NTP.
Настройте R1 в качестве хозяина NTP с уровнем слоя 4.
 
     R1(config)#ntp master 4
### Шаг 4. Настройте клиент NTP.
a.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время. Запишите текущее
время,  в следующей таблице
|Дата|Время|Часовой пояс|
| ------------ | ------------ | ------------ |
| 01.03.1993|0:34:11.859|UTC|
| 01.03.1993|0:34:58.246|UTC|
b.	Настройте S1 и S2 в качестве клиентов NTP. Используйте соответствующие команды NTP для получения
времени от интерфейса G0/0/1 R1, а также для периодического обновления календаря или аппаратных часов
коммутатора.
### Шаг 5. Проверьте настройку NTP.
a.	Используйте соответствующую команду show , чтобы убедиться, что S1 и S2 синхронизированы с R1.
    S1#show ntp associations
    
    address ref clock st when poll reach delay offset disp
    *~10.22.0.1 127.127.1.1 4 2 16 377 0.00 0.00 0.12
    * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured
    
<br>

    S2(config)#do show ntp associations
    
    address ref clock st when poll reach delay offset disp
    *~10.22.0.1 127.127.1.1 4 0 16 377 0.00 0.00 0.12
    * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured

b.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время и сравнить ранее
записанное время.
    
    S1#show clock 
    12:9:56.125 UTC Mon Apr 20 2026
<br>

    S2(config)# do sh clo
    12:10:35.539 UTC Mon Apr 20 2026
## Вопрос для повторения
### Вопрос <br>
Для каких интерфейсов в пределах сети не следует использовать протоколы обнаружения сетевых ресурсов?
Поясните ответ.
#### Ответ <br>
На внешние интерфейсы и порты в режиме access. В случае внешних интерфейсов, злоумышленники могут
получить информацию об устройстве и исходя из этого спланировать атаку. А в access портах, конечные
устройства не обрабатывают эту информацию, следовательно, и отправлять незачем. Также это экономит
пропускную способность канала и ресурсы процессора
