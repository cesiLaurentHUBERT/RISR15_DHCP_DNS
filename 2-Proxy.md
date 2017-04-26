# Installation d'un serveur Proxy

## Introduction

Nous allons nous baser sur le [serveur DHCP et le serveur DNS installés précédemment](informatique:linux:services_dhcp_dns).

## Désactivation de la passerelle Internet

Nous allons faire en sorte que la passerelle Internet ne fonctionne plus pour que tout flux réseau vers Internet transite obligatoirement par notre proxy.

A vous de trouver comment réaliser cela, d'après ce que nous avons vu précédemment.

## Installation et configuration de base de Squid

### Installation
L'installation se fait en installant le paquet `squid3`

Faire une sauvegarde du fichier de configuration `/etc/squid3/squid.conf`

### Configuration

#### Préparation
Modifier le fichier `/etc/squid3/squid.conf`

Pour ce faire, nous allons nous débarrasser des lignes de commentaires (elles sont dans le fichier de sauvegarde):



```
$ grep -v -e '^#' -e '^$' /etc/squid3/squid.conf | sudo \
tee /etc/squid3/squid.new

$ sudo mv /etc/squid3/squid.new etc/squid3/squid.conf
```

Analysez les lignes précédentes: 

* que se passe-t-il ? 
* A quoi sert la commande `tee`? 
* Que se passe-t-il si on utilise une redirection simple (avec le signe `>`) ?

#### Fichier de configuration
```
acl SSL_ports port 443
acl Safe_ports port 80          # http
acl Safe_ports port 21          # ftp
acl Safe_ports port 443         # https
acl Safe_ports port 70          # gopher
acl Safe_ports port 210         # wais
acl Safe_ports port 1025-65535  # unregistered ports
acl Safe_ports port 280         # http-mgmt
acl Safe_ports port 488         # gss-http
acl Safe_ports port 591         # filemaker
acl Safe_ports port 777         # multiling http
acl CONNECT method CONNECT
http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports
http_access allow localhost manager
http_access deny manager

http_access deny to_localhost


# We strongly recommend the following be uncommented to protect innocent
# web applications running on the proxy server who think the only
# one who can access services on "localhost" is a local user
#http_access deny to_localhost

#
# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
#

acl localnet src 172.16.81.0/24

# Example rule allowing access from your local networks.
# Adapt localnet in the ACL section to list your (internal) IP networks
# from where browsing should be allowed
http_access allow localnet

http_access allow localhost
# And finally deny all other access to this proxy
http_access deny all


#Disable IP-forwarding:
forwarded_for off

#Définition des logs
cache_access_log /var/log/squid/access.log
cache_log /var/log/squid/cache.log
cache_store_log /var/log/squid/store.log

http_port 3128
coredump_dir /var/spool/squid3
refresh_pattern ^ftp:           1440    20%     10080
refresh_pattern ^gopher:        1440    0%      1440
refresh_pattern -i (/cgi-bin/|\?) 0     0%      0
refresh_pattern .               0       20%     4320
```

Dans ce fichier de configuration, analysez chacune des lignes et indiquez son utilité


### Redémarrage de squid3

Quelle commande permet de redémarrer squid3 ?

Testez et vérifier que tout marche

Vous devriez constater que ce n'est pas le cas. Déterminez quelles opérations doivent être faites pour corriger les problèmes.


## Configuration des clients

Modifiez les clients pour qu'ils pointent vers le serveur proxy. Son FQDN sera proxy.exemple.cesi: modifiez la configuration de votre réseau en conséquence pour obtenir cela.


## Ajout de l'authentification

Nous allons faire en sorte que seuls les clients authentifiés puissent se connecter à internet. Voici un exemple de configuration pour faire cela:

```
auth_param basic program /usr/lib/squid3/basic_ncsa_auth /etc/squid3/passwords
auth_param basic realm ProxyCESI
acl authenticated proxy_auth REQUIRED
http_access allow authenticated

# Choose the port you want. Below we set it to default 3128.
http_port 3128
```

L'ajout d'une paire utilisateur/mot de passe se fait de la manière suivante:

```
sudo htpasswd -c -d /etc/squid3/passwords nom_utilisateur_a_ajouter
sudo service squid3 restart
```

Faites bien attention au warning et en déduire ce qu'il convient de faire concernant cette configuration.

Modifiez votre configuration en conséquence et testez-là


Vous devrez configurer les mots de passe pour les utilisateurs suivants:

* intervenant
* votre utilisateur
* Pierre
* Paul


## Filtrage

### Choix

Une des fonctions possibles d'un proxy est de filtrer le contenu autorisé ou non. Plusieurs solutions existent. Parmi elles, DansGuardian et SquidGuard.

1. Essayez de trouver d'autres alternatives à ces deux solutions
2. Pour chacune de ces solutions indiquez quelles sont leurs avantages et inconvénients en terme :

  1. de fonctionnalités
  2. de performances (ressources système consommées)


### Installation

Faites une préconisation par rapport aux avantages et inconvénients trouvés.

Installez l'applicatif choisi en y configurant une liste de serveurs autorisés/interdits



## Référence

[Finalisation de l'installation](informatique:linux:debian_squid_solutions)



### Liens

* https://memo-linux.com/installer-un-proxy-squid-et-un-filtrage-avec-squidguard-sous-debian/
* http://www.server-world.info/en/note?os=Debian_8&p=squid&f=3
* http://www.brennan.id.au/11-Squid_Web_Proxy.html



