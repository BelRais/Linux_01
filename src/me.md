# Проект Linux Network sreensrc/by deannven
## Part 1. Инструмент ipcalc
______
### 1.1. Сети и маски
Устанавлиеваем ipcalc:
> sudo apt install ipcalc

#### 1.1.1 Адрес сети:

> ipcalc 192.167.38.54/13

Вывод данной команды ниже:

![1.1.1](src/sreen/1.1.1.PNG)

#### 1.1.2 Перевод маски 255.255.255.0 в префиксную и двоичную запись, /15 в обычную и двоичную, 11111111.11111111.11111111.11110000 в обычную и префиксную:

1. Перевод маски 255.255.255.0 в префиксную и двоичную запись:

> ipcalc 255.255.255.0

После введения данной команды, мы видим, префиксную и двоичную запись. (скриншот ниже)

![1.1.2](src/sreen/1.1.2.PNG)

2. /15 в обычную и двоичную:

> ipcalc /15

После введения данной команды, мы видим, обычную и двоичную запись.

![1.1.1.2](src/sreen/1.1.1.2.PNG)

3. 11111111.11111111.11111111.11110000 в обычную и префиксную:

Согласно таблицы сетевых масок в битах 11111111.11111111.11111111.11110000 будет /28: 

> ipcalc /28

После введения данной команды, мы видим, обычную и префиксную. (скриншот ниже).

![1.1.2.3](src/sreen/1.1.2.3.PNG)

#### 1.1.3 Минимальный и максимальный хост в сети 12.167.38.4 при масках: /8, 11111111.11111111.00000000.00000000, 255.255.254.0 и /4:

#### Минимальный и максимальный хост в сети 12.167.38.4 при маске /8: 

> ipcalc 12.167.38.4/8

![1.1.3.1](src/sreen/1.1.3.1.PNG)

Минимальный = 12.0.0.1

Максимальный = 12.255.255.254

#### Минимальный и максимальный хост в сети 12.167.38.4 при 11111111.11111111.00000000.00000000: 
> ipcalc 12.167.38.4/16

![1.1.3.2](src/sreen/1.1.3.2.PNG)

Минимальный = 12.167.0.1

Максимальный = 12.167.255.254

#### Минимальный и максимальный хост в сети 12.167.38.4 при 255.255.254.0:

Netmask 255.255.254.0, это /23. 

> ipcalc 12.167.38.4/23

![1.1.3.3](src/sreen/1.1.3.3.PNG)

Минимальный = 12.167.38.1

Максимальный = 12.167.39.254

#### Минимальный и максимальный хост в сети 12.167.38.4 при /4:

> ipcalc 12.167.38.4/4

![1.1.3.4](src/sreen/1.1.3.4.PNG)

Минимальный = 0.0.0.1

Максимальный = 15.255.255.254

### 1.2. localhost
localhost (так называемый, «местный» от англ. local, или «локальный хост», по смыслу — этот компьютер) — в компьютерных сетях, стандартное, официально зарезервированное доменное имя для частных IP-адресов (в диапазоне 127.0.0.1 — 127.255.255.254, RFC 2606)

Таким образом к приложению работающим на localhost:
1. 194.34.23.100 - не возможно обратиться
2. 127.0.0.2 - возможно обратиться
3. 127.1.0.1 - возможно обратиться
4. 128.0.0.1 - не возможно обратиться

### 1.3. Диапазоны и сегменты сетей

#### Какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: 10.0.0.45, 134.43.0.2, 192.168.4.2, 172.20.250.4, 172.0.2.1, 192.172.0.1, 172.68.0.2, 172.16.255.255, 10.10.10.10, 192.169.168.1:

Диапазон частных(приватных) IP-адресов:

Class A   10.0.0.0 - 10.255.255.255

Class B   172.16.0.0 - 172.31.255.255

Class C   192.168.0.0 - 192.168.255.255

Иные являются публичными.

1. 10.0.0.45 - private, Class A
2. 134.43.0.2 - public
3. 192.168.4.2 - private, Class C
4. 172.20.250.4 - private, Class B
5. 172.0.2.1 - public
6. 192.172.0.1 - public
7. 172.68.0.2 - public
8. 172.16.255.255 - private, Class B
9. 10.10.10.10 - private, Class A
10. 192.169.168.1 - public

#### Какие из перечисленных IP адресов шлюза возможны у сети 10.10.0.0/18: 10.0.0.1, 10.10.0.2, 10.10.10.10, 10.10.100.1, 10.10.1.255:

Для начала определим минимальный и максимальный хост в сети 10.10.0.0 при /18:

> ipcalc 10.10.0.0/18

Минимальный = 10.10.0.1

Максимальный = 10.10.63.254

Далее смотрим что попало:

1. 10.0.0.1 - не может иметь ip - адрес шлюзов
2. 10.10.0.2 - может иметь
3. 10.10.10.10 - не может иметь ip - адрес шлюзов
4. 10.10.100.1 - не может иметь ip - адрес шлюзов
5. 10.10.1.255 - может иметь, broadcast

## Part 2. Статическая маршрутизация между двумя машинами
### Поднять две виртуальные машины (далее -- ws1 и ws2)

![part2_pre](src/sreen/part2_pre.PNG)

### С помощью команды ip a посмотреть существующие сетевые интерфейсы

![part2_pre2](src/sreen/part2_pre2.PNG)

### Описать сетевой интерфейс, соответствующий внутренней сети, на обеих машинах и задать следующие адреса и маски: ws1 - 192.168.100.10, маска /16, ws2 - 172.24.116.8, маска /12:

Для создания внутренней сети между ws1 и ws2, требуется включить 2 адаптер на 2 виртуалках. 

![part2_virtuha](src/sreen/part2_virtuha.PNG)

Прописываем на ws1 и ws2 команду:
> sudo vim /etc/netplan/00-installer-config.yaml

Если адаптер не можете увидеть при вводе ifconfig, тогда его возможно поднять

> sudo ifconfig enp0s8 up

Где enp0s8, это адаптер. Его через `ip a` увидеть возможно.

Настраиваем соединение для нового адаптера, так же не забываем отключить возможность получения с сервера dhcp ip адресов.

![address](src/sreen/address.PNG)

### Выполнить команду netplan apply для перезапуска сервиса сети:

![apply](src/sreen/apply.PNG)

### 2.1. Добавление статического маршрута вручную:

После настройки enp0s8, прокидываем ip между виртуалками:

ws1:
> sudo ip r add 172.24.116.8 dev enp0s8

ws2:
> sudo ip r add 192.168.100.10 dev enp0s8

![part2_zakid](src/sreen/part2_zakid.PNG)

Теперь проверим связь через ping.

![part2_ping](src/sreen/part2_ping.PNG)


### 2.2. Добавление статического маршрута с сохранением:

Прописываем следующее: 

![all](src/sreen/all.PNG)

Проверяем связь:

![all_ping](src/sreen/all_ping.PNG)

## Part 3. Утилита iperf3

Iperf3 — кроссплатформенная консольная клиент-серверная программа — генератор TCP и UDP трафика для тестирования пропускной способности сети. С ее помощью довольно просто измерить максимальную пропускную способность сети между сервером и клиентом и провести нагрузочное тестирование канала связи.

### 3.1. Скорость соединения

Перевести и записать в отчёт: 

1. 8 Mbps в MB/s = 1 MB/s
2. 100 MB/s в Kbps = 800000 Kbps
3. 1 Gbps в Mbps = 1000 Mbps

### 3.2. Утилита iperf3

Измерить скорость соединения между ws1 и ws2

Для измерения скорости на первой машине (ws1) пропишем:
> iperf3 -s -f K

Далее машина будет ждать получения сигнала, для последующего измерения.

На ws2 пропишем:
> iperf3 -c 172.24.116.8 -f K

![speed](src/sreen/speed.PNG)

Скорость отображается в Килобайтах. Так как в флаге -f прописали K.

## Part 4. Сетевой экран

### 4.1. Утилита iptables

#### Создать файл /etc/firewall.sh, имитирующий фаерволл, на ws1 и ws2:

Этот материал был крайне полезен https://www.opennet.ru/docs/RUS/iptables/#FILTERTABLE

````
#!/bin/sh

iptables –F
iptables -X

3) 
#Отрываем порт 22 (это у нас ssh)
iptables -A INPUT -p tcp --dport 22 -j ACCEPT #Добавляем в конец цепочки INPUT заданне правило (-A), с использованием протокола и порт или диапазон портов, на который адресован пакетtcp (-p __ --dport), добавляем действие ACCEPT (-j).

#Открываем порт 80 (это у нас http)
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

4) запретить echo reply (машина не должна "пинговаться”, т.е. должна быть блокировка на OUTPUT)

iptables -A OUTPUT -p icmp --icmp-type 0 -j DROP 

#Код 0 это Echo Reply по ссылке можете посмотреть типы ICMP.

5) разрешить echo reply (машина должна "пинговаться")
iptables -A OUTPUT -p icmp --icmp-type 0 -j ACCEPT

````

Для создания использую команду:
> sudo vim /etc/firewall.sh

![wall](src/sreen/wall.PNG)

Даем права файлу:
> sudo chmod +x /etc/firewall.sh

Не знаю на кой тут +x, он даст доступ денайт. 
Я ниже запускал через sudo.

Запускаем:
> /etc/firewall.sh

На ws1 в начале открываем порты 22 и 80. В отличии от ws2, будет пинговаться, так как последняя поправка ACCEPT

На ws2 в начале так же открываем порты 22 и 80. В отличии от ws1, не будет пинговаться, так как последняя поправка DROP

### 4.2. Утилита nmap

![nmap](src/sreen/nmap.PNG)


## Part 5. Статическая маршрутизация сети

### 5.1. Настройка адресов машин

#### Настроить конфигурации машин в etc/netplan/00-installer-config.yaml согласно сети на рисунке.

Поднимаем 5 машин

![5vir](src/sreen/5vir.PNG)

Настраиваем r1:

![r1](src/sreen/r1.PNG)

Настраиваем r2:

![r2](src/sreen/r2.PNG)

Настраиваем ws11:

![ws11](src/sreen/ws11.PNG)

Настраиваем ws22:

![ws22](src/sreen/ws22.PNG)

Настраиваем ws21:

![ws21](src/sreen/ws21.PNG)

#### Перезапустить сервис сети. Если ошибок нет, то командой ip -4 a проверить, что адрес машины задан верно. Также пропинговать ws22 с ws21. Аналогично пропинговать r1 с ws11.

Для r1:

![r1_ip](src/sreen/r1_ip.PNG)

Для r2:

![r2_ip](src/sreen/r2_ip.PNG)

Для ws11:

![ws11_ip](src/sreen/ws11_ip.PNG)

Для ws22:

![ws22_ip](src/sreen/ws22_ip.PNG)

Для ws21:

![ws21_ip](src/sreen/ws21_ip.PNG)


Пинг ws22 с ws21 (на кой не знаю):

![parapa_ping](src/sreen/parapa_ping.PNG)


### 5.2. Включение переадресации IP-адресов.

#### Для включения переадресации IP, выполните команду на роутерах:

r1:

![sys_r1](src/sreen/sys_r1.PNG)

r2:

![sys_r2](src/sreen/sys_r2.PNG)

#### Откройте файл /etc/sysctl.conf и добавьте в него следующую строку:

r1:

![r1_kra](src/sreen/r1_kra.PNG)

r2:

![r2_kra](src/sreen/r2_kra.PNG)

### 5.3. Установка маршрута по-умолчанию.

ws11:

![ws11_ip_r](src/sreen/ws11_ip_r.PNG)

ws21:

![ws21_ip_r](src/sreen/ws21_ip_r.PNG)

ws22:

![ws22_ip_r](src/sreen/ws22_ip_r.PNG)

### Пропинговать с ws11 роутер r2 и показать на r2, что пинг доходит. Для этого использовать команду:

![tcmp](src/sreen/tcmp.PNG)


### 5.4. Добавление статических маршрутов

#### Добавить в роутеры r1 и r2 статические маршруты в файле конфигураций. Пример для r1 маршрута в сетку 10.20.0.0/26:

r1: 

![r1_5.4](src/sreen/r1_5.4.PNG)

r2:

![r2_5.4](src/sreen/r2_5.4.PNG)

#### Запустить команды на ws11:

![ws11_5.4](src/sreen/ws11_5.4.PNG)

Вероятно, маршрут для адреса 10.10.0.0/18 был выбран, потому что в данной ситуации был задан более специфичный (более точный) маршрут, который соответствует этому адресу. 

Маршрут по-умолчанию 0.0.0.0/0 используется в случае, когда не существует более специфичных маршрутов для конкретных подсетей. Он указывает на то, что все пакеты, которые не соответствуют другим маршрутам, должны быть отправлены по этому маршруту.

#### 5.5. Построение списка маршрутизаторов:

1. Запускаем `tcpdump -tnv -i enp0s3` на r1
2. Запускаем `traceroute 10.20.0.10`

![trac](src/sreen/trac.PNG)

Принцип работы traceroute заключается в отправке серии пакетов с различными значениями TTL. Когда пакет достигает маршрутизатора на пути к назначению, его TTL уменьшается на 1. Если TTL становится равным 0, маршрутизатор отбрасывает пакет и отправляет обратно ICMP сообщение "Time Exceeded" (Время истекло) отправителю. Это ICMP сообщение содержит IP-адрес маршрутизатора, который его отправил.

Traceroute использует эту информацию для построения пути. Он отправляет первый пакет с TTL равным 1 и получает ICMP сообщение "Time Exceeded" от первого маршрутизатора на пути. Затем он увеличивает TTL на 1 и отправляет пакет с новым TTL. Процесс повторяется до тех пор, пока пакет не достигнет назначения.

Используя вывод tcpdump, мы можем проследить каждый пакет, проходящий через интерфейс enp0s3. Мы можем видеть IP-адрес каждого промежуточного узла (маршрутизатора), через который проходит пакет.

#### 5.6. Использование протокола ICMP при маршрутизации

1. Запускаем `tcpdump -n -i enp0s3 icmp` на r1
2. Запускаем `ping -c 1 10.30.0.111`

![icmp](src/sreen/icmp.PNG)

## Part 6. Динамическая настройка IP с помощью DHCP

### Для r2 настроить в файле /etc/dhcp/dhcpd.conf конфигурацию службы DHCP:

![r2_6](src/sreen/r2_6.PNG)

### Перезагрузить службу DHCP командой systemctl restart isc-dhcp-server. Машину ws21 перезагрузить при помощи reboot и через ip a показать, что она получила адрес. Также пропинговать ws22 с ws21.

![r2_dhcp](src/sreen/r2_dhcp.PNG)

![ws21_6](src/sreen/ws21_6.PNG)

### Указать MAC адрес у ws11, для этого в etc/netplan/00-installer-config.yaml надо добавить строки: macaddress: 10:10:10:10:10:BA, dhcp4: true

![ws11_mac](src/sreen/ws11_mac.PNG)

### Для r1 настроить аналогично r2, но сделать выдачу адресов с жесткой привязкой к MAC-адресу (ws11). Провести аналогичные тесты

Установим если у вас нет. 

> sudo apt-get install isc-dhcp-server

![r1_dhcp](src/sreen/r1_dhcp.PNG)

![r1_kra_kra](src/sreen/r1_kra_kra.PNG)

![r1_re](src/sreen/r1_re.PNG)

![r1_dhcp_status](src/sreen/r1_dhcp_status.PNG)

![ws11_dhcp4](src/sreen/ws11_dhcp4.PNG)

![box_ws11](src/sreen/box_ws11.PNG)

Старый ip адрес:

![ws_old_ip](src/sreen/ws_old_ip.PNG)

Новый ip адрес:

![ws11_ping](src/sreen/ws11_ping.PNG)

На роутере:

Чтобы перезагрузить dhcp
> systemctl restart isc-dhcp-server

Чтобы посмотреть статус dhcp
> sudo systemctl status isc-dhcp-server

На клиенте:

Сбросить IP
> dhclient -r enp0s3

Получить новый IP
> sudo dhclient enp0s3

## Part 7. NAT

### В файле /etc/apache2/ports.conf на ws22 и r1 изменить строку Listen 80 на Listen 0.0.0.0:80, то есть сделать сервер Apache2 общедоступным

> sudo apt-get install apache2

![apache_ws22](src/sreen/apache_ws22.PNG)

![apache_r1](src/sreen/apache_r1.PNG)

### Запустить веб-сервер Apache командой service apache2 start на ws22 и r1

![apache_status_ws22](src/sreen/apache_status_ws22.PNG)

![apache_status_r1](src/sreen/apache_status_r1.PNG)

### Добавить в фаервол, созданный по аналогии с фаерволом из Части 4, на r2 следующие правила:

Для создания использую команду:

> sudo vim /etc/firewall.sh

![r2_firewall](src/sreen/r2_firewall.PNG)

Даем права файлу:

> sudo chmod +x /etc/firewall.sh

Запускаем:

> sudo sh /etc/firewall.sh

### Проверить соединение между ws22 и r1 командой ping
При запуске файла с этими правилами, ws22 не должна "пинговаться" с r1

![you_shall_not_pass](src/sreen/you_shall_not_pass.PNG)

### Добавить в файл ещё одно правило:

![r2_firewall_new](src/sreen/r2_firewall_new.PNG)

### Проверить соединение между ws22 и r1 командой ping
При запуске файла с этими правилами, ws22 должна "пинговаться" с r1

![you_shall_pass](src/sreen/you_shall_pass.PNG)

### Добавить в файл ещё два правила:

Перед тем как влючим SNAT, проверим что происходит без редакции в таблице nat. 

![krakra](src/sreen/krakra.PNG)

Видно, что ip адрес ws22 виден r1.

5) включить SNAT, а именно маскирование всех локальных ip из локальной сети, находящейся за r2 (по обозначениям из Части 5 - сеть 10.20.0.0)

![krakra_2](src/sreen/krakra_2.PNG)

Видим, что сейчас ip адрес ws22 не виден r1, вместо этого виден ip адрес r2.


6) включить DNAT на 8080 порт машины r2 и добавить к веб-серверу Apache, запущенному на ws22, доступ извне сети

![dnat](src/sreen/dnat.PNG)

Благодаря `iptables -A FORWARD -i enp0s8 -j ACCEPT` входящие пакеты будут проходить по цепочки FORWARD. 

Благодаря `iptables -A FORWARD -o enp0s8 -j ACCEPT` исходящие пакеты будут проходить по цепочки FORWARD. 

### Проверить соединение по TCP для SNAT, для этого с ws22 подключиться к серверу Apache на r1 командой:

![ws22_telnet](src/sreen/ws22_telnet.PNG)

### Проверить соединение по TCP для DNAT, для этого с r1 подключиться к серверу Apache на ws22 командой telnet (обращаться по адресу r2 и порту 8080)

![r1_telnet](src/sreen/r1_telnet.PNG)

## Part 8. Дополнительно. Знакомство с SSH Tunnels

#### Запустить веб-сервер Apache на ws22 только на localhost (то есть в файле /etc/apache2/ports.conf изменить строку Listen 80 на Listen localhost:80)

![ws22_loc](src/sreen/ws22_loc.PNG)

#### Воспользоваться Local TCP forwarding с ws21 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws21



 

