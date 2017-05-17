# Initialisation

## Installation sudo
```bash
apt-get install sudo
adduser laurent sudo
sudo -u laurent echo bonjour
```
Se déconnecter de l'utilisateur root

Se déconnecter de l'utilisateur laurent

Se reconnecter

## Installation ne

Editer le fichier `/etc/apt/sources.list`:

```bash
sudo nano /etc/apt/sources.list
```
Commenter les lignes commençant par `deb http://security.debian.org` et `deb-src http://security.debian.org/`

Rajouter les lignes depuis https://wiki.debian.org/fr/SourcesList

```
deb http://deb.debian.org/debian jessie main contrib non-free
deb-src http://deb.debian.org/debian jessie main contrib non-free

deb http://deb.debian.org/debian jessie-updates main contrib non-free
deb-src http://deb.debian.org/debian jessie-updates main contrib non-free

deb http://security.debian.org/ jessie/updates main contrib non-free
deb-src http://security.debian.org/ jessie/updates main contrib non-free
```

### Mise à jour apt

```bash
sudo apt-get update
```

### Installation

```bash
sudo apt-get install ne
```
