# tp reseau

## I. Exploration locale en solo


🌞 Affichez les infos des cartes réseau de votre PC

nom, adresse MAC et adresse IP de l'interface WiFi 
```
PS C:\Users\💢vince> ipconfig /all

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wi-Fi 6E AX211 160MHz
   Adresse physique . . . . . . . . . . . : 💢E0-2E-0B-DF-2E-38 
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::c621:eec3:93a0:17dd%5(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 💢10.33.48.25(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : mercredi 11 octobre 2023 09:06:06
   Bail expirant. . . . . . . . . . . . . : jeudi 12 octobre 2023 09:06:06
   Passerelle par défaut. . . . . . . . . : 10.33.51.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.51.254
   IAID DHCPv6 . . . . . . . . . . . : 81800715
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2C-67-96-CA-00-DE-AB-CA-97-7B
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

nom, adresse MAC et adresse IP de l'interface Ethernet 

Mon pc ne contient pas ce port...  


🌞 Affichez votre gateway

```
 Passerelle par défaut. . . . . . . . . : 10.33.51.254
```

🌞 Déterminer la MAC de la passerelle

```10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique```

🌞 Il est possible que vous perdiez l'accès internet.

🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)

Aller dans les paramètres/Réseau et internet/WI-FI/WI-FI@YNOV

🌞 Utilisez l'interface graphique de votre OS pour changer d'adresse IP : 

```Carte réseau sans fil Wi-Fi : 

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::c621:eec3:93a0:17dd%5
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.255
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Passerelle par défaut. . . . . . . . . : 10.33.51.254
   ``````

🌞 Il est possible que vous perdiez l'accès internet.

Il est possible de perdre l'accès internet si par le plus grand des hasards nous avions choisit le même octet qu'une même personne. Ainsi c'est comme si nous avions la même adresse IP que cette personne. 

## II. Exploration locale en duo



III. Manipulations d'autres outils/protocoles côté client


🌞Exploration du DHCP, depuis votre PC

```Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wi-Fi 6E AX211 160MHz
   Adresse physique . . . . . . . . . . . : E0-2E-0B-DF-2E-38
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::c621:eec3:93a0:17dd%5(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 💢10.33.48.25(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : lundi 16 octobre 2023 10:34:56
   Bail expirant. . . . . . . . . . . . . : 💢mardi 17 octobre 2023 09:13:25
   Passerelle par défaut. . . . . . . . . : 10.33.51.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.51.254
   IAID DHCPv6 . . . . . . . . . . . : 81800715
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2C-67-96-CA-00-DE-AB-CA-97-7B
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
   ``````

Le DHCP ( protocole de configuration dynamique des hôtes) consiste à configurer automatiquement les paramètres IP d'une machine pour lui attribuer une adresse IP et un masque de sous réseau.

🌞** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**

  ```
  Serveurs DNS. . .  . . . . . . . . . . :💢 10.33.10.2
                                            8.8.8.8
```


🌞 Utiliser, en ligne de commande l'outil nslookup (Windows, MacOS) ou dig (GNU/Linux, MacOS) pour faire des requêtes DNS à la main
```PS C:\Users\vince> nslookup google.com 8.8.8.8
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:818::200e
          142.250.179.110
```

```PS C:\Users\vince> nslookup ynov.com 8.8.8.8
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::ac43:4ae2
          2606:4700:20::681a:be9
          2606:4700:20::681a:ae9
          104.26.10.233
          104.26.11.233
          172.67.74.226
```

IV. Wireshark

🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

>Nom :  
>Carte réseau sans fil Wi-Fi  
>Adresses IP :   
>10.33.48.115  
>Adresse MAC :   
>F4-C8-8A-E2-B6-BB
>Gateway/Passerelle :  
>10.33.51.254
>Adresse MAC Passerelle :  
>7c-5a-1c-cb-fd-a4  
 ### 2. Modifications des informations
 >Changement de l'adresse IP avec l'interface graphique.

 ## DNS

 ``` 
 >ipconfig /all
 Serveur DHCP . . . . . . . . . . . . . : 192.168.45.151
 ```

 ```
>nslookup google.com
Serveur :   UnKnown
Address:  192.168.45.151

Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:80c::200e
          172.217.20.174

>nslookup ynov.com
Serveur :   UnKnown
Address:  192.168.45.151

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:ae9
          2606:4700:20::ac43:4ae2
          2606:4700:20::681a:be9
          104.26.11.233
          172.67.74.226
          104.26.10.233

PS C:\WINDOWS\system32> nslookup 231.34.113.12
Serveur :   UnKnown
Address:  192.168.45.151

*** UnKnown ne parvient pas à trouver 231.34.113.12 : Non-existent domain
>nslookup 78.34.2.17
Serveur :   UnKnown
Address:  192.168.45.151

Nom :    cable-78-34-2-17.nc.de
Address:  78.34.2.17
 ```