# TP4 : DHCP

 ## I. DHCP Client

 
🌞 Déterminer

```
 Bail obtenu. . . . . . . . . . . . . . : vendredi 27 octobre 2023 09:06:34
   Bail expirant. . . . . . . . . . . . . : samedi 28 octobre 2023 09:06:33
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   ```

🌞 Capturer un échange DHCP

[capture DORA](tp4_dhcp_client.pcapng)

🌞 Analyser la capture Wireshark

C'est la 2ème trame (DHCP Offer) qui permet de voir les informations proposées au client.

## II. Serveur DHCP

### 3. Setup topologie
🌞 Preuve de mise en place

```[vince@DHCP ~]$ ping www.google.com
PING www.google.com (172.217.20.164) 56(84) bytes of data.
^C64 bytes from 172.217.20.164: icmp_seq=1 ttl=114 time=22.9 ms

--- www.google.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 22.897/22.897/22.897/0.000 ms
```

```[vince@node2 ~]$ ping www.google.com
PING www.google.com (172.217.20.164) 56(84) bytes of data.
^C64 bytes from 172.217.20.164: icmp_seq=1 ttl=114 time=19.2 ms

--- www.google.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 19.180/19.180/19.180/0.000 ms
```

```
[vince@node2 ~]$ traceroute www.google.com
traceroute to www.google.com (172.217.20.164), 30 hops max, 60 byte packets
 1  _gateway (10.4.1.254)  12.413 ms  10.475 ms  9.584 ms
 2  10.0.3.2 (10.0.3.2)  8.074 ms  7.553 ms  6.866 ms
```

### 4. Serveur DHCP
🌞 Rendu

```
[vince@DHCP ~]$ sudo systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; ena>
     Active: active (running) since Wed 2023-11-08 15:18:58 CET>
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
   Main PID: 11543 (dhcpd)
     Status: "Dispatching packets..."
      Tasks: 1 (limit: 4674)
     Memory: 5.2M
        CPU: 32ms
     CGroup: /system.slice/dhcpd.service
             └─11543 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.con
```

```
[vince@DHCP ~]$ sudo cat /etc/dhcp/dhcpd.conf
# create new
# specify domain name
option domain-name     "dhcp.tp4";
# specify DNS server's hostname or IP address

# default lease time
default-lease-time 600;
# max lease time
max-lease-time 7200;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnetmask
subnet 10.4.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.4.1.137 10.4.1.237;
    # specify broadcast address
    option broadcast-address 10.4.1.255;
    # specify gateway
}
```
```
 55  dnf -y install dhcp-server
   56  -y install dhcp-server
   57  dnf install dhcp-server
   58  sudo dnf -y install dhcp-server
   59  vi /etc/dhcp/dhcpd.conf
   60  sudo nano /etc/dhcp/dhcpd.conf
   61  systemctl enable --now dhcpd
   62  sudo systemctl enable --now dhcpd
   63  sudo systemctl status dhcpd
  ```

### 5. Client DHCP

🌞 Prouvez que
```
  2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:f7:27:c8 brd ff:ff:ff:ff:ff:ff
    inet 10.4.1.137/24 brd 10.4.1.255 scope global dynamic noprefixroute enp0s3
```
node1.tp4.b1 a enregistré un bail DHCP
```
rebind 3 2023/11/08 14:51:56;
expire 3 2023/11/08 14:53:11;
option dhcp-server-identifier 10.4.1.253;
```
```
[vince@node1 ~]$ ping 10.4.1.254
PING 10.4.1.254 (10.4.1.254) 56(84) bytes of data.
64 bytes from 10.4.1.254: icmp_seq=1 ttl=64 time=1.43 ms
64 bytes from 10.4.1.254: icmp_seq=2 ttl=64 time=1.59 ms
64 bytes from 10.4.1.254: icmp_seq=3 ttl=64 time=1.64 ms
```
```
[vince@node1 ~]$ ping 10.4.1.12
PING 10.4.1.12 (10.4.1.12) 56(84) bytes of data.
64 bytes from 10.4.1.12: icmp_seq=1 ttl=64 time=2.02 ms
64 bytes from 10.4.1.12: icmp_seq=2 ttl=64 time=2.22 ms
64 bytes from 10.4.1.12: icmp_seq=3 ttl=64 time=2.25 ms
```
🌞 Bail DHCP serveur

```
[vince@DHCP ~]$ cat /var/lib/dhcpd/dhcpd.leases
lease 10.4.1.137 {
  starts 3 2023/11/08 14:50:28;
  ends 3 2023/11/08 15:00:28;
  cltt 3 2023/11/08 14:50:28;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 08:00:27:f7:27:c8;
  uid "\001\010\000'\367'\310";
```
6. Options DHCP

🌞 Nouvelle conf !
```
[vince@DHCP ~]$ sudo cat /etc/dhcp/dhcpd.conf
# create new
# specify domain name
option domain-name     "dhcp.tp4";
# specify DNS server's hostname or IP address
option domain-name-servers     1.1.1.1;
# default lease time
default-lease-time 21600;
# max lease time
max-lease-time 22000;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnetmask
subnet 10.4.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.4.1.137 10.4.1.237;
    # specify broadcast address
    option broadcast-address 10.4.1.255;
    # specify gateway
    option routers 10.4.1.254;
}
```

🌞 Test !

redemandez une IP avec le client node1.tp4.b1

prouvez-que :

vous avez enregistré l'adresse d'un serveur DNS

sous Linux, on consulte le serveur DNS actuel en affichant le contenu du fichier /etc/resolv.conf



vous avez une nouvelle route par défaut qui a été récupérée dynamiquement
la durée de votre bail DHCP est bien de 6 heures


prouvez que vous avez un accès Internet après cet échange DHCP

🌞 Capture Wireshark

utilisez tcpdump pour capturer un échange DHCP complet entre node1.tp4.b1 et dhcp.tp4.b1


🦈 tp4_dhcp_server.pcapng
➜ Un vrai serveur DHCP qui donne tout ce qu'il faut aux clients pour qu'ils aient un accès au LAN (une adresse IP) et un accès internet en plus (l'adresse de la passerelle et l'adresse d'un serveur DNS joignable).