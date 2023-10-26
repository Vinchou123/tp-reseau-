# TP2 : Ethernet, IP, et ARP


🌞 Mettez en place une configuration réseau fonctionnelle entre les deux machines


[regarde c'est carrément comme un lien](./ping.pcapng)

## I.Setup IP:
>Adresses IP:  
>PC1: 192.168.11.20 (moi)  
>PC2: 192.168.11.21( mon camadre )  
>Adresses reseau:  
>192.168.11.0
>Adresses Broadcast:  
>192.168.11.254



🌞 Prouvez que la connexion est fonctionnelle entre les deux machines

```

>ping 192.168.11.21

Envoi d’une requête 'Ping'  192.168.11.21 avec 32 octets de données :
Réponse de 192.168.11.21 : octets=32 temps<1ms TTL=128
Réponse de 192.168.11.21 : octets=32 temps<1ms TTL=128
Réponse de 192.168.11.21 : octets=32 temps<1ms TTL=128
Réponse de 192.168.11.21 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 192.168.11.21:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```

## II.ARP my bro

🌞 Check the ARP table
```
>arp -a
Interface : 192.168.11.20 --- 0x7
  Adresse Internet      Adresse physique      Type
  192.168.11.21         a0-36-bc-6c-20-80     dynamique
  192.168.11.23         ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
---------------------------------------------------------------
Gateway:
Interface : 10.33.70.176 --- 0x10
  Adresse Internet      Adresse physique      Type
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
```
```
> arp -a

Interface : 192.168.56.1 --- 0x3
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.11.20 --- 0x7
  Adresse Internet      Adresse physique      Type
  192.168.11.21         a0-36-bc-6c-20-80     dynamique
  192.168.11.23         ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.247.1 --- 0x9
  Adresse Internet      Adresse physique      Type
  192.168.247.254       00-50-56-e0-e5-2a     dynamique
  192.168.247.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.132.1 --- 0xd
  Adresse Internet      Adresse physique      Type
  192.168.132.254       00-50-56-fe-eb-29     dynamique
  192.168.132.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 10.33.70.176 --- 0x10
  Adresse Internet      Adresse physique      Type
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 172.17.176.1 --- 0x3a
  Adresse Internet      Adresse physique      Type
  172.17.191.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

  🌞 Manipuler la table ARP

> arp -d


> arp -a

Interface : 192.168.56.1 --- 0x3
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.11.20 --- 0x7
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.247.1 --- 0x9
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.132.1 --- 0xd
  Adresse Internet      Adresse physique      Type
  192.168.132.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.33.70.176 --- 0x10
  Adresse Internet      Adresse physique      Type
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 172.17.176.1 --- 0x3a
  Adresse Internet      Adresse physique      Type
  172.17.191.255        ff-ff-ff-ff-ff-ff     statique
```

# III. DHCP

>wireshark






