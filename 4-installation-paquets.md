# Installation de logiciels

## Installation avec dpkg


```bash
$ wget https://launchpad.net/ubuntu/+archive/primary/+files/system-config-samba_1.2.63-0ubuntu4_all.deb
$ dpkg -i system-config-samba_1.2.63-0ubuntu4_all.deb

```

Une erreur se produit

Et si on relance apt-get install, on est bloqué:

```bash
$ sudo apt-get install python-libuser
Reading package lists... Done
Building dependency tree       
Reading state information... Done
You might want to run 'apt-get -f install' to correct these:
The following packages have unmet dependencies:
 python-libuser : Depends: libuser1 but it is not going to be installed
 system-config-samba : Depends: python-central (>= 0.6.7) but it is not installable
E: Unmet dependencies. Try 'apt-get -f install' with no packages (or specify a solution).
```

Pour corriger cela, on désinstalle: 

```bash
sudo dpkg -P system-config-samba 


