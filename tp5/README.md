# TP5 : TCP, UDP et services réseau

## I. First steps

🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP




discord - [capture trafic discord](tp5_service_1.pcapng)


youtube - [capture trafic youtube](tp5_service_2.pcapng)



instagram - [capture trafic instagram](tp5_service_3.pcapng)


microsoft teams - [capture trafic teams](tp5_service_4.pcapng)


netflix - [capture trafic netflix](tp5_service_5.pcapng)


## II. Setup Virtuel
- 
### 1. SSH

🌞 Examinez le trafic dans Wireshark

[capture clean avec le 3-way handshake, un peu de trafic au milieu et une fin de connexion](tp5_3_way.pcapng)

🌞 Demandez aux OS

```PS C:\Windows\system32> netstat -n -b

Connexions actives

  Proto  Adresse locale         Adresse distante       État
  TCP    10.4.1.1:56486         10.4.1.11:22           ESTABLISHED
 [ssh.exe]
 ```
 ```[vince@vm1 ~]$ sudo ss -tnp
[sudo] password for vince:
State   Recv-Q   Send-Q     Local Address:Port       Peer Address:Port    Process
ESTAB   0        0              10.4.1.11:22             10.4.1.1:56486    users:(("sshd",pid=2083,fd=4),("sshd",pid=2079,fd=4))
ESTAB   0        36             10.4.1.11:22             10.4.1.1:57573    users:(("sshd",pid=2212,fd=4),("sshd",pid=2208,fd=4))
```

### 2. Routage

🌞 Prouvez que

```
[vince@vm1 ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=18.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=22.5 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 18.050/20.251/22.453/2.201 ms
```


```
[vince@vm1 ~]$ ping google.com
PING google.com (142.250.179.78) 56(84) bytes of data.
64 bytes from par21s19-in-f14.1e100.net (142.250.179.78): icmp_seq=1 ttl=114 time=16.4 ms
64 bytes from par21s19-in-f14.1e100.net (142.250.179.78): icmp_seq=2 ttl=114 time=21.2 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 16.407/18.816/21.225/2.409 ms
```

### 3. Serveur Web

🌞 Installez le paquet nginx

```
[vince@web ~]$ sudo dnf install nginx
```

🌞 Créer le site web

```
[vince@web site_web_nul]$ history
    1  ping 8.8.8.8
    2  clear
    3  dnf install
    4  sudo mkdir
    5  cd var
    6  ls
    7  cd /var/
    8  ls
    9  sudo mkdir www
   10  ls
   11  cd www
   12  sudo mkdir site_web_nul/index.html
   13  sudo mkdir site_web_nul
   14  ls
   15  cd site_web_nul/
   16  sudo touch index.html
   17  sudo nano index.html
   18  history
   ```

   🌞 Donner les bonnes permissions
   ```
   [vince@web ~]$ sudo chown -R nginx:nginx /var/www/site_web_nul
   ```

🌞 Créer un fichier de configuration NGINX pour notre site web

```
[vince@web conf.d]$ sudo cat site_web_nul.conf
server {

listen 80;

index index.html;

server_name www.site_web_nul.b1;

root /var/www/site_web_nul;

}
```

🌞 Démarrer le serveur web !

```
[vince@web conf.d]$ sudo systemctl start nginx
[vince@web conf.d]$ sudo systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Thu 2023-11-16 11:55:23 CET; 26s ago
    Process: 11276 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 11277 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 11278 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 11279 (nginx)
      Tasks: 2 (limit: 4674)
     Memory: 2.0M
        CPU: 37ms
     CGroup: /system.slice/nginx.service
             ├─11279 "nginx: master process /usr/sbin/nginx"
             └─11280 "nginx: worker process"

Nov 16 11:55:23 web.tp5.b1 systemd[1]: Starting The nginx HTTP and reverse proxy server...
Nov 16 11:55:23 web.tp5.b1 nginx[11277]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Nov 16 11:55:23 web.tp5.b1 nginx[11277]: nginx: configuration file /etc/nginx/nginx.conf test is successf>
Nov 16 11:55:23 web.tp5.b1 systemd[1]: Started The nginx HTTP and reverse proxy server.
```

🌞 Ouvrir le port firewall

```
[vince@web conf.d]$ sudo firewall-cmd --add-port=80/tcp --permanent
success
[vince@web conf.d]$ sudo firewall-cmd --reload
success
```

🌞 Visitez le serveur web !

```
[vince@vm1 ~]$ curl http://10.5.1.12
<h1>MEOW</h1>
```

🌞 Visualiser le port en écoute

```
[vince@web conf.d]$ sudo ss -ltunp
Netid        State          Recv-Q         Send-Q                 Local Address:Port                 Peer Address:Port        Process
udp          UNCONN         0              0                          127.0.0.1:323                       0.0.0.0:*            users:(("chronyd",pid=659,fd=5))
udp          UNCONN         0              0                              [::1]:323                          [::]:*            users:(("chronyd",pid=659,fd=6))
👉tcp          LISTEN         0              511                          0.0.0.0:80                        0.0.0.0:*            users:(("nginx",pid=11280,fd=6),("nginx",pid=11279,fd=6))
tcp          LISTEN         0              128                          0.0.0.0:22                        0.0.0.0:*            users:(("sshd",pid=677,fd=3))
tcp          LISTEN         0              511                             [::]:80                           [::]:*            users:(("nginx",pid=11280,fd=7),("nginx",pid=11279,fd=7))
tcp          LISTEN         0              128                             [::]:22                           [::]:*            users:(("sshd",pid=677,fd=4))*
```

🌞 Analyse trafic

[tp5_web.pcapng](tp5_web.pcapng)

[tp5_webHTTP.pcapng](tp5_webHTTP.pcapng)
