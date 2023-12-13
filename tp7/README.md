# II. SSH

🌞 **Effectuez une connexion SSH en vérifiant le fingerprint**

```
PS C:\Users\vince> ssh vince@10.7.1.11
 SHA256:cQJtY+hEvdBdwSGvApGDEFiXRFHRMZi3HvWki5B8ySY.
```

```
[vince@john ~]$ sudo ssh-keygen -l -f /etc/ssh/ssh_host_ed25519_key
[sudo] password for vince:
256 SHA256:cQJtY+hEvdBdwSGvApGDEFiXRFHRMZi3HvWki5B8ySY /etc/ssh/ssh_host_ed25519_key.pub (ED25519)
```

> Fais l'exo normalement : d'abord vérifie l'empreinte depuis l'interface console de la VM. Pour l'écriture du rendu, tu te co en SSH en validant l'empreinte avec "yes" puis tu tapes de nouveau la commande pour vérifier l'empreinte, et tu peux donc copier/coller le résultat dans le rendu.

## 2. Conf serveur SSH

On va faire un truc très basique, pour manipuler un peu toujours les cartes réseau, les IP, les ports, toussa : on va choisir explicitement l'IP et le port où tourne notre serveur SSH sur `router`.

La configuration du serveur SSH se fait dans le fichier `/etc/ssh/sshd_config`, utilisez `nano` pour le modifier par exemple.

Il faut être admin pour modifier le fichier, il faudra donc préfixer votre commande avec `sudo`.

🌞 **Consulter l'état actuel**

```
[vince@routeur ~]$ ss -lnt
State            Recv-Q           Send-Q                     Local Address:Port                     Peer Address:Port           Process
LISTEN           0                128                              0.0.0.0:22                            0.0.0.0:*
LISTEN           0                128                                 [::]:22                               [::]:*
```

🌞 **Modifier la configuration du serveur SSH**

```
[vince@routeur ~]$ sudo cat /etc/ssh/sshd_config
Port 25000
#AddressFamily any
ListenAddress 10.7.1.254
#ListenAddress ::
```


🌞 **Prouvez que le changement a pris effet**

```
[vince@routeur ~]$ ss -ln
State            Recv-Q           Send-Q                     Local Address:Port                      Peer Address:Port          Process
LISTEN           0                128                           10.7.1.254:25000                          0.0.0.0:*
```
🌞 **N'oubliez pas d'ouvrir ce nouveau port dans le firewall**

```
PS C:\Users\vince> ssh vince@10.7.1.254 -p 25000
vince@10.7.1.254's password:
Last login: Wed Dec 13 19:09:16 2023
```

🌞 **Effectuer une connexion SSH sur le nouveau port**

```
PS C:\Users\vince> ssh vince@10.7.1.254 -p 25000
vince@10.7.1.254's password:
Last login: Wed Dec 13 19:09:16 2023
```

## 3. Connexion par clé



🌞 **Générer une paire de clés**

```
PS C:\Users\vince> ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
```

🌞 **Déposer la clé publique sur une VM**

```vince@Vincenzo MINGW64 ~
$ ssh-copy-id vince@10.7.1.11
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/c/Users/vince/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vince@10.7.1.11's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vince@10.7.1.11'"
and check to make sure that only the key(s) you wanted were added.

```

🌞 **Connectez-vous en SSH à la machine**

```
PS C:\Users\vince> ssh vince@10.7.1.11
Last login: Wed Dec 13 18:51:34 2023 from 10.7.1.1
[vince@john ~]$
```

### C. Changement de fingerprint


🌞 **Supprimer les clés sur la machine `router.tp7.b1`**

```
[vince@john ~]$ sudo rm /etc/ssh/ssh_host_*
[sudo] password for vince:
```
🌞 **Regénérez les clés sur la machine `router.tp7.b1`**

```
[vince@john ~]$ sudo ssh-keygen -A
ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519
```

🌞 **Tentez une nouvelle connexion au serveur**

```
PS C:\Users\vince> ssh vince@10.7.1.11
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:VrJrm/rSxrnV3ZbWvqf2D4JTQhRb5wWIA3aU3gyUzxg.
Please contact your system administrator.
Add correct host key in C:\\Users\\vince/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in C:\\Users\\vince/.ssh/known_hosts:25
Host key for 10.7.1.11 has changed and you have requested strict checking.
Host key verification failed.
```

