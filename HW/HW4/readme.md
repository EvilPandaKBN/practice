# Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW4/screen/topology.png)
## Таблица адресации
|Устройство|Интерфейс|IPv6-адрес|Link local IPv6-адрес|Длина префикса|Шлюз по умолчанию|
| ------------ | ------------ | ------------ |------------ |------------ |------------ |
|R1|G0/0/0|2001:db8:acad:a::1|fe80::1|64|-|
|R1|G0/0/1|2001:db8:acad:1::1|fe80::1|64|-|
|S1|Vlan 1|2001:db8:acad:1::b|fe80::b|64|-|
| PC-A|NIC|2001:db8:acad:1::3|SLACC|64|fe80::1|
| PC-B|NIC|2001:db8:acad:a::3|SLACC|64|fe80::1|
## Задачи
#### Часть 1. Ручная настройка IPv6-адресов
#### Часть 2. Ручная настройка IPv6-адресов
#### Часть 3. Проверка сквозного соединения
## Общие сведения/сценарий
В этой лабораторной работе  вы будете настраивать хосты и интерфейсы устройств с IPv6-адресами.  Для просмотра индивидуальных и групповых IPv6-адресов вы будете использовать команду show. Вы также будете проверять сквозное соединение с помощью команд ping and traceroute.<br>
Примечание: Маршрутизаторы, используемые в практических лабораторных работах CCNA, - это Cisco 4221 с Cisco IOS XE Release 16.9.4 (образ universalk9). В лабораторных работах используются коммутаторы Cisco Catalyst 2960 с Cisco IOS версии 15.2(2) (образ lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. Правильные идентификаторы интерфейса см. в сводной таблице по интерфейсам маршрутизаторов в конце лабораторной работы.<br>
Примечание:  Убедитесь, что у всех маршрутизаторов и коммутаторов была удалена начальная конфигурация. Если вы не уверены, обратитесь к инструктору.<br>
Примечание. Шаблон по умолчанию менеджера базы данных 2960 Switch Database Manager (SDM) не поддерживает IPv6. Перед назначением IPv6-адреса SVI VLAN 1 может понадобиться выполнение команды sdm prefer dual-ipv4-and-ipv6 default для включения IPv6-адресации.<br>
Примечание. Шаблон default bias, который по умолчанию используется диспетчером SDM (диспетчер базы данных коммутатора), не предоставляет возможностей адресации IPv6. Убедитесь, что SDM использует шаблон dual-ipv4-and-ipv6 или lanbase-routing. Новый шаблон будет использоваться после перезагрузки. <br>

    S1# show sdm prefer
Чтобы установить шаблон dual-ipv4-and-ipv6 в качестве шаблона SDM по умолчанию, выполните следующие действия:

    S1# configure terminal
    S1(config)# sdm prefer dual-ipv4-and-ipv6 default
    S1(config)# end
    S1# reload
## Необходимые ресурсы
•	1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
•	1 коммутатор (Cisco 2960 с ПО Cisco IOS версии 15.2(2) с образом lanbasek9 или аналогичная модель)<br>
•	2 ПК (Windows и программа эмуляции терминала, такая как Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Инструкции
## Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
### Шаг 1. Шаг 1. Настройте маршрутизатор.
Назначьте имя хоста и настройте основные параметры устройства.

        Current configuration : 1165 bytes
        !
        version 15.0
        no service timestamps log datetime msec
        no service timestamps debug datetime msec
        no service password-encryption
        !
        hostname S1
        !
        !
        enable password cisco
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
        !
        !
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
        end

### Шаг 2. Настройте коммутатор.
Назначьте имя хоста и настройте основные параметры устройства.

    Current configuration : 707 bytes
    !
    version 16.6.4
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    no service password-encryption
    !
    hostname R1
    !
    !
    !
    enable password cisco
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
    !
    !
    !
    !
    !
    line con 0
    password cisco
    login
    !
    line aux 0
    !
    line vty 0 4
    password cisco
    login
    !
    !
    !
    end

## Часть 2. Ручная настройка IPv6-адресов
### Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1.
a.	Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.

a.	Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.
       
        R1(config)#interface gigabitEthernet 0/0/0
        R1(config-if)#ipv6 address 2001:db8:acad:a::1/64
        R1(config-if)#no sh
        R1(config)#interface gigabitEthernet 0/0/1
        R1(config-if)#ipv6 address 2001:db8:acad:1::1/64 
        R1(config-if)#no sh

Введите команду show ipv6 interface brief, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.

    R1# show ipv6 interface brief
    GigabitEthernet0/0/0 [up/up]
        FE80::207:ECFF:FEA5:4701
        2001:DB8:ACAD:A::1
    GigabitEthernet0/0/1 [up/up]
        FE80::207:ECFF:FEA5:4702
        2001:DB8:ACAD:1::1
    GigabitEthernet0/0/2 [administratively down/down]
        unassigned
    Vlan1 [administratively down/down]
        unassigned
b.	Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введите локальные адреса канала на каждом интерфейсе Ethernet на R1.
c.	Используйте выбранную команду, чтобы убедиться, что локальный адрес связи изменен на fe80::1.<br>

    R1#show ipv6 interface brief 
    GigabitEthernet0/0/0       [up/up]
        FE80::1
        2001:DB8:ACAD:A::1
    GigabitEthernet0/0/1       [up/up]
        FE80::1
        2001:DB8:ACAD:1::1
    GigabitEthernet0/0/2       [administratively down/down]
        unassigned
    Vlan1                      [administratively down/down]
        unassigned
