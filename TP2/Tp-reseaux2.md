# TP2 : Hey yo tell a neighbor tell a friend

Dans ce TP, on va commencer à construire sur ce qu'on a fait au premier TP.

> ~~On va monter une interco IPsec entre tous les edge routers qui sont pas dans les mêmes zones BGP.~~ Pas du tout, on va encore envoyer des `ping` :d

C'est un TP qui se fait à 2, avec un câble que je vous fournis en cours. Le but : connectez vos deux PCs avec le câble, et faire mumuse.



# Sommaire

- [TP2 : Hey yo tell a neighbor tell a friend](#tp2--hey-yo-tell-a-neighbor-tell-a-friend)
- [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. Simplest LAN](#i-simplest-lan)
  - [0. Setup](#0-setup)
  - [1. Quelques pings](#1-quelques-pings)
- [II. Utilisation des ports](#ii-utilisation-des-ports)
  - [0. Intro blabla](#0-intro-blabla)
  - [1. Animal numérique](#1-animal-numérique)
- [III. Analyse de vos applications usuelles](#iii-analyse-de-vos-applications-usuelles)
  - [1. Serveur web](#1-serveur-web)
  - [2. Autres services](#2-autres-services)

# 0. Prérequis

# I. Simplest LAN

## 0. Setup


## 1. Quelques pings

🌞 **Prouvez que votre configuration est effective**

- avec une commande adaptée, affiche les informations liées à la carte réseau Ethernet
``` 
PS C:\Users\champ> ipconfig /all

Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Realtek PCIe GbE Family Controller
   Adresse physique . . . . . . . . . . . : 58-11-22-E7-ED-25
   DHCP activé. . . . . . . . . . . . . . : Non
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::f58a:d0f0:2eda:8e5e%6(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.1.1.2(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
   IAID DHCPv6 . . . . . . . . . . . : 56103202
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2E-45-09-E6-58-11-22-E7-ED-25
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

- en gros, je vous demande une commande plus spécifique que ipconfig, un truc qui retourne les infos que d'une seule carte réseau, pas toutes

``` 
PS C:\Users\champ> Get-NetIPAddress -InterfaceAlias "Ethernet"

IPAddress         : fe80::f58a:d0f0:2eda:8e5e%6
InterfaceIndex    : 6
InterfaceAlias    : Ethernet
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 10.1.1.2
InterfaceIndex    : 6
InterfaceAlias    : Ethernet
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore
```

🌞 **Tester que votre LAN + votre adressage IP est fonctionnel**

``` 
PS C:\Users\champ> ping 10.1.1.1

Envoi d’une requête 'Ping'  10.1.1.1 avec 32 octets de données :
Réponse de 10.1.1.1 : octets=32 temps=1 ms TTL=128
Réponse de 10.1.1.1 : octets=32 temps=1 ms TTL=128
Réponse de 10.1.1.1 : octets=32 temps=1 ms TTL=128
Réponse de 10.1.1.1 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 10.1.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 1ms, Moyenne = 1ms

```


🌞 **Capture de `ping`**

 voir le fichier ping.pcapng

# II. Utilisation des ports

## 0. Intro blabla



## 1. Animal numérique


🌞 **Sur le PC serveur**

``` 
PS C:\Users\champ\netcat-win32-1.11\netcat-1.11> .\nc
Cmd line: -l -p 8888
``` 

🌞 **Sur le PC serveur toujours**
```
PS C:\Users\champ netstat -a -b -n

TCP    10.1.1.2:8888          10.1.1.1:32253         ESTABLISHED
[nc.exe]

```

🌞 **Sur le PC client**

``` 
PS C:\Users\champ\netcat-win32-1.11\netcat-1.11> .\nc 10.1.1.1 6666
```

🌞 **Echangez-vous des messages**

``` 
PS C:\Users\champ\netcat-win32-1.11\netcat-1.11> .\nc 10.1.1.1 6666
hfvkhuvbkjvkhvjhvuhvuuhv
choucroute
PTDR T KI
TG
non
fils de tchoin
NUH NUH
manj té mor
choucette au FROMAGE !!!!!!!!!!

```

🌞 **Utilisez une commande qui permet de voir la connexion en cours**

version moi en client 
``` 
netstat

TCP    10.1.1.2:30458         10.1.1.1:6666          ESTABLISHED
[nc.exe]

```

version moi en serveur 

``` 
TCP    10.1.1.2:8888          Laptop-Georges:32253   ESTABLISHED
```


🌞 **Faites une capture Wireshark complète d'un échange**
version moi en client
```
voir le fichier netcat1.pcapng
```

version moi en serveur
```
voir le fichier netcat2.pcapng
```

🌞 **Inversez les rôles**



# III. Analyse de vos applications usuelles

## 1. Serveur web


➜ **Connectez-vous à un site en HTTP**



🌞 **Utilisez Wireshark pour capturer du trafic HTTP**

```
voir le fichier HTTP.pcapng 

```

## 2. Autres services



➜ **Choisissez 5 applications, quelques idées :**
```
opera gx
discord
titanfall 2
steam
helldivers 2
```



🌞 **Pour les 5 applications**

opera gx
``` 
netstat

TCP    10.33.77.216:63976     173.194.76.188:5228    ESTABLISHED
[opera.exe]

voir le fichier opera_gx.pcapng

```



discord
``` 
netstat

TCP    10.33.77.216:42769     162.159.133.234:443    ESTABLISHED
[Discord.exe]

voir le fichier discord.pcapng

```

titanfall 2
``` 
netstat

[Titanfall2.exe]
UDP    [::]:37015    

voir le fichier titanfall_2.pcapng

```
steam
``` 
netstat

TCP    10.33.77.216:42831     185.25.182.52:27035    ESTABLISHED
[steam.exe]

voir le fichier steam.pcapng

```


helldivers 2
``` 
netstat

TCP    10.33.77.216:5380      20.120.128.232:443     ESTABLISHED
[helldivers2.exe]

voir le fichier helldivers2.pcapng

```
