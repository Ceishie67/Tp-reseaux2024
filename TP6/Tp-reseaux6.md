# TP6 : Des bo services dans des bo LANs

> Dans ce TP, on construit encore et toujours sur ce qui a déjà été fait.

➜ **Ce qu'on garde des TPs précédents :**

- des VMs partout, et des réseaux privé-hôte (host-only) pour créer nos LANs
- le routeur qui permet à tout le monde de joindre internet
- un serveur DHCP qui propose tout ce qu'il faut aux clients (une IP libre, un DNS joignable, l'adresse de la passerelle)

➜ **Au menu de ce nouveau TP :**

- **deux LANs !**
  - un pour les clients
  - un pour nos serveurs
  - un peu comme le réseau de l'école
- **des services proposés au sein du LAN des serveurs**
  - un serveur DNS
  - un serveur Web


## Sommaire

- [TP6 : Des bo services dans des bo LANs](#tp6--des-bo-services-dans-des-bo-lans)
  - [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. Le setup](#i-le-setup)
  - [0. Schéma](#0-schéma)
  - [1. Tableau d'adressage](#1-tableau-dadressage)
  - [2. Marche à suivre](#2-marche-à-suivre)
  - [II. LAN clients](#ii-lan-clients)
  - [1. Serveur DHCP](#1-serveur-dhcp)
  - [2. Client](#2-client)
- [III. LAN serveurzzzz](#iii-lan-serveurzzzz)
  - [1. Serveur Web](#1-serveur-web)
  - [2. Serveur DNS](#2-serveur-dns)
  - [3. Serveur DHCP](#3-serveur-dhcp)

# 0. Prérequis

➜ **une machine Rocky Linux prête à être clonée**

- on s'en servira pour tout ce qui est routeur/serveur etc.
- descendez bien à 1CPU et 1Go de RAM pour économiser des ressources

➜ **votre machine Ubuntu prête à être clonée**

- pour le(s) client(s) toujours !
- idem, limitez les ressources (1CPU, 1Go de RAM, 128Mo mémoire vidéo vu que c'est avec interface graphique)

➜ **Ayez [les mémos](../../cours/memo/README.md) sous le coude !**

# I. Le setup

## 0. Schéma

![Lab 1 TP6](./img/lab1_tp6.drawio.svg)

> *J'ai inclus votre PC en transparence juste pour bien vous rappeler qu'il est là dans les réseaux, qu'il a aussi une interface branchée aux switches host-only, et que c'est pour cette raison qu'il peut ping tout le monde et vous permettre le SSH.*

## 1. Tableau d'adressage

| Machine          | LAN1 `10.6.1.0/24` | LAN2 `10.6.2.0/24` |
|------------------|--------------------|--------------------|
| `dhcp.tp6.b1`    | `10.6.1.253`       | x                  |
| `client1.tp6.b1` | DHCP               | x                  |
| `routeur.tp6.b1`  | `10.6.1.254`       | `10.6.2.254`       |
| `web.tp6.b1`     | x                  | `10.6.2.11`        |
| `dns.tp6.b1`     | x                  | `10.6.2.12`        |
| Votre PC         | `10.6.1.1`         | `10.6.2.1`         |

## 2. Marche à suivre

➜ **Créez deux nouveaux host-only**

- attribuez à votre PC les adresses IP indiquées dans le tableau d'adressage

➜ **Créez toutes les machines virtuelles**

- clones dans tous les sens
- Ubuntu (ou OS de votre choix) pour `client1.tp6.b1`
- Rocky Linux pour tous les autres
- branchez-les aux bons host-only :D

➜ **Allumez tout et attribuez les adresses IP indiquées**

- sauf `client1.tp6.b1`, no need pour le moment
- configuration réseau sur toutes les machines :
  - adresse IP statique sur les cartes host-only
  - indiquez l'adresse IP de votre routeur comme passerelle
    - laquelle ? Celle qui est dans ton LAN !
    - par exemple, une machine du LAN1, bah sa passerelle c'est `10.6.1.254` (et PAS `10.6.2.254`)
  - indiquez `1.1.1.1` comme DNS
- n'oubliez pas d'activer le routage sur `routeur.tp6.b1`
- **NOMMEZ VOS MACHINES** (configuration du `hostname`, voir mémo)

☀️ **Prouvez que...**

- une machine du LAN1 peut joindre internet (ping un nom de domaine)
```
[nath@dhcp-tp6-b1 ~]$ ping www.ynov.com
PING www.ynov.com (104.26.10.233) 56(84) bytes of data.
64 bytes from 104.26.10.233 (104.26.10.233): icmp_seq=1 ttl=54 time=15.2 ms
64 bytes from 104.26.10.233 (104.26.10.233): icmp_seq=2 ttl=54 time=14.6 ms
64 bytes from 104.26.10.233 (104.26.10.233): icmp_seq=3 ttl=54 time=16.0 ms
64 bytes from 104.26.10.233 (104.26.10.233): icmp_seq=4 ttl=54 time=15.6 ms
^C
--- www.ynov.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 14.586/15.357/16.035/0.536 ms
```
- une machine du LAN2 peut joindre internet (ping nom de domaine)
```
[nath@Web-tp6-b1 ~]$ ping www.ynov.com
PING www.ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=1 ttl=54 time=17.6 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=2 ttl=54 time=14.9 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=3 ttl=54 time=17.0 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=4 ttl=54 time=15.9 ms
^C
--- www.ynov.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3008ms
rtt min/avg/max/mdev = 14.894/16.354/17.559/1.030 ms
```
- une machine du LAN1 peut joindre une machine du LAN2 (ping une adresse IP)
```
[nath@dhcp-tp6-b1 ~]$ ping 10.6.2.11
PING 10.6.2.11 (10.6.2.11) 56(84) bytes of data.
64 bytes from 10.6.2.11: icmp_seq=1 ttl=63 time=1.09 ms
64 bytes from 10.6.2.11: icmp_seq=2 ttl=63 time=2.04 ms
64 bytes from 10.6.2.11: icmp_seq=3 ttl=63 time=1.20 ms
^C
--- 10.6.2.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 1.087/1.442/2.037/0.422 ms
```

> *Prouvez que tout fonctionne quoi ! :d Je veux voir le nom des machines dans le prompt, vous **devez donc avoir nommé vos machines**.*

## II. LAN clients

Partie dédiée à la configuration du LAN1 : `10.6.1.0/24`. Un réseau dans lequel des clients pourraient se connecter, accéder à internet, et plus tard accéder à nos services internes (un ptit site web de fou, ce sera partie III).

## 1. Serveur DHCP

> *A faire sur `dhcp.tp6.b1`.*

Dans cette section, install + config du serveur DHCP. Pour que tous les clients du LAN1 puisse avoir un accès au réseau dès leur connexion !

➜ **Installez et configurez un serveur DHCP sur `dhcp.tp6.b1`**

- pareil qu'au TP5
- il doit attribuer des IPs entre `.37` et `.137`
- il doit indiquer la bonne passerelle pour les clients
- il doit indiquer `1.1.1.1` comme serveur DNS aux clients

## 2. Client

> *A faire sur `client1.tp6.b1`.*

➜ **Allumez `client1.tp6.b1` et configurez sa carte réseau en DHCP**

- il devrait récupérer automatiquement une adresse IP auprès de votre serveur DHCP
- et apprendre l'adresse de la passerelle de ce réseau
- et l'adresse d'un DNS utilisable

☀️ **Prouvez que...**

- le client a bien récupéré une adresse IP en DHCP
  - avec un `ip a` le mot-clé `dynamic` doit être écrit sur la ligne qui contient l'adresse IP
  - (y'a pas ce mot-clé quand tu définis une IP statique)
```
[nath@client1-tp6-b1 ~]$ ip a
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:47:97:75 brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.37/24 brd 10.6.1.255 scope global dynamic noprefixroute enp0s8
       valid_lft 244sec preferred_lft 244sec
    inet6 fe80::a00:27ff:fe47:9775/64 scope link
       valid_lft forever preferred_lft forever
```
- vous avez bien `1.1.1.1` en DNS
  - commande dans le mémo pour consulter l'adresse IP du serveur DNS connu
``` 
  [nath@client1-tp6-b1 ~]$ cat /etc/resolv.conf
  # Generated by NetworkManager
  nameserver 1.1.1.1
  
```
- vous avez bien la bonne passerelle indiquée
  - idem, dans le mémo, pour afficher l'adresse de la passerelle actuellement configurée :)
- que ça `ping` un nom de domaine public sans problème magueule
```
[nath@client1-tp6-b1 ~]$ ping www.google.com
PING www.google.com (172.217.19.36) 56(84) bytes of data.
64 bytes from mrs08s03-in-f4.1e100.net (172.217.19.36): icmp_seq=1 ttl=112 time=17.3 ms
64 bytes from ham02s11-in-f36.1e100.net (172.217.19.36): icmp_seq=2 ttl=112 time=16.4 ms
64 bytes from mrs08s03-in-f4.1e100.net (172.217.19.36): icmp_seq=3 ttl=112 time=17.7 ms
^C
--- www.google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 16.364/17.148/17.739/0.577 ms

```
# III. LAN serveurzzzz

Troisième partie, dédiée à la configuration de nos deux serveurs. Pour l'instant c'est juste des Rocky sans âme :d

## 1. Serveur Web

> *A faire sur `web.tp6.b1`.*

# III. 1. Serveur Web

- [III. 1. Serveur Web](#iii-1-serveur-web)
  - [1. Intro serveur Web](#1-intro-serveur-web)
  - [2. Install this shiet](#2-install-this-shiet)
    - [3. Analyse et test](#3-analyse-et-test)

## 1. Intro serveur Web

On appelle *serveur Web* une machine qui exécute un programme qu'on appelle un *service Web*.

Un *service Web* est un programme qui écoute sur un port, attend la connexion de clients.

> *Par convention, le port sur lequel écoute les services Web HTTP c'est 80/tcp.*

Un client est supposé se connecter au port envoyer une requête HTTP, et le serveur renverra alors une page HTML qui correspond à la requête du client.

> Comme `nc` aux premiers TPs, sauf que quand tu te connectes, et que t'envoies la bonne requête, bah le service Web répond automatiquement une page HTML.

On va utiliser un service Web qui s'appelle NGINX, réputé pour ses performances et sa stabilité !

## 2. Install this shiet

> *On est sur notre (futur) beau serveur Web `web.tp6.b1`.*

On est pas en cours de Linux, ni en cours de dév Web, alors on va s'intéresser à la partie réseau uniquement, et limiter au maximum les interactions spécifiques à Linux.

➜ **Installez et démarrez le service web NGINX**

```bash
# installation du service NGINX
sudo dnf install -y nginx

# démarrage du service NGINX
sudo systemctl enable --now nginx
```

Devrait po y avoir d'erreurs, et ça tourne !

> Le service web NGINX propose une page d'accueil (toute moche) par défaut. On aura même pas besoin d'écrire une vieille page HTML pour tester :D Il permet si on le configure d'héberger les sites Web de notre choix dans une utilisation réelle.

### 3. Analyse et test

☀️ **Déterminer sur quel port écoute le serveur NGINX**

- commande dans le mémooooooo
- isolez la ligne intéressante avec un `... | grep <PORT>` une fois que vous avez repéré le port
```
[nath@Web-tp6-b1 ~]$ sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=11179,fd=6),("nginx",pid=11178,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=11179,fd=7),("nginx",pid=11178,fd=7))
```

> *C'est le port conventionnel ! Pour un service Web, qui reçoit donc des requêtes HTTP, c'est ce port qui est la convention :) C'est aussi écrit dans la configuration quelque port qu'il doit écouter sur ce port. Conf dans le dossier `/etc/nginx/` pour les curieux.*

☀️ **Ouvrir ce port dans le firewall**

- bien que NGINX soit en train d'écouter sur le port repéré, et attende la connexion de clients potentiels...
- ... ça empêche pas le firewall de bloquer les clients si on ouvre pas le port
- commande dans le mémooooo pour faire ça aussi
```
[nath@Web-tp6-b1 ~]$ sudo firewall-cmd --permanent --add-port=80/tcp
[sudo] password for nath:
success
[nath@Web-tp6-b1 ~]$ sudo firewall-cmd --reload
success
```


☀️ **Visitez le site web !**

- depuis `client1.tp6.b1`, ouvrez un navigateur, et visitez `http://10.6.2.11` (l'adresse IP du serveur web `web.tp6.b1`)
- normalement, page d'accueil !
  - sinon, tu t'es planté, vérifie tout !
- pour le compte-rendu, depuis le terminal de `client1.tp6.b1` : `curl http://10.6.2.11`
  - vous me mettez que les premières lignes du résultat SVP :D
  - la commande `curl` envoie une requête HTTP (comme un navigateur), en réponse on tout l'HTML de la page... bonne bouillie :D
```
[nath@client1-tp6-b1 ~]$ sudo curl http://10.6.2.11
[sudo] password for nath:
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/

      html {
        height: 100%;
        width: 100%;
      }
        body {
  background: rgb(20,72,50);
  background: -moz-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%)  ;
  background: -webkit-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%) ;
  background: linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%);
  background-repeat: no-repeat;
  background-attachment: fixed;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#3c6eb4",endColorstr="#3c95b4",GradientType=1);
        color: white;
        font-size: 0.9em;
        font-weight: 400;
        font-family: 'Montserrat', sans-serif;
        margin: 0;
        padding: 10em 6em 10em 6em;
        box-sizing: border-box;

      }


  h1 {
    text-align: center;
    margin: 0;
    padding: 0.6em 2em 0.4em;
    color: #fff;
    font-weight: bold;
    font-family: 'Montserrat', sans-serif;
    font-size: 2em;
  }
  h1 strong {
    font-weight: bolder;
    font-family: 'Montserrat', sans-serif;
  }
  h2 {
    font-size: 1.5em;
    font-weight:bold;
  }

  .title {
    border: 1px solid black;
    font-weight: bold;
    position: relative;
    float: right;
    width: 150px;
    text-align: center;
    padding: 10px 0 10px 0;
    margin-top: 0;
  }

  .description {
    padding: 45px 10px 5px 10px;
    clear: right;
    padding: 15px;
  }

  .section {
    padding-left: 3%;
   margin-bottom: 10px;
  }

  img {

    padding: 2px;
    margin: 2px;
  }
  a:hover img {
    padding: 2px;
    margin: 2px;
  }

  :link {
    color: rgb(199, 252, 77);
    text-shadow:
  }
  :visited {
    color: rgb(122, 206, 255);
  }
  a:hover {
    color: rgb(16, 44, 122);
  }
  .row {
    width: 100%;
    padding: 0 10px 0 10px;
  }

  footer {
    padding-top: 6em;
    margin-bottom: 6em;
    text-align: center;
    font-size: xx-small;
    overflow:hidden;
    clear: both;
  }

  .summary {
    font-size: 140%;
    text-align: center;
  }

  #rocky-poweredby img {
    margin-left: -10px;
  }

  #logos img {
    vertical-align: top;
  }

  /* Desktop  View Options */

  @media (min-width: 768px)  {

    body {
      padding: 10em 20% !important;
    }

    .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6,
    .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
      float: left;
    }

    .col-md-1 {
      width: 8.33%;
    }
    .col-md-2 {
      width: 16.66%;
    }
    .col-md-3 {
      width: 25%;
    }
    .col-md-4 {
      width: 33%;
    }
    .col-md-5 {
      width: 41.66%;
    }
    .col-md-6 {
      border-left:3px ;
      width: 50%;


    }
    .col-md-7 {
      width: 58.33%;
    }
    .col-md-8 {
      width: 66.66%;
    }
    .col-md-9 {
      width: 74.99%;
    }
    .col-md-10 {
      width: 83.33%;
    }
    .col-md-11 {
      width: 91.66%;
    }
    .col-md-12 {
      width: 100%;
    }
  }

  /* Mobile View Options */
  @media (max-width: 767px) {
    .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6,
    .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
      float: left;
    }

    .col-sm-1 {
      width: 8.33%;
    }
    .col-sm-2 {
      width: 16.66%;
    }
    .col-sm-3 {
      width: 25%;
    }
    .col-sm-4 {
      width: 33%;
    }
    .col-sm-5 {
      width: 41.66%;
    }
    .col-sm-6 {
      width: 50%;
    }
    .col-sm-7 {
      width: 58.33%;
    }
    .col-sm-8 {
      width: 66.66%;
    }
    .col-sm-9 {
      width: 74.99%;
    }
    .col-sm-10 {
      width: 83.33%;
    }
    .col-sm-11 {
      width: 91.66%;
    }
    .col-sm-12 {
      width: 100%;
    }
    h1 {
      padding: 0 !important;
    }
  }


  </style>
  </head>
  <body>
    <h1>HTTP Server <strong>Test Page</strong></h1>

    <div class='row'>

      <div class='col-sm-12 col-md-6 col-md-6 '></div>
          <p class="summary">This page is used to test the proper operation of
            an HTTP server after it has been installed on a Rocky Linux system.
            If you can read this page, it means that the software is working
            correctly.</p>
      </div>

      <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>


        <div class='section'>
          <h2>Just visiting?</h2>

          <p>This website you are visiting is either experiencing problems or
          could be going through maintenance.</p>

          <p>If you would like the let the administrators of this website know
          that you've seen this page instead of the page you've expected, you
          should send them an email. In general, mail sent to the name
          "webmaster" and directed to the website's domain should reach the
          appropriate person.</p>

          <p>The most common email address to send to is:
          <strong>"webmaster@example.com"</strong></p>

          <h2>Note:</h2>
          <p>The Rocky Linux distribution is a stable and reproduceable platform
          based on the sources of Red Hat Enterprise Linux (RHEL). With this in
          mind, please understand that:

        <ul>
          <li>Neither the <strong>Rocky Linux Project</strong> nor the
          <strong>Rocky Enterprise Software Foundation</strong> have anything to
          do with this website or its content.</li>
          <li>The Rocky Linux Project nor the <strong>RESF</strong> have
          "hacked" this webserver: This test page is included with the
          distribution.</li>
        </ul>
        <p>For more information about Rocky Linux, please visit the
          <a href="https://rockylinux.org/"><strong>Rocky Linux
          website</strong></a>.
        </p>
        </div>
      </div>
      <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
        <div class='section'>

          <h2>I am the admin, what do I do?</h2>

        <p>You may now add content to the webroot directory for your
        software.</p>

        <p><strong>For systems using the
        <a href="https://httpd.apache.org/">Apache Webserver</strong></a>:
        You can add content to the directory <code>/var/www/html/</code>.
        Until you do so, people visiting your website will see this page. If
        you would like this page to not be shown, follow the instructions in:
        <code>/etc/httpd/conf.d/welcome.conf</code>.</p>

        <p><strong>For systems using
        <a href="https://nginx.org">Nginx</strong></a>:
        You can add your content in a location of your
        choice and edit the <code>root</code> configuration directive
        in <code>/etc/nginx/nginx.conf</code>.</p>

        <div id="logos">
          <a href="https://rockylinux.org/" id="rocky-poweredby"><img src="icons/poweredby.png" alt="[ Powered by Rocky Linux ]" /></a> <!-- Rocky -->
          <img src="poweredby.png" /> <!-- webserver -->
        </div>
      </div>
      </div>

      <footer class="col-sm-12">
      <a href="https://apache.org">Apache&trade;</a> is a registered trademark of <a href="https://apache.org">the Apache Software Foundation</a> in the United States and/or other countries.<br />
      <a href="https://nginx.org">NGINX&trade;</a> is a registered trademark of <a href="https://">F5 Networks, Inc.</a>.
      </footer>

  </body>
</html>
```

## 2. Serveur DNS

> *A faire sur `dns.tp6.b1`.*

[**Document dédié au setup serveur DNS**](./dns.md)

## 3. Serveur DHCP

What ?! Again ?!

Nan nan juste, on va aller changer une ligne dans la configuration de `dhcp.tp6.b1`.

Bah oui ! On a notre propre serveur DNS maintenant ! Faut le dire à nos clients qu'ils peuvent l'utiliser :d

➜ **Editez la configuration du serveur DHCP sur `dhcp.tp6.b1`**

- faut qu'il file l'adresse IP `10.6.2.12` comme DNS à tous les nouveaux clients !

☀️ **Créez un nouveau client `client2.tp6.b1` vitefé**

- supprimez le `client1` si vous voulez, on a pu besoin
- récupérez une IP en DHCP sur ce nouveau `client2.tp6.b1`
- vérifiez que vous avez bien `10.6.2.12` comme serveur DNS à contacter
  - commande dans le mémo pour voir le serveur DNS connu actuellement

➜ **Vous devriez pouvoir visiter `http://web.tp6.b1` avec le navigateur, ça devrait fonctionner sans aucune autre action.**

