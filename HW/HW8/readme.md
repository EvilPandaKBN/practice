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
