# Part 1. Инструмент ipcalc

## 1.1 Сети и маски
1) Определение адреса сети 192.167.54\13

![Маска_13](screenshots/1.1_ipcalc_192.167.38.54__13.png)

2) Перевод в маски и двоичную запись в /15 в обычную и двоичную, 11111111.11111111.11111111.11110000 в обычную и префиксную

![Маска_15](screenshots/1.1_ipcalc_192.167.38.54__15.png)

3) Минимальный и максимальный хост в сети 12.167.38.4 при масках; /8, 11111111.11111111.00000000.00000000, 255.255.254.0 и /4

![255.255.254.0](screenshots/1.1_ipcalc_255.254.0.0.png)

## 1.2 Локальный хост

Определить и записать в отчёт, можно ли обратиться к приложению, работающему на localhost, со следующими IP: 194.34.23.100/16, 127.0.0.2/24, 127.1.0.1/8, 128.0.0.1/8

По адресам 194.34.23.100/16 и 128.0.0.1/8 обратиться к приложению не получится, потому что у них нет петли.
У 127.0.0.2/24 и 127.1.0.1/8 есть loopback, так что к ним обратиться можно.

![1.2_1](screenshots/1.2%20127.0.0.1__127.1.0.1.png)
![1.2_2](screenshots/1.2_128.0.0.1.png)
![1.2_3](screenshots/1.2_194.34.23.100.png)

## 1.3 Диапазоны и сегменты сетей:

1) Частные и публичные IP

10.0.0.45/8 - Частный
134.43.0.2/16 - Публичный
192.168.4.2/16 - Частный
172.20.250.4/12 - Частный
172.0.2.1/12 - Публичный
192.172.0.1/12 - Частично частный
172.68.0.2/12 - Публичный
172.16.255.255/12 - Частный
10.10.10.10/8 - Частный
192.169.168.1/16 - Публичный

![1.3_1](screenshots/1.3__1.png)
![1.3_2](screenshots/1.3__2.png)
![1.3_3](screenshots/1.3__3.png)

2) Возможные шлюзы

10.10.0.2
10.10.10.10
10.10.1.255

![1.3.2](screenshots/1.3.2.png)

# Part 2. Статическая маршрутизация между двумя машинами. 

С помощью команды ip a посмотреть существующие сетевые интерфейсы.

WS-1:

![ws1](screenshots/2.1_ip_a_ws1.png)

WS-2:

![ws2](screenshots/2.1_ip_a_ws2.png)

Описать сетевой интерфейс, соответствующий внутренней сети, на обеих машинах и задать следующие адреса и маски: ws1 - 192.168.100.10, маска /16, ws2 - 172.24.116.8, маска /12.

Сетевой интерфейс для внутренней сети - enp0s8.

WS-1:

![ws1](screenshots/2.1_netplan_apply_ws1.png)

WS-2:

![ws2](screenshots/2.1._netplan_apply_ws2.png)

Снова запускаем команду ip a на двух машинах.

WS-1:

![ws1](screenshots/2.1_ip_a_after_netplan_ws1.png)

WS-2:

![ws2](screenshots/2.1_ip_a_after_netplan_ws2.png)

2.1 Добавим статический маршрут с помощью команды ip r add

WS-1:

![ws1](screenshots/2.1_ip_r_add_ws1.png)

WS-2:

![ws2](screenshots/2.1_ip_r_add_ws2.png)

Пингуем:

WS-1:

![ws1](screenshots/2.1_ping_ws1.png)

WS-2:

![ws2](screenshots/2.1_ping_ws2.png)

2.2 Добавим статические маршруты в netplan, чтобы сохарнить: 

WS-1:

![ws1](screenshots/2.2_static_ws1.png)

WS-2:

![ws2](screenshots/2.2_static_ws2.png)

Проверяем и пингуем:

WS-1:

![ws1](screenshots/2.2_ip_a_ws1.png)

WS-2:

![ws2](screenshots/2.2_ip_a_ws2.png)

WS-1:

![ws1](screenshots/2.2_ping_ws1.png)

WS-2:

![ws2](screenshots/2.2_ping_ws2.png)

# Part 3. Утилита iperf3:
## 3.1. Скорость соединения:

Перевести и записать в отчёт: 8 Mbps в MB/s, 100 MB/s в Kbps, 1 Gbps в Mbps:

8 Mbps = 1 MS/s.
100 MB.s = 100000 Kbps.
1 Gbps = 1000 Mbps.

## 3.2 Измерить скорость соединения между ws1 и ws2:

WS-1:

![ws1](screenshots/3.2_iperf3_s_ws1.png)
![ws2](screenshots/3.2_iperf3_c_ws2.png)

WS-2:

![ws2](screenshots/3.2_iperf3_s_ws2.png)
![ws1](screenshots/3.2_iperf_c_ws1.png)

# Part 4. Сетевой экран:

## 4.1. Утилита iptables.

Добавляем в файлы etc/firewall.sh следующие правила:

WS-1:

![ws1](screenshots/4.1_sudo_nano_firewall.sh_ws1.png)

WS-2:

![ws2](screenshots/4.1_sudo_nano_firewall.sh_ws2.png)

(Открываем доступ для порта 22 и 80, запрещаем пинговаться (echo) для ws2, и разрешаем для ws1)

Проверяем:

WS-1:

![ws1](screenshots/4.1_ping_ws1_bad.png)

WS-2:

![ws2](screenshots/4.1_ping_ws2_good.png)

## 4.2. Утилита nmap:

Командой ping найти машину, которая не "пингуется", после чего утилитой nmap показать, что хост машины запущен.

![nmap](screenshots/4.2_nmap.png)

# Part 5. Статическая маршрутизация сети.

## 5.1. Настраиваем машины R1, R2, ws11, ws21, ws22

Netplan:

WS-11:

![ws11](screenshots/5.1_netplan_ws11.png)

WS-21:

![ws21](screenshots/5.1_netplan_ws21.png)

WS-22:

![ws22](screenshots/5.1_netplan_ws22.png)

R-1:

![r1](screenshots/5.1_netplan_R1.png)

R-2:

![r2](screenshots/5.1_netplan_R2.png)

После перезапуска проверяем подключение между машинами через команду ip -4 a

WS-11:

![ws11](screenshots/5.1_ws11_ip4a.png)

WS-21:

![ws21](screenshots/5.1_ws21_ip4a.png)

WS-22:

![ws22](screenshots/5.1_ws22_ip4a.png)

R-1:

![r1](screenshots/5.1_R1_ip4a.png)

R-2:

![r2](screenshots/5.1_R2_ip4a.png)

И проверяем пинг между машинами:

WS-11:

![ws11](screenshots/5.1_ping_ws11.png)

WS-21:

![ws21](screenshots/5.1_ping_ws21.png)

WS-22:

![ws22](screenshots/5.1_ping_ws22.png)

## 5.2. Включение переадресации IP-адресов.

Делается черещ утилиту sysctl. Задаем net_forward

R-1:

![r1](screenshots/5.2_R1.png)
![r1](screenshots/5.2_R1_systctl.png)
![r1](screenshots/5.2_R1_sysctl_p.png)

R-2:

![r2](screenshots/5.2_R2.png)
![r2](screenshots/5.2_R2_sysctl.png)
![r2](screenshots/5.2_R2_sysctl_p.png)

## 5.3. Установка маршрута по-умолчанию.

WS-11:

![ws11](screenshots/5.3_gayeway4_netplan_ws11.png)

WS-21:

![ws21](screenshots/5.3_gateway4_netplan_ws21.png)
WS-22:

![ws22](screenshots/5.3_gayeway4_netplan_ws22.png)

R-1:

![r1](screenshots/5.3_gateway4_netplan_r1.png)

R-2:

![r2](screenshots/5.3_gateway4_netplan_r2.png)

Теперь проверяем через команду ip r

WS-11:

![ws11](screenshots/5.3_ipr_ws11.png)

WS-21:

![ws21](screenshots/5.3_ipr_ws21.png)
WS-22:

![ws22](screenshots/5.3_ipr_ws22.png)

R-1:

![r1](screenshots/5.3_ipr_r1.png)

R-2:

![r2](screenshots/5.3_ipr_r2.png)

Пропинговать с ws11 роутер r2 и показать на r2, что пинг доходит.

![ping](screenshots/5.3_ping_from_ws11_to_r2.png)
![tcpdump](screenshots/5.3_tcpdump_r2.png)

## 5.4. Добавление статических маршрутов

Добавить в роутеры r1 и r2 статические маршруты в файле конфигураций.

R-1:

![r1](screenshots/5.4_netplan_r1.png)

R-2:

![r2](screenshots/5.4_netplan_r2.png)

Проверяем через команду ip r

R-1:

![r1](screenshots/5.3_ipr_r1.png)

R-2:

![r2](screenshots/5.4_ipr_r2.png)

Запускаем команду на ws11:

![ws11](screenshots/5.4_ws11.png)

## 5.5 Построение списка маршрутизаторов

Запустить на r1 команду дампа. При помощи утилиты traceroute построить список маршрутизаторов на пути от ws11 до ws21.

![traceroute](screenshots/5.5_traceroute_ws11_to_ws21.png)
![tcpdump](screenshots/5.5_tcpdump.png)

Путь строиться от узла к узлу до того момента, покаа не будет достигнута конечная точка. Каждый пакет проходит на своем пути определенное количество узлов, пока достигнет своей цели. На каждом узле добавляется счетчик, который отслеживает количество пройденых узлов.

## 5.6 Использование протокола ICMP при маршрутизации.

![tcpdump](screenshots/5.6_tcpdump.png)
![ping](screenshots/5.6_ping.png)

# Part 6. Динамическая настройка IP с помощью DHCP.

Указать MAC адрес у ws11, для этого в etc/netplan/00-installer-config.yaml надо добавить строки: macaddress: 10:10:10:10:10:BA, dhcp4: true.

![ws11](screenshots/6_netplan_ws11_macaddress.png)

Для r2 настроить в файле /etc/dhcp/dhcpd.conf конфигурацию службы DHCP:

![r2](screenshots/6_r2_dhcpd.png)

Добавляем nameserver 8.8.8.8 в resolv.conf

![r2](screenshots/6_r1_resolv.conf.png)

Перезагрузить службу DHCP командой systemctl restart isc-dhcp-server. Машину ws21 перезагрузить при помощи reboot и через ip a показать, что она получила адрес. Также пропинговать ws22 с ws21.

![r2](screenshots/6_r2_restart_dhcp.png)
![ws21](screenshots/6_ws21_ipa.png)
![ws22](screenshots/6_ping_ws11_to_ws22.png)

Для r1 настроить аналогично, но сделать выдачу адресов с жесткой привязкой к MAC-адресу (ws11). Провести аналогичные тесты.

![r1](screenshots/6_dhcpd_r1.png)
![r1](screenshots/6_r1_resolv.conf.png)

Запрашиваем ip a в ws21:

![ws21](screenshots/6_ws21_ipa.png)

Далее используем такие команды как sudo dhclient -v и sudo dhclient -r:

![ws21](screenshots/6_ws21_sudo_dhclient_v.png)
![ws21](screenshots/6_ws21_sudo_dhclient_r_v.png)

# Part 7. NAT

В файле /etc/apache2/ports.conf на ws22 и r2 изменить строку Listen 80 на Listen 0.0.0.0:80, то есть сделать сервер Apache2 общедоступным.

![ws22](screenshots/7_ws22_nano_apache.png)
![r2](screenshots/7_r2_nano_apache.png)

Запускаем веб-сервер Apache командой start (ws22, r1):

![ws22](screenshots/7_ws22_start_apache.png)
![r1](screenshots/7_r1_start_apache.png)

Добавляем фаервол через утилиту iptables

![r2](screenshots/7_r2_iptables.png)

Пропинговаться ws22 и r1. (Не должно проходить)

![ws22](screenshots/7_ping_ws22_to_r1_bad.png)

Разрешаем:

![ws22](screenshots/7_r2_approve.png)

Пингуем (Должно проходить):

![ws22](screenshots/7_ping_ws22_to_r1_good.png)

Добавляем:
![r2](screenshots/7_r2_iptables_add1.png)
![r2](screenshots/7_r2_iptables_add2.png)

Проверяем соединением по tcp для snat. проверяем через telnet

![r1](screenshots/7_telnet_from_r1.png)
![ws22](screenshots/7_ws22_telnet.png)

# Part 8. Знакомтсво с SSH Tunnels

Возвращаем порт 80 у ws22:

![ws22](screenshots/8_ws22_nano_apache.png)

Настраиваем ssh:

![ws22](screenshots/8_ssh_R_8080.png)

Проверяем подключение через telnet:

![ws22](screenshots/8_telnet_10.20.0,29.png)

