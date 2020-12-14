## VPN
1. Между двумя виртуалками поднять vpn в режимах
- tun
- tap
Прочуствовать разницу.

2. Поднять RAS на базе OpenVPN с клиентскими сертификатами, подключиться с локальной машины на виртуалку

3*. Самостоятельно изучить, поднять ocserv и подключиться с хоста к виртуалке 

### Задание 2. openvpn RAS

Сервер поднят и настроен по инструкции в методичке. Порядок команд не вывожу. Результаты тестирования:

##### На сервере:

#### [root@localhost openvpn]# ip r

    default via 10.0.2.2 dev eth0 proto dhcp metric 100

    10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100

    10.10.10.0/24 via 10.10.10.2 dev tun0

    10.10.10.2 dev tun0 proto kernel scope link src 10.10.10.1

    192.168.10.0/24 via 10.10.10.2 dev tun0

#### [root@localhost openvpn]# ip a
    
    1: lo: <LOOPBACK,UP,LOWER\_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000

    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

    inet 127.0.0.1/8 scope host lo

    valid\_lft forever preferred\_lft forever

    inet6 ::1/128 scope host

    valid\_lft forever preferred\_lft forever

    2: eth0: <BROADCAST,MULTICAST,UP,LOWER\_UP> mtu 1500 qdisc pfifo\_fast state UP group default qlen 1000

    link/ether 52:54:00:4d:77:d3 brd ff:ff:ff:ff:ff:ff

    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0

    valid\_lft 83862sec preferred\_lft 83862sec

    inet6 fe80::5054:ff:fe4d:77d3/64 scope link

    valid\_lft forever preferred\_lft forever

    3: eth1: <BROADCAST,MULTICAST,UP,LOWER\_UP> mtu 1500 qdisc pfifo\_fast state UP group default qlen 1000

    link/ether 08:00:27:2d:7f:9a brd ff:ff:ff:ff:ff:ff

    inet6 fe80::4465:dd40:763a:3f2c/64 scope link noprefixroute

    valid\_lft forever preferred\_lft forever

    4: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER\_UP> mtu 1500 qdisc pfifo\_fast state UNKNOWN group default qlen 100

    link/none

    inet 10.10.10.1 peer 10.10.10.2/32 scope global tun0

    valid\_lft forever preferred\_lft forever

    inet6 fe80::f531:b392:f47c:9748/64 scope link flags 800

    valid\_lft forever preferred\_lft forever

##### На клиенте:

#### [andrey@localhost vpnclient]$ openvpn --config client.conf

     Mon Dec 14 11:41:51 2020 OpenVPN 2.4.9 x86\_64-redhat-linux-gnu [SSL (OpenSSL)] [LZO] [LZ4] [EPOLL] [PKCS11] [MH/PKTINFO] [AEAD] built on Apr 24 2020

    Mon Dec 14 11:41:51 2020 library versions: OpenSSL 1.1.1c FIPS  28 May 2019, LZO 2.08

    Mon Dec 14 11:41:51 2020 WARNING: No server certificate verification method has been enabled.  See http://openvpn.net/howto.html#mitm for more info.

    Mon Dec 14 11:41:51 2020 TCP/UDP: Preserving recently used remote address: [AF\_INET]192.168.10.10:1207

    Mon Dec 14 11:41:51 2020 Socket Buffers: R=[212992->212992] S=[212992->212992]

    Mon Dec 14 11:41:51 2020 UDP link local (bound): [AF\_INET][undef]:1194

    Mon Dec 14 11:41:51 2020 UDP link remote: [AF\_INET]192.168.10.10:1207

#### [andrey@localhost vpnclient]$ ip r

    default via 192.168.253.161 dev enp2s0 proto dhcp metric 100

    192.168.10.0/24 dev vboxnet2 proto kernel scope link src 192.168.10.1

    192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown

    192.168.253.0/24 dev enp2s0 proto kernel scope link src 192.168.253.103 metric 100

#### [andrey@localhost vpnclient]$ ping -c 4 10.10.10.1

    PING 10.10.10.1 (10.10.10.1) 56(84) bytes of data.

    64 bytes from 10.10.10.1: icmp\_seq=1 ttl=61 time=1.95 ms

    64 bytes from 10.10.10.1: icmp\_seq=2 ttl=61 time=3.11 ms

    64 bytes from 10.10.10.1: icmp\_seq=3 ttl=61 time=1.80 ms

    64 bytes from 10.10.10.1: icmp\_seq=4 ttl=61 time=1.63 ms

    --- 10.10.10.1 ping statistics ---

    4 packets transmitted, 4 received, 0% packet loss, time 7ms

    rtt min/avg/max/mdev = 1.625/2.117/3.106/0.584 ms

#### [andrey@localhost vpnclient]$
