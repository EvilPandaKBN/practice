# Лабораторная работа. Доступ к сетевым устройствам по протоколу SSH
## Топология
![Топология](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW5/screen/Topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес|Маска подсети|Шлюз по умолчанию|
| ------------ | ------------ | ------------ |------------ |------------ |
| R1|G0/0/1|192.168.1.1|255.255.255.0|-|
| S1|Vlan 1|192.168.1.11|255.255.255.0|192.168.1.1|
| PC-A|NIC|192.168.1.3|255.255.255.0|192.168.1.1|
## Задачи
#### Часть 1. Настройка основных параметров устройства
#### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
#### Часть 3. Настройка коммутатора для доступа по протоколу SSH
#### Часть 4. SSH через интерфейс командной строки (CLI) коммутатора
## Общие сведения/сценарий
Раньше для удаленной настройки сетевых устройств в основном применялся протокол Telnet. Однако он не обеспечивает шифрование информации, передаваемой между клиентом и сервером, что позволяет анализаторам сетевых пакетов перехватывать пароли и данные конфигурации.<br>
Secure Shell (SSH) — это сетевой протокол, устанавливающий безопасное подключение с эмуляцией терминала к маршрутизатору или иному сетевому устройству. Протокол SSH шифрует все сведения, которые поступают по сетевому каналу, и предусматривает аутентификацию удаленного компьютера. Протокол SSH все больше заменяет Telnet — именно его выбирают сетевые специалисты в качестве средства удаленного входа в систему. SSH чаще всего используется для входа на удаленное устройство и выполнения команд. Но это может также передавать файлы по связанным протоколам SFTP или SCP.<br>
Чтобы протокол SSH мог работать, на сетевых устройствах, взаимодействующих между собой, должна быть настроена поддержка SSH. В этой лабораторной работе необходимо включить SSH-сервер на маршрутизаторе, после чего подключиться к этому маршрутизатору, используя ПК с установленным клиентом SSH. В локальной сети подключение обычно устанавливается с помощью Ethernet и IP.<br>
## Необходимые ресурсы
•	1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)<br>
•	1 коммутатор (Cisco 2960 с ПО Cisco IOS версии 15.2(2) с образом lanbasek9 или аналогичная модель)<br>
•	1 ПК (под управлением Windows с программой эмуляции терминала, например, Tera Term)<br>
•	Консольные кабели для настройки устройств Cisco IOS через консольные порты.<br>
•	Кабели Ethernet, расположенные в соответствии с топологией<br>
## Инструкции
## Часть 1. Настройка основных параметров устройств
В части 1 потребуется настроить топологию сети и основные параметры, такие как IP-адреса интерфейсов, доступ к устройствам и пароли на маршрутизаторе.
### Шаг 1. Создайте сеть согласно топологии.
### Шаг 2. Выполните инициализацию и перезагрузку маршрутизатора и коммутатора.
### Шаг 3. Настройте маршрутизатор.
a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.<br>
b.	Войдите в режим конфигурации.<br>
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
g.	Зашифруйте открытые пароли.<br>
h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.<br>
i.	Настройте и активируйте на маршрутизаторе интерфейс G0/0/1, используя информацию, приведенную в таблице адресации.<br>
j.  Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>

    Router#show startup-config 
    Using 826 bytes
    !
    version 16.6.4
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname Router
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
    ip address 192.168.1.1 255.255.255.0
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
    !
    ip flow-export version 9
    !
    !
    !
    banner motd ^C
    Warning!! Unauthorized access prohibited.
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


### Шаг 4. Настройте компьютер PC-A.
a.	Настройте для PC-A IP-адрес и маску подсети.<br>
b.	Настройте для PC-A шлюз по умолчанию.<br>
![PC-A.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW5/screen/PC-A.png)
### Шаг 5. Проверьте подключение к сети.
Пошлите с PC-A команду Ping на маршрутизатор R1. Если эхо-запрос с помощью команды ping не проходит, найдите и устраните неполадки подключения.<br>
![ping_PC-A_R1.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW5/screen/ping_PC-A_R1.png)
## Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
Подключение к сетевым устройствам по протоколу Telnet сопряжено с риском для безопасности, поскольку вся информация передается в виде открытого текста. Протокол SSH шифрует данные сеанса и обеспечивает аутентификацию устройств, поэтому для удаленных подключений рекомендуется использовать именно этот протокол. В части 2 вам нужно настроить маршрутизатор для приема соединений SSH по линиям VTY.
### Шаг 1. Настройте аутентификацию устройств.
При генерации ключа шифрования в качестве его части используются имя устройства и домен. Поэтому эти имена необходимо указать перед вводом команды crypto key.
a.	Задайте имя устройства.<br>
b.	Задайте домен для устройства.

    Router(config)#hostname R1
    R1(config)#ip domain-name HW5
    R1(config)#
### Шаг 2. Создайте ключ шифрования с указанием его длины.
    R1(config)#crypto key generate rsa 
    The name for the keys will be: R1.HW5
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.
    
    How many bits in the modulus [512]: 2048
    % Generating 2048 bit RSA keys, keys will be non-exportable...[OK]
    *Mar 1 0:14:54.856: %SSH-5-ENABLED: SSH 1.99 has been enabled
    R1(config)#ip ssh
    R1(config)#ip ssh v
    R1(config)#ip ssh version 2
### Шаг 3. Создайте имя пользователя в локальной базе учетных записей.
Настройте имя пользователя, используя admin в качестве имени пользователя и Adm1nP@55 в качестве пароля.

    R1(config)#username admin secret Adm1nP@55

### Шаг 4. Активируйте протокол SSH на линиях VTY.
a.	Активируйте протоколы Telnet и SSH на входящих линиях VTY с помощью команды transport input.

	R1(config-line)#transport input all
b.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.
    
    R1(config-line)#login local
### Шаг 5. Сохраните текущую конфигурацию в файл загрузочной конфигурации.
    R1#write memory
    Building configuration...
    [OK]
### Шаг 6. Установите соединение с маршрутизатором по протоколу SSH.
a.	Запустите Tera Term с PC-A.<br>
b.	Установите SSH-подключение к R1. Use the username admin and password Adm1nP@55. У вас должно получиться установить SSH-подключение к R1.<br>
![SSH_R1.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW5/screen/SSH_R1.png)
## Часть 3. Настройка коммутатора для доступа по протоколу SSH
В части 3 вам предстоит настроить коммутатор для приема подключений по протоколу SSH, а затем установить SSH-подключение с помощью программы Tera Term.
### Шаг 1. Настройте основные параметры коммутатора
a.	Подключитесь к коммутатору с помощью консольного подключения и активируйте привилегированный режим EXEC.<br>
b.	Войдите в режим конфигурации.<br>
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.<br>
d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.<br>
e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.<br>
f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.<br>
g.	Зашифруйте открытые пароли.<br>
h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.<br>
i.	Настройте и активируйте на коммутаторе интерфейс VLAN 1, используя информацию, приведенную в таблице адресации.<br>
j.  Сохраните текущую конфигурацию в файл загрузочной конфигурации.<br>

    Switch#show startup-config 
    Using 1310 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    service password-encryption
    !
    hostname Switch
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
    ip address 192.168.1.11 255.255.255.0
    shutdown
    !
    ip default-gateway 192.168.1.1
    !
    banner motd ^C
    Warning!! Unauthorized access prohibited.(switch)
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
    
### Шаг 2. Настройте коммутатор для соединения по протоколу SSH.
Для настройки протокола SSH на коммутаторе используйте те же команды, которые применялись для аналогичной настройки маршрутизатора в части 2.<br>
a.	Настройте имя устройства, как указано в таблице адресации.

    Switch(config)#hostname S1
b.	Задайте домен для устройства.

    S1(config)#ip domain-name HW5
c.	Создайте ключ шифрования с указанием его длины.

    S1(config)#crypto key generate rsa 
    The name for the keys will be: S1.HW5
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.
    
    How many bits in the modulus [512]: 2048
    % Generating 2048 bit RSA keys, keys will be non-exportable...[OK]
    
    *Mar 1 0:45:32.54: %SSH-5-ENABLED: SSH 1.99 has been enabled
    S1(config)#ip ssh ver
    S1(config)#ip ssh version 2
d.	Создайте имя пользователя в локальной базе учетных записей.

    S1(config)#username admin secret Adm1nP@55
e.	Активируйте протоколы Telnet и SSH на линиях VTY.

    S1(config-line)#transport input all
f.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.

    S1(config-line)#login local
### Шаг 3. Установите соединение с коммутатором по протоколу SSH.
Запустите программу Tera Term на PC-A, затем установите подключение по протоколу SSH к интерфейсу SVI коммутатора S1.<br>
Удалось ли вам установить SSH-соединение с коммутатором?
![SSH_S1.png](https://github.com/EvilPandaKBN/practice/blob/main/HW/HW5/screen/SSH_S1.png)
## Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора
Клиент SSH встроен в операционную систему Cisco IOS и может запускаться из интерфейса командной строки. В части 4 вам предстоит установить соединение с маршрутизатором по протоколу SSH, используя интерфейс командной строки коммутатора.
### Шаг 1. Посмотрите доступные параметры для клиента SSH в Cisco IOS.
Используйте вопросительный знак (?), чтобы отобразить варианты параметров для команды ssh.

    S1#ssh ?
    -l Log in using this user name
    -v Specify SSH Protocol Version
### Шаг 2. Установите с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH.
a.	Чтобы подключиться к маршрутизатору R1 по протоколу SSH, введите команду –l admin. Это позволит вам войти в систему под именем admin. При появлении приглашения введите в качестве пароля Adm1nP@55
    
    S1#ssh -l admin 192.168.1.1
    
    Password: 
    
    
    Warning!! Unauthorized access prohibited.
    
    
    R1>
b.	Чтобы вернуться к коммутатору S1, не закрывая сеанс SSH с маршрутизатором R1, нажмите комбинацию клавиш Ctrl+Shift+6. Отпустите клавиши Ctrl+Shift+6 и нажмите x. Отображается приглашение привилегированного режима EXEC коммутатора.

    R1>
    S1#
c.	Чтобы вернуться к сеансу SSH на R1, нажмите клавишу Enter в пустой строке интерфейса командной строки. Чтобы увидеть окно командной строки маршрутизатора, нажмите клавишу Enter еще раз.

    S1#
    [Resuming connection 1 to 192.168.1.1 ... ]

    R1>
d.	Чтобы завершить сеанс SSH на маршрутизаторе R1, введите в командной строке маршрутизатора команду exit.

    R1# exit
    
    [Connection to 192.168.1.1 closed by foreign host]
    S1#
#### Вопрос <br>
Какие версии протокола SSH поддерживаются при использовании интерфейса командной строки?
#### Ответ <br>
Версии 1 и 2
### Вопрос для повторения
Как предоставить доступ к сетевому устройству нескольким пользователям, у каждого из которых есть собственное имя пользователя?
#### Ответ <br>
Создать различных локальных пользователей и при необходимости задать им различные privilege level
