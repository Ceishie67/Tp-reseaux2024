# TP3 : 32°13'34"N 95°03'27"W

Un TP3 plutôt court dédié au protocole ARP.


## Sommaire

- [TP3 : 32°13'34"N 95°03'27"W](#tp3--321334n-950327w)
  - [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. ARP basics](#i-arp-basics)
- [II. ARP dans un réseau local](#ii-arp-dans-un-réseau-local)
  - [1. Basics](#1-basics)
  - [2. ARP](#2-arp)
  - [3. Bonus : ARP poisoning](#3-bonus--arp-poisoning)

# 0. Prérequis

- **tout se fait en ligne de commande**, sauf si c'est explicitement précisément autrement
- je tente un truc risqué, ceux du matin, vous servez de crash test
  - **vous allez utiliser vos partages de co dans la partie II**
  - **partie II qui se fait à 3 ou 4** (pas + SVP, c'est déjà le cirque à 4)
  - si ça fonctionne pas ce sera backup plan avec les câbles :c

# I. ARP basics


☀️ **Avant de continuer...**

- affichez l'adresse MAC de votre carte WiFi !
``` 
PS C:\Users\champ> ipconfig /all

Adresse physique . . . . . . . . . . . : 2C-3B-70-6B-D7-F1
```

☀️ **Affichez votre table ARP**

- allez vous commencez à devenir grands, je vous donne pas la commande, demande à m'sieur internet !
``` 
PS C:\Users\champ> arp -a

Interface : 192.168.56.1 --- 0x3
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.250.1 --- 0xb
  Adresse Internet      Adresse physique      Type
  192.168.250.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.33.77.216 --- 0xd
  Adresse Internet      Adresse physique      Type
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```
☀️ **Déterminez l'adresse MAC de la passerelle du réseau de l'école**

- la passerelle, vous connaissez son adresse IP normalement (cf TP1 ;) )
- si vous avez un accès internet, votre PC a forcément l'adresse MAC de la passerelle dans sa table ARP
``` 
PS C:\Users\champ> arp -a

10.33.79.254          7c-5a-1c-d3-d8-76    
```
☀️ **Supprimez la ligne qui concerne la passerelle**

- une commande pour supprimer l'adresse MAC de votre table ARP
```
après la commande
PS C:\Users\champ> arp -d 10.33.79.254 ; arp -a
```


- si vous ré-affichez votre table ARP, y'a des chances que ça revienne presque tout de suite !

☀️ **Prouvez que vous avez supprimé la ligne dans la table ARP**

- en affichant la table ARP
```
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```

- si la ligne est déjà revenue, déconnecte-toi temporairement du réseau de l'école, et supprime-la de nouveau

☀️ **Wireshark**

- capture `arp1.pcap`
- lancez une capture Wireshark, puis supprimez la ligne de la passerelle dans la table ARP pendant que la capture est en cours
- la capture doit contenir uniquement 2 trames :
  - un ARP request que votre PC envoie pour apprendre l'adresse MAC de la passerelle
  - et la réponse
```
voir le fichier arp1.pcapng
```


> *Si vous les voyez pas, ça veut dire que votre PC n'utilise pas du tout de connexion vers l'extérieur, genre vers internet. Ce qui paraît invraisemblable on est d'accord non ? Vous avez vu ce que ça fait avec `netstat` aux TPs précédents quand on affiche toutes les connexions réseau en cours ! Très peu de chance que votre PC ne contacte pas internet toutes les quelques secondes.*

# II. ARP dans un réseau local

> ⚠️ **Avant de continuer, regroupez-vous par 3 ou 4, et connectez-vous au même partage de co avec le téléphone de l'un d'entre vous.** Si ça fonctionne pas, vous pouvez faire cette partie sur le réseau de l'école mais moins bieng.

Dans cette situation, le téléphone agit comme une "box" :

- **c'est un switch**
  - il permet à tout le monde d'être connecté à un même réseau local
- **c'est aussi un routeur**
  - il fait passer les paquets du réseau local, à internet
  - et vice-versa
  - on dit que ce routeur est la **passerelle** du réseau local

## 1. Basics

> ⚠️ **Avant de continuer, regroupez-vous par 3 ou 4, et connectez-vous au même partage de co avec le téléphone de l'un d'entre vous.** Si ça fonctionne pas, vous pouvez faire cette partie sur le réseau de l'école mais moins bieng.

☀️ **Déterminer**

- pour la carte réseau impliquée dans le partage de connexion (carte WiFi ?)
- son adresse IP au sein du réseau local formé par le partage de co
``` 
PS C:\Users\champ> ipconfig /all

Adresse IPv4. . . . . . . . . . . . . .: 192.168.34.10
```
- son adresse MAC
``` 
PS C:\Users\champ> ipconfig /all

Adresse physique . . . . . . . . . . . : 2C-3B-70-6B-D7-F1
```

☀️ **DIY**

- changer d'adresse IP
  - vous pouvez le faire en interface graphique
- faut procéder comme au TP1 :
  - vous respectez les informations que vous connaissez du réseau
  - remettez la même passerelle, le même masque, et le même serveur DNS
  - changez seulement votre adresse IP, ne changez que le dernier nombre
- prouvez que vous avez bien changé d'IP
  - avec une commande !

```
PS C:\Users\champ> ipconfig

Adresse IPv4. . . . . . . . . . . . . .: 192.168.34.20
```

☀️ **Pingz !**

- vérifiez que vous pouvez tous vous `ping` avec ces adresses IP
``` 
PS C:\Users\champ> ping 192.168.34.100

Envoi d’une requête 'Ping'  192.168.34.100 avec 32 octets de données :
Réponse de 192.168.34.100 : octets=32 temps=157 ms TTL=128
Réponse de 192.168.34.100 : octets=32 temps=58 ms TTL=128
Réponse de 192.168.34.100 : octets=32 temps=2 ms TTL=128
Réponse de 192.168.34.100 : octets=32 temps=3 ms TTL=128

Statistiques Ping pour 192.168.34.100:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 2ms, Maximum = 157ms, Moyenne = 55ms
PS C:\Users\champ> ping 192.168.34.12

Envoi d’une requête 'Ping'  192.168.34.12 avec 32 octets de données :
Réponse de 192.168.34.12 : octets=32 temps=102 ms TTL=128
Réponse de 192.168.34.12 : octets=32 temps=45 ms TTL=128
Réponse de 192.168.34.12 : octets=32 temps=205 ms TTL=128
Réponse de 192.168.34.12 : octets=32 temps=164 ms TTL=128

Statistiques Ping pour 192.168.34.12:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 45ms, Maximum = 205ms, Moyenne = 129ms
```
- vérifiez avec une commande `ping` que vous avez bien un accès internet
``` 
PS C:\Users\champ> ping google.com

Envoi d’une requête 'ping' sur google.com [216.58.213.78] avec 32 octets de données :
Réponse de 216.58.213.78 : octets=32 temps=33 ms TTL=115
Réponse de 216.58.213.78 : octets=32 temps=67 ms TTL=115
Réponse de 216.58.213.78 : octets=32 temps=54 ms TTL=115
Réponse de 216.58.213.78 : octets=32 temps=50 ms TTL=115

Statistiques Ping pour 216.58.213.78:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 33ms, Maximum = 67ms, Moyenne = 51ms
```


> *Pour vérifier l'accès internet avec un `ping` il suffit d'envoyer un `ping` vers une adresse IP publique ou un nom de domaine public que vous connaissez. Genre `ping xkcd.com`, ça fonctionne, peu importe, du moment que c'est sur internet.*

## 2. ARP

☀️ **Affichez votre table ARP !**

- normalement, après les `ping`, vous avez appris l'adresse MAC de tous les autres
``` 

Adresse Internet      Adresse physique      Type
192.168.34.12         20-0b-74-37-40-d5     dynamique
192.168.34.100        80-30-49-40-9b-8b     dynamique

```

➜ **Wireshark that**

- lancez tous Wireshark
- videz tous vos tables ARP
- normalement, y'a ~~presque~~ pas de raisons que vos PCs se contactent entre eux spontanément donc les tables ARP devraient rester vides tant que vous faites rien
- à tour de rôle, envoyez quelques `ping` entre vous
- constatez les messages ARP spontanés qui précèdent vos `ping`
  - ARP request
    - envoyé en broadcast `ff:ff:ff:ff:ff:ff`
    - tout le monde les reçoit donc !
  - ARP reply
    - celui qui a été `ping` répond à celui qui a initié le `pingv
    - il l'informe que l'adresse IP qui a été `ping` correspond à son adresse MAC

☀️ **Capture arp2.pcap**

- ne contient que des ARP request et ARP reply
- contient tout le nécessaire pour que votre table ARP soit populée avec l'adresse MAC de tout le monde

``` 

voir le fichier arp2.pcapng
```