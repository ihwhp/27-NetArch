### Простые проверки с разных устройств

```
[vagrant@centralRouter ~]$ ip r
default via 192.168.255.1 dev eth1 proto static metric 108
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 107
192.168.0.0/28 dev eth2 proto kernel scope link src 192.168.0.1 metric 109
192.168.0.32/28 dev eth3 proto kernel scope link src 192.168.0.33 metric 110
192.168.0.64/26 dev eth4 proto kernel scope link src 192.168.0.65 metric 111
192.168.1.0/25 via 192.168.253.2 dev eth6 proto static metric 106
192.168.1.128/26 via 192.168.253.2 dev eth6 proto static metric 106
192.168.1.192/26 via 192.168.253.2 dev eth6 proto static metric 106
192.168.2.0/26 via 192.168.254.2 dev eth5 proto static metric 112
192.168.2.64/26 via 192.168.254.2 dev eth5 proto static metric 112
192.168.2.128/26 via 192.168.254.2 dev eth5 proto static metric 112
192.168.2.192/26 via 192.168.254.2 dev eth5 proto static metric 112
192.168.253.0/30 dev eth6 proto kernel scope link src 192.168.253.1 metric 106
192.168.254.0/30 dev eth5 proto kernel scope link src 192.168.254.1 metric 112
192.168.255.0/30 dev eth1 proto kernel scope link src 192.168.255.2 metric 108
[vagrant@centralRouter ~]$ ping 192.168.2.1 -c 3
PING 192.168.2.1 (192.168.2.1) 56(84) bytes of data.
64 bytes from 192.168.2.1: icmp_seq=1 ttl=64 time=1.48 ms
64 bytes from 192.168.2.1: icmp_seq=2 ttl=64 time=0.727 ms
64 bytes from 192.168.2.1: icmp_seq=3 ttl=64 time=0.875 ms

--- 192.168.2.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 0.727/1.027/1.481/0.328 ms
```
```
[vagrant@centralRouter ~]$ ping 192.168.1.1 -c 3
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=0.619 ms
64 bytes from 192.168.1.1: icmp_seq=2 ttl=64 time=0.772 ms
64 bytes from 192.168.1.1: icmp_seq=3 ttl=64 time=0.768 ms

--- 192.168.1.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 0.619/0.719/0.772/0.077 ms
```
```
[vagrant@centralRouter ~]$ ping 192.168.1.2 -c 3
PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
64 bytes from 192.168.1.2: icmp_seq=1 ttl=63 time=1.35 ms
64 bytes from 192.168.1.2: icmp_seq=2 ttl=63 time=1.34 ms
64 bytes from 192.168.1.2: icmp_seq=3 ttl=63 time=1.50 ms

--- 192.168.1.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 1.345/1.402/1.503/0.077 ms
```
```
[vagrant@office1Router ~]$ tracepath 77.88.8.8
 1?: [LOCALHOST]                                         pmtu 1500
 1:  gateway                                               1.088ms
 1:  gateway                                               1.197ms
 2:  192.168.255.1                                         1.473ms
 3:  no reply
 4:  no reply
 5:  no reply
 6:  *.*.*.*                                              3.397ms asymm 64
 7:  *.*.*.*                                              54.964ms asymm 63
 8:  *.*.*.*                                               2.904ms asymm 62
 9:  asr1.westcall.ru                                      3.587ms asymm 61
10:  asr-m9.westcall.ru                                    4.165ms asymm 60
11:  asr-m9.westcall.ru                                    3.788ms asymm 59
12:  dante.yndx.net                                        5.024ms asymm 58
13:  dns.yandex.ru                                         7.012ms reached
```
```
[vagrant@office2Server ~]$ tracepath 77.88.8.8
 1?: [LOCALHOST]                                         pmtu 1500
 1:  gateway                                               1.034ms
 1:  gateway                                               0.705ms
 2:  192.168.253.1                                         1.449ms
 3:  192.168.255.1                                         1.919ms
 4:  no reply
 5:  no reply
 6:  no reply
 7:  no reply
 8:  no reply
 9:  *.*.*.*                                               4.141ms asymm 63
10:  asr1.westcall.ru                                      4.456ms asymm 62
11:  asr-m9.westcall.ru                                    4.670ms asymm 61
12:  dante.yndx.net                                        5.347ms asymm 60
13:  sas-32z3-ae2.yndx.net                                 9.927ms asymm 59
14:  vla-32z1-ae4.yndx.net                                 8.899ms asymm 58
15:  dns.yandex.ru                                        10.481ms reached
     Resume: pmtu 1500 hops 15 back 57
```

## Теория
***использовалась утилита `sipcalc`***
 ### Сети Office1
```
 -[ipv4 : 192.168.2.0/26] - 0
[CIDR]
Host address            - 192.168.2.0
Host address (decimal)  - 3232236032
Host address (hex)      - C0A80200
Network address         - 192.168.2.0
Network mask            - 255.255.255.192
Network mask (bits)     - 26
Network mask (hex)      - FFFFFFC0
Broadcast address       - 192.168.2.63
Cisco wildcard          - 0.0.0.63
Addresses in network    - 64
Network range           - 192.168.2.0 - 192.168.2.63
Usable range            - 192.168.2.1 - 192.168.2.62
```
```
-[ipv4 : 192.168.2.64/26] - 0
[CIDR]
Host address            - 192.168.2.64
Host address (decimal)  - 3232236096
Host address (hex)      - C0A80240
Network address         - 192.168.2.64
Network mask            - 255.255.255.192
Network mask (bits)     - 26
Network mask (hex)      - FFFFFFC0
Broadcast address       - 192.168.2.127
Cisco wildcard          - 0.0.0.63
Addresses in network    - 64
Network range           - 192.168.2.64 - 192.168.2.127
Usable range            - 192.168.2.65 - 192.168.2.126
```
```
-[ipv4 : 192.168.2.128/26] - 0
[CIDR]
Host address            - 192.168.2.128
Host address (decimal)  - 3232236160
Host address (hex)      - C0A80280
Network address         - 192.168.2.128
Network mask            - 255.255.255.192
Network mask (bits)     - 26
Network mask (hex)      - FFFFFFC0
Broadcast address       - 192.168.2.191
Cisco wildcard          - 0.0.0.63
Addresses in network    - 64
Network range           - 192.168.2.128 - 192.168.2.191
Usable range            - 192.168.2.129 - 192.168.2.190
```
```
-[ipv4 : 192.168.2.192/26] - 0
[CIDR]
Host address            - 192.168.2.192
Host address (decimal)  - 3232236224
Host address (hex)      - C0A802C0
Network address         - 192.168.2.192
Network mask            - 255.255.255.192
Network mask (bits)     - 26
Network mask (hex)      - FFFFFFC0
Broadcast address       - 192.168.2.255
Cisco wildcard          - 0.0.0.63
Addresses in network    - 64
Network range           - 192.168.2.192 - 192.168.2.255
Usable range            - 192.168.2.193 - 192.168.2.254
```
### Сети Office2
``` 
-[ipv4 : 192.168.1.0/25] - 0
[CIDR]
Host address            - 192.168.1.0
Host address (decimal)  - 3232235776
Host address (hex)      - C0A80100
Network address         - 192.168.1.0
Network mask            - 255.255.255.128
Network mask (bits)     - 25
Network mask (hex)      - FFFFFF80
Broadcast address       - 192.168.1.127
Cisco wildcard          - 0.0.0.127
Addresses in network    - 128
Network range           - 192.168.1.0 - 192.168.1.127
Usable range            - 192.168.1.1 - 192.168.1.126
```
```
-[ipv4 : 192.168.1.128/26] - 0
[CIDR]
Host address            - 192.168.1.128
Host address (decimal)  - 3232235904
Host address (hex)      - C0A80180
Network address         - 192.168.1.128
Network mask            - 255.255.255.192
Network mask (bits)     - 26
Network mask (hex)      - FFFFFFC0
Broadcast address       - 192.168.1.191
Cisco wildcard          - 0.0.0.63
Addresses in network    - 64
Network range           - 192.168.1.128 - 192.168.1.191
Usable range            - 192.168.1.129 - 192.168.1.190
```
```
-[ipv4 : 192.168.1.192/26] - 0
[CIDR]
Host address            - 192.168.1.192
Host address (decimal)  - 3232235968
Host address (hex)      - C0A801C0
Network address         - 192.168.1.192
Network mask            - 255.255.255.192
Network mask (bits)     - 26
Network mask (hex)      - FFFFFFC0
Broadcast address       - 192.168.1.255
Cisco wildcard          - 0.0.0.63
Addresses in network    - 64
Network range           - 192.168.1.192 - 192.168.1.255
Usable range            - 192.168.1.193 - 192.168.1.254
```

### Сети Central
```
-[ipv4 : 192.168.0.0/28] - 0
[CIDR]
Host address            - 192.168.0.0
Host address (decimal)  - 3232235520
Host address (hex)      - C0A80000
Network address         - 192.168.0.0
Network mask            - 255.255.255.240
Network mask (bits)     - 28
Network mask (hex)      - FFFFFFF0
Broadcast address       - 192.168.0.15
Cisco wildcard          - 0.0.0.15
Addresses in network    - 16
Network range           - 192.168.0.0 - 192.168.0.15
Usable range            - 192.168.0.1 - 192.168.0.14
```
```
-[ipv4 : 192.168.0.32/28] - 0
[CIDR]
Host address            - 192.168.0.32
Host address (decimal)  - 3232235552
Host address (hex)      - C0A80020
Network address         - 192.168.0.32
Network mask            - 255.255.255.240
Network mask (bits)     - 28
Network mask (hex)      - FFFFFFF0
Broadcast address       - 192.168.0.47
Cisco wildcard          - 0.0.0.15
Addresses in network    - 16
Network range           - 192.168.0.32 - 192.168.0.47
Usable range            - 192.168.0.33 - 192.168.0.46
```
```
-[ipv4 : 192.168.0.64/26] - 0
[CIDR]
Host address            - 192.168.0.64
Host address (decimal)  - 3232235584
Host address (hex)      - C0A80040
Network address         - 192.168.0.64
Network mask            - 255.255.255.192
Network mask (bits)     - 26
Network mask (hex)      - FFFFFFC0
Broadcast address       - 192.168.0.127
Cisco wildcard          - 0.0.0.63
Addresses in network    - 64
Network range           - 192.168.0.64 - 192.168.0.127
Usable range            - 192.168.0.65 - 192.168.0.126
```
Тут свободны подсети: `192.168.0.16/28` `192.168.0.48/28` `192.168.0.128/25`

### Карта сети  
![Untitled Diagram drawio](https://user-images.githubusercontent.com/105001717/200868239-60854458-7376-4ba1-948f-3ade7d539f47.png)
