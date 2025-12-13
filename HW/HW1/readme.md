# Лабораторная работа. Базовая настройка коммутатора
## Топология
![](./screen/Topology.png)
## Таблица адресации
|Устройство|Интерфейс|IP адрес   |
| ------------ | ------------ | ------------ |
| S1|Vlan 1|192.168.1.2|
|   |   |255.255.255.0|
| PC-PT|NIC|192.168.1.10|
|   |   |255.255.255.0|
## Задачи
#### Часть 1. Проверка конфигурации коммутатора по умолчанию
#### Часть 2. Создание сети и настройка основных параметров устройства
•	Настройте базовые параметры коммутатора.<br>
•	Настройте IP-адрес для ПК.
#### Часть 3. Проверка сетевых подключений
•	Отобразите конфигурацию устройства.<br>
•	Протестируйте сквозное соединение, отправив эхо-запрос.<br>
•	Протестируйте возможности удаленного управления с помощью Telnet.
## Часть 1. Создание сети и проверка настроек коммутатора по умолчанию
В первой части лабораторной работы вам предстоит настроить топологию сети и проверить настройку коммутатора по умолчанию.
### Шаг 1. Создайте сеть согласно топологии.
a.	Подсоедините консольный кабель, как показано в топологии. На данном этапе не подключайте кабель Ethernet компьютера PC-A.<br>
b.	Установите консольное подключение к коммутатору с компьютера PC-A с помощью Tera Term или другой программы эмуляции терминала.

![](./screen/Connect.png)
#### Вопрос <br>
Почему нужно использовать консольное подключение для первоначальной настройки коммутатора? Почему нельзя подключиться к коммутатору через Telnet или SSH?<br>
#### Ответ <br>
На текущем этапе, не задан IP адрес, а также не заданы настройки авторизации для данных протоколов
###Шаг 2. Проверьте настройки коммутатора по умолчанию.<br>
a.	Предположим, что коммутатор не имеет файла конфигурации, сохраненного в энергонезависимой памяти (NVRAM). Консольное подключение к коммутатору с помощью Tera Term или другой программы эмуляции терминала предоставит доступ к командной строке пользовательского режима EXEC в виде Switch>. Введите команду enable, чтобы войти в привилегированный режим EXEC.
Откройте окно конфигурации
Обратите внимание, что измененная в конфигурации строка будет отражать привилегированный режим EXEC.
Убедитесь, что на коммутаторе находится пустой файл конфигурации по умолчанию, с помощью команды show running-config привилегированного режима EXEC.<br> Если конфигурационный файл был предварительно сохранен, его нужно удалить. В зависимости от модели коммутатора и версии IOS ваша конфигурация может слегка отличаться. Тем не менее, настроенных паролей или IP-адресов в конфигурации быть не должно. Выполните очистку настроек и перезагрузите коммутатор, если ваш коммутатор имеет настройки, отличные от настроек по умолчанию.<br>

Switch#show running-config<br> 
Building configuration...<br>
Current configuration : 1080 bytes<br>
!<br>
version 15.0<br>
no service timestamps log datetime msec<br>
no service timestamps debug datetime msec<br>
no service password-encryption<br>
!<br>
hostname Switch<br>
!<br>
!<br>
!<br>
!<br>
!<br>
!<br>
spanning-tree mode pvst<br>
spanning-tree extend system-id<br>
!<br>
interface FastEthernet0/1<br>
!<br>
interface FastEthernet0/2<br>
!<br>
interface FastEthernet0/3<br>
!<br>
interface FastEthernet0/4<br>
!<br>
interface FastEthernet0/5<br>
!<br>
interface FastEthernet0/6<br>
!<br>
interface FastEthernet0/7<br>
!<br>
interface FastEthernet0/8<br>
!<br>
interface FastEthernet0/9<br>
!<br>
interface FastEthernet0/10<br>
!<br>
interface FastEthernet0/11<br>
!<br>
interface FastEthernet0/12<br>
!<br>
interface FastEthernet0/13<br>
!<br>
interface FastEthernet0/14<br>
!<br>
interface FastEthernet0/15<br>
!<br>
interface FastEthernet0/16<br>
!<br>
interface FastEthernet0/17<br>
!<br>
interface FastEthernet0/18<br>
!<br>
interface FastEthernet0/19<br>
!<br>
interface FastEthernet0/20<br>
!<br>
interface FastEthernet0/21<br>
!<br>
interface FastEthernet0/22<br>
!<br>
interface FastEthernet0/23<br>
!<br>
interface FastEthernet0/24<br>
!<br>
interface GigabitEthernet0/1<br>
!<br>
interface GigabitEthernet0/2<br>
!<br>
interface Vlan1<br>
no ip address<br>
shutdown<br>
!<br>
!<br>
!<br>
!<br>
line con 0<br>
!<br>
line vty 0 4<br>
login<br>
line vty 5 15<br>
login<br>
!<br>
!<br>
!<br>
!<br>
end<br>
b.	Изучите текущий файл running configuration.
#### Вопрос <br>
Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?<br>
#### Ответ <br>
24
#### Вопрос <br>
Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?<br>
#### Ответ <br>
2
#### Вопрос <br>
Каков диапазон значений, отображаемых в vty-линиях?<br>
#### Ответ <br>
0-15<br>
c.	Изучите файл загрузочной конфигурации (startup configuration), который содержится в энергонезависимом ОЗУ (NVRAM).<br>

Switch#show startup-config<br>
startup-config is not present<br>
#### Вопрос <br>
Почему появляется это сообщение?<br>
#### Овтет <br>
В энергонезависимой памяти отсутствует сохраненная конфигурация, поскольку на текущий момент конфигурация не сохранялась<br>

d.	Изучите характеристики SVI для VLAN 1.<br>
#### Вопрос <br>
Назначен ли IP-адрес сети VLAN 1?<br>
#### Ответ <br>
Нет, не назначен
#### Вопрос <br>
Какой MAC-адрес имеет SVI? Возможны различные варианты ответов.<br>
#### Ответ <br>
Switch#show interfaces vlan 1 <br>
Hardware is CPU Interface, address is 0001.c748.a85d (bia 0001.c748.a85d)<br>
#### Вопрос <br>
Данный интерфейс включен?
#### Ответ <br>
Нет, не включен<br>

e.	Изучите IP-свойства интерфейса SVI сети VLAN 1.
#### Вопрос <br>
Какие выходные данные вы видите?
#### Ответ <br>
Название Vlan1<br>
Ip адрес Не задан<br>
Метод получения адреса Ручной<br>
Статус интерфейса administratively down<br>
Протокол down<br>

f.	Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.<br>
#### Вопрос <br>
Какие выходные данные вы видите?
#### Ответ <br>
Данные не изменились
g.	Изучите сведения о версии ОС Cisco IOS на коммутаторе.
#### Вопрос <br>
Под управлением какой версии ОС Cisco IOS работает коммутатор?<br>
#### ответ <br>
Switch#show version <br>
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)<br>
#### Вопрос <br>
Как называется файл образа системы?<br>
#### Ответ <br>
System image file is "flash:c2960-lanbasek9-mz.150-2.SE4.bin"<br>
h.	Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.<br>
Switch# show interface f0/6 
#### Вопрос <br>
Интерфейс включен или выключен?
Включен
Что нужно сделать, чтобы включить интерфейс?
Интерфейс включился после соединения физического соединения с ПК
Какой MAC-адрес у интерфейса?
            Switch#show interface f0/6
            FastEthernet0/6 is up, line protocol is up (connected)
Hardware is Lance, address is 0002.1681.9306 (bia 0002.1681.9306)
Какие настройки скорости и дуплекса заданы в интерфейсе?
Full-duplex, 100Mb/s
f.	Изучите флеш-память.
Выполните одну из следующих команд, чтобы изучить содержимое флеш-каталога.
Switch# show flash 
Switch# dir flash: 
В конце имени файла указано расширение, например .bin. Каталоги не имеют расширения файла.
Вопрос:
Какое имя присвоено образу Cisco IOS?
2960-lanbasek9-mz.150-2.SE4.bin
