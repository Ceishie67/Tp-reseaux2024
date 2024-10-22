# TP7 : On dit chiffrer pas crypter

Dans ce TP on va mettre en place des outils afin de mettre en oeuvre du chiffrement.

Au menuuu :

- serveur Web avec HTTPS
- serveur VPN


## Sommaire

- [TP7 : On dit chiffrer pas crypter](#tp7--on-dit-chiffrer-pas-crypter)
  - [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. Setup](#i-setup)
- [II. Serveur Web](#ii-serveur-web)
- [III. Serveur VPN](#iii-serveur-vpn)

# 0. Prérequis

➜ **une machine Rocky Linux prête à être clonée**

- on s'en servira pour tout ce qui est routeur/serveur etc.
- descendez bien à 1CPU et 1Go de RAM pour économiser des ressources

➜ **votre machine Ubuntu prête à être clonée**

- pour le(s) client(s) toujours !
- idem, limitez les ressources (1CPU, 1Go de RAM, 128Mo mémoire vidéo vu que c'est avec interface graphique)

➜ **Ayez [les mémos](../../cours/memo/README.md) sous le coude !**

# I. Setup

➜ Pour chaque machine

- donnez une IP statique
- pour l'accès internet
  - activer le routage sur le routeur
  - carte NAT sur le routeur
  - définir le routeur comme passerelle pour les autres
  - aussi, n'oubliez pas d'indiquer `1.1.1.1` en serveur DNS partout
- nommez vos machines (configuration du hostname)

# Serveur Web

- [Serveur Web](#serveur-web)
  - [0. Setup](#0-setup)
    - [A. Schéma](#a-schéma)
    - [B. Tableau adressage](#b-tableau-adressage)
  - [1. HTTP](#1-http)
    - [A. Install](#a-install)
    - [B. Configuration](#b-configuration)
    - [C. Tests client](#c-tests-client)
    - [D. Analyze](#d-analyze)
  - [2. On rajoute un S](#2-on-rajoute-un-s)
    - [A. Config](#a-config)
    - [B. Test test test analyyyze](#b-test-test-test-analyyyze)

## 0. Setup

### A. Schéma

![Webz part](./img/part_web.svg)

### B. Tableau adressage

| Machine          | LAN1 `10.7.1.0/24` |
|------------------|--------------------|
| `client1.tp7.b1` | `10.7.1.101`       |
| `web.tp7.b1`     | `10.7.1.11`        |
| `routeur.tp7.b1` | `10.7.1.254`       |

## 1. HTTP

### A. Install

> *Sur `web.tp7.b1`.*

➜ **Installer le service NGINX**

```bash
dnf install -y nginx
```

### B. Configuration

> *Sur `web.tp7.b1`.*

➜ **Créer un fichier de configuration pour un nouveau site web**

- créer un fichier dans le dossier `/etc/nginx/conf.d/` qui a l'extension `.conf`
- on va créer `/etc/nginx/conf.d/sitedefou.tp7.b1.conf`
- avec le contenu suivant (remplacez `<ADRESSE_IP>` par l'adresse IP du serveur web)

```nginx
server {
    # le nom que devront taper les clients pour visiter le site
    server_name   sitedefou.tp7.b1;

    # on indique sur quelle adresse IP et quel port écouter
    listen        <ADRESSE_IP>:80;

    location      / {
        # "root" pour "racine" : l'emplacement de la racine web
        # c'est à dire le dossier qui contient le site web
        root      /var/www/sitedefou.tp7.b1;
    }
}
```

➜ **Créer un nouveau site web :d**

```bash
# on crée le dossier qui contient le site web
sudo mkdir -p /var/www/sitedefou.tp7.b1

# on dév un gros site web
echo "meow !" | sudo tee /var/www/sitedefou.tp7.b1/index.html

# gestion des permissions sur le dossier
sudo chown nginx:nginx -R /var/www/sitedefou.tp7.b1
```

➜ **On démarre le service web NGINX**

```bash
sudo systemctl start nginx
```

🌞 **Lister les ports en écoute sur la machine**

- avec une commande adaptée (voir mémo)
- isolez la ligne qui concerne le service NGINX
```
[nath@web]$ sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1357,fd=6),("nginx",pid=1356,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=1357,fd=7),("nginx",pid=1356,fd=7))
```

🌞 **Ouvrir le port dans le firewall de la machine**

```
[nath@web]$ sudo firewall-cmd --permanent --add-port=80/tcp
success
[nath@web]$ sudo firewall-cmd --reload
success
```

### C. Tests client

> *Sur `client1.tp7.b1`.*

➜ **Ajoutez la ligne suivante au fichier `/etc/hosts`**

```
10.7.1.11 sitedefou.tp7.b1
```

> *Le fichier `/etc/hosts` c'est quand on veut un joli nom qui correspond à une IP, mais qu'on a la flemme de monter un serveur DNS juste pour ça :d*

🌞 **Vérifier que ça a pris effet**

- faites un `ping` vers `sitedefou.tp7.b1`
- visitez `http://sitedefou.tp7.b1`
  - pour le compte-rendu, vous pouvez faire un `curl` depuis le terminal
- **vous devriez avoir la page "meow !" quand vous visitez le site**

### D. Analyze

> *Sur `client1.tp7.b1`.*

🌞 **Capture `tcp_http.pcap`**

- **capturer une session TCP complète**
  - le début (SYN, SYN ACK, ACK)
  - des données échangées (PSH, PSH ACK)
    - en l'occurence ce sera votre requête HTTP
    - et la réponse HTTP du serveur (meow !)
  - une fin de session (ça dépend, le plus souvent : FIN, FIN ACK ou RST)
- cette session TCP doit être celle de l'échange HTTP pour récupérer la page du serveur web
- il faut donc capturer pendant que tu visites le site web

> ⚠️⚠️ *Remarquez que vous pouvez voir en clair le texte "meow !" dans la capture Wireshark. Toute la requête HTTP étouétou.*

🌞 **Voir la connexion établie**

- utilisez une commande `ss` sur le client pour voir le tunnel TCP établi
- il faut taper la commande au moment où vous visitez le site web (ou juste après :d)
- **visitez avec un navigateur** pour le voir, (car `curl` coupe presque instantanément le tunnel, il sera difficile de voir quoique ce soit avec `ss`)

## 2. On rajoute un S

### A. Config

> *Sur `web.tp7.b1`.*

➜ **Générer une paire de clé pour le chiffrement**

- l'une des deux sera stocké dans un certificat
- suivez juste le guide :d

```bash
# générer une paire de clé
openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -keyout server.key -out server.crt
# vous pouvez faire votre meilleur entrée entrée entrée... pour valider les choix par défaut

# déplacer la clé privée dans un endroit moins pourri
# il existe sur tous les OS un dossier destiné à stocker des clés de chiffrement
sudo mv server.key /etc/pki/tls/private/sitedefou.tp7.b1.key
# idem pour le certificat (qui contient la clé publique)
sudo mv server.crt /etc/pki/tls/certs/sitedefou.tp7.b1.crt

# gestion des permissions
sudo chown nginx:nginx /etc/pki/tls/private/sitedefou.tp7.b1.key
sudo chown nginx:nginx /etc/pki/tls/certs/sitedefou.tp7.b1.crt
```

➜ **Modifier la configuration NGINX**

- go modifier votre fichier `/etc/nginx/conf.d/sitedefou.tp7.b1.conf`

```nginx
server {
    server_name   sitedefou.tp7.b1;

    # on change le port d'écoute : 443 la convention pour HTTPS
    # il faut aussi ajouter la mention "ssl" pour activer le chiffrement
    listen        <ADRESSE_IP>:443 ssl;

    # et on indique les chemins vers la clé privée et le certificat
    ssl_certificate /etc/pki/tls/certs/sitedefou.tp7.b1.crt;
    ssl_certificate_key /etc/pki/tls/private/sitedefou.tp7.b1.key;

    location      / {
        root      /var/www/sitedefou.tp7.b1;
    }
}
```

➜ **Redémarrer le service web NGINX !**

```bash
sudo systemctl restart nginx
```
🌞 **Lister les ports en écoute sur la machine**

- avec une commande adaptée (voir mémo)
- isolez la ligne qui concerne le service NGINX qui écoute en HTTPS

🌞 **Gérer le firewall**

- ouvrir le nouveau port sur lequel NGINX écoute pour HTTPS
- fermer l'ancien port qui était utilisé pour HTTP

### B. Test test test analyyyze

> *Sur `client1.tp7.b1`.*

➜ **Tu devrais pouvoir visiter `https://sitedefou.tp7.b1` (bien `https` avec le 's')**

🌞 **Capture `tcp_https.pcap`**

- **capturer une session TCP complète**
  - le début (SYN, SYN ACK, ACK)
  - des données échangées (PSH, PSH ACK)
    - en l'occurence ce sera votre échange TLS
      - Wireshark affichera "TLS" dans la colonne protocole, mais si vous cliquez dessus, vous verrez que c'est bien contenu dans du TCP PSH
    - puis les données HTTP... chiffrées
  - une fin de session (ça dépend, le plus souvent : FIN, FIN ACK ou RST)
- cette session TCP doit être celle de l'échange HTTPS pour récupérer la page du serveur web
- il faut donc capturer pendant que tu visites le site web en HTTPS

> ⚠️⚠️ *Remarquez que ~~vous pouvez voir en clair le texte "meow !"~~ vous voyez plus rien dukoo dans la capture Wireshark. Juste y'a du trafic chiffré qui circule, c'est tout ce qu'on peut dire.*






