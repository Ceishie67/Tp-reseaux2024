# TP1 : Les premiers pas de bébé B1

![Alt text](image.png)

## I. Récolte d'informations

### 🌞 Adresses IP de ta machine

```
PS C:\Users\champ> ipconfig /all

Affiche l'adresse IP que ta machine a sur sa carte réseau WiFi

 Adresse IPv4. . . . . . . . . . . . . .: 10.33.77.216

Affiche l'adresse IP que ta machine a sur sa carte réseau ethernet

adresse IP  : il y en a pas 

```

### 🌞 Si t'as un accès internet normal, d'autres infos sont forcément dispos...

```
PS C:\Users\champ> ipconfig /all

Affiche l'adresse IP de la passerelle du réseau local

    Passerelle par défaut. . . . . . . . . : 10.33.79.254

Affiche l'adresse IP du serveur DNS que connaît ton PC

    Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8

Affiche l'adresse IP du serveur DHCP que connaît ton PC

    Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
```

## II. Utiliser le réseau

### 🌞 Envoie un ping vers...

#### toi-même !

```
PS C:\Users\champ> ping 10.33.77.216

Envoi d’une requête 'Ping'  10.33.77.216 avec 32 octets de données :
Réponse de 10.33.77.216 : octets=32 temps<1ms TTL=128
Réponse de 10.33.77.216 : octets=32 temps<1ms TTL=128
Réponse de 10.33.77.216 : octets=32 temps<1ms TTL=128
Réponse de 10.33.77.216 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 10.33.77.216:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms

```

#### vers l'adresse IP 127.0.0.1

``` 
PS C:\Users\champ> ping 127.0.0.1

Envoi d’une requête 'Ping'  127.0.0.1 avec 32 octets de données :
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 127.0.0.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```

### 🌞 On continue avec ping. Envoie un ping vers...

#### Ta passerelle

```
PS C:\Users\champ> ping 10.33.79.254

Envoi d’une requête 'Ping'  10.33.79.254 avec 32 octets de données :
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.

Statistiques Ping pour 10.33.79.254:
    Paquets : envoyés = 4, reçus = 0, perdus = 4 (perte 100%),
```
#### un(e) pote sur le réseau

``` 
PS C:\Users\champ> ping 10.33.77.149

Envoi d’une requête 'Ping'  10.33.77.149 avec 32 octets de données :
Réponse de 10.33.77.149 : octets=32 temps=80 ms TTL=128
Réponse de 10.33.77.149 : octets=32 temps=88 ms TTL=128
Réponse de 10.33.77.149 : octets=32 temps=11 ms TTL=128
Réponse de 10.33.77.149 : octets=32 temps=6 ms TTL=128

Statistiques Ping pour 10.33.77.149:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 6ms, Maximum = 88ms, Moyenne = 46ms
```

#### un site internet

``` 
PS C:\Users\champ> ping ynov.com

Envoi d’une requête 'ping' sur ynov.com [172.67.74.226] avec 32 octets de données :
Réponse de 172.67.74.226 : octets=32 temps=27 ms TTL=55
Réponse de 172.67.74.226 : octets=32 temps=24 ms TTL=55
Réponse de 172.67.74.226 : octets=32 temps=22 ms TTL=55
Réponse de 172.67.74.226 : octets=32 temps=15 ms TTL=55

Statistiques Ping pour 172.67.74.226:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 15ms, Maximum = 27ms, Moyenne = 22ms
```
### 🌞 Faire une requête DNS à la main

``` 
PS C:\Users\champ> nslookup www.thinkerview.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    www.thinkerview.com
Addresses:  2a06:98c1:3121::7
          2a06:98c1:3120::7
          188.114.96.7
          188.114.97.7
```

```
PS C:\Users\champ> nslookup www.wikileaks.org
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    wikileaks.org
Addresses:  51.159.197.136
          80.81.248.21
Aliases:  www.wikileaks.org
```

```
PS C:\Users\champ> nslookup www.torproject.org
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    www.torproject.org
Addresses:  2a01:4f8:fff0:4f:266:37ff:fe2c:5d19
          2a01:4f8:fff0:4f:266:37ff:feae:3bbc
          2620:7:6002:0:466:39ff:fe32:e3dd
          2620:7:6002:0:466:39ff:fe7f:1826
          2a01:4f9:c010:19eb::1
          116.202.120.166
          95.216.163.36
          116.202.120.165
          204.8.99.146
          204.8.99.144
```

## III. Sniffer le réseau

### 🌞 J'attends dans le dépôt git de rendu un fichier ping.pcap

voir fichier ping.pcap

### 🌞 Livrez un deuxième fichier : dns.pcap

```

PS C:\Users\champ> nslookup www.torproject.org

voir fichier DNS1.pcap
```

```

PS C:\Users\champ> nslookup www.wikileaks.org

voir fichier DNS2.pcap
```

```

PS C:\Users\champ> nslookup www.thinkerview.com

voir fichier DNS3.pcap
```

## IV. Network scanning et adresses IP

### 🌞 Effectue un scan du réseau auquel tu es connecté


```
nmap -sn -PR 10.33.64.0/20
Starting Nmap 7.95 ( https://nmap.org ) at 2024-09-27 17:35 Paris, Madrid (heure dÆÚtÚ)
Nmap scan report for 10.33.66.78
Host is up (0.044s latency).
MAC Address: E4:B3:18:48:36:68 (Intel Corporate)
Nmap scan report for 10.33.69.82
Host is up (0.45s latency).
MAC Address: 50:A6:D8:A9:55:02 (Apple)
Nmap scan report for 10.33.69.157
Host is up (0.088s latency).
MAC Address: 3A:71:A3:B0:D6:76 (Unknown)
Nmap scan report for 10.33.70.32
Host is up (0.093s latency).
MAC Address: 9C:FC:E8:37:07:94 (Intel Corporate)
Nmap scan report for 10.33.70.41
Host is up (0.10s latency).
MAC Address: CE:71:C6:DF:63:76 (Unknown)
Nmap scan report for 10.33.71.156
Host is up (0.040s latency).
MAC Address: FA:F1:D8:3F:F4:CF (Unknown)
Nmap scan report for 10.33.71.182
Host is up (0.11s latency).
MAC Address: 5C:E9:1E:B3:EB:86 (Apple)
Nmap scan report for 10.33.71.187
Host is up (0.083s latency).
MAC Address: 60:F2:62:D2:D8:1D (Intel Corporate)
Nmap scan report for 10.33.71.195
Host is up (0.037s latency).
MAC Address: D4:F3:2D:6F:9E:38 (Intel Corporate)
Nmap scan report for 10.33.71.209
Host is up (0.0040s latency).
MAC Address: BC:09:1B:FA:7D:37 (Intel Corporate)
Nmap scan report for 10.33.71.215
Host is up (0.013s latency).
MAC Address: 42:15:21:A4:A4:CB (Unknown)
Nmap scan report for 10.33.71.220
Host is up (0.012s latency).
Nmap done: 4096 IP addresses (145 hosts up) scanned in 165.80 seconds
```


### 🌞 Changer d'adresse IP

#### Ancienne adresse IP
``` 
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::b6ce:fb8d:a30a:3eb2%13
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.77.216
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
```

#### Nouvelle adresse IP 


``` 
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::b6ce:fb8d:a30a:3eb2%13
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.79.3
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . : 10.33.79.254

```