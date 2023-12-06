# TP6 : Un LAN maîtrisé meo ?

## I. Setup routeur

## II. Serveur DNS

1. Présentation

2. Setup

🌞 Dans le rendu, je veux

```[vince@localhost ~]$ sudo cat /etc/named.conf
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };

        recursion yes;

        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};



logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "tp6.b1" IN {
        type master;
        file "tp6.b1.db";
        allow-update { none; };
        allow-query {any; };
};

zone "1.4.10.in-addr.arpa" IN {
        type master;
        file "tp6.b1.rev";
        allow-update { none; };
        allow-query { any; };
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```


```[vince@localhost ~]$ sudo cat /var/named/tp6.b1.db

$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800
    3600
    1800
    604800
    86400
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns       IN A 10.6.1.101
john      IN A 10.6.1.11
```

```
[vince@localhost ~]$ sudo cat /var/named/tp6.b1.rev
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800
    3600
    1800
    604800
    86400
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Reverse lookup
101 IN PTR dns.tp6.b1.
11 IN PTR john.tp6.b1.`
```

``
[vince@localhost ~]$ sudo systemctl status named
● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; disabled; preset: disabled)
     Active: active (running) since Fri 2023-11-17 12:41:28 CET; 5 days ago
    Process: 2170 ExecStartPre=/bin/bash -c if [ ! "$DISABLE_ZONE_CHECKING" == "yes" ]; then /usr/sbin/named-checkconf -z "$NAMEDCONF"; else echo "Checking of zone files is disabled"; fi (code=exited, status=0/SUCCESS)
    Process: 2172 ExecStart=/usr/sbin/named -u named -c ${NAMEDCONF} $OPTIONS (code=exited, status=0/SUCCESS)
   Main PID: 2173 (named)
      Tasks: 5 (limit: 4674)
     Memory: 16.6M
        CPU: 2.799s
     CGroup: /system.slice/named.service
             └─2173 /usr/sbin/named -u named -c /etc/named.conf
``

```
[vince@localhost ~]$ ss -nl
tcp                LISTEN              0                   10                                                                127.0.0.1:53                                          0.0.0.0:*
```

🌞 Ouvrez le bon port dans le firewall

```
[vince@localhost ~]$  sudo firewall-cmd --add-port=53/udp --permanent
[sudo] password for vince:
success
[vince@localhost ~]$ sudo firewall-cmd --reload
success
```


3. Test

🌞 Sur la machine john.tp6.b1

```
[vince@johntp6 ~]$ dig john.tp6

; <<>> DiG 9.16.23-RH <<>> john.tp6
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 7368
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
```

```
[vince@johntp6 ~]$ dig ynov.com
;; ANSWER SECTION:
ynov.com.               300     IN      A       172.67.74.226
ynov.com.               300     IN      A       104.26.10.233
ynov.com.               300     IN      A       104.26.11.233
```


🌞 Sur votre PC

```
PS C:\Users\vince> nslookup john.tp6.b1 dns.tp6
```
