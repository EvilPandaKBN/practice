# Лабораторная работа. Развертывание коммутируемой сети с резервными каналами
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW7/screen/topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|
| ------------ | ------------ | ------------ |------------ |
| S1|Vlan 1|192.168.1.1|255.255.255.0|
| S2|Vlan 1|192.168.1.2|255.255.255.0|
| S3|Vlan 1|192.168.1.3|255.255.255.0|
## Цели
#### Часть 1. Создание сети и настройка основных параметров устройства
#### Часть 2. Выбор корневого моста
#### Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
#### Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов
## Общие сведения/сценарий
Избыточность позволяет увеличить доступность устройств в топологии сети за счёт устранения единой точки отказа. Избыточность в коммутируемой сети обеспечивается посредством использования нескольких коммутаторов или нескольких каналов между коммутаторами. Когда в проекте сети используется физическая избыточность, возможно возникновение петель и дублирование кадров.<br>
Протокол spanning-tree (STP) был разработан как механизм предотвращения возникновения петель на 2-м уровне для избыточных каналов коммутируемой сети. Протокол STP обеспечивает наличие только одного логического пути между всеми узлами назначения в сети путем намеренного блокирования резервных путей, которые могли бы вызвать петлю.<br>
В этой лабораторной работе команда show spanning-tree используется для наблюдения за процессом выбора протоколом STP корневого моста. Также вы будете наблюдать за процессом выбора портов с учетом стоимости и приоритета.<br>
Примечание. Используются коммутаторы Cisco Catalyst 2960s с Cisco IOS версии 15.0(2) (образ lanbasek9). Допускается использование других моделей коммутаторов и других версий Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. <br>
Примечание. Убедитесь, что все настройки коммутатора удалены и загрузочная конфигурация отсутствует. Если вы не уверены, обратитесь к инструктору.<br>
## Необходимые ресурсы
•	3 коммутатора (Cisco 2960 с операционной системой Cisco IOS 15.0(2) (образ lanbasek9) или аналогичная модель)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Часть 1:	Создание сети и настройка основных параметров устройства
В части 1 вам предстоит настроить топологию сети и основные параметры маршрутизаторов.
### Шаг 1:	Создайте сеть согласно топологии.
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.
### Шаг 2:	Выполните инициализацию и перезагрузку коммутаторов.
### Шаг 3:	Настройте базовые параметры каждого коммутатора.
a.	Отключите поиск DNS.<br>
b.	Присвойте имена устройствам в соответствии с топологией.<br>
c.	Назначьте class в качестве зашифрованного пароля доступа к привилегированному режиму.<br>
d.	Назначьте cisco в качестве паролей консоли и VTY и активируйте вход для консоли и VTY каналов.<br>
e.	Настройте logging synchronous для консольного канала.<br>
f.	Настройте баннерное сообщение дня (MOTD) для предупреждения пользователей о запрете несанкционированного доступа.<br>
g.	Задайте IP-адрес, указанный в таблице адресации для VLAN 1 на всех коммутаторах.<br>
h.	Скопируйте текущую конфигурацию в файл загрузочной конфигурации.<br>

    S1(config)#do sh run
    Building configuration...
    
    Current configuration : 1249 bytes
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
    ip address 192.168.1.1 255.255.255.0
    !
    banner motd ^C
    Warning! Unauthorized access prohibited
    ^C
    !
    !
    !
    line con 0
    password cisco
    logging synchronous
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

    S2#sh startup-config 
    Using 1250 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    no service password-encryption
    !
    hostname S2
    !
    enable password class
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
    ip address 192.168.1.2 255.255.255.0
    !
    banner motd ^C
    Warning! Unauthorized access prohibited.
    ^C
    !
    !
    !
    line con 0
    password cisco
    logging synchronous
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

    S3#sh startup-config 
    Using 1260 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    no service password-encryption
    !
    hostname S3
    !
    enable password class
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
    ip address 192.168.1.3 255.255.255.0
    !
    banner motd ^C
    Warning! Unauthorized access prohibited.
    ^C
    !
    !
    !
    line con 0
    password cisco
    logging synchronous
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
### Шаг 4:	Проверьте связь.
Проверьте способность компьютеров обмениваться эхо-запросами.<br>
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW7/screen/S1-S2.png)
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW7/screen/S1-S3.png)<br>
Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3? 
![](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW7/screen/S2-S3.png)<br>
Выполняйте отладку до тех пор, пока ответы на все вопросы не будут положительными.
## Часть 2:	Определение корневого моста
Для каждого экземпляра протокола spanning-tree (коммутируемая сеть LAN или широковещательный домен) существует коммутатор, выделенный в качестве корневого моста. Корневой мост служит точкой привязки для всех расчётов протокола spanning-tree, позволяя определить избыточные пути, которые следует заблокировать<br>
Процесс выбора определяет, какой из коммутаторов станет корневым мостом. Коммутатор с наименьшим значением идентификатора моста (BID) становится корневым мостом. Идентификатор BID состоит из значения приоритета моста, расширенного идентификатора системы и MAC-адреса коммутатора. Значение приоритета может находиться в диапазоне от 0 до 65535 с шагом 4096. По умолчанию используется значение 32768.
### Шаг 1:	Отключите все порты на коммутаторах.
### Шаг 2:	Настройте подключенные порты в качестве транковых.
### Шаг 3:	Включите порты F0/2 и F0/4 на всех коммутаторах.
### Шаг 4:	Отобразите данные протокола spanning-tree.
Введите команду show spanning-tree на всех трех коммутаторах. Приоритет идентификатора моста рассчитывается путем сложения значений приоритета и расширенного идентификатора системы. Расширенным идентификатором системы всегда является номер сети VLAN. В примере ниже все три коммутатора имеют равные значения приоритета идентификатора моста (32769 = 32768 + 1, где приоритет по умолчанию = 32768, номер сети VLAN = 1); следовательно, коммутатор с самым низким значением MAC-адреса становится корневым мостом (в примере — S2).

    S1#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        19
                 Port        4(FastEthernet0/4)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0005.5E4C.BECB
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Altn BLK 19        128.2    P2p
    Fa0/4            Root FWD 19        128.4    P2pS2# 
<br>
    
    S2#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        19
                 Port        4(FastEthernet0/4)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0002.4AA9.DA54
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Desg FWD 19        128.2    P2p
    Fa0/4            Root FWD 19        128.4    P2p
<br>
    
    S3#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 This bridge is the root
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0001.4204.DDCE
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Desg FWD 19        128.2    P2p
    Fa0/4            Desg FWD 19        128.4    P2p

В схему ниже запишите роль и состояние (Sts) активных портов на каждом коммутаторе в топологии.
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW7/screen/topology.png)<br>
С учетом выходных данных, поступающих с коммутаторов, ответьте на следующие вопросы.
#### Вопрос <br>
Какой коммутатор является корневым мостом? S3
#### Ответ <br>
S3
#### Вопрос <br>
Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?
#### Ответ <br>
Так как на коммутаторах одинаковый приоритет, был выбран коммутатор с наименьшим MAC адресом
#### Вопрос <br>
Какие порты на коммутаторе являются корневыми портами? 
#### Ответ <br>
S1 Fa0/4 и S2 Fa0/4
#### Вопрос <br>
Какие порты на коммутаторе являются назначенными портами?
#### Ответ <br>
? S3 Fa0/2 Fa0/4, S2 Fa0/2
#### Вопрос <br>
Какой порт отображается в качестве альтернативного и в настоящее время заблокирован?
#### Ответ <br>
S1 Fa0/2
#### Вопрос <br>
Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта
#### Ответ <br>
Так как при определении BID прочие параметры равны, то был заблокирован порт на коммутаторе  с наибольшим MAC адресом (сравнение шло между S1 F0/2 и S2 F0/2, поскольку другие порты корневые)
## Часть 3:	Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
Алгоритм протокола spanning-tree (STA) использует корневой мост как точку привязки, после чего определяет, какие порты будут заблокированы, исходя из стоимости пути. Порт с более низкой стоимостью пути является предпочтительным. Если стоимости портов равны, процесс сравнивает BID. Если BID равны, для определения корневого моста используются приоритеты портов. Наиболее низкие значения являются предпочтительными. В части 3 вам предстоит изменить стоимость порта, чтобы определить, какой порт будет заблокирован протоколом spanning-tree.
### Шаг 1:	Определите коммутатор с заблокированным портом.

     S1#show spanning-tree 
     VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        19
                 Port        4(FastEthernet0/4)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0005.5E4C.BECB
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Altn BLK 19        128.2    P2p
    Fa0/4            Root FWD 19        128.4    P2pS2# 
<br>
    
    S2#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        19
                 Port        4(FastEthernet0/4)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0002.4AA9.DA54
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Desg FWD 19        128.2    P2p
    Fa0/4            Root FWD 19        128.4    P2p

Заблокирован порт Fa0/2 на коммутаторе S1
### Шаг 2:	Измените стоимость порта.
Помимо заблокированного порта, единственным активным портом на этом коммутаторе является порт, выделенный в качестве порта корневого моста. Уменьшите стоимость этого порта корневого моста до 18, выполнив команду spanning-tree vlan 1 cost 18 режима конфигурации интерфейса.

    S1(config)# interface f0/2
    S1(config-if)# spanning-tree vlan 1 cost 18
### Шаг 3:	Просмотрите изменения протокола spanning-tree.
Повторно выполните команду show spanning-tree на обоих коммутаторах некорневого моста. Обратите внимание, что ранее заблокированный порт (S1 – F0/4) теперь является назначенным портом, и протокол spanning-tree теперь блокирует порт на другом коммутаторе некорневого моста.

    S1#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        18
                 Port        4(FastEthernet0/4)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0005.5E4C.BECB
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Desg LRN 19        128.2    P2p
    Fa0/4            Root FWD 18        128.4    P2p
<br>

    S2#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        19
                 Port        4(FastEthernet0/4)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0002.4AA9.DA54
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Altn BLK 19        128.2    P2p
    Fa0/4            Root FWD 19        128.4    P2p

#### Вопрос <br>
Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе? 
#### Ответ <br>
Потому что на сегменте сети S1-S2 коммутатор S1 предлагает стоимость маршрута до корневого коммутатора 18, а S2 19
### Шаг 4:	Удалите изменения стоимости порта.
a.	Выполните команду no spanning-tree vlan 1 cost 18 режима конфигурации интерфейса, чтобы удалить запись стоимости, созданную ранее.

    S1(config)# interface f0/2
    S1(config-if)# no spanning-tree vlan 1 cost 18
    
b.	Повторно выполните команду show spanning-tree, чтобы подтвердить, что протокол STP сбросил порт на коммутаторе некорневого моста, вернув исходные настройки порта. Протоколу STP требуется примерно 30 секунд, чтобы завершить процесс перевода порта.

    S1#show spanning-tree 
    VLAN0001
    Spanning tree enabled protocol ieee
    Root ID Priority 32769
    Address 0001.4204.DDCE
    Cost 19
    Port 4(FastEthernet0/4)
    Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
    
    Bridge ID Priority 32769 (priority 32768 sys-id-ext 1)
    Address 0005.5E4C.BECB
    Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
    Aging Time 20
    
    Interface Role Sts Cost Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2 Altn BLK 19 128.2 P2p
    Fa0/4 Root FWD 19 128.4 P2p
## Часть 4:	Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов
Если стоимости портов равны, процесс сравнивает BID. Если BID равны, для определения корневого моста используются приоритеты портов. Значение приоритета по умолчанию — 128. STP объединяет приоритет порта с номером порта, чтобы разорвать связи. Наиболее низкие значения являются предпочтительными. В части 4 вам предстоит активировать избыточные пути до каждого из коммутаторов, чтобы просмотреть, каким образом протокол STP выбирает порт с учетом приоритета портов.<br>
a.	Включите порты F0/1 и F0/3 на всех коммутаторах.<br>
b.	Подождите 30 секунд, чтобы протокол STP завершил процесс перевода порта, после чего выполните команду show spanning-tree на коммутаторах некорневого моста. Обратите внимание, что порт корневого моста переместился на порт с меньшим номером, связанный с коммутатором корневого моста, и заблокировал предыдущий порт корневого моста.<br>

    S1#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        19
                 Port        3(FastEthernet0/3)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0005.5E4C.BECB
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/1            Altn BLK 19        128.1    P2p
    Fa0/3            Root FWD 19        128.3    P2p
    Fa0/2            Altn BLK 19        128.2    P2p
    Fa0/4            Altn BLK 19        128.4    P2p

  <br>
  
    S2#show spanning-tree 
    VLAN0001
      Spanning tree enabled protocol ieee
      Root ID    Priority    32769
                 Address     0001.4204.DDCE
                 Cost        19
                 Port        3(FastEthernet0/3)
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     0002.4AA9.DA54
                 Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  20
    
    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa0/2            Desg FWD 19        128.2    P2p
    Fa0/3            Root FWD 19        128.3    P2p
    Fa0/4            Altn BLK 19        128.4    P2p
    Fa0/1            Desg FWD 19        128.1    P2p

#### Вопрос <br>
Какой порт выбран протоколом STP в качестве порта корневого моста на каждом коммутаторе некорневого
моста?  
#### Ответ <br>
S1 Fa0/3 и S2 Fa0/3
#### Вопрос <br>
очему протокол STP выбрал эти порты в качестве портов корневого моста на этих коммутаторах? 
#### Ответ <br>
У нас есть по два линка от коммутаторов S1 и S2 корневому коммутатору S3, и посколькольку при попарном
сравнении данные равны, были выбраны линки с наименьшими номерами портов
## Вопросы для повторения
#### Вопрос <br>
Какое значение протокол STP использует первым после выбора корневого моста, чтобы определить выбор порта? 
#### Ответ <br>
Root Path Cost
#### Вопрос <br>
Если первое значение на двух портах одинаково, какое следующее значение будет использовать протокол STP
при выборе порта?
#### Ответ <br>
Bridge ID
#### Вопрос <br>
Если оба значения на двух портах равны, каким будет следующее значение, которое использует протокол STP
при выборе порта? 
#### Ответ <br>
Port ID
