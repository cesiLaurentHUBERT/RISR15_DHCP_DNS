# Installation de Java
## Introduction

L'environnement d'exécution Java est nécessaire pour faire tourner certains serveurs (Tomcat, JBoss, Wildfly).

Il est nécessaire de l'installer en ajoutant de nouveaux dépôts car l'implémentation fournie par Oracle est réputée plus stable et mieux implémenter que la version libre de Java.

## Installation
### Ajout des dépôts

On se connecte en tant que root:

```bash
su -
echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee /etc/apt/sources.list.d/webupd8team-java.list
echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
```

Si une erreur survient, de ce type:

```
Executing: /tmp/apt-key-gpghome.keE0d6X51y/gpg.1.sh --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
gpg: failed to start the dirmngr '/usr/bin/dirmngr': No such file or directory
gpg: connecting dirmngr at '/tmp/apt-key-gpghome.keE0d6X51y/S.dirmngr' failed: No such file or directory
gpg: keyserver receive failed: No dirmngr
```

Alors exécuter les commandes suivantes:

```bash
apt-get remove gnupg
apt-get install --reinstall gnupg2
```

Puis relancer la commande:
```bash
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
```

### Installation des paquets

```bash
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

### Sélection du paquet par défaut
Pour sélectionner la version de Java, il peut être nécessaire de lancer la commande suivante:

```bash
sudo apt-get install oracle-java8-set-default
```
