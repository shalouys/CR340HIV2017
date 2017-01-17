# CR340

###Systèmes d'exploitation

### Cours 2 - Les systèmes d'exploitation modernes

---

# Les systèmes d'exploitation modernes

--
## Catégories de systèmes d'exploitation

* Systèmes d'exploitation serveur
* Systèmes d'exploitation poste de travail
* Systèmes d'exploitation mobile
* Systèmes d'exploitation embarqués

--
## Systèmes d'exploitation serveur
Fonctionnalités recherchées:
* Sécurité
* Performance
* Focus minimalisation des fonctionnalités
* Integration aux hyperviseurs et conteneriseurs

*Exemples*: Windows 2012, Linux
--
## Systèmes d'exploitation poste de travail
Fonctionnalités recherchées:
* Facilité d'utilisation
* Convivialité de l'interface graphique
* Intégration de fonctionnalités
* Support matériel

*Exemples*: Windows 10, Max OS X
--
## Systèmes d'exploitation mobiles:
Fonctionnalités recherchées:
* Facilité d'utilisation
* Support d'application

*Exemples*: Android, IOS
--
## Systèmes d'exploitation embarqués
Fonctionnalités recherchées:
* Minimisation de la taille
* API d'accès direct au matériel
* Performance

*Exemples*: RealRTOS, MBED
---
# Linux
--
## Distributions
Différentes organisations distribuent des versions différentes de Linux.  Toutes ces versions sont basées sur le même noyau, mais utilisent des logiciels et des éléments de configuration différents.  Les grandes classes de distribution disponibles sur le marché actuel sont les suivantes:

* Debian: Ubuntu, Debian,Knoppix, etc.
* Red Hat: RHEL, CentOS
* Distribution minimale:  CoreOS, Atomic, Alpine
--
## Structure de répertoire
* `/`: Racine du système
* `/etc`: Fichiers de configuration
* `/dev`: Fichier d'accès au matériel
* `/proc`: Fichier d'accès au noyau
* `/bin`: fichiers binaires exécutables systèmes
* `/sbin`: fichiers binaires statiques exécutables système
* `/lib`: bibliothèque systèmes
* `/usr/bin`: fichiers binaires exécutables utilisateur
* `/usr/sbin`: fichiers binaires statiques exécutables utilisateur
* `/usr/lib`: Bibliothèques utilisateur
* `/var`: Fichiers de support
* `/home`: Répertoires personnels des utilisateurs.

--
## Gestion des logiciels
Les logiciels sous Linux peuvent être installés soit manuellement soit par l'utilisation d'un gestionnaire d'installation.

Les installations manuelles dépendent de l'installation des bons outils de développement et bibliothèques.

Les gestionnaires d'installation varient d'une distribution à l'autre.  Les plus populaires sont apt (Debian) et yum (Red Hat).
--
## Configuration de base (démo)
---
# Windows
--
## Versions

* Poste de travail: Windows 10
* Serveur: Windows 2012 R2

--
## Structure de répertoire

* `c:\`: Racine du système
* `c:\Windows`: Fichiers du système d'exploitation
* `c:\Program Files`: Fichiers des applications
--
## Base de registre

La base de registre est utilisée pour stocker de l'information de configuration pour le système d'exploitation et les applications.

La base de registre stocke des couples clé/valeur, où la clé est représentée dans une arborescence.  Les sections principales de l'arborescence sont:

* `HKEY_CLASSES_ROOT`: Description des classes de fichiers
* `HKEY_LOCAL_MACHINE`: Configuration globale de la machine
* `HKEY_CURRENT_USER`: Configuration propre à l'utilisateur courant
* `HKEY_USERS`: Configuration des autres utilisateurs
* `HKEY_CURRENT_CONFIG`: Configuration globale liée ni à la machine, ni à un utilisateur
--
## Gestion des logiciels

Trois méthodes existent pour installer des logiciels dans Windows:

* Installation manuelle:  Installation à l'aide d'un fichier installeur.  L'API de Windows offre des fonctionnalités de gestion des installations et désinstallation, rendant la création de fichiers installeurs simples.
* App Store:  Depuis Windows 8, Windows permet l'installation d'application gratuite et payant depuis son App Store.
* Fonctionalités et rôles:  Dans Windows Server, l'installation des modules du système d'exploitation se font via l'ajout de fonctionnalité et de rôles dans Server Manager.
--
## Configuration de base (démo)
---
# MacOS X
--
## Versions
Max OS X existe seulement en version poste de travail, avec un module Serveur optionnel ajoutant certaines fonctionnalités.

La version actuelle de Mac OS X est Sierra, 10.12.0

Mac OS X suit un cycle de sortie d'un an.
--
## Structure de répertoire

La structure de répertoire de Mac OS X est basée sur BSD est très similaire à celle de Linux:

* `/`: Racine du système
* `/etc`: Fichiers de configuration
* `/dev`: Fichier d'accès au matériel
* `/Applications`: Répertoire d'installation des applications
* `/bin`: fichiers binaires exécutables systèmes
* `/sbin`: fichiers binaires statiques exécutables système
* `/Library`: bibliothèque systèmes
* `/usr/bin`: fichiers binaires exécutables utilisateur
* `/usr/sbin`: fichiers binaires statiques exécutables utilisateur
* `/usr/lib`: Bibliothèques et configuration des applications
* `/var`: Fichiers de support
* `/Users`: Répertoires personnels des utilisateurs.
* `/Users/[username]\Library`: Bibliothèques et configuration propres à l'utilisateur
--
## Gestion des logiciels

Mac OS X permet l'installation manuelle d'application, soit à l'aide d'un fichier installeur (si il y a des dépendances ou bibliothèques) ou en copiant directement l'application dans le répertoire `/Applications` (pour les applications individuelles)

On peut installer des logiciels à travers l'app store.

De manière additionnelle, on peut installer un gestionnaire de logiciel sur Mac OS X pour aller chercher des fonctionnalités similaires à apt et yum.  Le plus populaire est Homebrew (http://brew.sh/)
--
## Configuration de base (démo)

---
# Laboratoire
--
## Machines virtuelles
* ubuntu: Ubtuntu 16.04 Xenial
* centos: Centos 7
* windows: Windows 2012 R2 Standard
* osx: Mac OS X El Capitan
--
## Objectifs du laboratoire:
* Configurer le réseau sur l'interface secondaire
* S'assurer que les machines peuvent communiquer entre elles sur le réseau
* Installer Gnome sur la machine Centos
* Installer Brew sur la machine OS X
* Installer le rôle IIS sur la machine Windows
