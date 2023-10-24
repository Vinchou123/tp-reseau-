# TP3 : On va router des trucs

## 1. Echange ARP

🌞Générer des requêtes ARP

```[vince@localhost ~]$ ping 10.3.1.12
PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=2.93 ms
64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=2.34 ms
64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=0.980 ms
64 bytes from 10.3.1.12: icmp_seq=4 ttl=64 time=1.99 ms
64 bytes from 10.3.1.12: icmp_seq=5 ttl=64 time=4.36 ms
^C
--- 10.3.1.12 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4010ms
rtt min/avg/max/mdev = 0.980/2.521/4.364/1.118 ms
```
```[vince@localhost ~]$ ip neigh show
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:06 DELAY
💢10.3.1.12 dev enp0s3 lladdr 08:00:27:59:53:6f STALE
```
 
 ```
 2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:9c:0f:30 brd ff:ff:ff:ff:ff:ff
    inet 10.3.1.11/24 brd 10.3.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe9c:f30/64 scope link
```

## 2. Analyse de trames

🌞Analyse de trames

```[vince@localhost ~]$ sudo tcpdump -i enp0s3
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on enp0s3, link-type EN10MB (Ethernet), snapshot length 262144 bytes
11:08:24.586798 IP localhost.localdomain.ssh > 10.3.1.1.62341: Flags [P.], seq 2605553636:2605553696, ack 4089289136, win 501, length 60
11:08:24.587731 IP localhost.localdomain.ssh > 10.3.1.1.62341: Flags [P.], seq 60:96, ack 1, win 501, length 36
11:08:24.588783 IP 10.3.1.1.62341 > localhost.localdomain.ssh: Flags [.], ack 96, win 8192, length 0
11:08:24.589790 IP localhost.localdomain.ssh > 10.3.1.1.62341: Flags [P.], seq 96:204, ack 1, win 501, length 108
11:08:24.590806 IP localhost.localdomain.ssh > 10.3.1.1.62341: Flags [P.], seq 204:240, ack 1, win 501, length 36
11:08:24.591634 IP 10.3.1.1.62341 > localhost.localdomain.ssh: Flags [.], ack 240, win 8191, length 0
11:08:24.591891 IP localhost.localdomain.ssh > 10.3.1.1.62341: Flags [P.], seq 240:300, ack 1, win 501, length 60
11:08:24.612351 IP localhost.localdomain.ssh > 10.3.1.1.62341: Flags [P.], seq 300:368, ack 1, win 501, length 68
11:08:24.615829 IP 10.3.1.1.62341 > localhost.localdomain.ssh: Flags [.], ack 368, win 8191, length 0
11:08:24.616567 IP localhost.localdomain.ssh > 10.3.1.1.62341: Flags [P.], seq 368:436, ack 1, win 501, length 68
```


```[vince@localhost ~]$ sudo ip neigh flush all
[vince@localhost ~]$ ping 10.3.1.12
PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=4.42 ms
64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=2.93 ms
64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=2.05 ms
^C
--- 10.3.1.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 2.054/3.133/4.419/0.976 ms
[vince@localhost ~]$ ip n s
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:06 REACHABLE
10.3.1.12 dev enp0s3 lladdr 08:00:27:59:53:6f REACHABLE
```

[capture réseau ARP request/reply](./ping.pcapng)