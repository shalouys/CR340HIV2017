# CR340

###Systèmes d'exploitation

### Cours 10 - SELinux - Demo et lab
---
# Rappel
--
## Concepts
![SELinux](images/SELinux.jpg)
---
# Demo
--
## Manipuler le mode
* `sestatus`
* `getenforce`
* `setenfoce`
* Fichier `/etc/sysconfig/selinux`
--
## Manipuler la politique active
* `semodule`
* `semanage`
* `getsetbool`
* `setsebool`
--
## Manipuler l'accès au système de fichiers
* `ls -Z`
* `ps -Z`
* `chcon`
* `restorecon`
* `semanage`
* `sesearch`
--
## Manipuler les utilisateurs
* `semanage`
* `id -Z`
* `seinfo`
* `ausearch`
---
# Laboratoire évalué
--
## Laboratoire évaluer (20%)

A partir du fichier Vagrant du cours 10, configuré un CentOS 7 avec Selinux pour avoir les propriété suivantes:
* 2 instances de http (port 80 et 8080)
* Un répertoire racine pour chacune des instances
* Permissions SELinux empêchant un des serveurs de lire de répertoire racine de l'autre serveur
--
##A remettre:
* Liste des commandes utilisées et explication de la commande
* Capture d'écran du laboratoire qui fonctionne
