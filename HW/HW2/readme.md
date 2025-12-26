# Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/Topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|
| ------------ | ------------ | ------------ |------------ |
| S1|Vlan 1|192.168.1.2|255.255.255.0|
| S2|Vlan 1|192.168.1.2|255.255.255.0|
| PC-A|NIC|192.168.1.10|255.255.255.0|
| PC-B|NIC|192.168.1.10|255.255.255.0|
## Цели
#### Часть 1. Создание и настройка сети
#### Часть 2. Изучение таблицы МАС-адресов коммутатора
## Общие сведения/сценарий
Коммутатор локальной сети на уровне 2 предназначен для доставки кадров Ethernet всем узловым устройствам в локальной сети (LAN). Он записывает МАС-адреса узлов, отображаемые в сети, и сопоставляет их с собственными портами коммутатора Ethernet. Этот процесс называется созданием таблицы МАС-адресов. Получив кадр от ПК, коммутатор изучает МАС-адреса источника и назначения кадра. MAC-адрес источника регистрируется и сопоставляется с портом коммутатора, от которого он был получен. Затем по таблице MAC-адресов определяется МАС-адрес назначения. Если MAC-адрес назначения известен, кадр пересылается через соответствующий порт коммутатора, связанный с этим MAC-адресом. Если MAC-адрес неизвестен, то кадр отправляется по широковещательной рассылке через все порты коммутатора, кроме того, через который он был получен. Важно видеть и понимать работу коммутатора и то, как он осуществляет передачу данных по сети. Понимание функционала коммутатора особенно важно для сетевых администраторов, задача которых заключается в обеспечении безопасной и стабильной работы сети.<br>
Коммутаторы используются для соединения компьютеров в локальных сетях (LAN) и передачи данных между ними. Коммутаторы отправляют кадры Ethernet на узловые устройства, которые идентифицируются по МАС-адресам сетевых плат.<br>
В части 1 вам нужно построить топологию, состоящую из двух коммутаторов. В части 2 вам предстоит отправить эхо-запросы различным устройствам и посмотреть, как два коммутатора строят свои таблицы МАС-адресов.<br>
Примечание. В лабораторной работе используются коммутаторы Cisco Catalyst 2960s с операционной системой Cisco IOS 15.2(2) (образ lanbasek9). Допускается использование других моделей коммутаторов и других версий Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах.<br>
## Необходимые ресурсы
•	2 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.2(2) (образ lanbasek9) или аналогичная модель)<br>
•	2 ПК (Windows и программа эмуляции терминала, такая как Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
Примечание. Интерфейсы Fast Ethernet на коммутаторах Cisco 2960 определяют тип подключения автоматически, поэтому между коммутаторами S1 и S2 можно использовать прямой кабель Ethernet. При использовании коммутатора Cisco другой модели может потребоваться перекрестный кабель Ethernet.<br>
## Инструкции
## Часть 1. Создание и настройка сети
### Шаг 1. Подключите сеть в соответствии с топологией.
### Шаг 2. Настройте узлы ПК.
### Шаг 3. Выполните инициализацию и перезагрузку коммутаторов.
### Шаг 4. Настройте базовые параметры каждого коммутатора.<br>
a.	Настройте имена устройств в соответствии с топологией.<br>
b.	Настройте IP-адреса, как указано в таблице адресации.<br>
c.	Назначьте cisco в качестве паролей консоли и VTY.<br>
d.	Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.<br>
## S1
    S1#show running-config 
    Building configuration...
    
    Current configuration : 1193 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname S1
    !
    enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
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
    ip address 192.168.1.11 255.255.255.0
    !
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
## S2
    S2#show running-config 
    Building configuration...
    
    Current configuration : 1193 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname S2
    !
    enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
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
    ip address 192.168.1.12 255.255.255.0
    !
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
## PC-A
![PC-A](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/PC-A.png)
## PC-B
![PC-B](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/PC-B.png)
## Часть 2. Изучение таблицы МАС-адресов коммутатора
Как только между сетевыми устройствами начинается передача данных, коммутатор выясняет МАС-адреса и строит таблицу.
### Шаг 1. Запишите МАС-адреса сетевых устройств.
a.	Откройте командную строку на PC-A и PC-B и введите команду ipconfig /all.
![IpconfigPC-A.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/Ipconfig%20PC-A.png)
![IpconfigPC-B.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/Ipconfig%20PC-B.png)
#### Вопрос <br>
Назовите физические адреса адаптера Ethernet.<br>
#### Ответ <br>
MAC-адрес компьютера PC-A: 0009.7C11.8E32<br>
MAC-адрес компьютера PC-B: 0006.2A7E.0538<br>

b.	Подключитесь к коммутаторам S1 и S2 через консоль и введите команду show interface F0/1 на каждом коммутаторе.<br>
## S1
![S1FE01.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/S1FE01.png)
## S2
![S2FE01.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/S2FE01.png)
#### Вопрос <br>
Назовите адреса оборудования во второй строке выходных данных команды (или зашитый адрес — bia).<br>
#### Ответ <br>
МАС-адрес коммутатора S1 Fast Ethernet 0/1: 00d0.ba8d.7a01<br>
МАС-адрес коммутатора S2 Fast Ethernet 0/1: 00d0.ff23.6601<br>
### Шаг 2. Просмотрите таблицу МАС-адресов коммутатора.
Подключитесь к коммутатору S2 через консоль и просмотрите таблицу МАС-адресов до и после тестирования сетевой связи с помощью эхо-запросов.<br>

a. Подключитесь к коммутатору S2 через консоль и войдите в привилегированный режим EXEC.<br>

b. В привилегированном режиме EXEC введите команду show mac address-table и нажмите клавишу ввода.<br>

S2# show mac address-table<br>

Даже если сетевая коммуникация в сети не происходила (т. е. если команда ping не отправлялась), коммутатор может узнать МАС-адреса при подключении к ПК и другим коммутаторам.<br>

        S2#show mac address-table 
        Mac Address Table
        -------------------------------------------
        
        Vlan Mac Address Type Ports
        ---- ----------- -------- -----
        
        1 0009.7c11.8e32 DYNAMIC Fa0/1
        1 00d0.ba8d.7a01 DYNAMIC Fa0/1
#### Вопросы <br>
Записаны ли в таблице МАС-адресов какие-либо МАС-адреса?<br>
Какие МАС-адреса записаны в таблице? <br>
С какими портами коммутатора они сопоставлены и каким устройствам принадлежат?.<br>
#### Ответ <br>
В таблице записаны МАС-адреса S1 и PC-A. Они сопоставлены с портом коммутатора Fa0/1<br>
#### Вопрос <br>
Если вы не записали МАС-адреса сетевых устройств в шаге 1, как можно определить, каким устройствам принадлежат МАС-адреса, используя только выходные данные команды show mac address-table<br>
Работает ли это решение в любой ситуации?<br>
#### Ответ <br>
Можно использовать сопоставление портов, в нашем случае к порту fa0/1 подключен коммутатор и еще одно устройство за ним. Можно попробовать узнать устройство по трем первым байтам MAC в интернете. Эти решения работают не во всех ситуациях

### Шаг 3. Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.
a.	В привилегированном режиме EXEC введите команду clear mac address-table dynamic и нажмите клавишу Enter.<br>

S2# clear mac address-table dynamic<br>

b.	Снова быстро введите команду show mac address-table.<br>

    S2#clear mac address-table dynamic
    S2#show mac address-table
    Mac Address Table
    -------------------------------------------
    
    Vlan Mac Address Type Ports
    ---- ----------- -------- -----
    
    S2#
#### Вопрос <br>
Указаны ли в таблице МАС-адресов адреса для VLAN 1? Указаны ли другие МАС-адреса?<br>
#### Ответ <br>
Нет, таблица МАС-адресов пуста<br>

Через 10 секунд введите команду show mac address-table и нажмите клавишу ввода.<br>

    S2#show mac address-table
    Mac Address Table
    -------------------------------------------
    
    Vlan Mac Address Type Ports
    ---- ----------- -------- -----
    
    1 00d0.ba8d.7a01 DYNAMIC Fa0/1

#### Вопрос <br> 
Появились ли в таблице МАС-адресов новые адреса?<br>
#### Ответ <br>
Появился МАС-адрес свича S1<br>
### Шаг 4. С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.
a.	На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.

#### Вопрос <br>
Не считая адресов многоадресной и широковещательной рассылки, сколько пар IP- и МАС-адресов устройств было получено через протокол ARP?<br>
#### Ответ <br>
Адреса не были получены<br>

        C:\>arp -a
        No ARP Entries Found
b.	Из командной строки PC-B отправьте эхо-запросы на компьютер PC-A, а также коммутаторы S1 и S2.<br>
![Ping.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW2/screen/ping.png)
c.	Подключившись через консоль к коммутатору S2, введите команду show mac address-table.

    S2#show mac address-table 
    Mac Address Table
    -------------------------------------------
    
    Vlan Mac Address Type Ports
    ---- ----------- -------- -----
    
    1 0001.c70d.d6d0 DYNAMIC Fa0/1
    1 0006.2a7e.0538 DYNAMIC Fa0/18
    1 0009.7c11.8e32 DYNAMIC Fa0/1
    1 00d0.ba8d.7a01 DYNAMIC Fa0/1
    S2#
    
#### Вопрос <br>
Добавил ли коммутатор в таблицу МАС-адресов дополнительные МАС-адреса? Если да, то какие адреса и устройства?<br>
#### Ответ <br>
Появился МАС-адрес компьютеров PC-A и PC-Bа также MAC-адрес интерфейса Vlan 1 свича S1<br>
На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.
      
        C:\>arp -a
        Internet Address Physical Address Type
        192.168.1.1 0009.7c11.8e32 dynamic
        192.168.1.11 0001.c70d.d6d0 dynamic
        192.168.1.12 0030.a329.5c36 dynamic
#### Вопрос <br>
Появились ли в ARP-кэше компьютера PC-B дополнительные записи для всех сетевых устройств, которым были отправлены эхо-запросы?<br>
#### Ответ <br>
Да, появились<br>

### Вопрос для повторения
В сетях Ethernet данные передаются на устройства по соответствующим МАС-адресам. Для этого коммутаторы и компьютеры динамически создают ARP-кэш и таблицы МАС-адресов. Если компьютеров в сети немного, эта процедура выглядит достаточно простой. Какие сложности могут возникнуть в крупных сетях?<br>

#### Ответ <br>
В крупных сетях много устройств выполняют ARP, что создает нагрузку на сеть. Также при очень большом количестве хостов может переполняться таблица MAC-адресов коммутатора.
