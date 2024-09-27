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
Stats: 0:00:22 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 7.36% done; ETC: 17:40 (0:04:37 remaining)
Stats: 0:00:26 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 8.11% done; ETC: 17:40 (0:04:43 remaining)
Stats: 0:00:28 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 8.33% done; ETC: 17:41 (0:04:57 remaining)
Stats: 0:00:46 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 12.11% done; ETC: 17:42 (0:05:34 remaining)
Stats: 0:01:18 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 21.94% done; ETC: 17:41 (0:04:37 remaining)
Stats: 0:01:42 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 38.16% done; ETC: 17:40 (0:02:45 remaining)
Stats: 0:02:20 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 77.15% done; ETC: 17:38 (0:00:41 remaining)
Stats: 0:02:23 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 78.82% done; ETC: 17:38 (0:00:38 remaining)
Stats: 0:02:38 elapsed; 0 hosts completed (0 up), 4095 undergoing ARP Ping Scan
ARP Ping Scan Timing: About 97.37% done; ETC: 17:38 (0:00:04 remaining)
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
MAC Address: 26:8D:A5:2E:1A:A6 (Unknown)
Nmap scan report for 10.33.71.221
Host is up (0.16s latency).
MAC Address: 80:A9:97:2F:F2:3F (Apple)
Nmap scan report for 10.33.71.232
Host is up (0.041s latency).
MAC Address: A0:59:50:ED:0F:CB (Intel Corporate)
Nmap scan report for 10.33.72.92
Host is up (0.0040s latency).
MAC Address: 56:96:18:4C:10:2A (Unknown)
Nmap scan report for 10.33.72.126
Host is up (0.11s latency).
MAC Address: 3C:E9:F7:CF:26:FF (Intel Corporate)
Nmap scan report for 10.33.72.143
Host is up (0.015s latency).
MAC Address: F4:6D:3F:BD:AE:D9 (Intel Corporate)
Nmap scan report for 10.33.72.146
Host is up (0.0070s latency).
MAC Address: D8:80:83:CE:ED:5B (Cloud Network Technology Singapore PTE.)
Nmap scan report for 10.33.72.162
Host is up (0.0076s latency).
MAC Address: D4:D8:53:CC:75:9A (Intel Corporate)
Nmap scan report for 10.33.72.164
Host is up (0.0050s latency).
MAC Address: 84:7B:57:F3:5C:7D (Intel Corporate)
Nmap scan report for 10.33.72.165
Host is up (0.013s latency).
MAC Address: F0:B6:1E:08:05:7F (Intel Corporate)
Nmap scan report for 10.33.72.172
Host is up (0.0040s latency).
MAC Address: A0:29:42:24:B0:3F (Intel Corporate)
Nmap scan report for 10.33.72.192
Host is up (0.012s latency).
MAC Address: 44:E5:17:07:1B:24 (Intel Corporate)
Nmap scan report for 10.33.72.211
Host is up (0.0040s latency).
MAC Address: DC:8B:28:2E:C6:D8 (Intel Corporate)
Nmap scan report for 10.33.72.213
Host is up (0.0080s latency).
MAC Address: 28:39:26:A2:7A:71 (CyberTAN Technology)
Nmap scan report for 10.33.72.225
Host is up (0.0080s latency).
MAC Address: 64:D6:9A:E8:55:6C (Intel Corporate)
Nmap scan report for 10.33.72.227
Host is up (0.0050s latency).
MAC Address: F4:C8:8A:D6:D8:50 (Intel Corporate)
Nmap scan report for 10.33.72.232
Host is up (0.078s latency).
MAC Address: 82:83:E9:BA:1D:38 (Unknown)
Nmap scan report for 10.33.72.235
Host is up (0.12s latency).
MAC Address: CE:C2:56:17:2C:62 (Unknown)
Nmap scan report for 10.33.72.246
Host is up (0.088s latency).
MAC Address: C0:95:6D:30:48:98 (Apple)
Nmap scan report for 10.33.73.6
Host is up (0.78s latency).
MAC Address: 10:6F:D9:4E:21:11 (Cloud Network Technology Singapore PTE.)
Nmap scan report for 10.33.73.74
Host is up (0.28s latency).
MAC Address: A4:CF:99:98:08:79 (Apple)
Nmap scan report for 10.33.73.129
Host is up (1.3s latency).
MAC Address: 32:A3:7F:18:55:E1 (Unknown)
Nmap scan report for 10.33.73.137
Host is up (0.067s latency).
MAC Address: BA:64:01:0B:7B:F0 (Unknown)
Nmap scan report for 10.33.73.151
Host is up (0.025s latency).
MAC Address: 72:94:F2:8E:C8:36 (Unknown)
Nmap scan report for 10.33.73.159
Host is up (0.16s latency).
MAC Address: FE:92:CC:26:0E:B3 (Unknown)
Nmap scan report for 10.33.73.188
Host is up (0.22s latency).
MAC Address: B2:C1:5A:4D:AD:69 (Unknown)
Nmap scan report for 10.33.73.190
Host is up (0.078s latency).
MAC Address: 86:EB:C2:F5:89:A5 (Unknown)
Nmap scan report for 10.33.73.194
Host is up (0.046s latency).
MAC Address: 76:AC:82:34:87:8E (Unknown)
Nmap scan report for 10.33.73.199
Host is up (0.0040s latency).
MAC Address: 30:89:4A:0F:4F:1A (Intel Corporate)
Nmap scan report for 10.33.73.208
Host is up (0.0050s latency).
MAC Address: 00:91:9E:4B:AA:55 (Intel Corporate)
Nmap scan report for 10.33.73.218
Host is up (0.11s latency).
MAC Address: 8A:BB:06:B3:A9:23 (Unknown)
Nmap scan report for 10.33.73.219
Host is up (1.3s latency).
MAC Address: F6:99:7B:56:F4:56 (Unknown)
Nmap scan report for 10.33.73.221
Host is up (0.88s latency).
MAC Address: 5A:D6:9D:0D:B4:FE (Unknown)
Nmap scan report for 10.33.73.227
Host is up (0.036s latency).
MAC Address: 2A:4B:7F:A4:0D:EE (Unknown)
Nmap scan report for 10.33.74.3
Host is up (0.80s latency).
MAC Address: DA:1F:DD:60:E4:70 (Unknown)
Nmap scan report for 10.33.74.43
Host is up (1.6s latency).
MAC Address: BA:06:4D:C9:A1:AB (Unknown)
Nmap scan report for 10.33.74.58
Host is up (0.35s latency).
MAC Address: 74:A6:CD:9E:B1:39 (Apple)
Nmap scan report for 10.33.74.72
Host is up (0.0070s latency).
MAC Address: A8:41:F4:8D:B7:13 (AzureWave Technology)
Nmap scan report for 10.33.74.95
Host is up (1.3s latency).
MAC Address: 26:26:A6:A7:E3:A1 (Unknown)
Nmap scan report for 10.33.74.126
Host is up (0.089s latency).
MAC Address: 0E:4C:2A:7E:F7:BD (Unknown)
Nmap scan report for 10.33.74.251
Host is up (0.016s latency).
MAC Address: E4:C7:67:B5:A2:3E (Intel Corporate)
Nmap scan report for 10.33.75.111
Host is up (0.013s latency).
MAC Address: E6:04:42:6A:0E:F4 (Unknown)
Nmap scan report for 10.33.76.16
Host is up (0.12s latency).
MAC Address: 8A:43:CD:B0:14:13 (Unknown)
Nmap scan report for 10.33.76.22
Host is up (0.55s latency).
MAC Address: 10:9F:41:E5:B8:51 (Apple)
Nmap scan report for 10.33.76.29
Host is up (0.39s latency).
MAC Address: 02:4C:74:6D:30:35 (Unknown)
Nmap scan report for 10.33.76.42
Host is up (0.17s latency).
MAC Address: 9C:9E:D5:E5:41:C4 (Xiaomi Communications)
Nmap scan report for 10.33.76.43
Host is up (0.13s latency).
MAC Address: A8:41:F4:EA:29:B5 (AzureWave Technology)
Nmap scan report for 10.33.76.55
Host is up (0.010s latency).
MAC Address: A2:E2:9F:32:4F:E0 (Unknown)
Nmap scan report for 10.33.76.69
Host is up (0.83s latency).
MAC Address: 9A:2A:96:A9:FB:DF (Unknown)
Nmap scan report for 10.33.76.79
Host is up (1.1s latency).
MAC Address: 16:4A:66:0F:8D:11 (Unknown)
Nmap scan report for 10.33.76.82
Host is up (0.011s latency).
MAC Address: 1E:0C:13:74:7E:B2 (Unknown)
Nmap scan report for 10.33.76.100
Host is up (0.10s latency).
MAC Address: AA:36:F3:52:ED:2F (Unknown)
Nmap scan report for 10.33.76.101
Host is up (0.039s latency).
MAC Address: 26:0E:47:BA:39:43 (Unknown)
Nmap scan report for 10.33.76.103
Host is up (0.80s latency).
MAC Address: F2:CB:C6:D7:88:45 (Unknown)
Nmap scan report for 10.33.76.125
Host is up (0.047s latency).
MAC Address: A6:AF:CA:57:C1:98 (Unknown)
Nmap scan report for 10.33.76.145
Host is up (1.2s latency).
MAC Address: 20:91:DF:E0:D6:B6 (Apple)
Nmap scan report for 10.33.76.148
Host is up (0.33s latency).
MAC Address: 84:94:37:DD:E7:04 (Apple)
Nmap scan report for 10.33.76.151
Host is up (0.076s latency).
MAC Address: BE:86:A1:E1:E7:4D (Unknown)
Nmap scan report for 10.33.76.152
Host is up (0.072s latency).
MAC Address: 3A:47:F8:6A:82:04 (Unknown)
Nmap scan report for 10.33.76.153
Host is up (0.63s latency).
MAC Address: 32:AC:CC:4A:97:60 (Unknown)
Nmap scan report for 10.33.76.155
Host is up (0.099s latency).
MAC Address: CE:E0:40:DC:31:A7 (Unknown)
Nmap scan report for 10.33.76.178
Host is up (0.021s latency).
MAC Address: 0A:2B:83:B8:E3:AA (Unknown)
Nmap scan report for 10.33.77.10
Host is up (0.61s latency).
MAC Address: E6:BE:14:6C:1E:37 (Unknown)
Nmap scan report for 10.33.77.26
Host is up (0.027s latency).
MAC Address: 98:BD:80:D5:2B:BD (Intel Corporate)
Nmap scan report for 10.33.77.42
Host is up (0.0030s latency).
MAC Address: B0:7D:64:45:64:EC (Intel Corporate)
Nmap scan report for 10.33.77.44
Host is up (0.22s latency).
MAC Address: A0:80:69:B7:C9:7E (Intel Corporate)
Nmap scan report for 10.33.77.46
Host is up (0.73s latency).
MAC Address: 72:73:E7:4A:D3:FA (Unknown)
Nmap scan report for 10.33.77.48
Host is up (0.0040s latency).
MAC Address: A0:B3:39:66:01:25 (Intel Corporate)
Nmap scan report for 10.33.77.49
Host is up (0.018s latency).
MAC Address: E4:60:17:78:9B:FF (Intel Corporate)
Nmap scan report for 10.33.77.58
Host is up (0.50s latency).
MAC Address: 82:E7:B0:79:9C:58 (Unknown)
Nmap scan report for 10.33.77.62
Host is up (1.6s latency).
MAC Address: D6:F9:93:20:C1:12 (Unknown)
Nmap scan report for 10.33.77.64
Host is up (0.58s latency).
MAC Address: C0:95:6D:21:03:3A (Apple)
Nmap scan report for 10.33.77.73
Host is up (1.6s latency).
MAC Address: 5E:19:CD:85:77:43 (Unknown)
Nmap scan report for 10.33.77.74
Host is up (0.018s latency).
MAC Address: C2:BC:42:27:5B:3B (Unknown)
Nmap scan report for 10.33.77.76
Host is up (0.0040s latency).
MAC Address: 10:91:D1:24:76:C9 (Intel Corporate)
Nmap scan report for 10.33.77.79
Host is up (0.067s latency).
MAC Address: C2:71:2E:8F:43:65 (Unknown)
Nmap scan report for 10.33.77.105
Host is up (0.0060s latency).
MAC Address: 80:19:34:04:BA:B9 (Intel Corporate)
Nmap scan report for 10.33.77.138
Host is up (0.035s latency).
MAC Address: 3E:4A:B1:1F:61:34 (Unknown)
Nmap scan report for 10.33.77.142
Host is up (0.034s latency).
MAC Address: 16:9E:67:34:38:8F (Unknown)
Nmap scan report for 10.33.77.149
Host is up (0.039s latency).
MAC Address: 80:30:49:40:9B:8B (Liteon Technology)
Nmap scan report for 10.33.77.151
Host is up (0.014s latency).
MAC Address: D4:D8:53:78:45:B0 (Intel Corporate)
Nmap scan report for 10.33.77.152
Host is up (0.022s latency).
MAC Address: D4:D8:53:78:45:B2 (Intel Corporate)
Nmap scan report for 10.33.77.158
Host is up (0.19s latency).
MAC Address: F4:6A:DD:2C:70:19 (Liteon Technology)
Nmap scan report for 10.33.77.165
Host is up (0.0048s latency).
MAC Address: 34:C9:3D:22:97:2D (Intel Corporate)
Nmap scan report for 10.33.77.171
Host is up (0.0070s latency).
MAC Address: 9C:B6:D0:05:18:DB (Rivet Networks)
Nmap scan report for 10.33.77.176
Host is up (0.20s latency).
MAC Address: E2:B0:9E:38:4F:C3 (Unknown)
Nmap scan report for 10.33.77.177
Host is up (0.0056s latency).
MAC Address: 10:91:D1:24:64:C2 (Intel Corporate)
Nmap scan report for 10.33.77.180
Host is up (0.0060s latency).
MAC Address: 58:1C:F8:22:14:97 (Intel Corporate)
Nmap scan report for 10.33.77.181
Host is up (0.0050s latency).
MAC Address: 58:1C:F8:22:14:99 (Intel Corporate)
Nmap scan report for 10.33.77.183
Host is up (0.039s latency).
MAC Address: EA:0E:AE:3E:F9:CA (Unknown)
Nmap scan report for 10.33.77.186
Host is up (0.0040s latency).
MAC Address: 10:91:D1:07:4E:37 (Intel Corporate)
Nmap scan report for 10.33.77.189
Host is up (0.011s latency).
MAC Address: 04:ED:33:AF:67:20 (Intel Corporate)
Nmap scan report for 10.33.77.190
Host is up (0.0040s latency).
MAC Address: 00:D4:9E:0A:0B:EF (Intel Corporate)
Nmap scan report for 10.33.77.194
Host is up (0.0060s latency).
MAC Address: A0:02:A5:A2:A8:69 (Intel Corporate)
Nmap scan report for 10.33.77.198
Host is up (0.013s latency).
MAC Address: DE:ED:F5:CD:9E:C6 (Unknown)
Nmap scan report for 10.33.77.201
Host is up (0.0070s latency).
MAC Address: 5A:C6:62:F2:D4:C9 (Unknown)
Nmap scan report for 10.33.77.206
Host is up (0.059s latency).
MAC Address: B8:1E:A4:6C:56:97 (Liteon Technology)
Nmap scan report for 10.33.77.214
Host is up (0.074s latency).
MAC Address: 12:40:DA:DB:23:94 (Unknown)
Nmap scan report for 10.33.77.234
Host is up (2.2s latency).
MAC Address: 2E:85:02:B8:C4:5D (Unknown)
Nmap scan report for 10.33.77.244
Host is up (0.0080s latency).
MAC Address: 58:96:71:9C:C7:6E (Wistron Neweb)
Nmap scan report for 10.33.78.0
Host is up (0.0050s latency).
MAC Address: 74:97:79:66:39:B3 (Cloud Network Technology Singapore PTE.)
Nmap scan report for 10.33.78.45
Host is up (0.010s latency).
MAC Address: 50:C2:E8:33:21:19 (Cloud Network Technology Singapore PTE.)
Nmap scan report for 10.33.78.108
Host is up (0.72s latency).
MAC Address: 80:A9:97:20:7F:D0 (Apple)
Nmap scan report for 10.33.78.117
Host is up (0.025s latency).
MAC Address: 68:CA:C4:99:4C:7B (Apple)
Nmap scan report for 10.33.78.119
Host is up (0.011s latency).
MAC Address: 72:E6:EB:37:12:C5 (Unknown)
Nmap scan report for 10.33.78.127
Host is up (0.82s latency).
MAC Address: 82:1A:84:A6:E1:AE (Unknown)
Nmap scan report for 10.33.78.132
Host is up (1.7s latency).
MAC Address: 1A:8A:CE:F3:F7:10 (Unknown)
Nmap scan report for 10.33.78.137
Host is up (0.061s latency).
MAC Address: 64:A2:00:5C:F9:7A (Xiaomi Communications)
Nmap scan report for 10.33.78.147
Host is up (0.086s latency).
MAC Address: 94:BB:43:D9:E1:25 (AzureWave Technology)
Nmap scan report for 10.33.78.188
Host is up (0.27s latency).
MAC Address: E0:1F:88:F5:D7:E2 (Xiaomi Communications)
Nmap scan report for 10.33.78.190
Host is up (0.97s latency).
MAC Address: CE:38:75:32:40:75 (Unknown)
Nmap scan report for 10.33.78.193
Host is up (0.0050s latency).
MAC Address: 36:1B:C3:2D:BB:22 (Unknown)
Nmap scan report for 10.33.78.194
Host is up (0.52s latency).
MAC Address: BA:60:66:B4:33:3F (Unknown)
Nmap scan report for 10.33.78.199
Host is up (0.18s latency).
MAC Address: 22:4A:62:58:A0:03 (Unknown)
Nmap scan report for 10.33.78.200
Host is up (1.3s latency).
MAC Address: 8A:3F:34:DE:2F:76 (Unknown)
Nmap scan report for 10.33.78.202
Host is up (0.27s latency).
MAC Address: DA:9D:30:21:F2:5C (Unknown)
Nmap scan report for 10.33.78.205
Host is up (0.026s latency).
MAC Address: C8:B2:9B:61:44:0F (Intel Corporate)
Nmap scan report for 10.33.78.208
Host is up (0.0030s latency).
MAC Address: BC:03:58:B5:3E:C5 (Intel Corporate)
Nmap scan report for 10.33.78.209
Host is up (1.1s latency).
MAC Address: 0E:40:8F:CD:7C:CD (Unknown)
Nmap scan report for 10.33.78.224
Host is up (0.077s latency).
MAC Address: C2:2B:B5:2E:D7:B2 (Unknown)
Nmap scan report for 10.33.78.227
Host is up (0.16s latency).
MAC Address: 34:B9:8D:CA:C8:E4 (Xiaomi Communications)
Nmap scan report for 10.33.78.231
Host is up (0.028s latency).
MAC Address: 70:D8:23:D5:ED:D2 (Intel Corporate)
Nmap scan report for 10.33.78.239
Host is up (0.018s latency).
MAC Address: C8:8A:9A:E6:33:1D (Intel Corporate)
Nmap scan report for 10.33.78.242
Host is up (0.010s latency).
MAC Address: 4C:82:A9:1C:E3:B7 (Cloud Network Technology Singapore PTE.)
Nmap scan report for 10.33.78.243
Host is up (0.0070s latency).
MAC Address: FE:AE:43:11:71:29 (Unknown)
Nmap scan report for 10.33.78.244
Host is up (0.97s latency).
MAC Address: 82:E7:1E:1D:21:81 (Unknown)
Nmap scan report for 10.33.78.245
Host is up (4.0s latency).
MAC Address: C0:35:32:1C:AF:A9 (Liteon Technology)
Nmap scan report for 10.33.78.246
Host is up (0.31s latency).
MAC Address: 62:7E:0B:FB:EB:BD (Unknown)
Nmap scan report for 10.33.78.251
Host is up (0.18s latency).
MAC Address: 06:41:9B:38:AC:63 (Unknown)
Nmap scan report for 10.33.79.2
Host is up (0.040s latency).
MAC Address: 9E:1F:BE:9E:E2:74 (Unknown)
Nmap scan report for 10.33.79.7
Host is up (0.12s latency).
MAC Address: 2A:7A:A1:70:DD:B6 (Unknown)
Nmap scan report for 10.33.79.8
Host is up (0.97s latency).
MAC Address: C2:1E:93:71:01:BF (Unknown)
Nmap scan report for 10.33.79.254
Host is up (0.0093s latency).
MAC Address: 7C:5A:1C:D3:D8:76 (Sophos)
Nmap scan report for 10.33.77.216
Host is up.
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