# Лабраторная работа - Настройка DHCPv6 
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW8/screen/topology.png)
|Устройство|Интерфейс|IP адрес|
| ------------ | ------------ | ------------ |
| R1|G0/0/0|2001:db8:acad:2::1/64|
| R1|G0/0/0|fe80::1|
| R1|G0/0/1|2001:db8:acad:1::1/64|
| R1|G0/0/1|fe80::1|
| R2|G0/0/0|2001:db8:acad:2::2/64|
| R2|G0/0/0|fe80::2|
| R2|G0/0/1|2001:db8:acad:3::1/64|
| R2|G0/0/1|fe80::1|
| PC-A|NIC|DHCP|
| PC-B|NIC|DHCP|
## Задачи
#### Часть 1. Создание сети и настройка основных параметров устройства
#### Часть 2. Проверка назначения адреса SLAAC от R1
#### Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1
#### Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1
#### Часть 5. Настройка и проверка DHCPv6 Relay на R2
## Общие сведения/сценарий
Динамическое назначение глобальных индивидуальных IPv6-адресов можно настроить тремя способами:<br>
•	Автоматическая конфигурация адреса без сохранения состояния (Stateless Address Autoconfiguration, SLAAC)<br>
•	DHCPv6 без отслеживания состояния<br>
•	Адресация DHCPv6 с учетом состояний.<br>
При использовании SLACC для назначения адресов IPv6 хостам сервер DHCPv6 не используется. Поскольку DHCPv6 сервер не используется при реализации SLACC, хосты не могут получать дополнительную важную сетевую информацию, включая адрес сервера доменных имен (DNS), а также имя домена.<br>
При использовании Stateless DHCPv6 для назначения адресов IPv6 хосту сервер DHCPv6 используется для назначения дополнительной важной информации о сети, однако адрес IPv6 назначается с помощью SLACC.<br>
При использовании DHCPv6 с отслеживанием состояния, сервер DHCP назначает всю информацию, включая IPv6-адрес узла.<br>
Определение способа получения динамической IPv6-адресации зависит от установленных значений флагов, содержащихся в объявлениях маршрутизатора (сообщениях RA).<br>
В предложенном сценарии размеры компании увеличились, и сетевые администраторы больше не имеют возможности назначать IP-адреса для устройств вручную. Ваша задача - настроить маршрутизатор R2 для назначения адресов IPv6 в двух разных подсетях, подключенных к маршрутизатору R1.<br>
Примечание: Маршрутизаторы, используемые в практических лабораторных работах CCNA, - это Cisco 4221 с Cisco IOS XE Release 16.9.4 (образ universalk9). В лабораторных работах используются коммутаторы Cisco Catalyst 2960 с Cisco IOS версии 15.2(2) (образ lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. Правильные идентификаторы интерфейса см. в сводной таблице по интерфейсам маршрутизаторов в конце лабораторной работы.<br>
Примечание. Убедитесь, что у всех маршрутизаторов и коммутаторов была удалена начальная конфигурация. Если вы не уверены в этом, обратитесь к инструктору.<br>
## Необходимые ресурсы
•	2 маршрутизатора (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
•	2 коммутатора (Cisco 2960 с образом lanbasek9 Cisco IOS версии 15.2 (2) или аналогичным) - опционально<br>
•	2 ПК (ОС Windows с программой эмуляции терминалов, такой как Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Инструкции
## Часть 1. Создание сети и настройка основных параметров устройства
В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.
### Шаг 1. Создайте сеть согласно топологии.
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.
### Шаг 2. Настройте базовые параметры каждого коммутатора. (необязательно)
a.	Присвойте коммутатору имя устройства.<br>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Отключите все неиспользуемые порты.<br>
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>

    S1#show startup-config 
    Using 1473 bytes
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
    shutdown
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
    banner motd ^CUnauthorized access is prohibited!
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
    Using 1452 bytes
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
    shutdown
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
    banner motd ^CUnauthorized access is prohibited!^C
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
### Шаг 3. Произведите базовую настройку маршрутизаторов.

a.	Назначьте маршрутизатору имя устройства.<br>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Активация IPv6-маршрутизации<br>
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>

    R1#show startup-config 
    Using 738 bytes
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
    ipv6 unicast-routing
    !
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
    banner motd ^CUnauthorized access is prohibited!^C
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
    
    R2#show startup-config 
    Using 738 bytes
    !
    version 15.4
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
    ipv6 unicast-routing
    !
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
    banner motd ^CUnauthorized access is prohibited!^C
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
### Шаг 4. Настройка интерфейсов и маршрутизации для обоих маршрутизаторов.
a.	Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше.<br>
b.	Настройте маршрут по умолчанию на каждом маршрутизаторе, который указывает на IP-адрес G0/0/0 на другом маршрутизаторе.<br>
c.	Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1<br>
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW8/screen/R1-R2.png)
d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
## Часть 2. Проверка назначения адреса SLAAC от R1
В части 2 вы убедитесь, что узел PC-A получает адрес IPv6 с помощью метода SLAAC.<br>
Включите PC-A и убедитесь, что сетевой адаптер настроен для автоматической настройки IPv6.<br>
Через несколько минут результаты команды ipconfig должны показать, что PC-A присвоил себе адрес из сети 2001:db8:1::/64.<br>

    C:\>ipconfig
    
    FastEthernet0 Connection:(default port)
    
    Connection-specific DNS Suffix..: 
    Link-local IPv6 Address.........: FE80::20D:BDFF:FE00:6722
    IPv6 Address....................: 2001:DB8:ACAD:1:20D:BDFF:FE00:6722
    IPv4 Address....................: 0.0.0.0
    Subnet Mask.....................: 0.0.0.0
    Default Gateway.................: FE80::1
    0.0.0.0

#### Вопрос <br>
Откуда взялась часть адреса с идентификатором хоста?
#### Ответ <br>
При получении ip методом SLAAC используется mac адрес сетевой карты, для обеспечения уникальности ip адреса в сети
## Часть 3. Настройка и проверка сервера DHCPv6 на R1
В части 3 выполняется настройка и проверка состояния DHCP-сервера на R1. Цель состоит в том, чтобы предоставить PC-A информацию о DNS-сервере и домене.
### Шаг 1. Более подробно изучите конфигурацию PC-A.
a.	Выполните команду ipconfig /all на PC-A и посмотрите на результат.

    C:\>ipconfig /all
    
    FastEthernet0 Connection:(default port)
    
       Connection-specific DNS Suffix..: 
       Physical Address................: 000D.BD00.6722
       Link-local IPv6 Address.........: FE80::20D:BDFF:FE00:6722
       IPv6 Address....................: 2001:DB8:ACAD:1:20D:BDFF:FE00:6722
       IPv4 Address....................: 0.0.0.0
       Subnet Mask.....................: 0.0.0.0
       Default Gateway.................: FE80::1
                                         0.0.0.0
       DHCP Servers....................: 0.0.0.0
       DHCPv6 IAID.....................: 
       DHCPv6 Client DUID..............: 00-01-00-01-12-E0-96-5E-00-0D-BD-00-67-22
       DNS Servers.....................: ::
                                         0.0.0.0
    
    Bluetooth Connection:
    
       Connection-specific DNS Suffix..: 
       Physical Address................: 0001.6490.BCB7
       Link-local IPv6 Address.........: ::
       IPv6 Address....................: ::
       IPv4 Address....................: 0.0.0.0
       Subnet Mask.....................: 0.0.0.0
       Default Gateway.................: ::
                                         0.0.0.0
       DHCP Servers....................: 0.0.0.0
       DHCPv6 IAID.....................: 
       DHCPv6 Client DUID..............: 00-01-00-01-12-E0-96-5E-00-0D-BD-00-67-22
       DNS Servers.....................: ::0.0.0.0   

b.	Обратите внимание, что основной DNS-суффикс отсутствует. Также обратите внимание, что предоставленные адреса DNS-сервера являются адресами «локального сайта anycast», а не одноадресные адреса, как ожидалось.
### Шаг 2. Настройте R1 для предоставления DHCPv6 без состояния для PC-A.
a.	Создайте пул DHCP IPv6 на R1 с именем R1-STATELESS. В составе этого пула назначьте адрес DNS-сервера как 2001:db8:acad: :1, а имя домена — как stateless.com.

    R1(config)# ipv6 dhcp pool R1-STATELESS
    R1(config-dhcp)# dns-server 2001:db8:acad::254
    R1(config-dhcp)# domain-name STATELESS.com
b.	Настройте интерфейс G0/0/1 на R1, чтобы предоставить флаг конфигурации OTHER для локальной сети R1 и укажите только что созданный пул DHCP в качестве ресурса DHCP для этого интерфейса.

    R1(config)# interface g0/0/1
    R1(config-if)# ipv6 nd other-config-flag 
    R1(config-if)# ipv6 dhcp server R1-STATELESS
    
c.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
d.	Перезапустите PC-A.<br>
e.	Проверьте вывод ipconfig /all и обратите внимание на изменения.

    C:\> ipconfig /all
    
    FastEthernet0 Connection:(default port)
    
       Connection-specific DNS Suffix..: STATELESS.com 
       Physical Address................: 000D.BD00.6722
       Link-local IPv6 Address.........: FE80::20D:BDFF:FE00:6722
       IPv6 Address....................: 2001:DB8:ACAD:1:20D:BDFF:FE00:6722
       IPv4 Address....................: 0.0.0.0
       Subnet Mask.....................: 0.0.0.0
       Default Gateway.................: FE80::1
                                         0.0.0.0
       DHCP Servers....................: 0.0.0.0
       DHCPv6 IAID.....................: 1653655352
       DHCPv6 Client DUID..............: 00-01-00-01-12-E0-96-5E-00-0D-BD-00-67-22
       DNS Servers.....................: 2001:DB8:ACAD::254
                                         0.0.0.0
    
    Bluetooth Connection:
    
       Connection-specific DNS Suffix..: STATELESS.com 
       Physical Address................: 0001.6490.BCB7
       Link-local IPv6 Address.........: ::
       IPv6 Address....................: ::
       IPv4 Address....................: 0.0.0.0
       Subnet Mask.....................: 0.0.0.0
       Default Gateway.................: ::
                                         0.0.0.0
       DHCP Servers....................: 0.0.0.0
       DHCPv6 IAID.....................: 1653655352
       DHCPv6 Client DUID..............: 00-01-00-01-12-E0-96-5E-00-0D-BD-00-67-22
       DNS Servers.....................: ::
                                         0.0.0.0

f.	Тестирование подключения с помощью пинга IP-адреса интерфейса G0/1 R2.
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW8/screen/PC-A-R2.png)
## Часть 4. Настройка и проверка сервера DHCPv6 на R2
Посклольку в cpt отсутствует ralay, настриваем DHCP на R2
a.	Создайте пул DHCPv6 на R2 для сети 2001:db8:acad:3:aaa::/80. Это предоставит адреса локальной сети, подключенной к интерфейсу G0/0/1 на R2. В составе пула задайте DNS-сервер 2001:db8:acad: :254 и задайте доменное имя STATEFUL.com.

    R2(config)#ipv6 dhcp pool R2-STATEFUL
    R2(config-dhcpv6)#address prefix 2001:db8:acad:3:aaa::/80
    R2(config-dhcpv6)#dns-server 2001:db8:acad::254
    R2(config-dhcpv6)#domain-name STATEFUL.com
b.	Назначьте только что созданный пул DHCPv6 интерфейсу g0/0/1 на R2.

    R2(config)#interface g0/0/1
    R2(config-if)#ipv6 dhcp server R2-STATEFUL
    R2(config-if)#ipv6 nd managed-config-flag
a.	Перезапустите PC-B.<br>
b.	Откройте командную строку на PC-B и выполните команду ipconfig /all и проверьте выходные данные, чтобы увидеть результаты операции получения авдреса по DHCPv6.

       C:\>ipconfig /all
    
    FastEthernet0 Connection:(default port)
    
       Connection-specific DNS Suffix..: STATEFUL.com 
       Physical Address................: 00E0.8FBD.CC5A
       Link-local IPv6 Address.........: FE80::2E0:8FFF:FEBD:CC5A
       IPv6 Address....................: 2001:DB8:ACAD:3:AAA:FC5C:EECA:D027
       IPv4 Address....................: 0.0.0.0
       Subnet Mask.....................: 0.0.0.0
       Default Gateway.................: FE80::1
                                         0.0.0.0
       DHCP Servers....................: 0.0.0.0
       DHCPv6 IAID.....................: 1470462118
       DHCPv6 Client DUID..............: 00-01-00-01-73-5A-A3-20-00-E0-8F-BD-CC-5A
       DNS Servers.....................: 2001:DB8:ACAD::254
                                         0.0.0.0
    
    Bluetooth Connection:
    
       Connection-specific DNS Suffix..: STATEFUL.com 
       Physical Address................: 0002.169A.55B8
       Link-local IPv6 Address.........: ::
       IPv6 Address....................: ::
       IPv4 Address....................: 0.0.0.0
       Subnet Mask.....................: 0.0.0.0
       Default Gateway.................: ::
                                         0.0.0.0
       DHCP Servers....................: 0.0.0.0
       DHCPv6 IAID.....................: 1470462118
       DHCPv6 Client DUID..............: 00-01-00-01-73-5A-A3-20-00-E0-8F-BD-CC-5A
       DNS Servers.....................: ::
                                         0.0.0.0

c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R1 G0/0/1.
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW8/screen/PC-B-R1.png)



# Лабраторная работа - реализация DHCPv4
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW8/screen/TopologyV4.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|Шлюз по умолчанию|
| ------------ | ------------ | ------------ |------------ |------------ |
| R1|G0/0/0|10.0.0.1|255.255.255.252|-|
| R1|G0/0/1|-|-|-|
| R1|G0/0/1.100|192.168.1.1|255.255.255.192|-|
| R1|G0/0/1.200|192.168.1.65|255.255.255.224|-|
| R1|G0/0/1.1000|-|-|-|
| R2|G0/0/0|10.0.0.2|255.255.255.252|-|
| R2|G0/0/1|192.168.1.97|255.255.255.240|-|
| S1|Vlan 200|192.168.1.66|192.168.1.66 255.255.255.224||
| S2|Vlan 1|192.168.1.98|255.255.255.240||
| PC-A|NIC|DHCP|DHCP|DHCP|
| PC-B|NIC|DHCP|DHCP|DHCP|
## Таблица VLAN
|VLAN|Имя|Назначенный интерфейс|
| ------------ | ------------ | ------------ |
| 1| нет | S2: F0/18|
| 100| Клиенты | S1: F0/6 |
| 200| Управление | S1: VLAN 200 |
| 999| Parking_Lot | S1: F0/1-4, F0/7-24, G0/1-2 |
| 1000| Собственная | - |
## Задачи
Часть 1. Создание сети и настройка основных параметров устройства <br>
Часть 2. Настройка и проверка двух серверов DHCPv4 на R1 <br>
Часть 3. Настройка и проверка DHCP-ретрансляции на R2 <br>
## Общие сведения/сценарий
Протокол динамической конфигурации сетевого узла (DHCP) — сетевой протокол, позволяющий сетевым администраторам управлять и автоматизировать назначение IP-адресов. Без использования DHCP  для IPv4 администратору необходимо вручную назначать и настраивать IP-адреса, предпочтительные DNS-серверы и шлюзы по умолчанию. По мере увеличения сети и перемещении устройств из одной внутренней сети в другую это становится административной проблемой.<br>
В предложенном сценарии размеры компании увеличились, и сетевые администраторы больше не имеют возможности назначать IP-адреса для устройств вручную. Ваша задача заключается в настройке маршрутизатора R1 для назначения IPv4-адресов в двух разных подсетях. <br>
## Необходимые ресурсы
●	2 маршрутизатора (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
●	2 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.2(2) (образ lanbasek9) или аналогичная модель)<br>
●	2 ПК (ОС Windows с программой эмуляции терминалов, такой как Tera Term)<br>
●	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
●	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Инструкции
## Часть 1.	Создание сети и настройка основных параметров устройства
В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.
### Шаг 1.	Создание схемы адресации
Подсеть сети 192.168.1.0/24 в соответствии со следующими требованиями:
a.	Одна подсеть «Подсеть A», поддерживающая 58 хостов (клиентская VLAN на R1).<br>
Подсеть A: 192.168.1.0/26<br>
Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.100 . <br>
b.	Одна подсеть «Подсеть B», поддерживающая 28 хостов (управляющая VLAN на R1). <br>
Подсеть B: 192.168.1.64/27<br>
Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.200. Запишите второй IP-адрес в таблице
адресов для S1 VLAN 200 и введите соответствующий шлюз по умолчанию.<br>
c.	Одна подсеть «Подсеть C», поддерживающая 12 узлов (клиентская сеть на R2).<br>
Подсеть C: 192.168.1.96/28<br>
Запишите первый IP-адрес в таблице адресации для R2 G0/0/1.<br>
### Шаг 2.	Создайте сеть согласно топологии.
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.
## Шаг 3.	Произведите базовую настройку маршрутизаторов.
a.	Назначьте маршрутизатору имя устройства.
Откройте окно конфигурации
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
i.	Установите часы на маршрутизаторе на сегодняшнее время и дату.<br>

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
        end
<br>
        
        R2#sho star
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
### Шаг 4.	Настройка маршрутизации между сетями VLAN на маршрутизаторе R1
a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.<br>
b.	Настройте подинтерфейсы для каждой VLAN в соответствии с требованиями таблицы IP-адресации. Все субинтерфейсы используют инкапсуляцию 802.1Q и назначаются первый полезный адрес из вычисленного пула IP-адресов. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.<br>
c.	Убедитесь, что вспомогательные интерфейсы работают.

        	!
        	interface GigabitEthernet0/0/1.100
        	encapsulation dot1Q 100
        	ip address 192.168.1.1 255.255.255.192
        	!
        	interface GigabitEthernet0/0/1.200
        	encapsulation dot1Q 200
        	ip address 192.168.1.65 255.255.255.224
        	!
        	interface GigabitEthernet0/0/1.1000
        	encapsulation dot1Q 1000
        	no ip address
        	!
### Шаг 5.	Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов
a.	Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.<br>
b.	Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации
<br>
c.	Настройте маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом
маршрутизаторе.<br>
d.	Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.<br>
	R1#ping 10.0.0.2
	
	Type escape sequence to abort.
	Sending 5, 100-byte ICMP Echos to 10.0.0.2, timeout is 2 seconds:
	.!!!!
	Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/0 ms
	
e.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

        	R1(config)#do sh star
        	Using 1097 bytes
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
        	ip address 10.0.0.1 255.255.255.252
        	duplex auto
        	speed auto
        	!
        	interface GigabitEthernet0/0/1
        	no ip address
        	duplex auto
        	speed auto
        	!
        	interface GigabitEthernet0/0/1.100
        	encapsulation dot1Q 100
        	ip address 192.168.1.1 255.255.255.192
        	!
        	interface GigabitEthernet0/0/1.200
        	encapsulation dot1Q 200
        	ip address 192.168.1.65 255.255.255.224
        	!
        	interface GigabitEthernet0/0/1.1000
        	encapsulation dot1Q 1000
        	no ip address
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
        	ip route 0.0.0.0 0.0.0.0 10.0.0.2 
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
        	R2(config)#do sh star
        	Using 839 bytes
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
        	ip address 10.0.0.2 255.255.255.252
        	duplex auto
        	speed auto
        	!
        	interface GigabitEthernet0/0/1
        	ip address 192.168.1.97 255.255.255.240
        	duplex auto
        	speed auto
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
        	ip route 0.0.0.0 0.0.0.0 10.0.0.1 
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
### Шаг 6.	Настройте базовые параметры каждого коммутатора.
a.	Присвойте коммутатору имя устройства.<br>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные
команды таким образом, как будто они являются именами узлов.<br>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
f.	Зашифруйте открытые пароли.<br>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.<br>
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>
i.	Установите часы на маршрутизаторе на сегодняшнее время и дату.<br>
j.	Скопируйте текущую конфигурацию в файл загрузочной конфигурации.

        	R1#sh start
        	Using 1097 bytes
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
        	ip address 10.0.0.1 255.255.255.252
        	duplex auto
        	speed auto
        	!
        	interface GigabitEthernet0/0/1
        	no ip address
        	duplex auto
        	speed auto
        	!
        	interface GigabitEthernet0/0/1.100
        	encapsulation dot1Q 100
        	ip address 192.168.1.1 255.255.255.192
        	!
        	interface GigabitEthernet0/0/1.200
        	encapsulation dot1Q 200
        	ip address 192.168.1.65 255.255.255.224
        	!
        	interface GigabitEthernet0/0/1.1000
        	encapsulation dot1Q 1000
        	no ip address
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
        	ip route 0.0.0.0 0.0.0.0 10.0.0.2 
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
<br>
        
        S2#sh start
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
### Шаг 7.	Создайте сети VLAN на коммутаторе S1.
Примечание. S2 настроен только с базовыми настройками.<br>
a.	Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.<br>
b.	Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети,
рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.<br>
c.	Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети,
рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2.<br>
d.	Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа
и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.<br>

        S1(config)#do sh start
        Using 2791 bytes
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
        switchport access vlan 999
        switchport mode access
        shutdown
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
        interface Vlan200
        ip address 192.168.1.66 255.255.255.224
        !
        ip default-gateway 192.168.1.65
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
        
        S2(config)#do sh start
        Using 1508 bytes
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
        shutdown
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
        ip address 192.168.1.98 255.255.255.240
        shutdown
        !
        ip default-gateway 192.168.1.97
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
### Шаг 8.	Назначьте сети VLAN соответствующим интерфейсам коммутатора.
a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для
режима статического доступа.<br>
b.	Убедитесь, что VLAN назначены на правильные интерфейсы.

	S1(config)#interface f0/6
	S1(config-if)#switchport mode access 
	S1(config-if)#switchport access vlan 100
<br>

    S2(config)# int f0/18
    S2(config-if)#switchport mode access 
    S2(config-if)#switchport access vlan 1
### Шаг 9.	Вручную настройте интерфейс S1 F0/5 в качестве транка 802.1Q.
a.	Измените режим порта коммутатора, чтобы принудительно создать магистральный канал.<br>
b.	В рамках конфигурации транка  установите для native  VLAN значение 1000.<br>
c.	В качестве другой части конфигурации магистрали укажите, что VLAN 100, 200 и 1000 могут проходить по
транку.

	S1(config)#interface f0/5
	S1(config-if)#switchport mode trunk 
	S1(config-if)#switchport trunk native vlan 1000
	S1(config-if)#switchport trunk allowed vlan 100,200,1000
d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
e.	Проверьте состояние транка

	S1#show interfaces trunk 
	Port Mode Encapsulation Status Native vlan
	Fa0/5 on 802.1q trunking 1000
	
	Port Vlans allowed on trunk
	Fa0/5 100,200,1000
	
	Port Vlans allowed and active in management domain
	Fa0/5 100,200,1000
	
	Port Vlans in spanning tree forwarding state and not pruned
	Fa0/5 100,200,1000

### Часть 2.	Настройка и проверка двух серверов DHCPv4 на R1
### Шаг 1.	Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. Ниже приведен только пул DHCP
для подсети A
a.	Исключите первые пять используемых адресов из каждого пула адресов.

	R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5
	R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.101
b.	Создайте пул DHCP (используйте уникальное имя для каждого пула).<br>
c.	Укажите сеть, поддерживающую этот DHCP-сервер.<br>
d.	В качестве имени домена укажите CCNA-lab.com.<br>
e.	Настройте соответствующий шлюз по умолчанию для каждого пула DHCP.<br>
f.	Настройте время аренды на 2 дня 12 часов и 30 минут.<br>
g.	Затем настройте второй пул DHCPv4, используя имя пула R2_Client_LAN и вычислите сеть, маршрутизатор
по умолчанию, и используйте то же имя домена и время аренды, что и предыдущий пул DHCP.<br>

	R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5
	R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.101
	R1(config)#ip dhcp pool R1_Client_LAN
	R1(dhcp-config)#network 192.168.1.0 255.255.255.192
	R1(dhcp-config)#domain-name CCNA-lab.com
	R1(dhcp-config)#default-router 192.168.1.1
	R1(dhcp-config)#?
	default-router 
	dns-server 
	domain-name 
	exit 
	network 
	no 
	option 
	R1(dhcp-config)#exit
	R1(config)#ip dhcp pool R2_Client_LAN
	R1(dhcp-config)#network 192.168.1.96 255.255.255.240
	R1(dhcp-config)#domain-name CCNA-lab.com
	R1(dhcp-config)#default-router 192.168.1.97
	R1(dhcp-config)#end
    
Не удалость настроить время аренды, нет нужной команды
### Шаг 2.	Сохраните конфигурацию.
Сохраните текущую конфигурацию в файл загрузочной конфигурации.
### Шаг 3.	Проверка конфигурации сервера DHCPv4
a.	Чтобы просмотреть сведения о пуле, выполните команду show ip dhcp pool .
        
        R1#show ip dhcp pool
        
        Pool R1_Client_LAN :
         Utilization mark (high/low)    : 100 / 0
         Subnet size (first/next)       : 0 / 0 
         Total addresses                : 62
         Leased addresses               : 0
         Excluded addresses             : 2
         Pending event                  : none
        
         1 subnet is currently in the pool
         Current index        IP address range                    Leased/Excluded/Total
         192.168.1.1          192.168.1.1      - 192.168.1.62      0    / 2     / 62
        
        Pool R2_Client_LAN :
         Utilization mark (high/low)    : 100 / 0
         Subnet size (first/next)       : 0 / 0 
         Total addresses                : 14
         Leased addresses               : 0
         Excluded addresses             : 2
         Pending event                  : none
        
         1 subnet is currently in the pool
         Current index        IP address range                    Leased/Excluded/Total
         192.168.1.97         192.168.1.97     - 192.168.1.110     0    / 2     / 14
b.	 Выполните команду show ip dhcp bindings для проверки установленных назначений адресов DHCP.

        R1#show ip dhcp binding 
        IP address       Client-ID/              Lease expiration        Type
                         Hardware address
c.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.

        	R1#show ip dhcp ?
        	binding DHCP address bindings
        	conflict DHCP address conflicts
        	pool DHCP pools information
        	relay Miscellaneous DHCP relay information

Нет нужной команды
### Шаг 4.	Попытка получить IP-адрес от DHCP на PC-A
a.	Из командной строки компьютера PC-A выполните команду ipconfig /all.

        C:\>ipconfig /all
        
        FastEthernet0 Connection:(default port)
        
           Connection-specific DNS Suffix..: 
           Physical Address................: 0000.0CC7.0A60
           Link-local IPv6 Address.........: FE80::200:CFF:FEC7:A60
           IPv6 Address....................: ::
           IPv4 Address....................: 0.0.0.0
           Subnet Mask.....................: 0.0.0.0
           Default Gateway.................: ::
                                             0.0.0.0
           DHCP Servers....................: 0.0.0.0
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BD-4D-96-75-00-00-0C-C7-0A-60
           DNS Servers.....................: ::
                                             0.0.0.0
        
        Bluetooth Connection:
        
           Connection-specific DNS Suffix..: 
           Physical Address................: 0001.97BB.C1D8
           Link-local IPv6 Address.........: ::
           IPv6 Address....................: ::
           IPv4 Address....................: 0.0.0.0
           Subnet Mask.....................: 0.0.0.0
           Default Gateway.................: ::
                                             0.0.0.0
           DHCP Servers....................: 0.0.0.0
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BD-4D-96-75-00-00-0C-C7-0A-60
           DNS Servers.....................: ::
                                             0.0.0.0
b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP
адресе.

        C:\>ipconfig /all
        
        FastEthernet0 Connection:(default port)
        
           Connection-specific DNS Suffix..: CCNA-lab.com
           Physical Address................: 0000.0CC7.0A60
           Link-local IPv6 Address.........: FE80::200:CFF:FEC7:A60
           IPv6 Address....................: ::
           IPv4 Address....................: 192.168.1.6
           Subnet Mask.....................: 255.255.255.192
           Default Gateway.................: ::
                                             192.168.1.1
           DHCP Servers....................: 192.168.1.1
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BD-4D-96-75-00-00-0C-C7-0A-60
           DNS Servers.....................: ::
                                             0.0.0.0
        
        Bluetooth Connection:
        
           Connection-specific DNS Suffix..: CCNA-lab.com
           Physical Address................: 0001.97BB.C1D8
           Link-local IPv6 Address.........: ::
           IPv6 Address....................: ::
           IPv4 Address....................: 0.0.0.0
           Subnet Mask.....................: 0.0.0.0
           Default Gateway.................: ::
                                             0.0.0.0
           DHCP Servers....................: 0.0.0.0
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BD-4D-96-75-00-00-0C-C7-0A-60
           DNS Servers.....................: ::
                                             0.0.0.0
c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R0 G0/0/1.

        C:\>ping 192.168.1.1
        
        Pinging 192.168.1.1 with 32 bytes of data:
        
        Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
        Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
        Reply from 192.168.1.1: bytes=32 time=2ms TTL=255
        Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

## Часть 3.	Настройка и проверка DHCP-ретрансляции на R2
В части 3 настраивается R2 для ретрансляции DHCP-запросов из локальной сети на интерфейсе G0/0/1 на DHCP
сервер (R1).<br>
### Шаг 1.	Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1
a.	Настройте команду ip helper-address на G0/0/1, указав IP-адрес G0/0/0 R1.
	
    R2(config)#interface g 0/0/1
	R2(config-if)#ip helper-address 10.0.0.1
b.	Сохраните конфигурацию.
### Шаг 2.	Попытка получить IP-адрес от DHCP на PC-B
a.	Из командной строки компьютера PC-B выполните команду ipconfig /all.

        C:\>ipconfig /all
        
        FastEthernet0 Connection:(default port)
        
           Connection-specific DNS Suffix..: 
           Physical Address................: 0060.3E3D.C41D
           Link-local IPv6 Address.........: FE80::260:3EFF:FE3D:C41D
           IPv6 Address....................: ::
           IPv4 Address....................: 0.0.0.0
           Subnet Mask.....................: 0.0.0.0
           Default Gateway.................: ::
                                             0.0.0.0
           DHCP Servers....................: 0.0.0.0
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BE-51-78-CA-00-60-3E-3D-C4-1D
           DNS Servers.....................: ::
                                             0.0.0.0
        
        Bluetooth Connection:
        
           Connection-specific DNS Suffix..: 
           Physical Address................: 00E0.B0A3.CAAC
           Link-local IPv6 Address.........: ::
           IPv6 Address....................: ::
           IPv4 Address....................: 0.0.0.0
           Subnet Mask.....................: 0.0.0.0
           Default Gateway.................: ::
                                             0.0.0.0
           DHCP Servers....................: 0.0.0.0
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BE-51-78-CA-00-60-3E-3D-C4-1D
           DNS Servers.....................: ::
                                             0.0.0.0
b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP
адресе.

        C:\>ipconfig /all
        
        FastEthernet0 Connection:(default port)
        
           Connection-specific DNS Suffix..: CCNA-lab.com
           Physical Address................: 0060.3E3D.C41D
           Link-local IPv6 Address.........: FE80::260:3EFF:FE3D:C41D
           IPv6 Address....................: ::
           IPv4 Address....................: 192.168.1.102
           Subnet Mask.....................: 255.255.255.240
           Default Gateway.................: ::
                                             192.168.1.97
           DHCP Servers....................: 10.0.0.1
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BE-51-78-CA-00-60-3E-3D-C4-1D
           DNS Servers.....................: ::
                                             0.0.0.0
        
        Bluetooth Connection:
        
           Connection-specific DNS Suffix..: CCNA-lab.com
           Physical Address................: 00E0.B0A3.CAAC
           Link-local IPv6 Address.........: ::
           IPv6 Address....................: ::
           IPv4 Address....................: 0.0.0.0
           Subnet Mask.....................: 0.0.0.0
           Default Gateway.................: ::
                                             0.0.0.0
           DHCP Servers....................: 0.0.0.0
           DHCPv6 IAID.....................: 
           DHCPv6 Client DUID..............: 00-01-00-01-BE-51-78-CA-00-60-3E-3D-C4-1D
           DNS Servers.....................: ::
                                             0.0.0.0
c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R1 G0/0/1.

    C:\>ping 192.168.1.1
    
    Pinging 192.168.1.1 with 32 bytes of data:
    
    Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
    Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
    Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
    Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
    
    Ping statistics for 192.168.1.1:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 0ms, Average = 0ms
d.	Выполните show ip dhcp binding для R1 для проверки назначений адресов в DHCP.
        
        R1#show ip dhcp binding 
        IP address       Client-ID/              Lease expiration        Type
                         Hardware address
        192.168.1.6      0000.0CC7.0A60           --                     Automatic
        192.168.1.102    0060.3E3D.C41D           --                     Automatic
e.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.<br>
Нет необходимой команды
