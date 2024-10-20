# VLAN

## Задание.
1. Построение сети и настройка основных параметров устройства
2. Создание VLAN и назначение портов коммутаторов
3. Настройка Trunk между коммутаторами
4. Настройка маршрутизации между VLAN на маршрутизаторе
5. Проверка работы настроенной схемы


### 1. Построение сети и настройка основных параметров устройства.

#### 1. Построить топологию согласно схеме сети.
![](Topology.png)

1. Таблица IP-адресов:

| Device | Interface | IP Address   | Subnet Mask   | Default Gateway |
| ------ | --------- | ------------ | ------------- | --------------- |
| R1     | G0/0/1.3  | 192.168.3.1  | 255.255.255.0 | N/A             |
| R1     | G0/0/1.4  | 192.168.4.1  | 255.255.255.0 | N/A             |
| R1     | G0/0/1.8  | N/A          | N/A           | N/A             |
| S1     | VLAN 3    | 192.168.3.11 | 255.255.255.0 | 192.168.3.1     |
| S2     | VLAN 3    | 192.168.3.12 | 255.255.255.0 | 192.168.3.1     |
| PC-A   | NIC       | 192.168.3.3  | 255.255.255.0 | 192.168.3.1     |
| PC-B   | NIC       | 192.168.4.3  | 255.255.255.0 | 192.168.4.1     |

2. Таблица VLAN:

| VLAN | Name       | Interface Assigned                                         |
| ---- | ---------- | ---------------------------------------------------------- |
| 3    | Management | S1: VLAN 3                                                 |
|      |            | S2: VLAN 3                                                 |
|      |            | S1: F0/6                                                   |
|      |            |                                                            |
| 4    | Operations | S2: F0/18                                                  |
|      |            |                                                            |
| 7    | ParkingLot | S1: F0/2-4, F0/7-24, G0/1-2                                |
|      |            | S2: F0/2-17, F0/19-24, G0/1-2                              |
|      |            |                                                            |                      
| 8    | Native     | N/A                                                        |


#### 2. Настроить базовую конфигурацию на маршрутизаторе.
1. Подключитесь к маршрутизатору с помощью консоли и включите привилегированный режим EXEC.
2. Войдите в режим конфигурации.
3. Назначьте маршрутизатору имя устройства.
4. Отключите поиск DNS, чтобы маршрутизатор не пытался преобразовать неправильно введенные команды, как будто это имена хостов.
5. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
6. Назначьте cisco в качестве пароля консоли и включите вход.
7. Назначьте cisco в качестве пароля VTY и включите вход.
8. Зашифруйте пароли в виде открытого текста.
9. Создайте баннер, предупреждающий любого, кто получает доступ к устройству, о том, что несанкционированный доступ запрещен.
10. Сохраните текущую конфигурацию в файле конфигурации запуска.
11. Установите часы на маршрутизаторе.

##### Команды для настройки маршрутизатора

```shell
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable secret level 15 class
R1(config-line)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config)#line vty 0 15
R1(config-line)#password cisco
R1(config-line)#login
R1(config)#banner motd #Unauthorized access to this device is prohibited!#
R1#copy running-config startup-config 
R1#clock set 22:44:00  oct 13 2024
```

#### 3. Настроить базовую конфигурацию на коммутаторах.

1. Подключитесь к коммутатору с помощью консоли и включите привилегированный режим EXEC.
2. Войдите в режим конфигурации.
3. Назначьте коммутатору имя устройства.
4. Отключите поиск DNS, чтобы маршрутизатор не пытался преобразовать неправильно введенные команды в имена хостов.
5. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
6. Назначьте cisco в качестве пароля консоли и включите вход.
7. Назначьте cisco в качестве пароля vty и включите вход.
8. Зашифруйте пароли в виде открытого текста.
9. Создайте баннер, предупреждающий любого, кто получает доступ к устройству, о том, что несанкционированный доступ запрещен.
10. Установите часы на коммутаторе.
Примечание: используйте вопросительный знак (?), чтобы помочь с правильной последовательностью параметров, необходимых для выполнения этой команды.
11. Скопируйте текущую конфигурацию в конфигурацию запуска.

##### Команды для Switch 1

```shell
Switch>en
Switch#conf t
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#enable secret level 15 class
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config)#exit
S1(config)#banner motd #Unauthorized access to this device is prohibited!#
S1(config)#exit
S1#clock set 14:10:30 oct 14 2024
S1#copy running-config startup-config 
```

##### Команды для Switch 2

```shell
Switch>en
Switch#conf t
Switch(config)#hostname S2
S2(config)#no ip domain-lookup
S2(config)#enable secret level 15 class
S2(config)#line console 0
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#line vty 0 15
S2(config-line)#password cisco
S2(config-line)#login
S2(config)#exit
S2(config)#banner motd #Unauthorized access to this device is prohibited!#
S2(config)#exit
S2#clock set 14:15:00 oct 14 2024
S2#copy running-config startup-config 
```

####  Настйрока ПК.
Настройка производится согласно таблице IP-адресов.

| Device | Interface | IP Address   | Subnet Mask   | Default Gateway |
| ------ | --------- | ------------ | ------------- | --------------- |
| PC-A   | NIC       | 192.168.3.3  | 255.255.255.0 | 192.168.3.1     |
| PC-B   | NIC       | 192.168.4.3  | 255.255.255.0 | 192.168.4.1     |



### 2. Создание VLAN и назначение портов коммутаторов.

#### 1. Создание VLAN на обоих коммутаторах.

1. Создайте и назовите требуемые VLAN на каждом коммутаторе из таблицы выше.
2. Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации.
3. Назначьте все неиспользуемые порты на обоих коммутаторах VLAN ParkingLot, настройте их для режима статического доступа и административно деактивируйте их.

##### Пример настройки:

```shell
S1#conf t
S1(config)#vlan 3
S1(config-vlan)#name Management
S1(config)#interface Vlan3
S1(config-if)#ip address 192.168.3.11 255.255.255.0
S1(config) ip default-gateway 192.168.3.1
S1(config)#interface range fastEthernet0/2-4
S1(config-if-range)#description ParkingLot
S1(config-if-range)#shutdown
```


#### 2. Назначение VLAN указанным портам в таблице выше.

##### Пример настройки:

```shell
S1#conf t
S1(config)#interface Fa0/6
S1(config-if)#switchport mode access 
S1(config-if)#switchport access vlan 3
```

Проверить настройки можно командой **show vlan brief**:

```shell
S1#sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
3    Management                       active    Fa0/6
4    Operations                       active    
7    ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
8    Native                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

### 3. Настройка транка между коммутаторами.

#### 1.  Настройка транка на интерфейсе F0/1.

a. Измените режим порта коммутатора на интерфейсе F0/1 для принудительного транкинга. Обязательно сделайте это на обоих коммутаторах.
b. В рамках конфигурации транка установите собственный VLAN на 8 на обоих коммутаторах. Вы можете временно увидеть сообщения об ошибках, пока два интерфейса настроены для разных собственных VLAN.
c. В качестве другой части конфигурации транка укажите, что VLAN 3, 4 и 8 разрешено пересекать транк.
d. Выполните команду show interfaces trunk для проверки портов транкинга, собственного VLAN и разрешенных VLAN через транк.

##### Пример настройки:
```shell
S1#conf t
S1(config)#vlan 8
S1(config-vlan)#name Native
S1(config-vlan)#exit
S1(config)#interface fa0/1
S1(config-if)#switchport mode trunk 
S1(config-if)#switchport trunk native vlan 8
S1(config-if)#switchport trunk allowed vlan 3,4
```

Проверить настройки можно командой **show vlan brief**:

S1#show interfaces trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      8
Fa0/5       on           802.1q         trunking      8

Port        Vlans allowed on trunk
Fa0/1       3-4
Fa0/5       3-4

Port        Vlans allowed and active in management domain
Fa0/1       3,4
Fa0/5       3,4

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       3,4
Fa0/5       3,4


#### 2.  Настройка транка на интерфейсе F0/5 коммутатора S1.

1. Настройте F0/5 на S1 с теми же параметрами транка, что и F0/1. Это транк к маршрутизатору.
2. Сохраните текущую конфигурацию в файле конфигурации запуска на S1 и S2.
3. Выполните команду show interfaces trunk для проверки транкирования.

##### Пример настройки:
S1#conf t
S1(config-if)#interface FastEthernet0/5
S1(config-if)# switchport trunk native vlan 8
S1(config-if)# switchport trunk allowed vlan 3-4
S1(config-if)# switchport mode trunk


С помощью команды **show interfaces trunk** можно посмотреть все тракновые порты. Порта Fa0/5 нет в списке, так как порт Gi0/0/1 маршрутизатора R1 административно выключен, согласно изначальной заводской настройке, применяемой к маршртуизаторам.

S1#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      8

Port        Vlans allowed on trunk
Fa0/1       3-4

Port        Vlans allowed and active in management domain
Fa0/1       3,4

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       3,4


### 4. Настройка маршрутизации между VLAN на маршрутизаторе.

1. Активируйте интерфейс G0/0/1 на маршрутизаторе.
2. Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейсу для собственной VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.
3. Используйте команду show ip interface brief, чтобы проверить работоспособность подинтерфейсов.

##### Пример настройки:
```shell
R1#conf t
R1(config)#interface Gi0/0/1
R1(config-if)#no shutdown 
R1(config-if)#exit
R1(config)#interface GigabitEthernet0/0/1.3
R1(config-if)#description Management
R1(config-if)#encapsulation dot1Q 3
R1(config-if)#ip address 192.168.3.1 255.255.255.0
R1(config-if)#exit
R1(config)#interface GigabitEthernet0/0/1.4
R1(config-if)#description Operations
R1(config-if)#encapsulation dot1Q 4
R1(config-if)#ip address 192.168.4.1 255.255.255.0
R1(config-if)#exit
R1(config)#interface GigabitEthernet0/0/1.8
R1(config-if)#description Native
R1(config-if)#encapsulation dot1Q 8
R1(config-if)#no ip address
```

Просмотр краткой информации по интерфейсам с помощью команды **show ip interface brief**:

R1#sh ip int brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.3 192.168.3.1     YES manual up                    up 
GigabitEthernet0/0/1.4 192.168.4.1     YES manual up                    up 
GigabitEthernet0/0/1.8 unassigned      YES unset  up                    up 
Vlan1                  unassigned      YES unset  administratively down down


### 5. Проверка работоспособности маршрутизации между VLAN

#### 1. Выполните следующие тесты с ПК-А. Все должны быть успешными.

1. Пинг с ПК-A на его шлюз по умолчанию.
2. Пинг с ПК-A на ПК-B
3. Пинг с ПК-A на S2


##### Пример тестов:


###### a. Пинг с ПК-A на его шлюз по умолчанию.

C:\>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:

Reply from 192.168.3.1: bytes=32 time=2ms TTL=255
Reply from 192.168.3.1: bytes=32 time=1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 2ms, Average = 0ms



###### b. Пинг с ПК-A на ПК-B

C:\>ping 192.168.4.1

Pinging 192.168.4.1 with 32 bytes of data:

Reply from 192.168.4.1: bytes=32 time<1ms TTL=255
Reply from 192.168.4.1: bytes=32 time=1ms TTL=255
Reply from 192.168.4.1: bytes=32 time<1ms TTL=255
Reply from 192.168.4.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.4.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms



###### c. Пинг с ПК-A на S2

C:\>ping 192.168.3.12

Pinging 192.168.3.12 with 32 bytes of data:

Request timed out.
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.12:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms	



#### 2. Выполните следующий тест с ПК-Б.
Из командной строки на ПК-Б введите команду tracert на адрес ПК-А.
Вопрос: Какие промежуточные IP-адреса отображаются в результатах?

Вывод команды **tracert 192.168.3.3** с ПК-Б:
C:\>tracert 192.168.3.3

Tracing route to 192.168.3.3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.4.1
  2   0 ms      0 ms      0 ms      192.168.3.3

Trace complete.	


В трассировке виден промежуточный IP-адрес **192.168.4.1**, который является шлюзом для ПК-Б.
